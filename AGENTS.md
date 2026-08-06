# FastAPI behavior pack

This repository contains AI behavior only and must never contain a runnable FastAPI
application or generated framework source. Agents use it while creating or adopting
`apps/backend/` in a separate target monorepo.

Keep HTTP routes thin, domain logic in services, persistence in repositories, schemas
at explicit boundaries, configuration in typed settings, and dependencies explicit.
All writes require a task contract and target path lease; verification is independent.
