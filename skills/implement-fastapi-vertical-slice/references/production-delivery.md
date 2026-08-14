# FastAPI production delivery reference

Load this reference only for FastAPI creation, implementation, or verification.

## Foundations and lifecycle

- Pin compatible Python, FastAPI, Pydantic, SQLAlchemy/PostgreSQL driver, Alembic, server, test, and lint
  versions in the target lock. Respect an existing sync or async architecture.
- Validate settings at startup, separate public configuration from secrets, and never
  log credentials, tokens, request bodies containing secrets, or internal exceptions.
- Own shared clients and pools in application lifespan. Close them deterministically;
  do not construct database or HTTP clients per request.
- Provide liveness and dependency-aware readiness, structured errors, request IDs,
  trusted proxy/host behavior, bounded body sizes, and explicit CORS.
- Use PostgreSQL in every generated environment that validates persistence behavior;
  never treat SQLite tests as production database evidence.

## Domain, async, and persistence

- Routes translate HTTP; dependencies authenticate/authorize; services own business
  transactions; repositories own persistence; schemas define transport boundaries.
- Never call blocking database, filesystem, crypto, or network work directly from an
  async route. Choose consistent sync/async libraries and test concurrency-sensitive
  paths.
- Keep session ownership request-scoped. Commit or roll back in one defined layer;
  avoid hidden commits inside repositories. Use locking or idempotency for retried
  writes and external callbacks.
- Use additive Alembic migrations. Separate expand, backfill, cutover, and cleanup;
  verify that one head exists unless branching is intentional and documented.
- Make eager loading, pagination, ordering, tenant filters, and uniqueness behavior
  explicit. Test representative query volume.

## API and security boundaries

- Runtime response validation is mandatory; response models must not expose internal
  fields. Define stable error codes and do not return stack traces.
- Authorization is deny-by-default and tested for anonymous, wrong-role, wrong-tenant,
  and object ownership. Scope persistence queries before lookup.
- Decide token/session storage, rotation, revocation, clock skew, password reset,
  throttling, enumeration resistance, CSRF/CORS, and webhook signature/replay handling.
- Generate and validate OpenAPI from the app. Treat operation IDs and public schemas as
  versioned contracts consumed by the typed frontend client.
- Compose stable domain routers beneath `/api/v1`; require explicit response models,
  unique operation IDs, and contract-tested route ordering.

## Verification

- Focused lane: formatter, lint, strict type check, Alembic head/drift, changed service,
  repository, route, and authorization tests.
- Full lane: complete backend tests, database integration, startup/lifespan, OpenAPI
  contract, dependency failure, concurrency/idempotency, security scan, and health.
- Exercise success, malformed input, validation, conflict, not-found, unauthorized,
  forbidden, throttled, internal dependency failure, and cancellation where relevant.
- Evidence contains exact argv, cwd, exit code, tool version, requirement IDs, and
  artifacts. Missing infrastructure or skipped required checks blocks verification.
