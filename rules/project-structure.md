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
│   │   ├── session.py
│   │   └── migrations/
│   │       ├── env.py
│   │       └── versions/
│   └── domains/
│       └── <domain>/
│           ├── models.py
│           ├── schemas.py
│           ├── repository.py
│           ├── service.py
│           ├── routes.py
│           ├── dependencies.py
│           └── tests/
│               ├── factories.py
│               ├── test_service.py
│               ├── test_repository.py
│               └── test_api.py
├── tests/
│   ├── conftest.py
│   ├── contract/
│   └── integration/
└── scripts/
    ├── validate_project.py
    └── check_openapi_drift.py
```

Ownership:

- `app/main.py` assembles the application; it does not contain domain behavior.
- `api` owns versioned routing and cross-domain HTTP dependencies.
- `core` owns typed configuration, errors, logging, middleware, and security.
- `db` owns sessions, metadata, and Alembic infrastructure.
- each `domains/<domain>` package owns schemas, persistence, services, routes,
  dependencies, and focused tests.
- repositories perform persistence; services coordinate business transactions; routes
  only translate HTTP.

Generation order:

1. Resolve supported Python/FastAPI/Pydantic/SQLAlchemy/Alembic versions.
2. Create typed fail-closed configuration, application assembly, health, logging, and
   dependency locks.
3. Create database session and migration foundations.
4. Generate only requirement-backed domains.
5. Implement one schema-to-route vertical slice with service/repository tests.
6. Add target-owned Docker and CI after local checks are deterministic.
