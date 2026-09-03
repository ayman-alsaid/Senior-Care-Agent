# Senior Care Agent

> **Human-centered AI for aging with dignity — companionship first, safety always, humans accountable.**

[![Status](https://img.shields.io/badge/status-working_system-5F7A72?style=flat-square)](#evidence-status)
[![Tests](https://img.shields.io/badge/backend_tests-115%2F115-5B6F8A?style=flat-square)](#verification)
[![Security](https://img.shields.io/badge/adversarial_audits-2_rounds-6B6F86?style=flat-square)](#verification)
[![Languages](https://img.shields.io/badge/languages-English_%7C_Arabic_RTL-596B73?style=flat-square)](#engineering-scope)

**Senior Care Agent** is a full-stack AI companion and safety platform designed around an aging person's dignity, continuity, independence, and connection to family.

It is not a hospital system, and it is not an attempt to replace family, caregivers, or clinical judgment. The design thesis is narrower and more demanding:

> **AI should absorb the repetitive care checklist, preserve context, notice signals, and escalate responsibly — so humans can remain responsible for the relationship and the consequential decisions.**

The working system brings companionship, medication adherence, health observations, memories, emotional trends, alerts, and supervisor workflows into one bounded care architecture.

**Live evaluation:** https://senior-care.agentcraft.info  
**Engineering evidence:** this repository  
**Portfolio:** https://agentcraft.info

---

## Why this is an engineering problem, not a chatbot demo

A useful care companion must operate across very different kinds of responsibility:

```text
Conversation / Voice / Wearable Event
                │
                ▼
        Context + Intent
                │
       ┌────────┼─────────┐
       ▼        ▼         ▼
    Emotion   Memory    Care State
    trend     retrieval  medications / vitals
       └────────┼─────────┘
                ▼
         Bounded AI Response
                │
        ┌───────┴────────┐
        ▼                ▼
   Companion reply   Deterministic care workflows
                         │
                         ▼
              Alert → Acknowledge → Escalate
                         │
                         ▼
                    Human action
```

The important boundary is at the bottom: **the model may interpret and communicate; accountable workflows decide when people must be brought back into the loop.**

---

## One companion, five responsibilities

### 1. Confidant
Conversation with persistent context, memories, medication context, and longitudinal emotional observations. The goal is continuity rather than a stateless question-answer interface.

### 2. Escort
Scheduled reminders, daily check-ins, dose timelines, medication stock awareness, and routine care prompts reduce the repetitive coordination burden on family members.

### 3. Health supervisor
Wearable data can enter through a device-token-protected ingestion path. Health history, trends, summaries, risk assessment, and export workflows provide structured observations for supervisors and future clinical review.

### 4. Watcher
Missed doses, health anomalies, and care events become severity-ranked alerts with acknowledgement and resolution history. Monitoring without accountability is insufficient, so the alert lifecycle is auditable.

### 5. Lifesaver workflow
Emergency handling is modeled as an escalation chain rather than a generative answer. The system records delivery, acknowledgement, escalation, and resolution state so a critical event does not disappear into a chat transcript.

---

## Human-centered design boundaries

This project is deliberately built around boundaries as much as capabilities:

- **Companion, not clinician.** The platform supports care coordination; it does not diagnose or replace medical professionals.
- **Family bridge, not family replacement.** Automation handles repetitive monitoring and reminders while consequential action returns to people.
- **Longitudinal context, not surveillance by default.** Sensitive capabilities belong behind explicit settings and deployment controls.
- **Escalation, not autonomous authority.** Critical workflows surface information and involve responsible humans.
- **Synthetic evaluation data, not hidden patient data.** The public evaluation environment is documented as fictional demo data.

---

## Engineering scope

| Capability | Evidence status | Engineering significance |
|---|---|---|
| Agent chat + companion mode | **IMPLEMENTED / TESTED** | Persistent care-oriented conversational workflow |
| Emotion history | **IMPLEMENTED / TESTED** | Longitudinal observations available to the care workflow |
| Medication scheduling + adherence | **IMPLEMENTED / TESTED** | Dose states, intake logging, adherence and stock logic |
| Health data workflows | **IMPLEMENTED / TESTED** | Device ingestion, history, trends, export and emergency path |
| Memory retrieval | **IMPLEMENTED / TESTED** | Text + semantic retrieval through a shared abstraction |
| Alerts + escalation lifecycle | **IMPLEMENTED / TESTED** | Severity, acknowledgement, resolution and escalation state |
| Multi-tenant ownership enforcement | **TESTED / ADVERSARIALLY REVIEWED** | Cross-tenant isolation regression-locked after audit finding |
| PostgreSQL + pgvector production data layer | **IMPLEMENTED / ACCEPTANCE-CHECKED** | Production search/data path with migrations |
| English + Arabic RTL | **IMPLEMENTED / BUILD-VERIFIED** | Bilingual UI and companion experience |
| Installable PWA | **BUILD-VERIFIED** | Vite build + service worker generation |
| Real LLM / STT / TTS provider wiring | **NEXT** | Provider abstractions exist; richer live providers remain roadmap work |
| Family React Native app | **PLANNED** | Not presented as shipped |
| Physician portal / HL7-FHIR | **PLANNED** | Future care integration, not current evidence |

---

## Architecture

```text
React 18 / TypeScript / PWA / i18next EN+AR RTL
                         │
                         ▼
                  FastAPI API
                         │
      ┌──────────────────┼──────────────────┐
      ▼                  ▼                  ▼
 Agent Engine       Care Services       Security Layer
 intent/emotion     health/meds/memory  JWT/ownership/rate limits
      │                  │                  │
      └──────────────────┼──────────────────┘
                         ▼
                 SearchBackend
                  /            \
          SQLite dev/CI     PostgreSQL 16
          FTS5+sqlite-vec   tsvector+pgvector
                               │
                         Redis 7 limits
                               │
                     Scheduler worker
                 reminders/check-ins/sync
```

A key architectural decision is the **dual-database search abstraction**: development and CI can run without external services, while the production data path uses PostgreSQL full-text search plus pgvector/HNSW. The application code consumes a common search interface rather than coupling care logic to one database engine.

See [Architecture](docs/ARCHITECTURE.md) and [Engineering Decisions](docs/ENGINEERING_DECISIONS.md).

---

## Security designed adversarially

The strongest evidence in this project is not that no vulnerability was ever found. It is that adversarial review **did find a meaningful cross-tenant weakness**, the ownership model was redesigned, and the failure mode was converted into permanent regression coverage.

The source evidence records:

- **2 adversarial audit rounds**.
- A cross-tenant access vulnerability discovered and fixed.
- Unified resource → elderly profile → owning supervisor authorization.
- **13 cross-tenant attack attempts** in the isolation regression suite that must fail.
- Tenant-scoped alert listings and statistics.
- Device-token allowlisting for wearable ingestion.
- Redis atomic sliding-window rate limiting with in-memory fallback.
- JWT authentication, bcrypt, login limiting, disabled-user enforcement.
- Environment-based secrets.

That history is more valuable than a decorative "secure" badge because it demonstrates a security feedback loop:

```text
Threat assumption → adversarial test → failure → architectural fix → regression test
```

See [Security, Privacy & Safety](docs/SECURITY_PRIVACY_AND_SAFETY.md).

---

## Verification

Current source evidence records:

```text
115 / 115 backend tests passing
TypeScript strict check: 0 errors
Vite production build: successful
PWA service worker: generated
Adversarial security audits: 2 rounds
Cross-tenant isolation regression cases: 13 attack attempts
PostgreSQL acceptance script: 8 checks
```

The PostgreSQL acceptance path covers migrations, Arabic/mixed search, semantic retrieval/upsert behavior, text-search-vector updating, and tenant-related limits before cleaning up its test data.

These are **engineering verification claims**, not claims of clinical efficacy, medical-device certification, or real-world health outcome improvement.

See [Testing & Verification](docs/TESTING_AND_VERIFICATION.md).

---

## Synthetic demo disclosure

The evaluation environment uses **fictional demo data**. The documented dataset includes synthetic health readings, medications, dose schedules, memories, conversations, and alerts created to exercise the product experience.

This distinction matters: a polished health dashboard is not evidence of a clinical deployment. Public evaluation demonstrates software behavior with synthetic scenarios; it does **not** imply real-patient monitoring or validated medical outcomes.

See [Demo & Evaluation](docs/DEMO_AND_EVALUATION.md).

---

## What I would want a senior technical reviewer to inspect

Rather than judging the project by the number of screens, inspect these decisions:

1. **Where generative AI stops and deterministic care workflows begin.**
2. **How longitudinal memory and emotion context are separated from authority.**
3. **How a discovered tenant-isolation flaw became a reusable ownership layer and regression suite.**
4. **How the same care services run against lightweight CI storage and a production PostgreSQL/pgvector path.**
5. **How emergency events preserve acknowledgement and escalation accountability.**
6. **How demo claims are separated from clinical claims and future integrations.**

Those boundaries are the core engineering evidence.

---

## Evidence status

This public repository is an **Engineering Evidence / Technical Case Study**. Production source remains private.

The underlying project evidence supports a working core platform and production-oriented data layer. It does **not** support presenting the project as a certified medical device, a replacement for professional care, or proof of clinical outcomes.

For the evidence taxonomy used throughout AgentCraft, see [Evidence Matrix](docs/EVIDENCE_MATRIX.md).

---

## Roadmap boundaries

**Next:** real LLM / STT / TTS providers behind the existing abstractions.  
**Planned:** family mobile application.  
**Planned:** physician portal, weekly reports, and HL7/FHIR export.

These items are intentionally separated from implemented evidence.

---

## Repository map

```text
README.md
PORTFOLIO_NOTICE.md
docs/
  CASE_STUDY.md
  ARCHITECTURE.md
  ENGINEERING_DECISIONS.md
  SECURITY_PRIVACY_AND_SAFETY.md
  TESTING_AND_VERIFICATION.md
  DEMO_AND_EVALUATION.md
  EVIDENCE_MATRIX.md
  LIMITATIONS.md
evidence/
  README.md
```

---

## Portfolio notice

This repository documents engineering decisions, system behavior, verification evidence, and limitations for professional evaluation. It is **not the production source repository** and grants no operational or commercial rights to the private implementation.

Built by **Ayman Alsaid** under **AgentCraft**.  
https://agentcraft.info · contact@agentcraft.info