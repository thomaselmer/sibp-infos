# ADR-paragraph-82b-0002: One branch per person

- **Status:** Proposed
- **Date:** 2026-11-17
- **Decided by:** —

## Context
Several people work on the clean start, with and without AI agents. Agreed by Andreas on 2026-11-17.

## Decision
- Only people commit on `dev` and `main`.
- Each person gets a branch of their own (e.g. `andreas`), created from `dev`.
- Each person is responsible for their own branch: what is committed there, push, merge and commit attribution.
- AI development happens on the branch of the person the agent works for; feature branches are created from it.
  The agent follows that person's choices for push, merge and attribution and asks them when they are not known.

## Alternatives considered
- A single AI branch (`andreas`) for all AI work: replaced, every person gets their own.

## Consequences
- Agents never commit to `dev`, `main` or another person's branch.
- Rules for push, merge and attribution can differ per person; they are not fixed for the whole repository.
