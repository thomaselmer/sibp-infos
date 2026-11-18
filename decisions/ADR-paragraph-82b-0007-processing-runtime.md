# ADR-paragraph-82b-0007: Processing runtime — shared orchestrator processes, Redis, object storage

- **Status:** Proposed
- **Date:** 2026-11-18
- **Decided by:** —

## Context
The data model fixes one orchestrator per project, stored events consumed in one transaction, and jobs that never
touch the database (too many connections); results come back typed and are written in batches; workers send
heartbeats. How this runs was open. Chosen by Andreas on 2026-11-18 (architecture decision A5).

## Decision
- An `orchestrator` process hosts one engine per active project as an asyncio task. A project is claimed
  exclusively through a PostgreSQL lock; several orchestrator processes share the projects and take over after a
  crash. Idle projects are unloaded.
- Redis carries jobs to workers (job stream) and results back (result stream). Workers acknowledge jobs and send
  heartbeats; unacknowledged jobs of dead workers go back to queued.
- Job messages hold ids, texts and object-storage keys. Workers read images from object storage and save new files
  there; they never touch the database.
- PostgreSQL stays the source of truth; Redis holds only messages in transit.

## Alternatives considered
- One orchestrator process per project: simple, but many containers and slow starts with many projects.
- Orchestrator calls workers directly over HTTP: no extra component, but the orchestrator must track and balance
  the workers itself.
- RabbitMQ or NATS: very robust, but more to run and learn than Redis for the MVP.
- All inputs, images included, in the message: no storage access for workers, but messages of several MB.

## Consequences
- One more container (Redis) in every environment.
- After a Redis loss the engines re-send queued jobs from the database.
- Workers need read and write access to object storage.
