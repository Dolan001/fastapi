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
- Pass the executable source rules and complete every activated domain capability group.
- Resolve domain names from PRD product nouns first; when unnamed, choose familiar capability
  vocabulary instead of generic architecture labels.
- Use resource/use-case schema and route packages, not generic direction/layer filenames, and split
  cohesive modules at the contract's 300-line boundary.
