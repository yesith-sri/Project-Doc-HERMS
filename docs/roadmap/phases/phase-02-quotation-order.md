---
title: Phase 2 - Quotation to Order
date: 2026-08-17
tags:
  - herms
  - roadmap
  - quotations
  - orders
status: draft
---

# Phase 2: Quotation → Order

## Goal

Implement the commercial flow: a quotation with the correct automatic price, a status machine, and conversion into an order without re-entry.

## Scope

- Pricing resolver as a single function used everywhere: fixed list for Recurring, manual entry for New (BR-1).
- Quotation status machine: `Sent → Accepted | Rejected | Expired` (FR-2.3).
- Accept → Order with no line-item re-entry (FR-2.4).
- Quotation and order line items store the price used at the time, never a live reference to the current price.

## WhatsApp (FR-2.2) is Deferred to Phase 5

- Generate the quotation PDF and write a row to the `outbox` table.
- Do not call the WhatsApp provider yet. Phase 5 drains the outbox.
- Until then the UI offers "copy link / download PDF".

## Business Rules

- BR-1: recurring customers always use their stored fixed price list; new customers use custom per-item prices.

## Requirements Traceability

- FR-2.1: quotation generation with unit price and total.
- FR-2.2 (partial): quotation delivery, deferred to Phase 5.
- FR-2.3: quotation status.
- FR-2.4: conversion to order without re-entering lines.

## Dependencies

Phase 1 master data, pricing, and audit.

## Exit Criteria

- A recurring customer's quotation prices itself with zero manual input.
- An accepted quotation produces an order with identical line items.

## Delivery Priority

**Must have.**
