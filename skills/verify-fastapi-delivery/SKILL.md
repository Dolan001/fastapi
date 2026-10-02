---
name: verify-fastapi-delivery
description: Independently verify changed FastAPI slices or backend delivery with focused and affected full checks.
---

# Verify FastAPI delivery

Reconstruct requirements, inspect the diff, validate structure, and run format, lint,
strict types, migration checks, affected units/APIs, OpenAPI drift, authorization
negatives, startup/health, and security checks. Capture commands and results. Do not
edit source or accept missing evidence.
Independently validate domain names against the PRD/fallback policy, schema and route package
filenames, cohesive ownership, and the 300-line split limit.

Reuse a passing shared full-matrix report only when its revision and workspace inputs
still match. For an individual feature, run only focused changed-slice checks; never
rerun the complete backend, contract, integration, and browser matrix per feature.
Use the commands approved in `.ai/test-commands.json`, not ad hoc shell substitutes.
Verify the built runtime image by immutable digest and fail on fixable critical OS or
application vulnerabilities.

Independently apply
`../implement-fastapi-vertical-slice/references/production-delivery.md`; do not reuse an
implementer's unsupported completion claim.
For database or API work, also apply
`../implement-fastapi-vertical-slice/references/database-api-architecture.md` and require
truthful `.ai/evidence/database-verification.json` evidence from a disposable PostgreSQL
database.
Run the built service against that database, load a versioned deterministic synthetic dataset, and
test the API over live HTTP. Cover success, validation failure, authentication, authorization, and
a write/read persistence round-trip. Remove the test data afterward. Record `seed-data` and
`api-live` checks plus the `test_data` and live API results in
`.ai/evidence/backend-verification.json`; production or private data is forbidden.
Perform the complete database and live HTTP matrix in one uniquely named Compose project. Keep
PostgreSQL private to its network, connect by service name, and reuse the same healthy service for
empty, prior-schema, seed, HTTP, persistence, and cleanup checks. Do not use changing host ports,
ad hoc database containers, host HTTP tools, or an in-process ASGI client as live evidence.
