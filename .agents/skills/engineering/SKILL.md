---
name: engineering
description: >-
  Product discovery, requirements, project management, and documentation standards.
  Activate at the start of any project, feature, or planning phase.
---

# Engineering Standard: Product Discovery & Management

## 1. Product Discovery Checklist
Before writing code, establish clarity on these twelve pillars:

| Pillar | Focus | Deliverable / Goal |
| :--- | :--- | :--- |
| **Problem Statement** | Core Pain | Exactly what user problem are we solving? |
| **Target Users** | Persona | Who will use this? What is their technical level? |
| **User Stories** | Value | "As a... I want to... so that..." format for key paths. |
| **Functional Reqs** | Scope | What the system *must* do (behaviors, outputs). |
| **Non-Functional Reqs** | Quality | Performance targets, concurrent users, availability. |
| **Constraints** | Limits | Deadlines, budgets, mandatory tech stacks, regulators. |
| **Assumptions** | Unknowns | Explicitly list assumptions to validate early. |
| **Success Criteria** | Metrics | How we define success (e.g., latency <200ms, zero data loss). |
| **Out of Scope** | Boundaries | What we are *not* building in this cycle. |
| **Risks & Mitigation** | Safety | Technical or business blockages and fallbacks. |
| **Dependencies** | External | Libraries, APIs, databases, or team blockers. |
| **MVP Boundary** | Focus | The minimum set of features to deliver value. |

## 2. Requirements Engineering
- **Breakdown:** Decompose complex epics into small, independent, and verifiable tasks (1-3 days maximum).
- **Critical Path:** Sequence tasks based on technical dependency. Build core data flow and database schemas before UI.
- **MoSCoW Prioritization:**
  - **Must Have:** Core to the system; failure means launch abort.
  - **Should Have:** High value, but can work around in MVP.
  - **Could Have:** Low cost, nice to have if time permits.
  - **Won't Have:** Deferred to future iterations.

## 3. Project Management & Scope Control
- **No Scope Creep:** Do not add "nice-to-have" features during implementation. Create a new issue/ticket for them.
- **Technical Debt:** Document shortcuts taken for speed. Allocate 20% of sprint/cycle capacity to clean them up.
- **Risk Log:** Track unresolved requirements or third-party APIs. Address highest-risk assumptions first.

## 4. Documentation Standard
Every production repository must contain:
1. **[README.md](file:///home/latexjo/Projects/Skills-for-projects/README.md):** Purpose, prerequisites, quickstart (run/test locally in <5 mins).
2. **Setup Guides:** `.env.example` showing all environment variables with descriptions, types, and defaults.
3. **Architecture Decisions:** Context, decision, consequences, and alternatives considered (use ADR format in `docs/adr/`).
4. **API Specs:** OpenAPI (Swagger) or equivalent, kept in sync automatically.
5. **Operational Docs:** Runbooks for deployment, migrations, backups, and rollback.

## 5. Dependency Management
- **Ruthless Selection:** Do not install a library for trivial helper functions (<50 lines of code).
- **Audit Checklist:** Before adding a dependency, verify:
  - Is it actively maintained (commits in the last 3-6 months)?
  - Is it free of critical vulnerabilities (`npm audit` or `go nogo`)?
  - Is the license permissive (MIT, Apache 2.0)?
  - What is the bundle/image size impact?
- **Locking:** Pin exact versions in lockfiles (`package-lock.json`, `go.sum`, etc.).
