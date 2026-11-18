# paragraph82b — agent guide

Supports certified offices, inspectors (Prüfer) and companies through the recurring §82b review
(wiederkehrende Prüfung) in Austria. This repository holds the concept, the decisions and the project documents;
the application sources live in the submodule `the-real-and-final-paragraph82b/`.

This file states the rules for agents completely. Decisions with background are in `decisions/`.

## Layout
| Path | Content |
|---|---|
| `infos/` | Thomas's concept — see "Concept area" below |
| `decisions/` | ADRs `ADR-paragraph-82b-NNNN` |
| `docs/` | Project documents: `Architecture.md` (software architecture), `Data_Model.md` (data model); final result only; the matching `.html` (`Architecture.html`, `Data_Model.html`) shows the history of versions and changes and is published as an artifact |
| `TASKS.md` | Open work |
| `the-real-and-final-paragraph82b/` | Application sources (submodule) |

## Concept area `infos/`
- `infos/CLAUDE.md` governs all work in `infos/` and takes precedence there over this file: session start,
  planning phase, one focus per session, IDs (`D`, `V`, `A`, `P`) instead of paths, frontmatter, filing rules,
  handover, and the protection rule for that file.
- Thomas decides the content of the concept. Agents mark proposals as such until Thomas confirms them.
- Files in `infos/` are German and keep their own structure and naming; the rules in this file about language,
  file names and ADRs do not apply there.
- Never change files in `infos/` as a side effect of other work.

## Core rules
1. **Never invent.** Do not make up facts, requirements, names, paths, versions, APIs, behaviour or decisions.
   If information is missing, look it up in `infos/`, `decisions/` and `docs/`; if it is still missing, ask.
2. **Suggest, people decide.** Unclear decisions get options labelled `Suggestion:` with trade-offs; a suggestion
   is never treated as fact. Only people accept decisions; agents write ADRs only as `Proposed`.
3. **No AI material in the source submodule.** `the-real-and-final-paragraph82b/` holds only product artifacts —
   what would be there if no AI had been used. No AI instruction files, plans or notes, and no trace of AI tools,
   this repository's ADR IDs or its paths in files, commit messages, branch names, PR descriptions or code
   comments there. Keep its README and other product artifacts consistent in the same task.
4. **One source of truth.** Agent instructions live in `AGENTS.md` files; tool-specific files (e.g. `CLAUDE.md`)
   only point to them. Exception: `infos/CLAUDE.md`, which stays as it is.
5. **English** for all files outside `infos/`.
6. **Confidential data.** All data of this project is confidential. Never write secrets (passwords, tokens, keys,
   connection strings), real personal, customer or financial data, or excerpts from real documents or databases
   into any file, commit or external service. Refer to them by name instead (e.g. "see variable `DB_URL`").
   Examples and tests use made-up data. When unsure, ask.
7. **Local development only.** Agents develop and test only locally; they do not change, deploy to or query shared
   or live environments.
8. **Engineering quality.**
   - No duplication: search for existing implementations and reuse them; report duplication you find.
   - Use the most efficient and suitable algorithms, data structures and methods (no N+1 queries, no needless
     re-computation, requests or re-renders).
   - No unintended side effects: check the coupled parts of a change and name them in the completion report.
   - User experience first: fast, smooth, with clear loading, empty and error states. A change that degrades UX
     or performance is not complete.

## Git
- Only people commit on `dev` and `main`. Each person has a branch of their own (e.g. `andreas`), created from
  `dev`, and is responsible for it (ADR-paragraph-82b-0002).
- Agents work on the branch of the person they work for, or on feature branches created from it. Push, merge and
  commit attribution follow that person's choice; ask them when it is not known.
- Never commit to `dev`, `main` or another person's branch.

## Architecture
- Modular monolith: one Python codebase, run as the processes `api`, `orchestrator` and `worker`; modules use
  each other only through their public interfaces (ADR-paragraph-82b-0004).
- Event-driven job pool, not a pipeline; any stage can be reset by invalidating its jobs (ADR-paragraph-82b-0003).
- Details: `docs/Architecture.md`.

## Decisions (ADRs)
- **Threshold:** an ADR only when a decision is hard to reverse, would surprise a reader without context, and
  resulted from a real trade-off. Other decisions change this file or a document directly after a person approves
  them; the reason goes into the commit message.
- **New ADR:** next free number (never reused), file `decisions/ADR-paragraph-82b-NNNN-<short-kebab-title>.md` with
  the sections Status, Date, Decided by, Context, Decision, Alternatives considered, Consequences. Write only what is
  known; unknown parts stay `TBD — ask`. Status `Proposed`.
- **Change an accepted ADR:** typo → fix; amendment → edit and add `(amended <YYYY-MM-DD>)` where it changed;
  replacement → new ADR with `Replaces: <old ID>`, old one archived; no longer valid → archive. Archive =
  move to `decisions/history/` with status `Archived <YYYY-MM-DD>; was Accepted`.

## Tasks
Tasks are tracked in `TASKS.md` (sections In progress, Blocked, Open, Done). Open concept questions stay in
`infos/` under its own rules.
1. **Start.** Clarify the goal and how to tell it is done; ask if unclear. Add the task to **In progress**:
   `- [ ] T-<n> <title> — started <YYYY-MM-DD>` (IDs sequential, never reused).
2. **Blocked** on an answer → move it to **Blocked** with the exact question.
3. **Pause.** Leave it in **In progress** with one indented `Next:` line. Entries stay short; longer material goes
   into a document in `docs/` linked from the entry.
4. **Finish.** See below, then the Done line `- [x] T-<n> <title> — done <YYYY-MM-DD> — <one-line outcome>`.
   Keep the last 10 Done entries.

## Finishing a task
- Verify the result against the goal; report what was checked and which parts are coupled.
- Run `git status` in this repository and in the submodule; report any AI material in the submodule.
- Decisions above the threshold → ADR as `Proposed`; others are approved by a person in chat.

## Tech stack
Python/FastAPI, SQLAlchemy, Next.js/TypeScript, PostgreSQL with pgvector. Details follow in the submodule.

## Related
The earlier module prototype (`AlpsoftGmbH/paragraph_82b`) serves only as reference for concepts and comparison.
