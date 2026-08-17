---
title: Phase 5 - Async Spine and Notifications
date: 2026-08-17
tags:
  - herms
  - roadmap
  - notifications
  - sqs
  - outbox
status: draft
---

# Phase 5: Async Spine and Notifications

## Goal

Make communication reliable and asynchronous: drain the outbox, deliver via SQS, and integrate the notification provider.

## Why the Queue Arrives Now

Phases 2–4 wrote to an outbox instead of calling providers directly, so nothing has to be rewritten — only drained.

## Scope

- Transactional outbox → publisher → SQS. The API writes the outbox row in the same transaction as the business change; a publisher moves rows to SQS.
- Do **not** `PutMessage` to SQS from inside the request path — a post-commit failure silently loses the notification.
- Notification Lambda consumes SQS, has its own deploy, a DLQ attached, and an alarm on DLQ depth > 0.
- Providers: WhatsApp Business API primary, automatic SMS fallback on delivery failure (SRS §4.3), email for back-office notices.
- Idempotency key per notification — SQS delivers at-least-once, and customers must not receive a quotation twice.

## Notifications to Wire Up

- Quotation to customer (FR-2.2).
- Note link to field staff (FR-11.1).
- Admin alert on pending approval; sales/finance alert on approval (FR-11.4).
- Link resend/regenerate with attribution (FR-11.5).

## Requirements Traceability

- FR-2.2: quotation delivery.
- FR-11.4, FR-11.5: approval alerts and link resend.
- SRS §4.3: notification strategy and fallback.

## Dependencies

Phases 2–4 outbox writes; confirmed notification provider.

## Exit Criteria

- Killing the WhatsApp credential causes messages to fall back to SMS.
- Messages reach the DLQ only after both primary and fallback fail.
- Re-delivery never produces a duplicate quotation or link.

## Delivery Priority

**Production gate.**
