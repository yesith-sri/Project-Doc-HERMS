---
title: Phase 3 - Field Delivery and Return Operations
date: 2026-08-17
tags:
  - herms
  - roadmap
  - delivery
  - retention
status: draft
---

# Phase 3: Field Delivery and Return Operations

## Goal

Digitize delivery and return collection through fast, mobile-friendly Delivery Notes and Retention Notes.

## Why This Comes Third

This is the operational heart of the rental lifecycle. It depends on accepted orders from Phase 2 and produces the submitted records that Store Admins approve in Phase 4.

## Scope

- Delivery Note generation from an order.
- Retention Note generation against an order and prior delivery.
- Mobile-responsive, pre-filled forms for field staff.
- Unique, scoped, time-bound or single-use submission tokens.
- Token resend, regeneration, expiry, and revocation.
- Delivery quantities: issued and handed over.
- Delivery mismatch reasons: Damaged, Missing, Not Accepted, and Other with required detail.
- Partial retention notes.
- Returned, balance, missing, and damaged quantities.
- Cumulative return reconciliation.
- Discrepancy records created from note mismatches.
- Field correction until Store Admin approval counting begins.

## Business Rules

- BR-2: delivery mismatches require an allowed reason.
- BR-3: returned plus balance plus missing/damaged must equal the original delivered quantity when fully reconciled.
- BR-4: submitted notes do not change stock before approval.

## Requirements Traceability

- FR-3.1 to FR-3.4: Delivery Notes.
- FR-4.1 to FR-4.7: Retention Notes and partial returns.
- FR-5.1: discrepancy registry.
- FR-11.1 to FR-11.5: link-based note submission.
- Sections 3.3, 3.4, 3.5, 3.11, and Appendix 11 of `Project Overview.md`.

## Dependencies

Phase 1 security and token services, plus Phase 2 customers, equipment, and orders.

## Acceptance Criteria

- A field user can complete a typical note on a mobile browser in under two minutes.
- A token exposes only its assigned note and order.
- Expired, revoked, and already-submitted links cannot submit data.
- Delivery mismatches require a reason and required detail where applicable.
- Multiple partial returns are supported.
- Full reconciliation is enforced when the order is marked Fully Returned.
- Note submission creates a pending record and does not modify stock.

## Delivery Priority

**Must have.** Partial returns are explicitly required for launch even though FR-4.6 is marked Should Have.
