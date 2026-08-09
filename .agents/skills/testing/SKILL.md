---
name: testing
description: >-
  Quality assurance standard covering unit, integration, and E2E testing strategies,
  resilient error handling, and code review criteria. Activate when writing tests or reviews.
---

# Testing & Quality Standard

## 1. Test Pyramid & Execution
Quality is verified at multiple levels. Focus testing effort where confidence is highest relative to run cost:

```
        E2E          ◄── Few: Test critical user flows end-to-end
       /   \
  Integration        ◄── Moderate: Database integrations, API handlers
     /       \
       Unit          ◄── Many: Pure functions, helpers, validations
```

- **Unit Tests:** Must be extremely fast, isolated, and deterministic. Mock all external file systems, network adapters, and databases.
- **Integration Tests:** Verify interactions between layers (e.g., DB repositories, queue workers, API routers). Run these against real ephemeral databases (e.g., in docker containers).
- **End-to-End (E2E) Tests:** Focus only on critical paths (e.g., signup flow, checkout flow). Do not write E2E tests for minor UI details.

## 2. Test Content Rules
- **Behavior, Not Implementation:** Assert on the observable public output of functions, not on private properties or internal methods.
- **Deterministic:** Avoid flakiness. Never rely on current system times, random number generators, or external network availability without explicit stubbing.
- **AAA Pattern:** Explicitly structure test cases using the Arrange-Act-Assert format. Keep these blocks readable.

## 3. Error Handling Standard
Errors must be deliberately categorized and handled. Do not swallow exceptions.
- **Classification:** Differentiate between validation errors (400), authentication failures (401), authorization failures (403), resource not found (404), conflicts (409), rate limits (429), and internal server crashes (500).
- **Production Safety:**
  - Never return stack traces, internal framework errors, or raw database queries to public API clients.
  - Log full error traces internally with corresponding request IDs.
  - Expose a clean, friendly message to the end user.

## 4. Code Review Checklist
Every non-trivial merge request must be evaluated using these guidelines:

| Review Area | Target Check |
| :--- | :--- |
| **Correctness** | Does the code solve the specific business requirements? |
| **Edge Cases** | How does the code handle null values, empty lists, or timeouts? |
| **Security** | Are there open SQL injections, XSS, or missing authorization policies? |
| **Performance** | Are we introducing N+1 queries, unindexed filters, or memory leaks? |
| **Testing** | Are the additions covered by automated tests? Do they test behavior? |
| **Backward Compat** | Will this deployment break current API users or active sessions? |

**Reviewer Mindset:** Ask *"What could break here?"* and *"How does this code fail?"* before checking if it runs successfully under the happy path.
