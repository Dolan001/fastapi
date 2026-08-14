# Generated FastAPI structure

The agent generates this structure under target `apps/backend/`. Domain names are
derived from reconciled requirements.

```text
apps/backend/
├── pyproject.toml
├── requirements.lock
├── requirements-dev.lock
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
│   │   ├── dependencies.py
│   │   └── v1/
│   │       ├── router.py
│   │       └── health.py
│   ├── core/
│   │   ├── config.py
│   │   ├── errors.py
│   │   ├── logging.py
│   │   ├── middleware.py
│   │   └── security.py
│   ├── db/
│   │   ├── base.py
│   │   ├── engine.py
│   │   ├── session.py
│   │   ├── health.py
│   │   ├── naming.py
│   │   └── migrations/
│   │       ├── env.py
│   │       └── versions/
│   └── domains/
│       └── <domain>/
│           ├── models.py
│           ├── schemas/
│           │   ├── __init__.py
│           │   ├── commands.py
│           │   └── views.py
│           ├── repository.py
│           ├── queries.py
│           ├── service.py
│           ├── routes.py
│           ├── dependencies.py
│           └── tests/
│               ├── factories.py
│               ├── test_service.py
│               ├── test_repository.py
│               ├── test_queries.py
│               ├── test_schemas.py
│               └── test_api.py
├── tests/
│   ├── conftest.py
│   ├── contract/
│   └── integration/
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

Generation order:

1. Resolve supported Python/FastAPI/Pydantic/SQLAlchemy/Alembic versions.
2. Create typed fail-closed configuration, PostgreSQL engine/session/readiness,
   application assembly, logging, and dependency locks.
3. Create named metadata and Alembic foundations without application `create_all`.
4. Generate only requirement-backed domains.
5. Implement one constrained/indexed model-to-route slice with service, repository,
   query, schema, transaction, and API tests.
6. Generate and review Alembic revisions; prove an empty PostgreSQL database reaches head.
7. Add query budgets/plans, database evidence, Docker, and CI after checks are deterministic.
