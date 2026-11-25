# Page embeddings and title features (T-20)

Spec, 2026-11-25. Slice 1 of `Grouping_and_Classification.md`. Status: `[ready-for-agent]`.
Decisions behind it: data model v42, architecture v39 (sections 6 and 7).

## Problem Statement

After a run, a project's pages are read (text, page features, classification), but nothing more happens to them.
The later steps need two things that are missing:

- **Embeddings:** checking for duplicate pages and, later, searching for evidence need a vector per page. No page
  gets one; there is no `embed_page` job type and no way to call an embedding model.
- **Title features:** document classification and a later trained model need all 11 `title_*` features on every
  page. `ocr_page` reads none of them, and only 5 of the 11 columns exist.

## Solution

- `ocr_page` reads all 11 `title_*` features on every page in its existing model call and stores them.
- A new job type `embed_page` runs after `ocr_page` for every page classified `process` with text. It embeds the
  page text with the configured embedding model (`text-embedding-3-small`, 1536 dimensions) and stores the vector
  with the model's name. Pages classified `keep` or `discard`, and pages without text, get no embedding.
- The processing page shows embedding as its own stage with exact counts, like reading.
- Embedding calls go through the same model layer as chat calls: model slots, fallbacks, rate limits, token usage,
  recorded answers and the fake provider all work for them.

## User Stories

1. As a user, I want every page I upload and process to be prepared for search, so that later steps can find
   duplicates and evidence without me doing anything.
2. As a user, I want the processing page to show a stage for preparing pages for search, with how many pages are
   done of how many, so that I see progress.
3. As a user, I want plans, photos and discarded pages to skip this stage, so that I do not pay for work that is
   not needed.
4. As a user, I want a page that was read again (re-read, turned by hand) to get a new embedding, so that search
   never uses outdated text.
5. As a user, I want pages read before this change to get their embedding on my next "Verarbeitung starten", so
   that older projects catch up without a rerun.
6. As a user, I want a failed embedding to be retried like any other job, so that a temporary provider problem
   does not leave pages behind.
7. As a user, I want the costs of embeddings to appear in the token usage like the costs of reading, so that the
   cost per page is complete.
8. As a developer, I want the embedding model named in `job_models` in the configuration, so that switching it
   needs no code change.
9. As a developer, I want the embedding model's fallbacks and `max_parallel` to work exactly as for chat models, so
   that there is one way to configure models.
10. As a developer, I want a rate limit, an unavailable model or a full model to put the `embed_page` job back to be
    tried later, so that embedding behaves like reading.
11. As a developer, I want the backend to refuse to start when the configured embedding model's vector size does
    not match the database column, so that wrong vectors are never stored.
12. As a developer, I want the page text cut to a token limit from the configuration before embedding, so that a
    very long page never makes the call fail.
13. As a developer, I want each stored vector to carry the name of the model that made it (the fallback's name when a
    fallback answered), so that stale vectors can be found after a model change.
14. As a developer, I want a reset to before `embed_page` to delete all embeddings of the project, so that I can
    embed again with another model on pages already read.
15. As a developer, I want a reset to before `ocr_page` to delete the embeddings too, since they follow from the text.
16. As a developer, I want the fake provider to answer embedding calls with a deterministic vector of the right size,
    so that end-to-end runs and tests need no paid calls.
17. As a developer, I want embedding calls recorded and replayed on the golden dataset like chat calls, found by the
    text, so that quality runs are repeatable.
18. As a developer, I want `ocr_page` to store all 11 `title_*` features on every page, so that classification and a
    trained model get the same complete features everywhere.
19. As a developer, I want the `title_*` fields described to the model with the prototype's wording, so that prompts
    that worked stay as they are.
20. As a developer, I want the recorded `ocr_page` answers to be reported as outdated after the schema change, so
    that I know to record them again instead of getting silent wrong replays.
21. As a developer, I want `embed_page` to hold a write target on its page, so that the concurrency rule keeps it from
    running while the page is read again.
22. As a reviewer of quality runs, I want the cost and time per page of `embed_page` reported like other job types,
    so that model choices can be compared.

## Implementation Decisions

- **Title features:** the page reading schema of `ocr_page` gains the 6 missing `title_*` fields
  (`verhandlungsleiter`, `anwesende_personen`, `verhandlungsschrift`, `bescheid`, `spruch`, `betreff`) next to the 5
  existing ones; descriptions taken from the prototype's `DocumentTypeFeatures`. `PAGE_FEATURES` gains the 6
  nullable boolean columns (migration). A page read before this change keeps them empty until it is read again
  (stage reset before `ocr_page`, a development tool).
- **Model layer:** the AI module gets an embedding call beside the chat call, sharing its rules: the model is found
  by name in the configuration (provider, fallbacks, `max_parallel`); each call holds a slot of the model it calls;
  unavailable → next fallback, all unavailable → `Unavailable`; rate limit → `RateLimited`; no free slot →
  `ModelBusy`. It returns the vector, the name of the model that answered and the token count (input only).
- **Fake provider:** answers embedding calls with a deterministic unit vector derived from the text, of the
  configured size, using no tokens. Refused in prod, as today.
- **Recordings:** embedding calls are recorded and replayed per model, found by the text; the fingerprint of what
  shapes the answer is the model's vector size (no prompt). Same modes as today (live, record, replay).
- **Configuration:** `job_models.embed_page: text-embedding-3-small`; the model under `models` with its provider;
  an `embed` section with the vector size (1536), the token limit for the input and the job timeout. The token
  limit defaults to the prototype's 6,000. Startup check: the configured vector size equals the column's size.
- **Schema:** the vector extension enabled in a migration; `PAGE.embedding` as a 1536-dimension vector (nullable)
  and `PAGE.embedding_model` (nullable). No index yet (the duplicate check in T-25 decides which).
- **Job type `embed_page`:** one per page; write target on the page; runs in the lane of its model's provider.
  Precondition: `ocr_page` done, classification `process`, text not empty. Input: the page text. Output: vector,
  model, tokens. `apply_result` writes `embedding` and `embedding_model` through the Akt module. Reset: clears
  `embedding` and `embedding_model` of the project's pages.
- **Follow-ups:** `ocr_page`'s `apply_result` returns an `embed_page` job when the page is `process` with text, and
  clears an existing embedding when the page is read again. "Start run" also queues `embed_page` for every `process`
  page with text and without an embedding whose last `embed_page` job is missing or failed (catches pages read before
  this change).
- **Stage reset:** `embed_page` comes after `ocr_page` in the registry's chain order, so a reset before `ocr_page`
  also undoes `embed_page`.
- **Progress:** a new stage `embed_page` with the label "Seiten für die Suche aufbereiten"; its total is the number
  of `process` pages with text.
- **Token usage:** booked per job to the model that answered, as for `ocr_page`.

## Testing Decisions

- A good test drives the real job pool from the outside (upload or ingest, start run, run to the end) and checks
  stored results; it does not test private functions or call internals.
- **Main seam:** an integration test through the real job pool with the fake provider, like the existing
  `ocr_page` integration tests. It checks: every `process` page with text has a 1536-value embedding and the model
  name; `keep` and `discard` pages have none; all 11 `title_*` features are stored; token usage is booked to the
  answering model; a reset before `embed_page`, and before `ocr_page`, removes the embeddings and "start run" queues
  them again; pages read without an embedding get one on the next start; a rate limit or an unavailable model puts
  the job back to be tried later.
- **Model layer:** the existing unit tests for fallbacks, slots and recordings get the embedding call as a second
  case.
- **Settings:** the startup check for a mismatching vector size, next to the existing settings tests.
- **End to end:** the existing Playwright flow with the fake provider keeps passing and shows the new stage.
- **Golden tier:** recorded `ocr_page` answers become outdated (answer schema changed); the replay names what to
  record. Recording again is done by Andreas.

## Out of Scope

- The duplicate check on pages (`check_page_duplicate`, T-25) and any vector index.
- Embeddings of key infos (`embed_keyinfo`).
- Batching several pages into one embedding call (only if rate limits show it is needed).
- Using the `title_*` features (grouping and classification come in T-21 to T-24).
- Upload redesign, batches and `PAGE.sequence` (T-19).

## Further Notes

- Plan, decisions and their reasons: `Grouping_and_Classification.md` (sections "Models and page features").
- After merging, running "Verarbeitung starten" on an existing project embeds its pages; the `title_*` features of
  pages read before need a reset before `ocr_page` and a new run.

## Tickets

Task graph for `implement-spec` (entries in `TASKS.md`, parent T-20). Frontier: T-28 and T-29.

### T-28: Title features on every page
**What to build:** `ocr_page` reads all 11 `title_*` features on every page and stores them.
**Blocked by:** none (can start immediately). **Status:** ready-for-agent
- [ ] The page reading schema has all 11 `title_*` fields, described with the prototype's wording
- [ ] `PAGE_FEATURES` has the 6 missing nullable columns (migration up and down)
- [ ] A run with the fake provider stores all 11 features for every read page
- [ ] The fake provider's answers include the new fields
- [ ] Replaying recorded `ocr_page` answers made before the change reports them as outdated

### T-29: Embedding call in the model layer
**What to build:** the AI module can embed a text with a configured model under the same rules as chat calls.
**Blocked by:** none (can start immediately). **Status:** ready-for-agent
- [ ] An embedding call returns vector, answering model and input tokens
- [ ] Fallbacks, model slots (`max_parallel`), `RateLimited`, `Unavailable` and `ModelBusy` behave as for chat calls
- [ ] The fake provider returns a deterministic unit vector of the configured size, using no tokens; refused in prod
- [ ] Recordings record and replay embedding calls per model, found by the text
- [ ] `config/<env>.yaml`: `job_models.embed_page`, the model under `models`, an `embed` section (vector size 1536,
  input token limit 6,000, job timeout); input longer than the limit is cut
- [ ] The backend refuses to start when the configured vector size does not match 1536

### T-30: `embed_page` after reading
**What to build:** after a run, every `process` page with text has an embedding; the processing page shows the stage.
**Blocked by:** T-29. **Status:** ready-for-agent
- [ ] Migration: vector extension, `PAGE.embedding` (1536, nullable), `PAGE.embedding_model` (nullable)
- [ ] Job type `embed_page`: one per page, write target on the page, lane of its model's provider
- [ ] `ocr_page` queues `embed_page` for a `process` page with text, none for `keep`, `discard` or empty text
- [ ] The vector and the answering model are stored; token usage is booked to that model
- [ ] Progress stage "Seiten für die Suche aufbereiten" with exact counts
- [ ] Integration test through the real job pool with the fake provider; the Playwright flow still passes

### T-31: Catch-up and undo for embeddings
**What to build:** embeddings stay consistent with the text when pages are read again, reset, or were read before.
**Blocked by:** T-30. **Status:** ready-for-agent
- [ ] "Start run" queues `embed_page` for `process` pages with text and without an embedding whose last job is
  missing or failed
- [ ] Reading a page again clears its embedding and queues a new `embed_page`
- [ ] A reset before `embed_page` clears all embeddings of the project; a reset before `ocr_page` does too
- [ ] Integration tests for all three cases
