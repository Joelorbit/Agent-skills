# Engineering Constitution

> Do not blindly implement requests. First determine what the production-quality
> implementation requires. Identify missing requirements, architectural risks,
> security gaps, edge cases, and data integrity concerns. Prefer the simplest
> robust solution. Never silently make a dangerous assumption. If an assumption
> is unavoidable, state it and choose the safest reasonable default.

---

## Development Phases

Every significant implementation follows these phases in order.
Skip a phase only when explicitly justified.

```
PHASE 1  — Understand       Clarify problem, users, constraints
PHASE 2  — Plan             Scope, requirements, MVP boundary
PHASE 3  — Design           Architecture, data model, API contracts
PHASE 4  — Threat Model     Security risks and mitigations
PHASE 5  — Implement        Build incrementally, smallest working units
PHASE 6  — Test             Verify correctness at every level
PHASE 7  — Review           Correctness, security, performance
PHASE 8  — Harden           Error handling, edge cases, input validation
PHASE 9  — Document         README, API docs, architecture decisions
PHASE 10 — Deploy           Config, migrations, health checks
PHASE 11 — Verify           Smoke test, monitoring, rollback readiness
```

---

## Universal Coding Principles

| Principle                   | Guidance                                             |
| --------------------------- | ---------------------------------------------------- |
| Readable                    | The next developer understands it without asking you |
| Small                       | Functions, classes, and modules do one thing well    |
| Typed                       | Explicit types at trust boundaries, no unsafe casts  |
| Testable                    | Pure logic separated from side effects               |
| KISS                        | Simplest solution that handles real requirements     |
| YAGNI                       | Don't build what isn't needed yet                    |
| DRY                         | Without over-abstraction                             |
| Composition over inheritance | Where appropriate                                    |

**Avoid:** God classes, circular dependencies, deep nesting (>3 levels),
magic numbers/strings, swallowed exceptions, TODO-driven architecture.

---

## Quick Decision Framework

Before merging anything:

1. Is this the simplest solution that handles real requirements?
2. What happens when this fails?
3. What happens at scale?
4. Can this be exploited?
5. Can the next developer understand this without asking me?
6. Can this be rolled back?

---

## Definition of Done

A feature is done when:

```
✅ Requirements satisfied          ✅ Logging implemented
✅ Architecture respected          ✅ Documentation updated
✅ Types valid, lint passes        ✅ API documented
✅ Tests pass                      ✅ Performance acceptable
✅ Security reviewed               ✅ Accessibility considered
✅ All error paths handled         ✅ CI passes
✅ Migrations created (if needed)  ✅ Rollback strategy exists
```

---

## Available Engineering Skills

Activate the relevant skill when working on that concern:

| Skill            | Covers                                                     |
| ---------------- | ---------------------------------------------------------- |
| `engineering`    | Product discovery, requirements, project management, docs  |
| `security`       | Auth, authorization, vulnerabilities, AI safety, secrets   |
| `architecture`   | System design, layered architecture, repository structure  |
| `backend`        | API design, external APIs, caching, payments               |
| `frontend`       | UI states, components, accessibility, responsive design, EyuTheme |
| `database`       | Schema design, integrity, SQL performance, backups         |
| `testing`        | Test strategy, error handling, code review                 |
| `devops`         | CI/CD, Docker, deployment, observability, Git              |
| `handoff`        | Agent/session handoffs, context transfer, continuation     |
| `skill-creator`  | Authoring new skills: structure, frontmatter, verification |
