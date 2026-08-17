---
title: Phase 0 - Walking Skeleton and Deploy Seams
date: 2026-08-17
tags:
  - herms
  - roadmap
  - foundation
  - ci-cd
status: draft
---

# Phase 0: Walking Skeleton and Deploy Seams

## Goal

Prove that one request travels browser → Nginx → Lambda → Neon → back, in CI, on day one. The diagram has four independently deployed pieces; prove the seams before there is any business logic to debug on top of them.

## Scope

| Area | Decision |
|---|---|
| Repo | Monorepo: `apps/web` (TanStack), `apps/api` (Hono), `apps/notifier`, `packages/db`, `packages/shared` |
| DB | Neon PostgreSQL + Drizzle migrations, checked into git, forward-only |
| API | Hono on Lambda Function URL, `GET /api/health` returning DB round-trip time |
| Web | TanStack Start on Hostinger VPS, Nginx serving static plus proxying `/api/*` same-origin |
| CI/CD | Two pipelines: VPS rsync/deploy and Lambda deploy. Migrations run as a gated step, never automatically on cold start |
| Observability | Structured JSON logs with request ID propagated web → api → notifier |
| Seed | `pnpm db:seed` — 3 customers, 8 equipment items, 1 user per role |

## Why This Comes First

Every later phase debugs on top of these seams. If deployment, database access, or request routing is unreliable, the failure surfaces inside business logic where it is far more expensive to fix.

## Long-lead Items Started in Parallel

Do not block this phase on them:

- WhatsApp Business API / BSP account application (SRS §4.3, §10.2).
- Client confirmation of printed DN/RN field layout (SRS §10.2, §11.1, §11.2).

## Decisions Locked Here

- Neon driver choice (HTTP/serverless driver vs classic pooled `pg`) — decide now and never revisit.
- `proxy_read_timeout` on Nginx must exceed the Lambda's own timeout.
- Migration strategy: forward-only, gated, never auto-applied on cold start.

## Requirements Traceability

- Sections 2.4 to 2.6, 6.1, 6.3 of `Project Overview.md`.
- `Untitled Diagram.drawio.png`: the four deployed pieces and their seams.

## Exit Criteria

- A pushed commit deploys both web and API automatically.
- Health check is green from a real browser.
- The rollback procedure is written down and tested once.
- The 3-second dashboard NFR (§6.1) is measurable from day one as a cold-start + database round-trip baseline.

## Delivery Priority

**Must have.** This is the foundation for every other phase.
