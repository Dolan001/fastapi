---
name: create-fastapi-monorepo-backend
description: Create the FastAPI backend structure in a new target monorepo after architecture and task contracts are approved.
---

# Create FastAPI backend

Generate only declared structure paths. Resolve supported versions, lock dependencies,
configure typed settings, lifespan, health, logging, request IDs, structured errors,
PostgreSQL engine/session ownership, migrations, and OpenAPI. Create tables only through
reviewed Alembic revisions and create only required domains. Add Docker and CI inside
the target and validate connection, migration, import, and startup behavior before feature work.

Read `../../rules/project-structure.md`, then load
`../implement-fastapi-vertical-slice/references/production-delivery.md` before deciding
async, transaction, authentication, migration, or deployment behavior.
For PostgreSQL, SQLAlchemy models, Alembic, queries, schemas, routes, and URL composition,
also read `../implement-fastapi-vertical-slice/references/database-api-architecture.md`.
