---
title: Phase 6 - Payments, Invoicing and Expenses
date: 2026-08-17
tags:
  - herms
  - roadmap
  - finance
  - payments
status: draft
---

# Phase 6: Payments, Invoicing and Expenses

## Goal

Track amounts owed and paid per order and customer, and record business expenses.

## Scope

- Invoice value per order from the order's own priced lines — never recomputed from current item prices (FR-8.1).
- Payments including partials (FR-8.2).
- Outstanding balance per customer and per order (FR-8.3).
- Expenses independent of orders (FR-8.4).
- Monthly income vs expenses and net position (FR-8.5).

## Key Decision

Store money as integer minor units; never use floating-point numbers.

## Requirements Traceability

- FR-8.1 to FR-8.5.

## Dependencies

Phase 2 orders; Phase 1 audit.

## Exit Criteria

- Three partial payments against one invoice leave a correct balance to the cent.
- Monthly figures match the sum of their rows.

## Delivery Priority

**Must have.**
