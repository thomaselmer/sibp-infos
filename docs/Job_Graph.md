# paragraph82b — Job Graph

Status: **draft**, describes T-35 (branch `engine-robustness` in the source submodule, not merged yet) · 2026-11-26

How the scheduling core of the processing runtime links jobs. Code: `backend/app/modules/processing/engine/graph.py`
(after the merge of T-35). The same diagrams as a page: `Job_Graph.html`, published as an artifact:
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
