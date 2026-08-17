---
title: Phase 5 - Finance, Claims, and Reporting
date: 2026-08-17
tags:
  - herms
  - roadmap
  - finance
  - reporting
status: draft
---

# Phase 5: Finance, Claims, and Reporting

## Goal

Complete the financial-control workflow and provide management visibility over stock, discrepancies, payments, and business performance.

## Why This Comes Fifth

Claims depend on confirmed discrepancies and historical prices. Financial balances depend on orders and approved claims. Dashboards are meaningful only after the transactional records from Phases 2 and 4 are reliable.

## Scope

- Order invoice values.
- Partial payment recording.
- Customer and order outstanding balances.
- Expense recording.
- Monthly income, expenses, and net position.
- Damage-claim creation for customer-responsible damage.
- Finance/Accounts confirmation before adding a claim to the customer balance.
- Price-at-date lookup and claim price snapshots.
- Stock, discrepancy, payment, and financial dashboards.
- Missing/damaged item and customer rankings.
- Optional report export.

## Business Rules

- BR-5: only customer-responsible damage may be billed to the customer.
- Claims require Finance confirmation before affecting the customer balance.
- BR-6: claims use the price in effect on the damage-record date.
- Price history is immutable; corrections use offsetting entries.

## Requirements Traceability

- FR-5.2 to FR-5.5: discrepancy reporting and status.
- FR-8.1 to FR-8.5: payments, invoices, expenses, and monthly totals.
- FR-9.1, FR-9.2, and FR-9.5 to FR-9.7: claims and historical prices.
- FR-10.1 to FR-10.7: dashboard and reporting.
- Sections 3.8, 3.9, 3.10, and 9 of `Project Overview.md`.

## Dependencies

Phases 1, 2, and 4, including immutable audit records and price-history services.

## Acceptance Criteria

- Partial payments correctly update order and customer balances.
- Customer-responsible damage can produce a claim.
- Staff-responsible damage cannot be billed to the customer.
- No claim affects the balance before Finance confirmation.
- Claim values use the historical price effective on the damage date.
- Dashboard totals reconcile with source transactions.
- Financial changes include actor, timestamp, and before/after audit data.

## Delivery Priority

**Must have.** Report export is **Should have** and may follow the first MVP release if necessary.
