---
title: Phase 7 - Damage Claims and Price Escalation
date: 2026-08-17
tags:
  - herms
  - roadmap
  - claims
  - escalation
status: draft
---

# Phase 7: Damage Claims and Price Escalation

## Goal

Implement customer damage claims and the automatic six-month price escalation.

## Why Last of the Business Logic

Claims need Phase 4's responsible party and Phase 6's balances. Nothing else depends on claims, so this phase has no downstream pressure.

## Damage Claims

- Claim raiseable only where the responsible party is `Customer` (FR-9.1, BR-5).
- Finance review required before the claim hits the customer balance (FR-9.6, FR-9.2) — this closes the open item in SRS §10.2.
- Claim price = the price in effect on the date the damage was recorded (FR-9.5, BR-6). This is a point-in-time lookup against `price_history`, not `item.current_price`. It is the single most commonly mis-implemented rule in this SRS.

## Price Escalation

- +10% per item every six months from a configurable effective date (FR-9.3).
- Each run logs to price history with old/new price and effective date (FR-9.4).

## Scheduler Design

- Run a daily EventBridge Scheduler job that asks "is any item due for escalation today?", rather than a six-monthly cron. A six-monthly job that fails has a six-month blast radius and no retry surface.
- Make the job idempotent per `(item, effective_date)`.
- Link expiry does **not** need EventBridge. Check the token's expiry at read time. The diagram marks the scheduler optional — escalation is the only thing that genuinely requires it.

## Requirements Traceability

- FR-9.1 to FR-9.7.
- BR-5, BR-6.
- EventBridge Scheduler (or equivalent) from the architecture diagram.

## Dependencies

Phase 4 responsible party and discrepancy state; Phase 6 balances; Phase 1 immutable price history.

## Exit Criteria

- A damage recorded before an escalation date bills at the old price after the escalation has run.
- Re-running the escalation job twice in one day produces one price-history row, not two.

## Delivery Priority

**Must have.**
