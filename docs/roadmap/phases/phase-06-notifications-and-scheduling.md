---
title: Phase 6 - Notifications and Scheduled Automation
date: 2026-08-17
tags:
  - herms
  - roadmap
  - notifications
  - scheduling
status: draft
---

# Phase 6: Notifications and Scheduled Automation

## Goal

Make communication, link lifecycle management, and scheduled pricing automation reliable in production.

## Why This Comes Sixth

Basic workflows can be developed against a provider abstraction, but production delivery requires reliable asynchronous processing. Notifications and scheduled actions must not compromise database transactions or create duplicate business effects.

## Scope

- Transactional outbox from the Hono backend.
- AWS SQS queue with retry policy and dead-letter queue.
- Separate Notification Lambda deployment.
- WhatsApp Business API provider integration.
- Provider delivery status, retry, failure, and duplicate suppression.
- SMS fallback only after client confirmation and provider validation.
- Store Admin notification for pending approvals.
- Sales and Finance notification after approval.
- EventBridge Scheduler or an equivalent trusted scheduler.
- Idempotent 10% price escalation per equipment item every six months.
- Immutable price-history entries with effective date and reason.
- Link expiry enforcement in the request path.
- Optional cleanup of expired tokens.

## Architecture Rules

- Do not publish directly to SQS after a database mutation without an outbox or equivalent recovery mechanism.
- A notification retry must never create a second quotation, order, note, claim, or payment.
- A scheduler retry must never apply the same price escalation twice.
- The Lambda Function URL must enforce its own security even when requests normally arrive through Nginx.

## Requirements Traceability

- FR-2.2: quotation delivery.
- FR-9.3, FR-9.4, and FR-9.7: scheduled escalation and immutable history.
- FR-10.7: escalation visibility.
- FR-11.1 to FR-11.5: links and operational notifications.
- BR-6 and Sections 4.3, 4.4, and 10.2 of `Project Overview.md`.

## Dependencies

Phases 1 through 5, confirmed notification provider, price-as-of service, and immutable history.

## Acceptance Criteria

- Failed notifications retry without duplicating business records.
- Poison messages reach the dead-letter queue and trigger an operational alert.
- Every notification is traceable to its source event.
- Expired and revoked links cannot be used.
- Each six-month escalation is applied once even after scheduler retries.
- Previous price, new price, effective date, and reason are immutable and auditable.
- Damage claims use the correct historical price.
- Missed scheduler runs can be detected and safely recovered.

## Delivery Priority

**Production gate.** The EventBridge mechanism is optional, but reliable six-month escalation and notification behavior are mandatory requirements for a compliant launch.
