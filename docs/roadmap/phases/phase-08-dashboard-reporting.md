---
title: Phase 8 - Dashboard and Reporting
date: 2026-08-17
tags:
  - herms
  - roadmap
  - dashboard
  - reporting
status: draft
---

# Phase 8: Dashboard and Reporting

## Goal

Provide management visibility. Every tile now reads real, reconciled data.

## Scope

- Stock quantity and value by item (FR-10.1).
- Pending vs received payments this month, with prior-month trend (FR-10.2).
- Monthly income vs expenses, net (FR-10.3).
- Open missing/damaged with value, reason, responsible party, filterable by date/customer/item (FR-10.4, FR-5.2).
- Ranked most-missing/damaged items and most-associated customers (FR-10.5, FR-5.3, FR-5.4).
- Pending escalation confirmations (FR-10.7).
- PDF/Excel export (FR-10.6).

## Performance Decision

The 3-second NFR (§6.1) over years of data means pre-aggregated rollup tables or materialized views refreshed on approval. Do not compute rankings by scanning the ledger per page load.

## Requirements Traceability

- FR-10.1 to FR-10.7.
- FR-5.2, FR-5.3, FR-5.4.
- SRS §9.

## Dependencies

Phases 3–7 transactional records and rollups.

## Exit Criteria

- Each dashboard total reconciles to its source transactions.
- Dashboard load meets the 3-second NFR with multi-year data.

## Delivery Priority

**Must have.** Export (FR-10.6) is Should Have and may follow the first release if necessary.
