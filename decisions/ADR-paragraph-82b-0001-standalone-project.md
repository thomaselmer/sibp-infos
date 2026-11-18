# ADR-paragraph-82b-0001: Standalone project with its own AI files

- **Status:** Accepted
- **Date:** 2026-11-17
- **Decided by:** Andreas (2026-11-17)

## Context
paragraph-82b starts over from Thomas's concept; the earlier code becomes `paragraph-82b-prototype` and is kept only
as reference. Andreas wants the new project to be standalone: a main repository holds the AI files, guidance files,
ADRs, the concept and project documents, and includes the application sources as a submodule. Both repositories are
`shared`.

Core rule 3 ("AI material lives in ai-workspace") and ADR-global-0022 (shared repositories get no AI material) do
not allow this.

## Decision
For paragraph-82b, **Core rule 3** is set aside for the main repository only:
- The main repository holds the project's AI material: guidance files (`AGENTS.md` and pointers), ADRs, the
  concept, plans and documents such as the data model.
- The source submodule follows Core rule 3 and ADR-global-0022 unchanged: no AI material and no trace of it.
- ai-workspace keeps only a thin entry for the project that links to the main repository.
- Workspace rules the project needs (e.g. never invent, confidential data, engineering quality) are copied into the
  main repository. Following the workspace convention (ADR-global-0030), they are stated completely in the main
  repository's `AGENTS.md`; ADRs are written only for decisions above the ADR threshold (amended 2026-11-17, T-2;
  before: one ADR per copied rule).

Core rule 1 stays in force.

## Alternatives considered
- Keep all AI material in ai-workspace, as for the prototype: rejected by Andreas; the project should be usable
  without ai-workspace.

## Consequences
- Agents working on paragraph-82b read the main repository's guidance files; ai-workspace only points there.
- Copied rules can drift from the workspace originals; the main repository is the source of truth for this project.
- The source submodule's commits, branch names and texts still carry no AI material.
- The main repository brings its own files and rules; they take precedence. Copied workspace rules only add to them;
  a conflict is decided by Andreas.
