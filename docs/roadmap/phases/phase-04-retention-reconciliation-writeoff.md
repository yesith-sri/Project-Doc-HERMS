---
title: Phase 4 - Retention Note, Reconciliation and Write-off
date: 2026-08-17
tags:
  - herms
  - roadmap
  - retention
  - reconciliation
  - stock
status: draft
---

# Phase 4: Retention Note, Reconciliation and Write-off

## Goal

Digitize returns, enforce cumulative reconciliation, and write off confirmed missing or damaged stock.

## Scope

- Retention Note per line: `returned_qty`, `balance_qty`, `missing_damaged_qty` (FR-4.1).
- Cumulative reconciliation across all retention notes for the order, enforced when the order is marked `Fully Returned` — not on each partial note (FR-4.2, BR-3).
- Multiple partial retention notes per order (FR-4.6) — required for launch despite its `S` priority, because it changes the reconciliation model.
- Shortfall requires type and responsible party `Customer | Staff Member` (FR-4.3). This field is what makes Phase 7 billing possible — no responsible party, no claim (BR-5).
- Approval gate reused unchanged (FR-4.5).
- Returned quantity increases stock; confirmed missing/damaged is written off from usable stock (FR-6.3).
- Reversal window (FR-6.5): Store Admin may reverse a write-off within a configurable 7 days with a reason; beyond that, System Admin override. All reversals are new audit rows, never edits.
- Discrepancy resolution states: `Resolved | Written Off | Claimed` (FR-5.5).

## Reconciliation Invariant

```
returned_qty + balance_qty + missing_damaged_qty = delivered_qty
```

Getting this wrong makes partial returns impossible. Enforce it cumulatively, not per note.

## Requirements Traceability

- FR-4.1 to FR-4.7: Retention Note lifecycle and correction.
- FR-6.3, FR-6.5: write-off and reversal.
- FR-5.5: discrepancy resolution statuses.
- BR-3 and BR-5.

## Dependencies

Phase 3 ledger, approval gate, and discrepancy registry.

## Exit Criteria

- An order delivered 100, returned 60 + 30 + 8 missing + 2 damaged reconciles and closes.
- The same order with 99 accounted for refuses to close.

## Delivery Priority

**Must have.**
