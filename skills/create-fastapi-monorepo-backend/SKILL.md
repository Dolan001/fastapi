---
name: create-fastapi-monorepo-backend
description: Create the FastAPI backend structure in a new target monorepo after architecture and task contracts are approved.
---

# Create FastAPI backend

Generate core paths, one declared dependency-lock alternative, and only requirement-triggered
domain capability groups. Resolve supported versions, lock dependencies,
configure typed settings, lifespan, health, logging, request IDs, structured errors,
PostgreSQL engine/session ownership, migrations, and OpenAPI. Create tables only through
reviewed Alembic revisions and create only required domains. Add Docker and CI inside
the target and validate structure, source policy, connection, migration, import, and startup
behavior before feature work.
Docker Compose is the only runtime provider for PostgreSQL and any requirement-backed Redis,
Celery worker, or Celery Beat service. Do not inspect, install, or use host daemons. Pull missing
pinned database/broker images through Compose; build workers and Beat from the locked backend image.
Generate a normalized unique Compose project name and distinct purpose-specific database names.
First emit the PRD-to-domain map. Use explicit PRD nouns or justified familiar capability names,
then scaffold resource/use-case schema and route packages—never generic direction/layer files.

Use a currently supported minimal Python base image, pin the verified runtime image by
digest, run as a non-root user, and keep build and runtime stages separate. Re-resolve
the base digest instead of copying a permanent example digest. Build without stale
cache for release verification and block acceptance on fixable critical image findings.

Read `../../rules/project-structure.md`, then load
`../implement-fastapi-vertical-slice/references/production-delivery.md` before deciding
async, transaction, authentication, migration, or deployment behavior.
For PostgreSQL, SQLAlchemy models, Alembic, queries, schemas, routes, and URL composition,
also read `../implement-fastapi-vertical-slice/references/database-api-architecture.md`.
