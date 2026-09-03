# Testing & Verification

## Recorded verification state

The project source records the following engineering checks:

```text
Backend test suite:          115 / 115 passing
TypeScript strict check:     0 errors
Vite production build:      successful
PWA service worker:         generated
Adversarial audits:         2 rounds
Isolation regression:       13 cross-tenant attack attempts
PostgreSQL acceptance:      8-point verification script
```

## Backend tests

The documented pytest suite runs against in-memory SQLite without network access. This supports deterministic testing of the application core and avoids external AI-provider variability in CI.

## Isolation regression

The security suite is especially important because it derives from a discovered cross-tenant vulnerability. The source records 13 cross-tenant attack attempts that must fail after the centralized ownership fix.

This is a regression claim, not a claim of exhaustive penetration testing.

## PostgreSQL acceptance

The source describes an eight-point acceptance script for the production-oriented data path. Its documented coverage includes:

- database migrations;
- Arabic and mixed-language search;
- semantic search behavior;
- semantic upsert behavior;
- automatic text-search-vector updates;
- tenant-related limits;
- cleanup of verification data.

The purpose is to catch differences between the lightweight development database and the PostgreSQL/pgvector path.

## Frontend verification

Strict TypeScript compilation is recorded with zero errors, and the Vite production build completes with PWA service-worker generation.

## What these checks prove

They support claims about software behavior, build integrity, selected authorization invariants, and the documented database path.

## What these checks do not prove

They do not prove:

- clinical efficacy;
- medical accuracy across populations;
- medical-device certification;
- emergency-service reliability in every geography;
- security against every threat class;
- production performance at population scale;
- effectiveness of future real LLM/STT/TTS integrations.

Those require different evidence.