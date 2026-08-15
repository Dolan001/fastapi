---
name: implement-fastapi-background-work
description: Implement requirement-backed durable FastAPI background jobs, scheduled work, notifications, or external effects with Celery, Redis, and a PostgreSQL outbox. Use when a FastAPI slice activates background tasks.
---

# Implement FastAPI background work

Complete the background-task capability group. Read
`../implement-fastapi-vertical-slice/references/production-delivery.md` section “External effects and
files”. Keep task entrypoints thin, scalar-ID based, idempotent, bounded, observable, and independently
verified. FastAPI `BackgroundTasks` is allowed only for explicitly disposable work.
