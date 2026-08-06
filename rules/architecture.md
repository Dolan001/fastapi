# Architecture rules

- Generate backend source only in target `apps/backend/`.
- Keep routes thin and domain writes in services.
- Keep persistence behind repositories and transaction boundaries.
- Use typed request/response schemas and typed settings.
- Make dependencies and authorization explicit.
- Treat OpenAPI as the frontend/backend integration boundary.
