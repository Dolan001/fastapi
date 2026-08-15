# FastAPI agent pack

Code-free agents, skills, commands, hooks, and rules for generating or adopting a
FastAPI backend inside a target monorepo. This repository is never copied as an
application scaffold.
Generated backends use PostgreSQL, SQLAlchemy 2, Alembic-owned schema,
service/repository/query boundaries, explicit Pydantic schemas, thin versioned routers,
and measured query checks.
When requirements need durable deferred work, the conditional background-task capability adds and
verifies Celery, Redis, transactional outbox delivery, worker health, and idempotency tests; FastAPI
in-process background tasks remain limited to disposable work.
The executable structure contract supports dependency-lock alternatives, requirement-triggered
domain capabilities, and source-policy checks without storing application templates.
