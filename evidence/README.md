# Evidence Index

This directory is reserved for inspectable evidence that can be published without exposing private production source or sensitive data.

## Evidence currently represented in the case study

- Backend verification: **115/115 tests** recorded in project evidence.
- Frontend verification: strict TypeScript **0 errors** and successful production/PWA build.
- Security verification: **2 adversarial audit rounds**.
- Isolation regression: **13 cross-tenant attack attempts** documented as required failures.
- Production-data verification: **8-point PostgreSQL acceptance script**.
- Public evaluation: explicitly **synthetic demo data**.

## Suitable future artifacts

Where safe to publish, this directory can later contain:

```text
evidence/
  screenshots/          # synthetic/demo UI only
  diagrams/             # architecture and escalation flows
  controlled-runs/      # dated acceptance outputs
  security/             # sanitized regression summaries
  measurements/         # only genuine measured results
```

No private source, credentials, real medical records, access tokens, internal infrastructure secrets, or identifiable patient information should be published here.

## Evidence rule

A screenshot demonstrates a rendered state. A test result demonstrates a tested invariant. A synthetic scenario demonstrates software behavior. None of those alone demonstrates clinical efficacy.