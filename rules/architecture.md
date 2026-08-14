# Architecture rules

- Generate backend source only in target `apps/backend/`.
- Keep routes thin and domain writes in services.
- Keep persistence behind repositories and transaction boundaries.
- Use typed request/response schemas and typed settings.
- Make dependencies and authorization explicit.
- PostgreSQL is the only generated runtime database. SQLAlchemy models and Alembic own
  schema; query modules own reads; services own transactions; schemas are side-effect free.
- Compose domain routers below `/api/v1` with stable prefixes, tags, and operation IDs.
- Treat OpenAPI as the client/backend integration boundary.
