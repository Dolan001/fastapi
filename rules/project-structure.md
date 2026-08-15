# Generated FastAPI structure

The agent generates this structure under target `apps/backend/`. Domain names are
derived from reconciled requirements.

```text
apps/backend/
├── pyproject.toml
├── <resolved dependency lock>
├── alembic.ini
├── .env.example
├── .gitignore
├── .dockerignore
├── Dockerfile
├── README.md
├── app/
│   ├── __init__.py
│   ├── main.py
│   ├── api/
│   │   ├── __init__.py
│   │   ├── dependencies.py
│   │   └── v1/
│   │       ├── __init__.py
│   │       ├── router.py
│   │       └── health.py
│   ├── core/
│   │   ├── __init__.py
│   │   ├── config.py
│   │   ├── errors.py
│   │   ├── logging.py
│   │   ├── middleware.py
│   │   └── security.py
│   ├── db/
│   │   ├── __init__.py
│   │   ├── base.py
│   │   ├── engine.py
│   │   ├── session.py
│   │   ├── health.py
│   │   ├── naming.py
│   │   └── migrations/
│   │       ├── __init__.py
│   │       ├── env.py
│   │       └── versions/
│   └── domains/
│       ├── __init__.py
│       └── <domain>/
│           ├── __init__.py
│           ├── models.py
│           ├── service.py
│           └── tests/
│               └── test_service.py
├── tests/
│   └── conftest.py
└── scripts/
    ├── validate_project.py
    ├── check_database.py
    ├── check_migration_plan.py
    ├── check_query_plans.py
    └── check_openapi_drift.py
```

Ownership:

- `app/main.py` assembles the application; it does not contain domain behavior.
- `api` owns versioned routing and cross-domain HTTP dependencies.
- `core` owns typed configuration, errors, logging, middleware, and security.
- `db` owns PostgreSQL engine/pool policy, request sessions, readiness, named metadata,
  and Alembic infrastructure.
- each `domains/<domain>` package owns schemas, persistence, services, routes,
  dependencies, and focused tests.
- repositories perform persistence without committing; query modules own optimized reads;
  services coordinate business transactions; schemas are side-effect-free transport
  boundaries; routes only translate HTTP.
- domain routers compose beneath `/api/v1` with stable prefixes, tags, response models,
  dependencies, and operation IDs.

Conditional structure:

- Resolve exactly one lock strategy: `uv.lock`, `poetry.lock`, `pdm.lock`, or the pair
  `requirements.lock` plus `requirements-dev.lock`.
- Every domain requires only `__init__.py`, its models, service, and service tests.
- Adding `repository.py` activates persistence and requires repository tests; adding `queries.py`
  activates optimized reads and requires query tests.
- Adding routes, dependencies, or schemas activates the JSON API group and requires command/view
  schemas plus schema/API tests.
- Add factories only where they reduce test duplication.
- Add root contract and integration suites when cross-domain or infrastructure behavior requires
  them; keep focused tests with their owning domain.
- When external systems are required, add only the relevant files below `app/adapters/`, such as
  `email.py`, `object_storage.py`, or `tasks.py`. Domain services depend on narrow interfaces rather
  than SDK details. Omit the directory when the PRD has no external systems.
- If FastAPI must also render HTML, add `app/web/<domain>/routes.py`, `templates/`, and `static/`.
  Keep those web routes separate from `app/api/v1` and reuse domain queries/services.
- If reliable deferred work is required, add an outbox/job domain, worker entrypoint, and retry/
  idempotency tests. Do not substitute FastAPI in-process background tasks for durable delivery.

Generation order:

1. Resolve supported Python/FastAPI/Pydantic/SQLAlchemy/Alembic versions.
2. Create typed fail-closed configuration, PostgreSQL engine/session/readiness,
   application assembly, logging, and dependency locks.
3. Create named metadata and Alembic foundations without application `create_all`.
4. Generate only requirement-backed domains and conditional capability groups.
5. Implement one constrained/indexed model-to-route slice with service, repository,
   query, schema, transaction, and API tests.
6. Generate and review Alembic revisions; prove an empty PostgreSQL database reaches head.
7. Add query budgets/plans, database evidence, Docker, and CI after checks are deterministic.
