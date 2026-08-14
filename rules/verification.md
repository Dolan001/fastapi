# Verification rules

Require formatting, linting, strict typing, migration drift checks, unit/API/contract
tests, dependency audit, secret scan, security review, integration evidence, and
independent verification. Report unavailable runtime infrastructure as blocked.
Database evidence must prove PostgreSQL connectivity, empty-database Alembic upgrade,
current head, second-run idempotence, drift-free metadata, expected tables/constraints/
indexes, and measured plans or query budgets for affected hot paths.
