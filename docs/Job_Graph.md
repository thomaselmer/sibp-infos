# paragraph82b — Job Graph

Status: current, describes the job pool after T-35 (submodule `1c32b2b`) · 2026-11-26

How the scheduling core of the processing runtime links jobs. Code: `backend/app/modules/processing/engine/graph.py`. The same diagrams as a page: `Job_Graph.html`, published as an artifact:
https://claude.ai/artifact/6xjiJ2m2AQugA58F12gza8. Runtime in general: [`Architecture.md`](Architecture.md), section 5;
tables `JOB`, `JOB_TARGET`, `JOB_DEPENDENCY`: [`Data_Model.md`](Data_Model.md).

## 1. Where the graph sits

The graph is pure logic: no database, no Redis. The engine (one per project) is the shell around it. It writes a
decision to PostgreSQL first and only then tells the graph, so the graph only ever holds committed state.

```mermaid
flowchart LR
  EV["User event<br/>e.g. Verarbeitung starten"] --> ENG
  subgraph ENG["Engine (one per project)"]
    direction TB
    H["Event handler / job type<br/>returns new jobs"] --> DB1["Write jobs to PostgreSQL"]
    DB1 --> GA["graph.add"]
    GA --> GR["graph.runnable(free slots)"]
    GR --> DB2["Mark queued in PostgreSQL"]
    DB2 --> GS["graph.sent<br/>(attempt + 1)"]
    RES["Result arrives"] --> GD["graph.decide<br/>→ transitions"]
    GD --> AP["apply_result writes result<br/>(savepoint) + follow-up jobs"]
    AP --> DB3["Write transitions to PostgreSQL"]
    DB3 --> GC["graph.commit"]
    GC --> GA
  end
  GS --> LQ[("Redis lane queue<br/>one per provider")]
  LQ --> W["Worker<br/>runs the task, no database"]
  W --> RS[("Redis result stream<br/>one per project")]
  RS --> RES
```

## 2. What one job goes through

Status in the database: `waiting`, `queued`, `done`, `failed`; "running" exists only in Redis. Done and failed jobs
leave the graph and stay in the database. New in T-35: the attempt cap (`max_attempts` in the configuration), a
result that cannot be written fails one job instead of stopping the project, and a job waiting on a failed job fails
too instead of waiting forever.

```mermaid
stateDiagram-v2
  [*] --> waiting: created
  waiting --> queued: runnable (dependencies done, targets free) → sent, attempt + 1
  queued --> done: result OK
  queued --> waiting: 1st failure → timeout × 2
  queued --> waiting: lease expired → same timeout
  queued --> failed: 2nd failure
  queued --> failed: attempts reached max_attempts
  queued --> failed: result cannot be written (no retry)
  waiting --> failed: a job it waits on failed
  done --> [*]
  failed --> [*]
```

## 3. How jobs block each other

- **Dependencies** (`JOB_DEPENDENCY`): "B may only start after A is done". Not used by any job type yet;
  `decide_border` (T-21) will be the first, waiting for the reading of every page in its window.
- **Targets** (`JOB_TARGET`): a job reads or writes objects (a file, a page). It only starts if none of its write
  targets is in use and none of its read targets is being written.
- Among runnable jobs: lower priority first (0 = "Apply changes", 1 = normal), then creation order.

Example: only `embed_page` of page 2 can start (thick border); `embed_page` of page 3 waits for its target, the
border waits for the reading of page 3.

```mermaid
flowchart LR
  subgraph done["done"]
    O1["ocr_page<br/>page 1"]
    O2["ocr_page<br/>page 2"]
  end
  subgraph pool["in the pool"]
    O3["ocr_page<br/>page 3<br/>writes page 3"]
  end
  subgraph waiting["waiting"]
    E3["embed_page<br/>page 3<br/>writes page 3"]
    E2["embed_page<br/>page 2<br/>writes page 2"]
    B23["decide_border 2|3<br/>(T-21, later)<br/>reads pages 1–4"]
  end
  O3 -. "target busy:<br/>page 3 is written" .-> E3
  O1 -- "dependency done" --> B23
  O2 -- "dependency done" --> B23
  O3 -- "dependency open" --> B23
  E2:::ready
  classDef ready stroke-width:3px
```

## 4. Job types and how they follow each other

Today the chain runs through follow-ups: when a job's result is written, its job type returns the next jobs. Dashed:
planned in the slices T-21 to T-24.

```mermaid
flowchart LR
  SR(["Verarbeitung starten"]) --> IF["ingest_file<br/>per file · writes file"]
  SR -. "catch-up: pages not read<br/>or not embedded" .-> OP
  IF --> OP["ocr_page<br/>per page · writes page"]
  OP -- "if process + text" --> EP["embed_page<br/>per page · writes page"]
  OP -.-> DB["decide_border<br/>per pair of pages"]
  DB -.-> AD["assemble_document<br/>per stretch"]
  AD -.-> FS["find_sections<br/>per document"]
  FS -.-> CD["classify_document<br/>per document"]
```

## 5. Inside each job type

Every job type has two halves: the **engine part** (`load_inputs` reads what the task needs from PostgreSQL,
`apply_result` writes the result and returns the follow-up jobs) and the **worker part** (the task: no database,
reads and writes files in Garage, calls models). Rate limits, unavailable or busy models put a job back to be tried
later (`TryLater`), which the graph treats like an expired lease.

### ingest_file

```mermaid
flowchart TB
  subgraph E1["Engine: load_inputs"]
    A1["File: path, type"]
  end
  subgraph W["Worker task (no database)"]
    B1["Read the original from Garage"] --> B2{"PDF or image?"}
    B2 -- "PDF" --> B3["Split into pages, render each<br/>(limits: pages per PDF, pixels)"]
    B2 -- "image" --> B4["One page"]
    B3 --> B5["Store each page image + preview<br/>one at a time"]
    B4 --> B5
  end
  subgraph E2["Engine: apply_result"]
    C1["Add PAGE rows"] --> C2{"Page within the<br/>tier's page limit?"}
    C2 -- "yes" --> C3["Follow-up: ocr_page"]
    C2 -- "no" --> C4["Stored, not read"]
  end
  A1 --> B1
  B5 --> C1
```

### ocr_page

The worker part is a LangGraph flow: read, clean, turn and read again while the page is rotated (at most `max_turns`).

```mermaid
flowchart TB
  subgraph E1["Engine: load_inputs"]
    A1["Page: image key"]
  end
  subgraph W["Worker task: LangGraph flow"]
    R["read: shrink image (image_max_side, JPEG),<br/>ask the model for text, features,<br/>classification, rotation"] --> CL["clean: strip trailing field dumps<br/>from the text"]
    CL --> Q{"rotated and<br/>turns left<br/>(max_turns)?"}
    Q -- "yes" --> T["turn the image upright"] --> R
    Q -- "no" --> S["if turned: store the turned image<br/>+ preview under its own name"]
    R -. "rate limit, unavailable,<br/>model busy" .-> TL["try later"]
  end
  subgraph E2["Engine: apply_result"]
    C1["Write text, classification,<br/>PAGE_FEATURES (incl. 11 title_*),<br/>turned image; clear the embedding"] --> C2{"process<br/>and text?"}
    C2 -- "yes" --> C3["Follow-up: embed_page"]
    C2 -- "no" --> C4["Done"]
  end
  A1 --> R
  S --> C1
```

### embed_page

```mermaid
flowchart TB
  subgraph E1["Engine: load_inputs"]
    A1["Page: text"]
  end
  subgraph W["Worker task"]
    B1["Model chain: embedding model, then its fallbacks"] --> B2["Each model cuts the text<br/>to max_input_tokens (tiktoken)"]
    B2 --> B3["Embed (holds a slot of the model)"]
    B3 --> B4{"vector has<br/>1536 values?"}
    B4 -- "no" --> B5["invalid answer: job fails"]
    B3 -. "rate limit, unavailable,<br/>model busy" .-> TL["try later"]
  end
  subgraph E2["Engine: apply_result"]
    C1["Write PAGE.embedding<br/>and embedding_model"]
  end
  A1 --> B1
  B4 -- "yes" --> C1
```

## Graph operations

| Operation | What it does |
|---|---|
| `add` | Adopts new or loaded jobs with their open dependencies and targets |
| `runnable(limit)` | Waiting jobs with all dependencies done and targets free, by priority then age, at most the free slots |
| `sent` | Marks jobs as in the pool under a new attempt |
| `decide` | Turns a result (done, failed, lease expired) into transitions; ignores older attempts |
| `fail` | Lasting failure without retry, e.g. a result that cannot be written |
| `commit` | Applies transitions after the database is written; done jobs free their dependents |
| `drop` | Forgets deleted jobs (stage reset) |
| `is_idle` | Nothing in the pool and nothing that could run: the engine may be unloaded |
