# Hotel Equipment Rental Management System (HERMS)

**Project Documentation Repository** — Vizualabs (Pvt) Ltd

---

## About

**HERMS** is a business-management system for a company that rents cutlery and crockery (spoons, forks, soup bowls, and related equipment) to hotels and event organisers.

It replaces the current manual, paper-based process with a fully digital workflow covering the complete rental lifecycle:

> **Quotation → Order Acceptance → Delivery Note → Retention (Return) Note → Stock & Discrepancy Tracking → Damage Claims → Pricing Escalation → Payments → Reporting Dashboard**

## Key Features

| Area | Description |
|------|-------------|
| Quotation & Pricing | Fixed price list for recurring customers, custom per-order pricing for new customers |
| Delivery Notes | Store-issued vs. handed-over quantity tracking with mismatch reasons |
| Retention Notes | Partial return tracking with cumulative reconciliation |
| Stock Management | Live stock quantity & value with discrepancy write-off |
| Damage Claims | Customer-responsible damage billing with finance confirmation |
| Auto Price Escalation | Automatic 10% price increase every 6 months |
| Payments | Pending vs. received payments, monthly income & expenses |
| Dashboard | Management visibility across stock, discrepancies & financials |

## Repository Contents

| Document | Description |
|----------|-------------|
| [Project Overview.md](Project%20Overview.md) | Software Requirements Specification (SRS) v1.0 — functional & non-functional requirements, business rules, data model, workflow, and open items. |
| [Implementation Roadmap](docs/roadmap/README.md) | Dependency-aware delivery phases (0–10), MVP scope, agentic workflow strategy, and open decisions. |
| [Invariants](docs/roadmap/invariants.md) | The 12 non-negotiable rules that hold in every phase. |
| [Architecture](docs/architecture/system-architecture.md) | System, backend, database schema, and frontend design. |

## Document Status

| Field | Value |
|-------|-------|
| **Version** | 1.0 |
| **Status** | Draft for Review |
| **Date** | 11 August 2026 |

> Items in [Section 10.2](Project%20Overview.md#102-open-items-for-client-confirmation) are still pending client confirmation.

## Quick Links

- [Functional Requirements](Project%20Overview.md#3-system-features-functional-requirements)
- [Business Rules](Project%20Overview.md#7-business-rules)
- [Process Workflow](Project%20Overview.md#8-process-workflow-overview)
- [Data Requirements](Project%20Overview.md#5-data-requirements)
- [Open Items](Project%20Overview.md#102-open-items-for-client-confirmation)
- [Implementation Roadmap](docs/roadmap/README.md)
- [Invariants](docs/roadmap/invariants.md)

## User Roles

The system is used by: **Business Owner / Management**, **Sales / Order Staff**, **Delivery / Field Staff**, **Store Admin**, **Finance / Accounts Staff**, and **System Administrator**.

## Contact

**Vizualabs (Pvt) Ltd** — [info@vizualabs.com](mailto:info@vizualabs.com)
