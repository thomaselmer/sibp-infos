# Tasks — paragraph82b

Progress tracker only; keeps the next session oriented. Done keeps the last 10 entries.
Format and rules: "Tasks" in `AGENTS.md`.

## In progress
- [ ] T-36 Reading the golden dataset share — started 2026-11-26
  Next: implement and test the dataset reader on the integration branch.
- [ ] T-33 Golden dataset on dev, read with the real models and recorded: worker volume for recordings, `MODEL_RECORDINGS` in the dev `.env`, command `app.tools.set_tier` (Enterprise for ~560 pages); Architecture v43 — started 2026-11-25
  Changed 2026-11-26 (golden tests, `docs/Golden_Tests.md`): the one recording run; workers record straight into `ds_recordings` on the share (mounted on the dev core host at `/mnt/nas/paragraph82b/goldendataset`) instead of a volume, so no copy step; Andreas inspects the results in the app, findings go to `ds_findings`
  Next: built (submodule `0721def`, not pushed); T-28 merged (`8066360`); Andreas: push, add `MODEL_RECORDINGS` to the dev `.env`, empty "Mein Betrieb" (reset dev, readme) if needed, set its tier, upload the input folder, start; then copy the recordings into the golden dataset and remove the line again
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
- [ ] T-43 Upload page, seen during the golden upload on dev (Andreas, 2026-11-26): (1) "Ihre Uploads" stays at its old state until the whole upload ends (with ~550 files it looks as if nothing happened); show the files as they arrive, without reloading the list for every file; (2) two select buttons ("Dateien auswählen" in the drop zone, "Ordner auswählen" below) feel redundant; one control for files or a folder; (3) refused files show only in the upload box (a few while uploading, all in the summary) and never in the list, files the browser leaves out (system and hidden files) nowhere; show both, with the reason; takes "names of skipped files shown" from T-27
- [ ] T-34 Golden tests with labels and approved results on the dev core host: replay workflow started by hand (no record mode), `ds_` folders on the share, matching by hash, proposals (classification, order with checks, attachments), reports, snapshot per run, measures for classification and grouping, nothing uploaded to GitHub; architecture section 11 — spec `docs/Golden_Tests.md` — Blocked by: T-33; tickets T-36 to T-42
- [ ] T-37 [ready-for-agent] Measures and report: page classification, grouping exact/split/joined, `ds_reports`, fail when worse — parent T-34; Blocked by: T-36; spec `docs/Golden_Tests.md`, section "Tickets"
- [ ] T-38 [ready-for-agent] Proposals and approval: `*.proposed.csv`, approval by rename, lapse on change, attachments as subfolders, differences fail — parent T-34; Blocked by: T-37; spec `docs/Golden_Tests.md`, section "Tickets"
- [ ] T-39 [ready-for-agent] Order proposals with content checks: `position`, `check` — parent T-34; Blocked by: T-38; spec `docs/Golden_Tests.md`, section "Tickets"
- [ ] T-40 [ready-for-agent] Snapshots: command into `ds_snapshots`, one at the end of every golden run — parent T-34; Blocked by: T-36; spec `docs/Golden_Tests.md`, section "Tickets"
- [ ] T-41 [ready-for-agent] Findings table on the share and anonymous error catalogue — parent T-34; Blocked by: T-36; spec `docs/Golden_Tests.md`, section "Tickets"
- [ ] T-42 [ready-for-agent] Golden run on the dev core host: manual workflow, own stack, tier, replay only, no uploads; architecture section 11 — parent T-34; Blocked by: T-37, T-40; spec `docs/Golden_Tests.md`, section "Tickets"
- [ ] T-32 Certificate and address for dev: Let's Encrypt via DNS challenge (provider, token) instead of Caddy's own authority, DNS entry for the address (left open in T-11)
- [ ] T-26 Paged, collapsible groups: files by status on the upload page, pages by classification on the processing page, api paging and group filters — plan in `docs/Previews_and_Paging.md`; after T-19 (batches, paths, `PAGE.sequence`) and T-18 Q11 (document view)
- [ ] T-27 Files before processing: one WebP preview per file (first page) made when the upload is confirmed, shown in the file list with the original on click; remove a file before processing (row, original, preview; `storage_used` goes down), processed files not yet; names of skipped files shown — same file table as T-26, build together; workflow to be confirmed by Thomas; open: removing a processed file (block, or remove its pages and results like a reset)
- [ ] T-21 Slice 3: `decide_border` with the tier limit, rules behind a replaceable decision seam, per batch — needs T-19 — `docs/Grouping_and_Classification.md`; replaces T-17
- [ ] T-22 Slice 4: `assemble_document` with simple attachment links, and the "Dokumente" view and upload into an existing document (from T-19; the user chooses the place, no batch, `PAGE.sequence` in between) — needs T-21 — `docs/Grouping_and_Classification.md`; also: the golden test measures this stage a second time on golden documents built from the labels (`docs/Golden_Tests.md`)
- [ ] T-23 Slice 5: `find_sections` with `DOCUMENT_FEATURES` — needs T-22 — `docs/Grouping_and_Classification.md`
- [ ] T-24 Slice 6: `classify_document`, `check_reference_numbers`, `NOTE` — needs T-23 — `docs/Grouping_and_Classification.md`; also: the golden test measures this stage a second time on golden documents built from the labels (`docs/Golden_Tests.md`)
- [ ] T-25 Slice 7: duplicate checks for files, pages and documents, reusing `NOTE` — needs T-24 — `docs/Grouping_and_Classification.md`; takes T-14 and T-15
- [ ] T-7 Fill in `config/prod.yaml` in the source submodule before the first deployment (left empty on purpose, 2026-11-23; `dev.yaml` filled in T-9)

## Done
- [x] T-35 Robustness of engine, orchestrator and worker — done 2026-11-26 — design review findings, job graph, token meter (used/wasted, played-back answers count none), `ds_recordings`; two review rounds each; pushed and deployed (submodule `3f7b1ff`; Architecture and data model v44)
- [x] T-20 Slice 1: page embeddings and title features — done 2026-11-25 — implemented, two review rounds fixed, merged into `andreas` (submodule `8066360`, not pushed)
- [x] T-31 Catch-up and undo for embeddings — done 2026-11-25 — start run catches up, re-read clears and re-queues, resets tested (submodule `32078b8`)
- [x] T-30 `embed_page` after reading — done 2026-11-25 — vector columns, job type, progress stage (submodule `c6dc4d8`)
- [x] T-29 Embedding call in the model layer — done 2026-11-25 — shared fallback/slot rules, fake, recordings, `embed` config (submodule `1a7d559`)
- [x] T-28 Title features on every page — done 2026-11-25 — all 11 read and stored (submodule `3fe960a`)
- [x] T-19 Upload redesign — done 2026-11-25 — folders dropped or picked keep their paths, batch per "Verarbeitung starten" ordered by path, `PAGE.sequence` 1000 apart, tier limit in Akt order, already uploaded by path and size (submodule `ab9ef97`, review fixes `edcc865`: batch at the press, hidden folders left out, `FILE.pages_before`; Architecture v42, data model v43); upload into a document moved to T-22
- [x] T-11 Reach the dev environment directly — done 2026-11-25 — Caddy in front of frontend, api and Garage on the dev host, deployed from `andreas` for now (Architecture v35); the deploy switches to `dev` with T-9
- [x] T-13 Page previews — done 2026-11-25 — WebP previews at ingest and for turned pages, one-off command for older pages, browser cache with the image version in the links, content types (submodule `572030b`, `2855bcf`; Architecture v40); run the command on dev after the next deploy
- [x] T-18 Plan the job types from embed_page up to classify_document — done 2026-11-25 — plan in `docs/Grouping_and_Classification.md`, data model v42, architecture v39; slices are T-19 to T-25
