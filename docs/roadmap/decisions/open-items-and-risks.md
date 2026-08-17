---
title: HERMS Open Decisions and Implementation Risks
date: 2026-08-17
tags:
  - herms
  - roadmap
  - decisions
  - risks
status: needs-review
---

# Open Decisions and Implementation Risks

These items should be resolved before the affected phase is marked complete. They are based on the SRS, the `IMPORTANT .docx` client notes, and the supplied architecture diagram.

## Client Decisions

| ID | Decision | Affected phase | Recommended direction |
|---|---|---|---|
| D-01 | Confirm the primary notification provider and fallback behavior. | 2, 3, 6 | Confirm WhatsApp Business API first; define SMS fallback and email scope separately. |
| D-02 | Confirm the exact printed/PDF Delivery Note and Retention Note layouts. | 3 | Reproduce the existing paper fields before finalizing forms. |
| D-03 | Confirm whether price escalation is automatic or awaits Business Owner confirmation. | 5, 6 | Keep the escalation calculation deterministic; make confirmation a separate approval state only if the client requires it. |
| D-04 | Confirm whether damage claims require a separate approval before affecting balances. | 5 | Require Finance confirmation as specified by FR-9.6. |
| D-05 | Confirm the launch role model and store/branch scope. | 1, 4 | Keep the six SRS roles and include store scope in users, stock, notes, and approvals. |

## Requirement Clarifications

| ID | Clarification | Risk if unresolved |
|---|---|---|
| R-01 | FR-9.1 says current unit price, while FR-9.5 and BR-6 require the price on the damage date. | Incorrect claim values and financial disputes. |
| R-02 | FR-11.5 mentions SMS fallback, but the requirement itself only covers resend/regeneration. | Fallback behavior may be implemented without a clear requirement or acceptance test. |
| R-03 | FR-4.7 references FR-7.7, but FR-7.7 does not exist. | Reopening permissions and audit behavior remain undefined. |
| R-04 | Define whether original delivered quantity means approved handed-over quantity or another quantity. | Return reconciliation and stock totals may disagree. |
| R-05 | Define stock semantics when issued and handed-over quantities differ. | Delivery stock-out may be calculated incorrectly. |
| R-06 | Define the responsible party for delivery mismatches. | Claims and discrepancy ownership may be inconsistent. |

## Architecture Risks and Controls

| Risk | Control |
|---|---|
| Public Lambda Function URL is treated as secure because Nginx normally proxies it. | Enforce authentication, token validation, rate limiting, request validation, and monitoring at the Lambda boundary. |
| Database mutation succeeds but direct SQS publication fails. | Use a transactional outbox or equivalent durable event publication mechanism. |
| Lambda and Neon connection behavior causes failures under reuse or idle periods. | Define pooling, connection reuse, transaction boundaries, migrations, backups, and recovery testing. |
| Scheduler retries apply price escalation more than once. | Use an idempotency key based on equipment item and escalation period. |
| Notification retries create duplicate business records. | Separate business mutations from notification delivery and deduplicate by source event. |
| Agent prompt injection or excessive permissions causes unauthorized actions. | Use read-only access first, allowlisted tools, server-side authorization, human approval, and complete audit logging. |

## Resolution Rule

No implementation should silently choose a behavior that affects stock, money, price history, authorization, or customer responsibility. Record the decision, update the SRS, and add or update acceptance criteria before closing the relevant roadmap phase.
