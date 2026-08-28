---
name: verify-fastapi-delivery
description: Independently verify changed FastAPI slices or backend delivery with focused and affected full checks.
---

# Verify FastAPI delivery

Reconstruct requirements, inspect the diff, validate structure, and run format, lint,
strict types, migration checks, affected units/APIs, OpenAPI drift, authorization
negatives, startup/health, and security checks. Capture commands and results. Do not
edit source or accept missing evidence.

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
