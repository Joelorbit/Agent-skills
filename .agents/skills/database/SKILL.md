---
name: database
description: >-
  Database engineering standard covering schema design, referential integrity,
  SQL query performance, and migration procedures. Activate when interacting with database layers.
---

# Database Engineering Standard

## 1. Schema Design & Referential Integrity
Data integrity must be enforced directly in the database engine, never exclusively in the application logic.
- **Normalization:** Start with Third Normal Form (3NF). Only denormalize when profiling proves a performance bottleneck.
- **Strict Constraints:**
  - Mark columns `NOT NULL` by default.
  - Enforce string limits (`VARCHAR(255)` instead of unchecked `TEXT` unless unlimited text is needed).
  - Use `CHECK` constraints to enforce valid value ranges (e.g., `CHECK (price_cents >= 0)`).
- **Foreign Keys:** Always define foreign keys with explicit delete behaviors (e.g., `ON DELETE RESTRICT` or `ON DELETE CASCADE`). Never allow orphaned rows.
- **Uniqueness:** Use unique indexes to prevent duplicates.
  - *Bad:* Querying if a record exists in application code, then inserting (prone to race conditions).
  - *Good:* Define a `UNIQUE` constraint, attempt insertion, and catch the unique violation exception.

## 2. SQL Query Performance
- **Eager Loading:** Avoid N+1 query patterns. Eager load relations or use batch dataloaders in API/GraphQL endpoints.
- **Index Optimization:**
  - Create indexes for columns used in `WHERE` clauses, join conditions, and `ORDER BY` fields.
  - Use composite indexes carefully. Keep the leftmost column prefix rules in mind.
  - Avoid indexing high-write, low-read columns to preserve write performance.
- **Query Plans:** Inspect execution plans (`EXPLAIN ANALYZE` in PostgreSQL) for queries on tables exceeding 50,000 rows. Avoid sequential scans on indexable fields.
- **Cursor Pagination:** Use cursor-based pagination (e.g., `WHERE id > :last_id LIMIT :limit`) for large tables. Avoid deep offset queries (`LIMIT :limit OFFSET :offset`), as they require scanning all skipped rows.

## 3. Transaction Boundaries & Locks
- **Atomicity:** Wrap multi-row operations that must succeed or fail together inside database transactions.
- **Short Transactions:** Keep transactions as short as possible. Do not execute slow network requests, external API calls, or CPU-heavy parsing inside a database transaction block.
- **Lock Prevention:** Access tables in a consistent alphabetical or dependency order across all application flows to prevent deadlocks.

## 4. Migration Strategy
- **Version Control:** All database changes must be written as version-controlled migration files. Never make manual schema changes in production.
- **Zero-Downtime:** Schema changes must be backward compatible with currently running application code.
  - *Adding Columns:* Add as nullable or with a default value.
  - *Renaming Columns:* Add a new column, dual-write to both, copy historical data, update reads to the new column, and drop the old column.
- **Rollback Verification:** Write and test rollbacks for every schema migration.
