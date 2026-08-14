---
name: verify-fastapi-delivery
description: Independently verify changed FastAPI slices or backend delivery with focused and affected full checks.
---

# Verify FastAPI delivery

Reconstruct requirements, inspect the diff, validate structure, and run format, lint,
strict types, migration checks, affected units/APIs, OpenAPI drift, authorization
negatives, startup/health, and security checks. Capture commands and results. Do not
edit source or accept missing evidence.

Independently apply
`../implement-fastapi-vertical-slice/references/production-delivery.md`; do not reuse an
implementer's unsupported completion claim.
For database or API work, also apply
`../implement-fastapi-vertical-slice/references/database-api-architecture.md` and require
truthful `.ai/evidence/database-verification.json` evidence from a disposable PostgreSQL
database.
