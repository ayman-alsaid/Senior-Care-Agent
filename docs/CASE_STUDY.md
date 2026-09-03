# Senior Care Agent — Canonical Engineering Case Study

## Thesis

Senior Care Agent explores a difficult product boundary: how can an AI system provide continuous companionship and practical care support without quietly turning itself into a clinician, surveillance authority, or replacement for human relationships?

The implemented answer is a companion-first full-stack platform that combines conversation, longitudinal emotional context, medication workflows, health observations, memories, alerts, and accountable escalation.

The core design principle is:

> **Automate the care checklist. Preserve the human relationship. Escalate consequential decisions back to people.**

## The product problem

Elderly care is not one workflow. A family may simultaneously need social connection, medication reminders, health history, memory support, emergency awareness, and an auditable way to know whether someone responded to an alert.

Building each as an isolated feature creates fragmentation. Senior Care Agent instead models them around one elderly profile and one longitudinal companion context.

## The five-role model

The source system describes the companion through five practical responsibilities:

- **Confidant:** conversational continuity with memories, medication context, and emotional history.
- **Escort:** routine reminders, daily check-ins, medication schedules and stock awareness.
- **Health Supervisor:** structured wearable observations, trends, summaries, risk assessment and export.
- **Watcher:** continuous event handling and severity-ranked alerts.
- **Lifesaver workflow:** emergency escalation with delivery, acknowledgement and resolution state.

This framing matters because it makes the product architecture follow human needs rather than a menu of AI capabilities.

## Generative versus deterministic responsibility

The conversational pipeline can use intent, emotion analysis, retrieved memories and recent exchanges to compose a contextual response. Care-critical state is handled differently.

Medication dose status, stock counts, alert severity, ownership authorization, acknowledgement state and escalation are application logic. They should not depend on an LLM deciding what is true.

That separation is one of the central engineering decisions in the project.

## Longitudinal context

The platform is designed around continuity rather than isolated prompts. Conversations, emotional observations and memories can accumulate around an elderly profile. Memory search supports lexical and semantic retrieval through a shared SearchBackend abstraction.

This gives the companion useful context while keeping retrieval infrastructure replaceable between lightweight development/CI and the production data path.

## Medication intelligence

The implemented medication workflow includes:

- dose states: upcoming, due, taken, skipped, missed;
- rolling adherence percentage per medication;
- stock state derived from remaining count and reorder threshold;
- automatic stock decrement after recorded intake;
- separate handling for as-needed medication.

These are software care-coordination features. They are not evidence of improved clinical outcomes.

## Alert accountability

Alerts move through information, warning, critical and emergency severity. The important property is not merely that an alert can be generated, but that the system records its lifecycle: creation, delivery, acknowledgement, escalation and resolution.

For a care system, "an alert was emitted" is a weak guarantee. "A responsible person acknowledged it, or escalation continued" is the more useful systems property.

## Security failure converted into evidence

The project underwent two adversarial audit rounds. A cross-tenant authorization weakness was found: an authenticated supervisor could access another elderly profile's data. The response was not a route-by-route patch. Authorization was centralized around resource → elderly profile → owning supervisor resolution, and the failure mode was locked into an isolation regression suite containing 13 cross-tenant attack attempts.

This creates a strong evidence chain:

**finding → architectural correction → permanent regression coverage**.

The project should therefore not claim perfect security. It can claim that a meaningful isolation flaw was discovered, fixed, and regression-tested.

## Dual-database engineering

Development and automated testing can run against SQLite with FTS5 and sqlite-vec. The production-oriented data layer uses PostgreSQL 16, tsvector/GIN and pgvector/HNSW. A SearchBackend abstraction exposes the same text, semantic and conversation search concepts to care services.

An eight-point PostgreSQL acceptance script verifies the production data path, including migrations, Arabic/mixed search, semantic behavior and tenant-related limits.

## Background care workflows

A scheduler worker handles medication reminders, daily check-ins and wearable synchronization independently from request handling. A file lock prevents duplicate scheduling across processes.

This matters because time-based care workflows should survive independently of a browser session or conversational request.

## Public evaluation boundary

The public evaluation environment is documented as using fictional demo data. The source describes synthetic health readings, medications, dose schedules, memories, conversations and alerts. This is appropriate for product evaluation but must not be confused with a real clinical deployment.

## Verification evidence

Source evidence records:

- 115/115 backend tests passing;
- strict TypeScript check with zero errors;
- successful Vite production build and generated PWA service worker;
- two adversarial audit rounds;
- 13 cross-tenant attack attempts in the isolation regression suite;
- an eight-point PostgreSQL acceptance script.

These establish software engineering evidence. They do not establish clinical efficacy, regulatory approval, medical-device certification, or population-scale safety.

## Current boundary and roadmap

The source marks the core platform and production data layer as shipped. Real LLM/STT/TTS provider wiring is the next phase. A React Native family app, physician portal, weekly reports, and HL7/FHIR export are planned.

Those roadmap items remain explicitly separate from current implementation claims.

## What this project demonstrates

Senior Care Agent is evidence of more than full-stack implementation. It demonstrates a design approach for high-consequence AI systems:

- model human needs before AI features;
- constrain generative behavior with deterministic application state;
- make escalation accountable;
- treat privacy isolation as architecture, not UI;
- preserve a testable offline development path;
- disclose synthetic evaluation data;
- convert security failures into regression evidence;
- keep clinical and regulatory claims outside what software tests can prove.

That boundary-oriented engineering is the central case study.