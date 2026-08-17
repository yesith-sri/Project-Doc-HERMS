---
title: HERMS Database Schema
date: 2026-08-17
tags:
  - herms
  - architecture
  - database
status: draft
---

# HERMS Database Schema

The database is **Neon PostgreSQL**, managed with **Drizzle** migrations that are checked into git and run forward-only as a gated CI step (never auto-applied on Lambda cold start).

## Driver

Use Neon's **HTTP/serverless driver** — no classic pooled `pg` connection per invocation. This decision is locked in Phase 0 (see [Architecture Risks](../roadmap/architecture-risks.md)).

## Core Entities

Indicative columns from SRS §5.1, expanded with the append-only and outbox tables required by the invariants.

| Table | Purpose | Key columns |
|---|---|---|
| `customer` | Customer master | name, type (`Recurring`/`New`), contact, fixed price list, outstanding balance |
| `equipment_item` | Equipment master | name, category, current unit price, unit of measure |
| `price_history` | Immutable price log | item, old price, new price, effective date, reason (escalation/negotiated) |
| `quotation` | Quotation | customer, lines (item, qty, unit price used), total, status (`Sent/Accepted/Rejected/Expired`) |
| `order` | Order | linked quotation, customer, status, priced line snapshot |
| `delivery_note` | Delivery Note | order, lines (issued qty, handed-over qty, reason), submitted-by, approved-by, status |
| `retention_note` | Retention Note | order/delivery ref, lines (returned, balance, missing/damaged, reason, responsible party), submitted-by, approved-by, status |
| `stock_ledger` | Append-only stock movements | source note id, item, quantity delta, direction (in/out/write-off), timestamp |
| `discrepancy` | Discrepancy registry | source note, item, type, reason, responsible party, value, status (`Open/Resolved/Written Off/Claimed`) |
| `payment` | Payments | order/customer, amount (minor units), date, method, remaining balance |
| `expense` | Expenses | category, amount (minor units), date, description |
| `audit_log` | Audit trail | actor, action, entity, before/after JSON, timestamp |
| `outbox` | Notification intents | event type, payload, status, idempotency key, created-at |
| `user` | Users and roles | name, role, contact, credentials/token |

## Append-only Invariants

Three tables are never updated or deleted in place:

| Table | Invariant | Enforcement |
|---|---|---|
| `stock_ledger` | I-1 | Only the approval transaction inserts rows; current quantity is derived, never a mutable `qty` column. |
| `price_history` | I-3 | Database trigger rejects `UPDATE`/`DELETE`; corrections are new offsetting rows. |
| `audit_log` | I-8 | Every sensitive change appends a row; reversals are new rows, never edits. |

## Money

All monetary values are integers in **minor units (cents)** (I-10). No floats, no `number` arithmetic on currency. Rounding rules are declared in one shared module.

## Price Freezing

Quotations, orders, and invoices store the unit price used at creation time (I-11). Historical documents never join to `equipment_item.current_price`. Claim amounts are looked up from `price_history` as at the date of damage (I-4).

## Reconciliation

Cumulative reconciliation across all retention notes for an order is enforced when the order is marked `Fully Returned` (I-6):

```
Σ returned + Σ balance + Σ missing_damaged = delivered
```

## Performance

- Current stock quantity is a sum or materialized view refreshed inside the approval transaction.
- Dashboard rankings use pre-aggregated rollup tables or materialized views, refreshed on approval — never a per-page-load ledger scan (3-second NFR, §6.1).
- Schema is multi-store-ready (§6.5) without building multi-branch features.

## Retention and Backups

- Transactional records are retained indefinitely (§5.2) unless the client specifies otherwise.
- Backup/restore rehearsal with documented RPO/RTO is scheduled for Phase 9.
