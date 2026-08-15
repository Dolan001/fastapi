# Verification rules

Require formatting, linting, strict typing, migration drift checks, unit/API/contract
tests, dependency audit, secret scan, security review, integration evidence, and
independent verification. Report unavailable runtime infrastructure as blocked.
Database evidence must prove PostgreSQL connectivity, empty-database Alembic upgrade,
current head, second-run idempotence, drift-free metadata, expected tables/constraints/
indexes, and measured plans or query budgets for affected hot paths.
For affected features, also require race-safe conflict handling, authenticated ownership,
static-before-dynamic route behavior, response-field filtering, and durable external-effect
failure tests. Generate only the checks relevant to requirements; missing required checks block.
The structure gate must also report no source-rule violations and no incomplete conditional
capability groups.
When background tasks are active, Redis broker, Celery worker startup, enqueue/consume, retry,
idempotency, duplicate-delivery, outbox, terminal-failure, and optional schedule evidence are required.
When realtime is active, require authentication/origin/membership tests, persisted-before-publish
ordering, cursor replay, deduplication, rate/payload limits, real-Redis multi-worker fan-out,
Redis recovery, slow-consumer cancellation, and graceful shutdown evidence.
