# FastAPI WebSockets

- Use a lifespan-managed Redis client and a per-process connection registry. Redis Pub/Sub or Streams
  coordinates instances; PostgreSQL remains authoritative and HTTP cursor resync covers lost events.
- Authenticate with secure cookies or a short-lived single-use HTTPS-issued ticket. Validate Origin,
  expiry, revocation, tenant, membership, and each command. Never use reusable query-string JWTs.
- A WebSocket endpoint validates frames and delegates to services. Open a fresh `AsyncSession` per
  command; never retain request sessions, ORM objects, or transactions for the socket lifetime.
- Persist state, stream sequence, deduplication record, and outbox atomically before acknowledgement.
  Broadcast versioned scalar JSON only after commit. Bound subscriptions and queues per connection.
- Supervise reader, writer, heartbeat, and Redis subscription tasks with structured cancellation.
  Drain on shutdown and close with a retryable code; never swallow task exceptions.
- Configure Redis TLS/auth, connection pools, health/readiness, publish timeouts, payload/rate limits,
  queue overflow policy, and metrics. Presence/typing uses TTL and is explicitly best-effort.
- Validate stored attachment ownership and scan state; reject arbitrary client URLs.
- Prove multi-worker fan-out with real Redis plus auth, ordering, dedupe, cursor resume, Redis
  outage/recovery, slow consumers, cancellation, and graceful-deploy tests.
