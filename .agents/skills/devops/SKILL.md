---
name: devops
description: >-
  DevOps standard covering CI/CD pipelines, Docker containerization, production deployment checklist,
  observability, logging patterns, and Git workflow rules. Activate when shipping code.
---

# DevOps & Operations Standard

## 1. Automated CI/CD Pipelines
No manual build or deployment steps are allowed.
- **Continuous Integration (CI) Stages:** Enforce the following execution order for every Pull Request:
  ```
  Install Dependencies ──► Lint Code ──► Run Typechecks ──► Run Tests ──► Build Artifact
  ```
- **Deployment Safety:** Do not deploy code that has failed any CI stages.
- **Rollback Readiness:** Every deployment must have a verified, quick rollback mechanism.

## 2. Docker Best Practices
- **Multi-Stage Builds:** Compile code in a heavy build stage, then copy final artifacts into a minimal runtime image.
- **Minimal Images:** Use lightweight base images (e.g., `alpine`, `distroless`) to reduce the container attack surface.
- **Non-Root Execution:** Never run container application processes as `root` in production. Define a dedicated user.
- **Pruning:** Use a strict `.dockerignore` file to exclude local `node_modules`, build caches, git history, and secrets.
- **No Embedded Secrets:** Never hardcode secrets, passwords, or API keys inside the Docker image. Inject them at runtime using environment variables.

## 3. Pre-Production Deployment Checklist
Before triggering a production deploy, verify:
- [ ] Environment variables are fully populated and validated on application boot.
- [ ] Database migrations are executed and verified backward compatible.
- [ ] HTTPS is enforced. CORS origins are restricted to explicit white lists.
- [ ] Security headers (HSTS, CSP, X-Frame-Options) are configured.
- [ ] Logging systems are active and sending JSON logs to a centralized collector.
- [ ] Health checks (`/health` and `/ready` endpoints) are responding correctly.
- [ ] Production databases have automated backup schedules and retention rules active.

## 4. Observability & Logging
- **Structured Logs:** Output all production application logs in JSON format.
- **Required Metadata:** Every log entry must include `timestamp`, `level` (INFO/WARN/ERROR/DEBUG), `request_id` (traced across service hops), `user_id` (if authenticated), and `duration_ms` (for performance profiling).
- **Log Scrubbing:** Never log user passwords, access tokens, API credentials, or credit card numbers.
- **Health Checks vs Readiness:**
  - `/health` or `/live`: Quick check verifying if the process is running.
  - `/ready`: Verifies database connectivity, cache response, and crucial external dependencies.

## 5. Git Workflow & Commits
- **Branch Names:** Prefix branches semantic-first: `feature/`, `fix/`, `refactor/`, `docs/`, `chore/`.
- **Commits:** Keep commits small, atomic, and well-described.
- **Conventional Commits:** Format messages: `feat(auth): add MFA verification` or `fix(billing): correct stripe tax rounding`.
- **Clean Branches:** Do not commit broken compile states to long-lived shared branches.
