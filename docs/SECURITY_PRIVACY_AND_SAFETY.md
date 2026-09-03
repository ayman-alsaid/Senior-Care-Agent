# Security, Privacy & Safety

Senior Care Agent handles a category of data where security language must be precise. This document records what the project evidence supports and what it does not.

## Adversarial review

The source records **two adversarial audit rounds**.

A significant cross-tenant authorization weakness was discovered: an authenticated supervisor could access another elderly profile's resources. The remediation centralized ownership enforcement around the chain:

```text
requested resource → elderly profile → owning supervisor
```

The corrected invariant was then locked into `test_isolation.py`, documented as **13 cross-tenant attack attempts that must fail**.

This is evidence of a security improvement loop, not evidence that the system can never contain another vulnerability.

## Documented controls

| Area | Control |
|---|---|
| Resource ownership | Unified supervisor ownership enforcement |
| Multi-tenancy | Tenant-scoped alert listing/statistics |
| Wearable ingestion | Server-configured device-token allowlist |
| Authentication | JWT access/refresh + bcrypt |
| Abuse resistance | Login limiting + disabled-user enforcement |
| API rate limits | Redis atomic sliding window with in-memory fallback |
| Secrets | Environment configuration; no committed secrets claimed |
| Auditability | Alert acknowledgement/resolution/escalation history |

## Privacy design

The source describes per-profile settings and sensitivity levels, including stronger control around camera and voice-clone capabilities. The public evaluation dataset is synthetic rather than real patient data.

The public evidence does **not** establish compliance certification under HIPAA, GDPR, medical-device regulation, or any other jurisdiction-specific regime. Such claims require separate legal, operational and deployment evidence.

## Safety boundary

The system should be understood as care support and escalation infrastructure.

It does not establish:

- medical diagnosis accuracy;
- autonomous clinical decision authority;
- guaranteed emergency response;
- certified medical-device behavior;
- validated health outcome improvement.

Critical alerts are valuable because they bring humans into the loop with context and an auditable state machine. They do not remove the need for caregivers, clinicians, emergency services or deployment-specific safety procedures.

## Security lesson

For this project, the strongest security story is not "security was designed in." It is:

> **An adversarial test found a real isolation failure; the authorization architecture changed; the failure became permanent regression coverage.**

That is the evidence this repository intentionally exposes.