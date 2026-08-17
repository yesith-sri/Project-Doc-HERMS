---
title: Phase 3 - Stock Ledger, Delivery Note and Approval Gate
date: 2026-08-17
tags:
  - herms
  - roadmap
  - stock
  - delivery
  - approval
status: draft
---

# Phase 3: Stock Ledger, Delivery Note and Approval Gate ⭐

## Goal

Implement the highest-risk, highest-value capability: an append-only stock ledger, link-based Delivery Note submission, and the Store Admin approval gate. Budget the most time here.

## Scope

### Stock Ledger

- Append-only ledger, never a mutable `qty` column.
- Every row cites a source note ID (FR-6.1, NFR §6.6).
- Current quantity is a sum or a materialized view refreshed inside the approval transaction.

### Delivery Note

- Per line: `issued_qty` vs `handed_over_qty` (FR-3.1).
- Mismatch forces a reason from `Damaged | Missing | Not Accepted | Other`, with `Other` requiring free text (FR-3.2).
- Mismatch auto-creates a discrepancy row (FR-3.3) — the registry write path ships here; ranked reports wait for Phase 8.

### Tokenized Link Submission

- Unique, time-bound, scoped to a single note (FR-3.4, FR-11.1, FR-11.2, FR-12.2).
- Mobile form pre-filled with order lines, no login.
- The token is the identity for attribution (FR-12.3).
- Field staff may correct a submitted note until the admin starts counting (FR-4.7, delivery side).

### Approval Gate

- Submitted note lands in a Pending Approval queue visible to the Store Admin and any Deputy for that store (FR-7.1).
- Admin enters a physical count; a difference from the submitted figure is flagged for review before approval (FR-7.2, FR-7.3).
- Stock moves only inside the approval transaction (FR-3.5, BR-4). There must be no other code path that writes to the ledger.

## Business Rules

- BR-2: delivery mismatch reasons.
- BR-4: no stock update before Store Admin count and approval.

## Requirements Traceability

- FR-6.1, FR-6.2: stock ledger and value.
- FR-3.1 to FR-3.5: Delivery Note lifecycle.
- FR-11.1, FR-11.2, FR-11.3, FR-11.5: link submission and resend.
- FR-7.1 to FR-7.5: approval workflow.
- FR-12.2: field staff without full login.
- FR-5.1: discrepancy registry write path.

## Dependencies

Phase 2 orders; Phase 1 identity, tokens, audit, and transactions.

## Exit Criteria

- An integration test asserts stock is unchanged after submission and changed only after approval.
- A second test asserts no route other than approval can insert a ledger row.
- Both tests are permanent regression guards.

## Delivery Priority

**Must have.** This is the release-blocking data-integrity control.
