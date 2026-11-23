# Tasks — paragraph82b

Progress tracker only; keeps the next session oriented. Done keeps the last 10 entries.
Format and rules: "Tasks" in `AGENTS.md`.

## In progress
- [ ] T-8 Garage with uploads straight from the browser, and tiers that limit storage and processed pages — started 2026-11-23
  Next: built (submodule `6ac4125`, Architecture v33, data model v40); Andreas decides the default tier (Basis set as placeholder); open: removing never-confirmed uploads, the border rule once `decide_border` exists, pricing (D19) with Thomas
- [ ] T-4 Proof of concept of the processing in the source submodule — thin slice first, measured against the golden dataset (`docs/Architecture.md`) — started 2026-11-18
  Next: job pool, ingest_file, api and frontend built; ocr_page and the stage reset (development tool) built (submodule `e5c3b95`); next: first run on the golden dataset, Andreas reviews the results and reports findings in the terminal; stage 2 fallback (result too weak) and title_* still open
- [ ] T-3 Software architecture for the MVP, platform-wide — `docs/Architecture.md` (end result + history), decisions one by one with Andreas; data model also as `docs/Data_Model.md` — started 2026-11-18
  Next: architecture for the processing unit complete; object storage Garage decided v26 (built when needed); ADRs 0002–0008 wait for Andreas; golden dataset format when Andreas says it is ready; A8, A10, A11 deferred
- [ ] T-1 Data model for the clean start — `docs/Data_Model.html` (v32), no open questions for the scope up to key info extraction; deviations from the concept wait for Thomas — started 2026-11-16
  Next: settle the deviations from the concept with Thomas

## Blocked

## Open
- [ ] T-7 Fill in `config/dev.yaml` and `config/prod.yaml` in the source submodule before the first deployment (left empty on purpose, 2026-11-23)

## Done
- [x] T-6 Remove references to this repository from the source submodule — done 2026-11-23 — references removed from the code (submodule `31d40f8`); code map in Architecture section 4
- [x] T-5 Limits for uploaded files before ocr_page — done 2026-11-23 — limits per file, request, PDF and photo; large pages rendered smaller, one at a time, off the event loop (submodule `19a8cb5`, Architecture v27)
- [x] T-2 Set up the main repository — done 2026-11-18 — branches `dev`/`andreas`, `AGENTS.md`, `decisions/` (ADR 0001–0003), `TASKS.md`, data model moved to `docs/`
