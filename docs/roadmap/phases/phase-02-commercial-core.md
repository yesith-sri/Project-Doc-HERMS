---
title: Phase 2 - Commercial Core
date: 2026-08-17
tags:
  - herms
  - roadmap
  - quotations
  - orders
status: draft
---

# Phase 2: Commercial Core

## Goal

Implement the commercial flow from customer request through quotation acceptance and order creation.

## Why This Comes Second

Delivery and retention notes require a valid order. The order must also preserve the exact pricing that was agreed in the quotation, so pricing and quotation data must exist before operational workflows begin.

## Scope

- Customer and equipment master data.
- Recurring versus New customer classification.
- Fixed customer price lists for recurring customers.
- Custom per-item pricing for new-customer quotations.
- Quotation line-item price snapshots.
- Quotation statuses: Sent, Accepted, Rejected, and Expired.
- Conversion of an accepted quotation into an order without re-entering lines.
- Order totals and invoice-value foundation.
- Notification-provider abstraction for sending quotations.

## Business Rules

- BR-1: recurring customers always use their stored fixed price list.
- New customers use custom per-item quotation prices.
- A quotation or order must retain the price used at that time.
- Invalid quotation status transitions must be rejected.

## Requirements Traceability

- FR-1.1 to FR-1.5: customer and pricing management.
- FR-2.1 to FR-2.4: quotation management.
- FR-8.1: invoice value per order.
- Section 3.1, Section 3.2, and Section 5.1 of `Project Overview.md`.

## Dependencies

Phase 1 authentication, authorization, audit, database, and validation services.

## Acceptance Criteria

- Sales staff can create customers and equipment items.
- Recurring customers automatically receive their fixed prices.
- New customers can receive custom prices per quotation line.
- Accepted quotations create orders without duplicate manual entry.
- Historical quotation and order prices remain unchanged after later price changes.
- Every quotation transition is authorized and auditable.

## Delivery Priority

**Must have.** This is the first business capability and the source record for all rental operations.
