# Issue tracker: TASKS.md

Tasks for this repository live in `TASKS.md` at the root (format and rules: "Tasks" in `AGENTS.md`).
There are no GitHub issues or pull requests; skill steps that would open them are skipped and their text is
reported in the chat.

## Conventions

- A ticket is one entry `T-<n>` in `TASKS.md`; IDs are sequential and never reused.
- A body longer than a line goes into a document in `docs/`, linked from the entry.
- Triage state is a marker in the entry, e.g. `[needs-info]` (see `triage-labels.md`).
- Blocking: `Blocked by: T-<n>` in the entry.
- Closing: the Done line from "Finishing a task" in `AGENTS.md`. Dropping a task is Andreas's decision.

## When a skill says "publish to the issue tracker"

Add an entry under **Open** in `TASKS.md`; put a longer body into `docs/<Title>.md` and link it.

## When a skill says "fetch the relevant ticket"

Read the entry in `TASKS.md` and the documents it links.

## Wayfinding operations

Used by `/wayfinder`.

- **Map**: a document `docs/<effort>.md` (Notes / Decisions so far / Fog), linked from one task entry.
- **Child ticket**: one task entry per question, with `[wayfinder:<type>]` (research, prototype, grilling, task)
  and `Map: docs/<effort>.md`.
- **Blocking**: `Blocked by: T-<n>, T-<n>`; unblocked when every listed task is done.
- **Frontier**: open, unblocked, unclaimed child entries; the lowest ID wins.
- **Claim**: move the entry to **In progress** before any work.
- **Resolve**: write the answer into the map under "Decisions so far" and close the entry with a Done line.
