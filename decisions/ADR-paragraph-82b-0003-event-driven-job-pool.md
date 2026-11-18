# ADR-paragraph-82b-0003: Event-driven job pool instead of a pipeline

- **Status:** Proposed
- **Date:** 2026-11-17
- **Decided by:** —

## Context
The prototype processed projects in a fixed, idempotent pipeline. For the clean start Andreas decided on a
different processing model (2026-11-17).

## Decision
- Processing runs as an event-driven job pool, not as a pipeline.
- The prototype's idempotent-pipeline rule does not apply.
- Entry points stay as before: any stage can be reset; a reset is modelled as invalidating that stage's jobs.

## Alternatives considered
- Idempotent, restartable pipeline as in the prototype: replaced.

## Consequences
Details (events, job types, invalidation) are still to be designed; see the processing area of the concept
(`infos/`, D15).
