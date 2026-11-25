# Tasks — paragraph82b

Progress tracker only; keeps the next session oriented. Done keeps the last 10 entries.
Format and rules: "Tasks" in `AGENTS.md`.

## In progress
- [ ] T-20 Slice 1: `embed_page` (text-embedding-3-small) and the 6 missing `title_*` features read by `ocr_page` on every page — spec `docs/Page_Embeddings_and_Title_Features.md`; plan `docs/Grouping_and_Classification.md`; takes the embedding part of T-15 — started 2026-11-25
- [ ] T-28 Title features on every page: `ocr_page` reads and stores all 11 `title_*` — parent T-20; spec `docs/Page_Embeddings_and_Title_Features.md`, section "Tickets" — started 2026-11-25
- [ ] T-29 Embedding call in the model layer: fallbacks, slots, fake, recordings, `embed` config — parent T-20; spec `docs/Page_Embeddings_and_Title_Features.md`, section "Tickets" — started 2026-11-25
  Next: implement-spec running on local branch `page-embeddings` in the submodule; T-28 and T-29 in progress, then T-30, T-31
- [ ] T-10 Uploads survive interruptions: clear progress, warning before leaving, continue by name and size without duplicates, clean up abandoned uploads — started 2026-11-23
  Next: sign of life built, backend and browser (submodule `d7e566d`, `5c162af`; Architecture v37, data model v41), pushed; interrupt an upload on dev and see it shown as interrupted after 2 min, then done; order of continued uploads: handled by T-19 (batch, order by path)
- [ ] T-9 Container images and a distributed deployment: workers on a remote host via SSH tunnel (stopover), then hosts by role on the office network with automatic deploy from self-hosted runners (target) — started 2026-11-23
  Next: first automatic deploy to the core host green (runner on the core host, `P82B_CORE_HOST` set, `.env` at the runner's `~/paragraph82b-dev.env`); submodule pushed up to `5c162af` and deployed from `andreas` (for now); left: merge `andreas` into `dev`, then switch the deploy job to `dev`
- [ ] T-4 Proof of concept of the processing in the source submodule — thin slice first, measured against the golden dataset (`docs/Architecture.md`) — started 2026-11-18
  Next: job pool, ingest_file, api and frontend built; ocr_page and the stage reset (development tool) built (submodule `e5c3b95`); next: first run on the golden dataset, Andreas reviews the results and reports findings in the terminal; stage 2 fallback (result too weak) still open; title_* in T-20
- [ ] T-3 Software architecture for the MVP, platform-wide — `docs/Architecture.md` (end result + history), decisions one by one with Andreas; data model also as `docs/Data_Model.md` — started 2026-11-18
  Next: architecture for the processing unit complete; object storage Garage decided v26 (built when needed); ADRs 0002–0008 wait for Andreas; golden dataset format when Andreas says it is ready; A8, A10, A11 deferred
- [ ] T-1 Data model for the clean start — `docs/Data_Model.html` (v32), no open questions for the scope up to key info extraction; deviations from the concept wait for Thomas — started 2026-11-16
  Next: settle the deviations from the concept with Thomas; also pricing (D19) and plan names (D16) for the tiers

## Blocked

## Open
- [ ] T-32 Certificate and address for dev: Let's Encrypt via DNS challenge (provider, token) instead of Caddy's own authority, DNS entry for the address (left open in T-11)
- [ ] T-26 Paged, collapsible groups: files by status on the upload page, pages by classification on the processing page, api paging and group filters — plan in `docs/Previews_and_Paging.md`; after T-19 (batches, paths, `PAGE.sequence`) and T-18 Q11 (document view)
- [ ] T-27 Files before processing: one WebP preview per file (first page) made when the upload is confirmed, shown in the file list with the original on click; remove a file before processing (row, original, preview; `storage_used` goes down), processed files not yet; names of skipped files shown — same file table as T-26, build together; workflow to be confirmed by Thomas; open: removing a processed file (block, or remove its pages and results like a reset)
- [ ] T-30 [ready-for-agent] `embed_page` after reading, with progress stage — parent T-20; Blocked by: T-29; spec `docs/Page_Embeddings_and_Title_Features.md`, section "Tickets"
- [ ] T-31 [ready-for-agent] Catch-up and undo for embeddings: start run, re-read, resets — parent T-20; Blocked by: T-30; spec `docs/Page_Embeddings_and_Title_Features.md`, section "Tickets"
- [ ] T-21 Slice 3: `decide_border` with the tier limit, rules behind a replaceable decision seam, per batch — needs T-19 — `docs/Grouping_and_Classification.md`; replaces T-17
- [ ] T-22 Slice 4: `assemble_document` with simple attachment links, and the "Dokumente" view and upload into an existing document (from T-19; the user chooses the place, no batch, `PAGE.sequence` in between) — needs T-21 — `docs/Grouping_and_Classification.md`
- [ ] T-23 Slice 5: `find_sections` with `DOCUMENT_FEATURES` — needs T-22 — `docs/Grouping_and_Classification.md`
- [ ] T-24 Slice 6: `classify_document`, `check_reference_numbers`, `NOTE` — needs T-23 — `docs/Grouping_and_Classification.md`
- [ ] T-25 Slice 7: duplicate checks for files, pages and documents, reusing `NOTE` — needs T-24 — `docs/Grouping_and_Classification.md`; takes T-14 and T-15
- [ ] T-7 Fill in `config/prod.yaml` in the source submodule before the first deployment (left empty on purpose, 2026-11-23; `dev.yaml` filled in T-9)

## Done
- [x] T-19 Upload redesign — done 2026-11-25 — folders dropped or picked keep their paths, batch per "Verarbeitung starten" ordered by path, `PAGE.sequence` 1000 apart, tier limit in Akt order, already uploaded by path and size (submodule `ab9ef97`, Architecture v41); upload into a document moved to T-22
- [x] T-11 Reach the dev environment directly — done 2026-11-25 — Caddy in front of frontend, api and Garage on the dev host, deployed from `andreas` for now (Architecture v35); the deploy switches to `dev` with T-9
- [x] T-13 Page previews — done 2026-11-25 — WebP previews at ingest and for turned pages, one-off command for older pages, browser cache with the image version in the links, content types (submodule `572030b`, `2855bcf`; Architecture v40); run the command on dev after the next deploy
- [x] T-18 Plan the job types from embed_page up to classify_document — done 2026-11-25 — plan in `docs/Grouping_and_Classification.md`, data model v42, architecture v39; slices are T-19 to T-25
- [x] T-16 Frontend tests and recorded model answers — done 2026-11-25 — vitest (19), Playwright on a made-up mini Akt with the fake provider (4 flows), golden tier with recordings per model, local only, skipped with a note without the dataset (submodule `07c71b5`, `b55f421`, review fixes `87b4a26`; Architecture v38)
- [x] T-8 Garage with uploads straight from the browser, and tiers that limit storage and processed pages — done 2026-11-25 — built (submodule `6ac4125`, Architecture v33, data model v40); Basis stays the default for now; the border rule moved to T-17, pricing to T-1
- [x] T-12 Limit parallel calls per model, fallbacks included — done 2026-11-25 — slot per model call incl. fallbacks, full model delays the job, provider semaphore stays (submodule `944ef51`, Architecture v36)
- [x] T-6 Remove references to this repository from the source submodule — done 2026-11-23 — references removed from the code (submodule `31d40f8`); code map in Architecture section 4
- [x] T-5 Limits for uploaded files before ocr_page — done 2026-11-23 — limits per file, request, PDF and photo; large pages rendered smaller, one at a time, off the event loop (submodule `19a8cb5`, Architecture v27)
- [x] T-2 Set up the main repository — done 2026-11-18 — branches `dev`/`andreas`, `AGENTS.md`, `decisions/` (ADR 0001–0003), `TASKS.md`, data model moved to `docs/`
