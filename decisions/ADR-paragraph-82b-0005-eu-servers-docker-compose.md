# ADR-paragraph-82b-0005: Containers on servers of a European provider

- **Status:** Proposed
- **Date:** 2026-11-18
- **Decided by:** —

## Context
The concept requires EU hosting, GDPR compliance, backups and later encryption per tenant (D14). Customer content is
confidential (D06): business secrets and open defects. The prototype ran on Docker Compose. Chosen by Andreas on
2026-11-18 (architecture decision A2).

## Decision
- The application runs as containers with Docker Compose on rented servers of a European provider.
- No US cloud provider holds customer data.
- Environments: local (development; the only place agents work), dev (live test version), prod (customers).
- All environments run the same images; configuration only through environment variables.
- Files are stored in self-run Garage (S3-compatible) in every environment (Thomas and Andreas, 2026-11-23;
  replaces MinIO, whose community Docker images are no longer published).

## Alternatives considered
- Managed services in an EU region of a US cloud provider: less operations work and autoscaling, but higher cost and
  a US provider (CLOUD Act) holding confidential customer data.
- Managed Kubernetes at a European provider: autoscaling and standard tooling, but more operations know-how than the
  MVP needs.

## Consequences
- Scaling workers, backups and updates are done by the team; workers are scaled by hand in the MVP.
- Moving to Kubernetes later stays possible because everything already runs as containers.
- Open: concrete provider and location, managed or self-run PostgreSQL, backup schedule.
