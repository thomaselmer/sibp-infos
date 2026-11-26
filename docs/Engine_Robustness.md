# Engine, orchestrator and worker: robustness fixes (T-35)

Draft, 2026-11-26. Findings of a design review of the processing runtime (engine, orchestrator, worker, job pool).
The overall design (one serial engine per project as the only writer, PostgreSQL as truth, Redis as transport,
stateless workers, the `JobType` seam) holds; the findings are about failure paths and scaling ceilings. Fixes are
proposals (**Suggestion**) until Andreas decides them. Paths are relative to `the-real-and-final-paragraph82b/backend/app/`
at submodule commit `901e9cd`; line numbers may drift.

Priority: 1–3 and 6 first; 4–5 before a second orchestrator process runs.

**Status 2026-11-26:** Andreas chose to fix all findings and the smaller points before the golden test run, and to
build the pure scheduling core (job graph) now; targets stay as they are until T-21. All built on the submodule
branch `engine-robustness`; Architecture v44 and data model v44 describe the result.

## Findings

1. **Poison result stalls a project for good.** `apply_result` raising (`modules/processing/engine/engine.py:163`)
   rolls back the whole result batch and unloads the engine (`modules/processing/engine/orchestrator.py:145`); the
   next check reclaims the project, reads the result stream from `"0"`, hits the same result, crashes again, every
   5 s. Realistic during a deploy with workers on a different image version (T-9).
   **Suggestion:** a savepoint per result; on failure the job goes to `failed` with the error and the batch goes on.

2. **Unbounded retries after an expired lease.** A lease expiry puts the job back to `waiting` with no cap
   (`engine.py:193-201`). A job that kills its worker (e.g. memory in PDF rendering in `ingest_file`) wanders
   through all workers forever. Also: a lease expiry after a failure writes `error=None`, so `failed_once` is lost
   when `load` rebuilds it from `error` (`engine.py:106`).
   **Suggestion:** a maximum attempt count in `config/<env>.yaml`; keep "failed once" in its own state, not in `error`.

3. **Messages lost in a gap leave a project busy forever.** Three two-step sequences are not atomic: worker ack then
   result post (`modules/processing/engine/worker.py:114-118`), reaper reap then post (`orchestrator.py:211-212`),
   engine commit then push (`engine.py:318-325`). A failure in between leaves a job `queued` with no result; the
   engine is never idle, never unloaded, and the repair in `load` never runs.
   **Suggestion:** ack + `XADD` in one Lua script (same Redis), reap + `XADD` likewise (a pool operation
   `complete(message, result)` and a `reap()` that posts itself); plus a periodic reconcile in the engine of
   `_in_pool` against Redis and the result stream.

4. **No fencing between engines.** Several orchestrator processes are intended (`docs/Architecture.md`, section on
   the orchestrator process). The advisory lock sits on its own connection, the engine writes through pooled
   sessions, and a lost lock is noticed only by the check every 5 s (`orchestrator.py:173`); two engines can write
   one project meanwhile (duplicate jobs, results applied twice).
   **Suggestion:** a claim epoch on the project, set at claim and checked in every engine transaction.

5. **Database connection ceiling.** One PostgreSQL connection per held project (`orchestrator.py:111`);
   `max_engines` 200 exceeds PostgreSQL's default `max_connections` 100 plus the session pool. `check` also tries the
   lock of projects held by other processes every 5 s, opening and closing a connection each time (`orchestrator.py:170`).
   **Suggestion:** one lock connection per process holding all its session locks (`pg_advisory_unlock` on unload);
   together with 4.

6. **Paid work done twice on takeover.** `load` re-sends queued jobs missing in Redis (`engine.py:112-117`), also
   those already acked whose result waits in the stream; that result is then ignored as an older attempt
   (`engine.py:159`). Every orchestrator restart during a run (each deploy) repeats those model calls.
   **Suggestion:** read the stream backlog before deciding what is lost.

7. **Full table scan every 5 s.** `_OPEN_WORK` (`modules/processing/queries.py:11-18`) filters by `status`; the only
   index is `(project_id, status)` and done jobs are kept, so the table grows without bound.
   **Suggestion:** a partial index `WHERE status IN ('waiting', 'queued')`.

8. **A worker runs one job at a time.** `run_once` awaits each job (`worker.py:66-83`) although model jobs are
   I/O-bound; dev allows 8 OpenAI calls at once, `P82B_WORKERS` defaults to 1.
   **Suggestion:** N jobs at once per process (config); the Redis semaphore stays the global cap.

9. **Priority has no effect.** `start_run` is a user event, so its jobs are FAST (`engine.py:136`) and follow-ups
   inherit it (`engine.py:175`): every job is FAST, "Apply changes" cannot overtake another project's run.
   **Suggestion:** `start_run` jobs NORMAL; FAST only for applying changes.

10. **Tokens counted twice on replay.** Tokens are recorded before the staleness check (`engine.py:157`); a crash
    between commit and `XDEL` (`orchestrator.py:141-142`) replays them.
    **Suggestion:** unique key on `(job_id, attempt)` in `token_usage`.

### Smaller points
- `runnable()` sorts all jobs and builds the full list on every call, also via `is_idle` in every check
  (`engine.py:249-279`); stop at the free slots.
- `reset` is detected with `hasattr` (`modules/processing/jobs/base.py:117`); make it part of the protocol.
- Idle workers take and release lane semaphore slots on every poll (`worker.py:70-77`); a busy worker can then
  skip a lane. Taking the slot and the job in one script avoids it.
- Jobs depending on a failed job wait forever and are invisible in progress; matters once `decide_border` (T-21)
  uses dependencies.

## Deepening (Suggestion)
- **Pool interface:** `complete` and a self-posting `reap` instead of callers chaining `ack`/`post`/`reap`
  (removes finding 3 by construction, shrinks the worker).
- **Pure scheduling core:** dependencies, target conflicts, priority and retry policy as a pure job-graph module
  (add jobs, report an outcome and get the transition, next jobs up to N); the engine becomes the shell doing
  PostgreSQL and Redis. Rules tested without Postgres; findings 2 and 9 live in one place.
- **Targets:** fixed `file_id`/`page_id` columns need a migration for each new target kind (documents, batches in
  T-21/T-22). A generic kind plus id avoids it but loses the foreign keys; decide before T-21.
