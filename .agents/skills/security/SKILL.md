---
name: security
description: >-
  Security engineering standard covering authentication, authorization, vulnerabilities,
  AI safety, and secrets management. Activate when implementing security controls.
---

# Security Standard

## 1. Core Principles
- **Defense in Depth:** Never rely on a single line of defense. Layer network rules, service boundaries, and internal validators.
- **Least Privilege:** Default to deny. Grant only the exact permissions needed for a specific action or service.
- **Zero Trust on Input:** Sanitize and validate every request at the trust boundary.

## 2. Input Trust Boundaries
Treat the following sources as untrusted:
- User inputs, HTTP headers, query/path parameters, and cookies.
- File uploads (content, name, and MIME type).
- External API responses and LLM outputs.
- Client-side application state (localStorage, cookies).

At every boundary:
1. Validate format, type, and bounds using schemas (e.g., Zod, JSON Schema).
2. Escape/encode output for the target context (HTML, SQL, OS shells).

## 3. Common Vulnerabilities Mitigation

| Vulnerability | Mitigation Strategy |
| :--- | :--- |
| **SQL Injection** | Use parameterized queries / ORMs. Never concatenate raw SQL strings. |
| **XSS** | Escape variables before rendering in HTML. Set a strict Content-Security-Policy (CSP). |
| **CSRF** | Use `SameSite=Lax` or `Strict` cookies. Validate anti-CSRF tokens for state mutations. |
| **SSRF** | Allowlist egress domains. Never pass user-controlled URLs to internal fetch agents. |
| **IDOR / BOLA** | Validate resource ownership on every transaction. Do not trust user-provided IDs blindly. |
| **Command Injection** | Avoid executing shell processes. If necessary, use parameterized executors with strict limits. |
| **Path Traversal** | Sanitize paths and resolve canonical absolute paths. Do not allow `../` sequences. |

## 4. Authentication (AuthN) Standard
- **Passwords:** Hash with Argon2id or bcrypt. Never store plaintext. Enforce strong entropy rules.
- **Sessions/Tokens:** Short access token TTLs (<15 mins). Use secure HttpOnly, Secure, and SameSite cookies.
- **OAuth:** Always validate `state` parameters to prevent CSRF. Use PKCE for public clients.
- **Limits:** Lock accounts or introduce exponential delays on failed logins. Enable rate limiting on login/reset routes.

## 5. Authorization (AuthZ) Standard
- **Enforcement:** Enforce authorization policies server-side on every request. Client-side hiding is UX only.
- **Model:** Map operations to explicit permissions. Prefer Role-Based (RBAC) or Attribute-Based (ABAC) models.
- **Checks:** Execute checks at the application/domain layer (closest to the resource access) to prevent bypass.

## 6. Secrets Management
- **Never Commit Secrets:** Do not commit API keys, private keys, database passwords, or JWT secrets.
- **Configuration:** Use environment variables. Use secret managers in production.
- **Safeguards:** Maintain `.env.example` with blank/fake configurations for local onboarding.

## 7. File Upload Security
- **Type Check:** Validate file contents via magic bytes (not file extension or MIME headers).
- **Renaming:** Rename files to random UUIDs upon upload to prevent path manipulation.
- **Storage:** Store uploaded files outside of public executable web directories (e.g., in isolated S3 buckets).
- **Access:** Serve files using time-limited signed URLs.

## 8. AI & LLM Safety
- **Prompt Injection:** Treat all LLM outputs as untrusted user input. Never execute raw LLM text as code or commands.
- **Tool Access:** Strictly validate parameters passed from LLM tool-calling before execution. Set budget limits.
- **Privacy:** Scrub PII and secrets before sending prompts to external APIs.
