# ADR-paragraph-82b-0008: Shared schema, the project is the unit of separation

- **Status:** Proposed
- **Date:** 2026-11-18
- **Decided by:** —

## Context
A project (Betriebsobjekt) belongs to no single tenant: tenants get access to it through project access, a company
and an engineering office can see the same project, and access is transferred, e.g. when the office changes (D05,
D10). After revocation there must be no access at all (D06). Chosen by Andreas on 2026-11-18 (architecture
decision A7).

## Decision
- All tenants share one database schema.
- Content rows hang on project_id. PostgreSQL Row Level Security lets a request see only rows of projects its tenant
  or user has access to; the permission check in the application stays the first line.
- Tenant-owned data (users, plans, branding, settings) carries tenant_id.
- Files are stored under `projects/<project>/…`.
- Later encryption uses one data key per project, wrapped once for every tenant with access; revoking access
  deletes that tenant's wrapping.

## Alternatives considered
- Schema or database per tenant: strong separation, but shared and transferred projects do not fit, and many schemas
  make migrations heavy.
- Files under `tenants/<tenant>/projects/<project>/…` (decided briefly in architecture v9): files would move on every
  transfer, and a shared project would sit under one tenant.

## Consequences
- Revoking or transferring access changes only the access records; no data moves.
- Every content table needs project_id (directly or through its parent) for Row Level Security.
- The data model adds TENANT and the project's link to tenants when tenants come into its scope.
