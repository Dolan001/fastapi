# FastAPI PostgreSQL and API architecture

## Stable verification runtime

- Use one uniquely named Compose project for the entire backend verification attempt. Start its
  PostgreSQL service once, wait on the Compose health condition, and reuse it until every database
  and HTTP check has finished.
- Do not publish PostgreSQL to host port 5432 and do not allocate a sequence of random or fixed host
  forwarding ports. Backend, migration, seed, and test processes connect to `postgres:5432` (or the
  declared service name) on the Compose network.
- Create separate disposable databases inside that one PostgreSQL service for empty-to-head and
  prior-schema upgrade checks. Drop those databases and seeded records during cleanup; do not restart
  the service between checks.
- Build and run the backend service on the same network. Exercise live HTTP from a project-owned
  Python test runner or test container using the backend service URL. Do not require host HTTP tools,
  and do not count an in-process ASGI client as live HTTP evidence.
- If a command fails, capture service health and logs before teardown. Never retry by creating a new
  ad hoc PostgreSQL container or changing the host port.

Load this reference for any SQLAlchemy model, Alembic revision, repository, query,
service, schema, dependency, route, or router change.

## PostgreSQL engine and sessions

- Generate PostgreSQL configuration only; never silently fall back to SQLite. Validate one
  secret-backed database URL and public pool/timeout settings without logging credentials.
- For new targets choose one consistent SQLAlchemy 2 sync or async stack. Prefer async only
  when workload and dependencies justify it; never execute blocking database calls in an
  `async def` route. Record the choice and compatible psycopg driver.
- Construct the engine and session factory once. Configure pool sizing from deployment
  concurrency, `pool_pre_ping`, recycle policy, connection/application name, connect timeout,
  statement timeout, lock timeout, SSL, and UTC deliberately. Avoid unbounded overflow.
- Yield one Session/AsyncSession per request and close it reliably. Never share a session
  across requests or concurrent tasks. Liveness is process-only; readiness executes a bounded
  `SELECT 1` and verifies the Alembic revision is at all heads.

## SQLAlchemy models and schema

- Use SQLAlchemy 2 typed declarative mappings and a shared metadata naming convention so
  primary-key, foreign-key, unique, check, and index names remain stable across migrations.
- Use UUID or another requirements-backed public identifier, timezone-aware timestamps, and
  `Numeric`/Decimal for money or points. Specify nullability, length, precision, server
  defaults, relationship cascade/passive-delete behavior, and foreign-key actions explicitly.
- Express durable invariants with named PostgreSQL constraints. Python/Pydantic validation
  improves errors but never replaces database enforcement.
- Normalize identities deliberately. For case-insensitive usernames, emails, slugs, or other
  natural keys, pair canonical application input with a PostgreSQL functional unique index or
  another explicitly chosen database representation. A preflight lookup improves the error but
  never replaces catching the unique violation, rolling back the session, and mapping the named
  constraint to a stable conflict code.
- Prefer database-generated creation timestamps and deliberate update/version columns where
  multiple writers exist. Keep ORM and database defaults consistent; never rely on an
  application-only default for a value required by every writer.
- Design indexes from observed filters, joins, ordering, uniqueness, and plans. Consider
  composite left-prefix order, partial/expression/covering indexes, GIN/GiST/BRIN, and
  PostgreSQL-specific data types only when requirements and measured workload justify them.

## Alembic and table creation

- Alembic is the only production table-creation and schema-change mechanism. Never call
  `Base.metadata.create_all()` during application startup; restrict it to isolated tests only
  when the test explicitly does not claim migration coverage.
- Import every model into the metadata used by `env.py`; enable type/default comparison as
  appropriate. Autogenerate a candidate, then manually review names, data safety, PostgreSQL
  operations, downgrade/forward recovery, and changes autogenerate cannot detect.
- Prefer expand/backfill/cutover/contract. Batch large backfills, avoid long locks, create
  large indexes concurrently with correct transaction handling, and separate destructive
  cleanup behind explicit approval.
- Verify a clean PostgreSQL database upgrades from base to all heads, `alembic current
  --check-heads` passes, `alembic check` finds no drift, a second `upgrade head` is a no-op,
  and affected downgrade/forward recovery works where safe.

## Repositories, queries, and transactions

- Repositories perform persistence primitives and never commit. Query modules/selectors own
  reusable read statements and return domain-relevant results. Services own transaction scope,
  authorization-independent invariants, idempotency, and orchestration.
- Frame a complete write with one explicit `session.begin()` boundary. Roll back failures;
  do not hold transactions across HTTP calls or background work. Use row locks, optimistic
  versions, unique constraints, or idempotency records for concurrency as required.
- Scope tenant/owner predicates before lookup. Whitelist filter and ordering fields, cap page
  sizes, require deterministic ordering with a unique tie-breaker, and prefer cursor/keyset
  pagination for large or frequently changing datasets.
- Select only required columns. Use `joinedload` for suitable scalar relations and
  `selectinload` for collections; forbid implicit lazy-loading in async response serialization.
  Use aggregates, bulk operations, and streaming/chunking only with measured semantics.
- Use SQLAlchemy result APIs intentionally: `scalar_one_or_none()` when cardinality must be zero
  or one, `scalars().unique().all()` when joined collection loading can duplicate entities, and
  explicit ordering before every paginated result. Do not hide cardinality errors with `first()`.
- Capture sanitized PostgreSQL `EXPLAIN` plans and query-count/round-trip budgets for affected
  hot paths. Add indexes only after plan evidence and include realistic data cardinality.

## Pydantic schemas, routes, and URLs

- Separate command/input schemas from public read/output schemas. Use strict validation,
  explicit aliases, bounded collections/strings, and `from_attributes` only on output models.
  Never expose ORM models or secret/internal columns directly.
- Response models are mandatory for JSON APIs. Schemas validate and serialize only; they do
  not open sessions, query, commit, or contain business transactions.
- Each domain exports one `APIRouter` with a stable plural resource prefix, tag, dependencies,
  documented errors, and unique explicit operation IDs. The version router composes domains
  under `/api/v1`; `main.py` only assembles lifespan, middleware, exceptions, and routers.
- Routes translate HTTP and call a service/query. Declare request, response, path/query types,
  status codes, authentication/authorization dependencies, pagination, and error contracts.
  Keep routes thin and never commit directly from a route or repository.
- Contract-test route uniqueness, fixed/static path ordering before dynamic paths, OpenAPI
  operation IDs, security schemes, response filtering, and the absence of undocumented APIs.

## Required verification

- Test constraints against PostgreSQL, service transactions and concurrency, repository/query
  plans, Pydantic input/output and secret exclusion, dependency authorization, pagination/
  filter/order limits, router composition, OpenAPI, and success plus negative API behavior.
- Write `.ai/evidence/database-verification.json` without secrets. It must truthfully record
  connection, migration, schema, and query verification required by the workflow schema.
- Run API tests through PostgreSQL dependency overrides. Savepoints may isolate tests while
  exercising commits; a separate empty-database Alembic lane proves migrations, never `create_all`.
