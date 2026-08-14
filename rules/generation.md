# Generation rules

- Inspect before generation and preserve compatible target conventions.
- Generate the smallest requirement-backed vertical slice.
- Never copy runnable code from this behavior pack.
- Fail closed on missing or invalid production configuration.
- Default migrations to additive operations and require approval for destructive
  schema changes or major dependency upgrades.
- Never fall back to SQLite or call metadata `create_all` in application startup.
- Require PostgreSQL connection/pool/timeouts/readiness and derive named constraints and
  indexes from measured query shapes rather than speculation.
