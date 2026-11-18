# ADR-paragraph-82b-0006: AI models per job type, flows with LangChain and LangGraph

- **Status:** Proposed
- **Date:** 2026-11-18
- **Decided by:** —

## Context
Every processing step that uses a model sends confidential page content to a provider (D06). Quality and cost
differ per step (OCR on poor photos, extraction, embeddings), and the concept leaves model choice and cost open
(D14). The concept asks whether LangChain, used in the prototype, is the right basis (D19). Chosen by Andreas on
2026-11-18 (architecture decisions A3 and A4).

## Decision
- The model is configured per job type (provider, model, parameters) and can be switched for one job type without
  changing the others.
- Start: all job types use the OpenAI API with one API key.
- Allowed providers: processing in the EU and storage of the content excluded by contract; US or European.
- AI flows are built with LangChain and LangGraph and run inside the worker's job task; they return the job's typed
  result and never touch the database.
- Token usage is recorded per call and returned with the job result.

## Alternatives considered
- Only European providers: consistent with ADR-paragraph-82b-0005, but OCR quality on poor photos is unproven.
- Self-hosted open-weight models: content never leaves our servers, but GPU cost, operations and likely lower quality.
- An own thin model layer instead of LangChain/LangGraph: fewer dependencies, but the prototype's flows and team
  experience are on LangChain/LangGraph.

## Consequences
- The OpenAI account's terms (data retention, region) must be checked before real customer data is processed in prod.
- Changing the embedding model means re-embedding a project.
- Switching a model is preceded by a comparison on test data.
