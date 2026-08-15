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

## Application composition and delivery surfaces

- Keep the application factory or `main.py` declarative: create the app, register lifespan,
  middleware, exception handlers, and composed version routers. Do not import domain models or
  issue database statements there.
- Build domain routers with local resource paths, then mount them through one version router.
  Put fixed routes such as `/me`, `/search`, or `/token` before `/{resource_id}` and verify route
  uniqueness. Do not repeat `/api/v1` inside every domain router.
- Generate server-rendered templates, static mounts, or media routes only when the PRD requires
  a FastAPI-owned web surface. Keep web and JSON adapters separate while reusing the same queries
  and services. Never expose user uploads through an unrestricted development media mount in
  production.

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
- Derive actor, owner, tenant, and role from authenticated dependencies. Do not trust a client
  supplied `user_id`, `owner_id`, or tenant identifier for authorization or ownership assignment.
- Make authentication failures enumeration-resistant. Store passwords with a current adaptive
  password hasher; store reset/API tokens only as hashes; require purpose, expiry, single use,
  revocation, and safe key rotation. Keep public and private user response schemas separate.

## External effects and files

- Treat email, object storage, webhooks, queues, and image processing as adapters behind service
  interfaces. Bound upload size while streaming, verify decoded content rather than only filename
  or MIME type, generate server-side object keys, and enforce content/dimension policies.
- Do not use in-process background tasks for work that must survive restart. Write a transactional
  outbox/job record in the same database transaction, then deliver asynchronously with retry,
  idempotency, observability, and dead-letter handling. Use in-process tasks only for explicitly
  disposable work.
- Use Celery with Redis as the default durable worker. Async functions improve request concurrency;
  they do not provide durable queuing, retry, or execution after process failure. Add Celery Beat
  only for requirement-backed schedules and a result backend only when application behavior reads
  task results.
- Task messages contain versioned scalar IDs, never SQLAlchemy sessions/models, request objects,
  secrets, or large files. Each task opens its own session, checks an idempotency key, uses bounded
  exponential retry with jitter, records terminal failures, and defines time limits and queue
  routing. Worker readiness proves Redis connectivity and enqueue-to-consume behavior.
- Define compensation for database/object-store split operations. Never leave a committed row
  pointing to a missing object or delete the old object before the replacement is durable.

## Verification

- Focused lane: formatter, lint, strict type check, Alembic head/drift, changed service,
  repository, route, and authorization tests.
- Full lane: complete backend tests, database integration, startup/lifespan, OpenAPI
  contract, dependency failure, concurrency/idempotency, security scan, and health.
- Exercise success, malformed input, validation, conflict, not-found, unauthorized,
  forbidden, throttled, internal dependency failure, and cancellation where relevant.
- Override dependencies—not global engine state—in API tests. Exercise ownership, route-order,
  response-field filtering, upload validation, outbox retry/idempotency, and failure between
  database commit and each required external effect.
- When background work is active, also prove worker startup, duplicate delivery, retry exhaustion,
  terminal failure visibility, graceful shutdown, and scheduled dispatch when configured.
- Evidence contains exact argv, cwd, exit code, tool version, requirement IDs, and
  artifacts. Missing infrastructure or skipped required checks blocks verification.
