# Tasks — paragraph82b

Progress tracker only; keeps the next session oriented. Done keeps the last 10 entries.
Format and rules: "Tasks" in `AGENTS.md`.

## In progress
- [ ] T-4 Proof of concept of the processing in the source submodule — thin slice first, measured against the golden dataset (`docs/Architecture.md`) — started 2026-11-18
  Next: job pool, ingest_file, api and frontend built; file store passed into the worker tasks (submodule `cca8a29`); next T-5, then ocr_page from the prototype (needs OPENAI_API_KEY in `.env`), measured on the golden dataset
- [ ] T-3 Software architecture for the MVP, platform-wide — `docs/Architecture.md` (end result + history), decisions one by one with Andreas; data model also as `docs/Data_Model.md` — started 2026-11-18
  Next: architecture for the processing unit complete; object storage Garage decided v26 (built when needed); ADRs 0002–0008 wait for Andreas; golden dataset format when Andreas says it is ready; A8, A10, A11 deferred
- [ ] T-1 Data model for the clean start — `docs/Data_Model.html` (v32), no open questions for the scope up to key info extraction; deviations from the concept wait for Thomas — started 2026-11-16
  Next: settle the deviations from the concept with Thomas

## Blocked

## Open
- [ ] T-5 Limits for uploaded files before ocr_page: pixels per rendered page, pages per file, total upload size and file count, image pixel limit; rendering and file writes off the event loop; pages written to the store one by one (code review 2026-11-23)
- [ ] T-6 Remove references to this repository from the source submodule (ADR and task IDs, concept decision IDs, "Architecture section" in docstrings), rule 3 (code review 2026-11-23)

## Done
- [x] T-2 Set up the main repository — done 2026-11-18 — branches `dev`/`andreas`, `AGENTS.md`, `decisions/` (ADR 0001–0003), `TASKS.md`, data model moved to `docs/`
