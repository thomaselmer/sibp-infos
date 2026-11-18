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
- Redis carries jobs to workers (one sorted queue per lane) and results back (one result stream per orchestrator
  process). Taking a job is a lease extended by heartbeats; jobs of dead workers go back to queued.
- Job messages hold ids, texts and object-storage keys. Workers read images from object storage and save new files
  there; they never touch the database.
- PostgreSQL stays the source of truth; Redis holds only messages in transit.
- One shared pool for all projects: workers take whatever runnable job comes next; results go to the stream of the
  orchestrator process that holds the project.
- Lanes per model (plus a lane `code`), each a sorted queue ranked by priority, then time; jobs from user actions
  get the fast lane priority.
- A worker takes a slot of the model's semaphore (lease with expiry) before it takes a job from that lane; a job
  answered with a rate limit goes back with a growing delay and does not count as an attempt.
- A cap of jobs in the pool per project, over all lanes.

## Alternatives considered
- One orchestrator process per project: simple, but many containers and slow starts with many projects.
- Orchestrator calls workers directly over HTTP: no extra component, but the orchestrator must track and balance
  the workers itself.
- RabbitMQ or NATS: very robust, but more to run and learn than Redis for the MVP.
- Redis streams (two per lane for priority): acknowledgement built in, but only two priority levels and more streams.
- Putting a job back into the pool when the model's semaphore is full: causes busy loops and reorders jobs.
- All inputs, images included, in the message: no storage access for workers, but messages of several MB.

## Consequences
- One more container (Redis) in every environment.
- After a Redis loss the engines re-send queued jobs from the database.
- Workers need read and write access to object storage.
