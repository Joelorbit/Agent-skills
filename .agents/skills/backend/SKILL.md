---
name: backend
description: >-
  Backend engineering standard covering API design, external API integrations,
  caching strategies, and payment systems. Activate when building server-side features.
---

# Backend Engineering Standard

## 1. API Design Standard
We build RESTful APIs by default.
- **Resource Naming:** Use plural nouns for resources (e.g., `/api/v1/projects`). Avoid verbs in resource paths.
- **HTTP Semantics:**
  - `GET`: Idempotent read. Returns 200 OK.
  - `POST`: Non-idempotent resource creation. Returns 201 Created.
  - `PUT`: Idempotent full replacement of a resource. Returns 200 OK or 204 No Content.
  - `PATCH`: Idempotent partial update. Returns 200 OK.
  - `DELETE`: Idempotent deletion. Returns 200 OK or 204 No Content.
- **Payload Design:** Wrap root collection responses in objects (e.g., `{"data": [...]}`) to enable future metadata extension (e.g., pagination).
- **No Details Exposure:** Never expose internal database auto-increment IDs to public clients. Use UUIDs, HashIDs, or ULIDs.

## 2. Request Handling & Validation
- **Boundary Validation:** Parse, validate, and sanitize all input payloads immediately at the entry point controller before running any application logic.
- **Consistent Errors:** Return standardized JSON error shapes for validation failures:
  ```json
  {
    "error": {
      "code": "bad_request",
      "message": "Validation failed",
      "details": {
        "email": "Must be a valid email address"
      }
    }
  }
  ```

## 3. External API Integrations
Always treat third-party APIs as untrusted and unstable.
- **Timeouts:** Enforce explicit connection and read timeouts on all outgoing HTTP clients (default: 5s connection, 10s read). Never block worker threads indefinitely.
- **Retries:** Use exponential backoff with jitter only for idempotent requests or operations using idempotency keys.
- **Circuit Breakers:** Implement circuit breakers for critical external services. If a dependency is down, degrade functionality gracefully instead of crashing.
- **Contracts:** Map external API payloads immediately to internal domain objects to isolate third-party changes.

## 4. Caching Strategy
- **TTL Constraint:** Every cached key *must* have a defined Time-To-Live (TTL).
- **Deterministic Keys:** Namespace keys cleanly: `domain:resource_type:resource_id` (e.g., `billing:invoice:inv_123`).
- **Cache Invalidation:** Use event-driven invalidation. Don't rely solely on TTL to sync database updates with the cache.
- **Cache Stampede Prevention:** For high-traffic reads, use locking, single-flight groups, or background revalidation to prevent concurrent requests from crushing the database on cache miss.

## 5. Payment Systems Standard
- **Never Trust the Client:** Do not use frontend-reported payment status to fulfill orders.
- **Webhooks:** Fulfill orders asynchronously using cryptographically signed webhooks sent directly from the payment provider (e.g., Stripe, PayPal).
- **Idempotency:** Webhook processing must be idempotent. Store payment intent IDs or transaction IDs and verify they have not been processed before updating billing state.
- **State Machine:** Model payment flows using a explicit state machine:
  `Created ──► Pending ──► Succeeded | Failed | Refunded`
  Validate all transitions. Do not use a simple boolean flag like `is_paid`.
