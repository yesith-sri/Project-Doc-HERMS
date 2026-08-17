---
title: Phase 1 - Platform Foundation and Control Plane
date: 2026-08-17
tags:
  - herms
  - roadmap
  - foundation
status: draft
---

# Phase 1: Platform Foundation and Control Plane

## Goal

Establish the secure, auditable, and deployable foundation required by every later HERMS workflow.

## Why This Comes First

Customers, orders, stock, claims, payments, notifications, and agent tools all depend on identity, authorization, reliable data transactions, and traceable changes. Building business screens before these controls would create rework and risk corrupting stock or financial data.

## Scope

- Hostinger VPS deployment with HTTPS and Nginx.
- Frontend hosting and `/api/*` reverse proxy.
- Hono.js monolith on an AWS Lambda Function URL.
- Neon PostgreSQL schema, migrations, backups, and serverless connection strategy.
- Authentication and role-based access control for all six SRS roles.
- Store assignment and Deputy Store Admin permissions.
- Secure note-token validation for field staff without full login.
- Transaction boundaries, validation, idempotency keys, and consistent error handling.
- Immutable audit log for stock, price, discrepancy, payment, claim, token, and approval actions.
- Secrets/configuration management, structured logs, correlation IDs, health checks, and alerts.
- Transactional outbox design for reliable event publication.

## Requirements Traceability

- FR-7.5: complete approval audit trail.
- FR-9.7: immutable price history.
- FR-12.1 to FR-12.3: role-based access and attributable actions.
- Sections 2.3 to 2.6, 5, and 6 of `Project Overview.md`.
- Section 10.2 open items must be resolved before the affected workflows are finalized.

## Dependencies

None. This phase requires architecture approval and client decisions, but no business phase must precede it.

## Acceptance Criteria

- A protected staging environment is reachable through HTTPS and Nginx.
- Direct access to the Lambda Function URL is also authenticated, authorized, rate-limited, and monitored.
- Users can only access permitted roles, stores, and actions.
- Every sensitive mutation records actor, timestamp, source record, before/after values, and correlation ID.
- Repeated requests cannot duplicate a business effect.
- Database changes and their resulting events have a defined recovery path.
- Secrets are not stored in source control or documentation.

## Delivery Priority

**Must have.** This is the foundation for the MVP and the safety boundary for future agentic features.
