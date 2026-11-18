# ADR-paragraph-82b-0004: Modular monolith with three processes

- **Status:** Proposed
- **Date:** 2026-11-18
- **Decided by:** —

## Context
The processing engine needs one transaction for consuming an event and writing its effects (new jobs,
dependencies, stale marks, job results). Akten range from about 15 to more than 5,000 pages, so the LLM load varies
widely. The team is small. Chosen by Andreas on 2026-11-18 (architecture decision A1).

## Decision
- One Python codebase (FastAPI) with strictly separated modules; a module uses another only through its public
  interface, checked by import rules in CI.
- It runs as three processes from the same code: `api`, `orchestrator`, `worker`. The Next.js frontend is separate.
- One PostgreSQL database (with pgvector) shared by the modules.
- Module list and responsibilities: `docs/Architecture.md`, section 4.

## Alternatives considered
- Services per domain with their own databases: the one-transaction rule breaks across services (sagas or outbox
  needed), and operating several services costs a small team too much.
- Simple monolith with processing as background tasks in the API: large Akten would slow the API down and the LLM
  load could not be scaled on its own.

## Consequences
- Workers scale independently of the API; orchestrators scale with the number of active projects.
- Module boundaries need discipline and a CI check; splitting a module into a service later stays possible but
  costs a migration.
