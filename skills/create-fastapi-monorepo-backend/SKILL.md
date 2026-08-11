---
name: create-fastapi-monorepo-backend
description: Create the FastAPI backend structure in a new target monorepo after architecture and task contracts are approved.
---

# Create FastAPI backend

Generate only declared structure paths. Resolve supported versions, lock dependencies,
configure typed settings, lifespan, health, logging, request IDs, structured errors,
database sessions, migrations, and OpenAPI. Create only required domains. Add Docker
and CI inside the target and validate import/startup structure before feature work.

Read `../../rules/project-structure.md`, then load
`../implement-fastapi-vertical-slice/references/production-delivery.md` before deciding
async, transaction, authentication, migration, or deployment behavior.
