# Domain docs

How the engineering skills read this repository's domain documentation.

## Before exploring, read these

- `GLOSSARY.md` at the root.
- `decisions/`: ADRs that touch the area you work in (`ADR-paragraph-82b-<nnnn>-<slug>.md`). New ADRs follow
  "Decisions (ADRs)" in `AGENTS.md` and are always written as Proposed; the skills' own ADR format, numbering
  and `docs/adr/` are not used.
- `docs/Architecture.md` and `docs/Data_Model.md` for the current architecture and data model.
- The concept in `infos/` is referenced by its IDs (e.g. D17, V04); `infos/CLAUDE.md` rules there.

If a file does not exist, proceed silently. `/domain-modeling` adds terms to `GLOSSARY.md` when they get settled.

## Use the glossary's vocabulary

Name domain concepts with the terms from the glossary (English, each mapped to one concept term). If a term is
missing, either you are inventing language (reconsider) or there is a real gap (note it for `/domain-modeling`).

## Flag ADR conflicts

If your output contradicts an existing ADR, say so explicitly instead of overriding it silently:

> _Contradicts ADR-paragraph-82b-0007 (processing runtime), but worth reopening because…_
