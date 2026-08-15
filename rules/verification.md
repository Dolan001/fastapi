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
