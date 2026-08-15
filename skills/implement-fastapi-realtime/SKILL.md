---
name: implement-fastapi-realtime
description: Implement requirement-backed FastAPI WebSocket chat, notifications, presence, or live events with Redis and PostgreSQL durability. Use for FastAPI slices containing realtime behavior.
---

# Implement FastAPI realtime

Complete the realtime capability group and ordinary FastAPI vertical-slice contract. Keep endpoint
loops as protocol adapters; services own commands and fresh sessions own transactions. Require
independent realtime evidence before handoff.

Read `references/fastapi-websockets.md` for every realtime slice. Also read the vertical-slice
production and database references when persistence, attachments, notifications, or tasks change.
