---
title: Phase 4 - Store Approval and Inventory Control
date: 2026-08-17
tags:
  - herms
  - roadmap
  - stock
  - approval
status: draft
---

# Phase 4: Store Approval and Inventory Control

## Goal

Make Store Admin approval the authoritative gate for stock movement and discrepancy resolution.

## Why This Comes Fourth

Phase 3 creates submitted operational notes, but those notes are not authoritative until a physical count is reviewed and approved. This phase implements the central data-integrity rule in the SRS.

## Scope

- Pending Approval queue by assigned store.
- Store Admin and Deputy Store Admin visibility.
- Physical count entry against submitted notes.
- Difference detection and review.
- Approval, rejection, reopening, and correction states.
- Atomic stock-out from approved Delivery Notes.
- Atomic stock-in from approved Retention Notes.
- Confirmed missing/damaged write-off from usable stock.
- Live stock ledger and stock value.
- Discrepancy statuses: Open, Resolved, Written Off, and Claimed.
- Configurable reversal window and privileged reversal workflow.

## Business Rules

- BR-4: no stock quantity changes before Store Admin physical count and approval.
- Approved stock movements must reference their source note and actor.
- Reversals create new audit entries and never edit historical records.

## Requirements Traceability

- FR-3.5 and FR-4.5: approval before stock updates.
- FR-5.2 to FR-5.5: discrepancy visibility and lifecycle.
- FR-6.1 to FR-6.5: stock ledger and write-offs.
- FR-7.1 to FR-7.5: Store Admin approval workflow.
- Sections 3.5, 3.6, 3.7, 6.3, and 6.6 of `Project Overview.md`.

## Dependencies

Phase 1 transactions, authorization, audit, and idempotency, plus Phase 3 submitted notes.

## Acceptance Criteria

- Submitted notes remain pending until Store Admin approval.
- Stock never changes before physical count and approval.
- Approval and stock posting are atomic and idempotent.
- Count differences require review before final approval.
- Every stock movement links to an approved source note and accountable actor.
- Write-offs reduce usable stock only after discrepancy confirmation.
- Reversal behavior is permission-controlled, time-bound, and fully audited.

## Delivery Priority

**Must have.** This phase is a release-blocking data-integrity control, not an optional enhancement.
