---
name: architecture
description: >-
  System architecture, layered design, repository structure, and component boundaries.
  Activate when designing a new feature, restructuring the repository, or setting up modules.
---

# Architecture Standard

## 1. Core Principles
- **Keep it Simple:** Prefer a modular monolith. Avoid distributed systems (microservices) until scaling or organizational boundaries demand them.
- **Explicit Boundaries:** Define clear entry/exit points for components. Modules should hide implementation details and expose a clean API interface.
- **Separation of Concerns:** Maintain clean logic separation across layers.

## 2. Layered Architecture
Strictly enforce the following layer dependencies (top-down only):

```
┌─────────────────────────────────────────────────────────┐
│                      Presentation                       │
│    (HTTP API Controllers, CLI Commands, UI Views)       │
└────────────────────────────┬────────────────────────────┘
                             │ (uses)
                             ▼
┌─────────────────────────────────────────────────────────┐
│                       Application                       │
│     (Use Cases, Workflows, Orchestrators, Tx Boundaries)│
└────────────────────────────┬────────────────────────────┘
                             │ (uses)
                             ▼
┌─────────────────────────────────────────────────────────┐
│                         Domain                          │
│     (Business Rules, Entities, Pure Logic, Policies)    │
└────────────────────────────┬────────────────────────────┘
                             │ (implements abstractions/uses)
                             ▼
┌─────────────────────────────────────────────────────────┐
│                     Infrastructure                      │
│     (DB Repositories, Queue Consumers, HTTP Clients)    │
└─────────────────────────────────────────────────────────┘
```

- **Domain Rules:** The domain layer must not depend on database libraries, framework helpers, or communication protocols.
- **Infrastructure Isolation:** Database queries, network requests, and external library code belong exclusively in the infrastructure layer.

## 3. System Design Checklist
Before implementing a new subsystem, define:

| Subsystem Component | Requirement | Design Strategy |
| :--- | :--- | :--- |
| **Component Boundaries** | Logical grouping | Group code by business domain (e.g., `billing`, `users`), not by technology type (e.g., `controllers`, `models`). |
| **Data Flow** | Traceability | Map how data is fetched, processed, mutated, and saved. |
| **Authentication Flow**| Security context | Define where the security context is parsed and how it is propagated. |
| **Error Propagation** | Predictability | Establish how internal errors map to presentation-layer errors. |
| **Configuration** | Dynamism | Separate deployment config (e.g., URLs, timeouts) from code. |

## 4. Repository Structure
Adopt a consistent codebase layout. Adapt this structure based on language conventions:

```text
/
├── apps/               # Entry points (web API, worker, CLI runner)
├── packages/           # Internal shared libraries, utilities
├── migrations/         # Version-controlled DB schema files
├── config/             # Config templates and environment rules
├── tests/              # E2E and integration test suites
├── docs/               # System architecture documentation, ADRs
├── .env.example        # Environment variables template
├── README.md           # Getting started and setup guide
└── docker-compose.yml  # Local dependency orchestration
```

## 5. Architectural Decision Records (ADRs)
- Document non-trivial architectural changes in a markdown file under `docs/adr/`.
- Every ADR must describe the context, decision, consequences (both positive and negative), and alternatives rejected.
