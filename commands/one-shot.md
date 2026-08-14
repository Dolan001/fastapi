# One-shot backend creation

Create `apps/backend/` inside the target monorepo after requirements and architecture
are approved. Use `rules/project-structure.md` as the ownership blueprint and
`rules/project-structure.json` as the machine-readable structural contract. Generate
dependency-safe vertical slices from task contracts; do not copy a prebuilt
application tree.
Use PostgreSQL only, create schema through reviewed Alembic revisions, generate explicit
command/view schemas and thin versioned routers, and emit database verification evidence
before the backend gate.
