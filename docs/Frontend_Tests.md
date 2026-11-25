# Frontend tests and recorded model answers (T-16)

Plan agreed with Andreas on 2026-11-25. Prerequisite of T-13 (`docs/Previews_and_Paging.md`).

## Why
`docs/Architecture.md` section 11 defines only backend test levels; the frontend had no tests (only `typecheck`).
Not a decision against them, a gap.

## Tools
- **vitest + Testing Library** for frontend logic and components; the api replaced by Mock Service Worker.
- **Playwright** for end-to-end runs: the whole application started next to local development
  (`frontend/e2e/stack.sh`) with its own database `app_e2e`, Redis database 14 and ports (api 8083, frontend 3083);
  PostgreSQL, Redis and Garage from docker compose. The bucket is shared, separated by project as in the backend
  tests; each run removes its previous projects' files.

## Model calls in end-to-end runs
Model calls go through the page reader that `ai.make_reader` builds from the config (`job_models`, `models`); the
replacements plug in there, the rest of the path (job pool, slots, lanes, worker, results, status stream) stays real.

- **A — `provider: fake`:** answers without a network call by reading a coloured bar along the top edge of made-up
  pages (red irrelevant, blue plan, none process; a fake that only sees the image cannot read a word), so every
  classification group can be produced. No delay (no test needs one). Refused by the settings in `prod`.
- **B — `provider: replay`:** replays recorded real answers of the golden dataset.
  - Recorded once with the live model by a command Andreas starts (it costs per call); only calls without a
    recording are made.
  - **Stored in `<P82B_GOLDEN_DATASET_DIR>/recordings/<model>/`**, never in Git (the answers hold real text).
    Exception to section 11 ("neither folder is ever written to"): only the record command writes, and only into
    `recordings/`. The golden dataset fixture skips `recordings/` (top-level folders are document types).
  - **Per model:** each call is recorded under the model that was called, also a fallback's call. A recording holds
    what the model did with that page image: its answer, or the error that sent the call on to the next fallback. A rate
    limit is not recorded: it is no fallback case (the job is tried again with the same model).
    Replay plays the same chain again: the first model fails as recorded, the fallback answers as recorded. When a
    fallback is needed again later, its answer is already there.
  - Looked up by the image sent to the model. Each recording keeps a fingerprint of the logic that shaped it (prompt
    and answer schema, image preparation, `max_turns`); when the logic changes, the recording is outdated and the
    record command records it again (it replaces the old one).
  - A call without a recording, or with an outdated one, makes the test fail and names what to record; it never
    falls back to A.

## Two tiers
| Tier | Data | Runs |
|---|---|---|
| always | made-up pages, provider A; vitest everywhere | without the drive; later in CI |
| golden | golden dataset, provider B | locally only, when the drive is there |

- Exception to rule 6 ("tests use made-up data") and section 11 for the golden tier, decided by Andreas.
- When the golden tier is skipped (drive or recordings missing), the run says so clearly at the end: the datasets
  were missing and not everything was tested.
- The golden tier checks both: (a) real answers pass the whole path intact (worker, database, api, frontend), and
  (b) the results match the expected values of the golden dataset (a quality run on recordings).
- Test output of the golden tier holds no screenshots, videos or traces (they would show real pages); its output
  folder is git-ignored and removed after the run.

## Scope of the always tier (confirmed)
- **vitest:** upload in `lib/api.ts` (parallel files, progress, skipped and refused files, failed send, api error
  text); upload page (loading, empty, error, progress, refused list, warning before leaving, status labels);
  processing page (stage rows, reset confirmation, start and its error, pages empty and filled); sidebar; format
  helpers with hand-written expected values.
- **Playwright (made-up pages, provider A):** 1 upload PDF and PNG to ready, unsupported file refused; 2 processing
  runs live to done, pages open, markers give their classification; 3 re-sending a file skips it, no duplicate;
  4 on a long page only the content scrolls.

## Built (submodule `07c71b5`, `b55f421`; Architecture v38)
- Classifications are not shown in the frontend yet; the end-to-end flow checks them once T-13 shows the page
  groups.
- Page images are stored without a content type; browsers show them anyway. Set with the previews in T-13.

## Open
- Label file format of the golden dataset not settled yet (Andreas, 2026-11-25). (b) can compare only what the golden dataset labels and the code already produces: today ocr_page exists, while
  the labels cover borders (subfolder = document), document type (group folder) and key infos; the label file
  format follows when the dataset is ready.
