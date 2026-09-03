# Architecture

## System shape

```text
Elderly user / Supervisor / Wearable
               │
               ▼
React PWA ─── FastAPI API ◄── device-token ingestion
               │
     ┌─────────┼───────────┐
     ▼         ▼           ▼
Agent       Care         Security
Engine      Services     + Ownership
     │         │           │
     └─────────┼───────────┘
               ▼
        SearchBackend
         /          \
 SQLite dev/CI   PostgreSQL production path
 FTS5+vec        tsvector/GIN + pgvector/HNSW
                    │
                  Redis
                    │
             Scheduler worker
```

## Agent interaction pipeline

The documented pipeline is:

```text
input (text / voice / wearable event)
              │
              ▼
            Intent
              │
              ▼
       Emotion analysis
              │
              ▼
        Memory retrieval
              │
              ▼
     Language-aware generation
              │
              ▼
 Conversation / emotion / alert logging
              │
              ▼
 reply + care state + supervisor visibility
```

The architecture does not make the generated response the source of truth for care state. Medication status, alert lifecycle, authorization and escalation remain application-controlled workflows.

## Search architecture

A single environment-selected database configuration supports two modes:

**Development / CI**
- SQLite
- FTS5
- sqlite-vec

**Production-oriented data path**
- PostgreSQL 16
- tsvector + GIN
- pgvector + HNSW
- Alembic migrations

The `SearchBackend` abstraction provides text search, semantic search and conversation search without requiring care services to know which database engine is active.

## Scheduling architecture

A standalone APScheduler worker is responsible for medication reminders, daily check-ins and wearable synchronization. A file lock prevents duplicate firing across multiple processes. The worker is independently restartable and supervised by the deployment process manager.

## Security architecture

Authorization resolves a requested resource through its elderly profile to the owning supervisor. This unified dependency was introduced after adversarial testing exposed a cross-tenant access weakness.

Other documented controls include:

- JWT access/refresh authentication;
- bcrypt password hashing;
- login rate limiting;
- disabled-user enforcement;
- Redis sliding-window limiting implemented atomically in Lua;
- in-memory limiter fallback;
- wearable device-token allowlist;
- environment-based secrets;
- tenant-scoped alert queries and statistics.

## Frontend

The source describes React 18 + strict TypeScript + Vite + Tailwind, packaged as an installable PWA. i18next provides English and Arabic with RTL support.

## Deployment shape

The documented production stack uses Docker Compose for the application, PostgreSQL/pgvector and Redis, with health checks and supervised processes.

This evidence repository documents the architecture; it does not expose the private production source.