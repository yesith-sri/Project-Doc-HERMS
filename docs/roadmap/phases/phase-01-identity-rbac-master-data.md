---
title: Phase 1 - Identity, RBAC and Master Data
date: 2026-08-17
tags:
  - herms
  - roadmap
  - identity
  - rbac
  - master-data
status: draft
---

# Phase 1: Identity, RBAC and Master Data

## Goal

Establish users, roles, customers, equipment items, append-only price history, and the audit-log foundation.

## Why This Comes First (After the Skeleton)

Nothing can be attributed to a user until users exist, and nothing can be priced until items and price lists exist. The append-only price-history and audit-log tables are retroactive invariants that later phases depend on, so they must be correct from day one.

## Scope

- Six roles (SRS §2.3): Business Owner, Sales, Field Staff, Store Admin, Finance, System Admin.
- Route-level RBAC, not UI-only enforcement.
- Equipment item master: name, category, current unit price, unit of measure.
- Customer master with `type = Recurring | New` (FR-1.1).
- Fixed price list per recurring customer (FR-1.2).
- New → Recurring transition (FR-1.4).
- `price_history` table built append-only (FR-1.5, FR-9.7).
- `audit_log` table foundation (FR-7.5, NFR §6.2, §6.6): actor, action, entity, before/after JSON, timestamp.

## Key Decisions

- Corrections to prices are new offsetting rows, never `UPDATE`.
- Enforce the append-only rule with a database trigger, not a code convention — the escalation job in Phase 7 depends on this being true retroactively.
- Every later phase writes to `audit_log`; the schema must be final enough to avoid later migration churn.

## Requirements Traceability

- FR-12.1, FR-12.3: role-based access and attributable actions.
- FR-1.1 to FR-1.5: customer and pricing master data.
- FR-9.7: immutable price history.
- FR-7.5 and NFR §6.2, §6.6: audit trail.

## Dependencies

Phase 0 deploy seams and database migrations.

## Exit Criteria

- A Sales user can see customers but not the audit log.
- A Field Staff user can reach neither.
- Price edits appear in history as new rows and cannot be edited or deleted.

## Delivery Priority

**Must have.**
