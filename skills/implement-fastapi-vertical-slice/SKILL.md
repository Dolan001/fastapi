---
name: implement-fastapi-vertical-slice
description: Implement one requirement-linked FastAPI slice from schemas and persistence through routes and API tests within leased paths.
---

# Implement FastAPI slice

Follow the task contract and local patterns. Keep request/response schemas at the
boundary, domain logic in services, persistence in repositories, authorization in
dependencies, and transactions explicit. Add migrations and negative API tests. Run
focused format, lint, strict types, migration, OpenAPI, and test checks; stop for
independent verification.
Complete every activated capability group and pass the pack's executable source rules.

Before writing, map requirement IDs to a familiar bounded-context name. Prefer an exact PRD product
term; otherwise select conventional capability vocabulary and record why. Name schema and route
modules after resources/use cases, express create/read/update direction in Pydantic class names, and
split any domain layer that would exceed 300 lines.

Read `references/production-delivery.md` for lifecycle, async, transaction, security,
error-contract, migration, and verification rules.
Read `references/database-api-architecture.md` whenever the slice creates or changes a
model, Alembic revision, repository, query, schema, dependency, route, or router.
