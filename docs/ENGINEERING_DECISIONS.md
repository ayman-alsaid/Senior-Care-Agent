# Engineering Decisions

## 1. Companion first, not medical automation first

The product is organized around a persistent companion because the source thesis treats connection and continuity as primary needs. Health and medication workflows are integrated into that relationship context rather than presented as an isolated clinical dashboard.

**Boundary:** this does not make the companion a clinician.

## 2. Keep consequential care state deterministic

Generative AI can compose language-aware responses from retrieved context. Medication dose state, adherence calculations, stock state, alert severity, ownership checks, acknowledgement and escalation remain explicit application logic.

**Reason:** care-critical state should be inspectable and testable without depending on model improvisation.

## 3. Escalation is a workflow, not a sentence

An emergency response needs delivery, acknowledgement, escalation and resolution state. Merely generating advice in chat is not enough.

**Reason:** accountability requires durable state and named human responsibility.

## 4. Centralize tenant ownership after adversarial failure

A cross-tenant weakness discovered during adversarial review led to a unified resource → elderly profile → owning supervisor authorization layer.

**Reason:** ownership invariants are safer when centralized than when independently reimplemented in every route.

**Evidence:** the failure mode is regression-covered by 13 cross-tenant attack attempts.

## 5. Support offline, deterministic development

AI provider interfaces have mock implementations for development and tests. The core test suite runs without network access.

**Reason:** deterministic CI is more valuable than tests whose outcome depends on external model availability, latency or cost.

**Boundary:** source evidence marks richer real LLM/STT/TTS provider wiring as the next phase.

## 6. Abstract search across development and production storage

The same care services consume a SearchBackend while SQLite/FTS5/sqlite-vec and PostgreSQL/tsvector/pgvector implement environment-specific search behavior.

**Reason:** preserve lightweight local testing without designing the production data layer around SQLite limitations.

## 7. Separate scheduled care from web requests

Medication reminders, daily check-ins and wearable synchronization run in a standalone scheduler worker.

**Reason:** time-based care behavior should not require an active UI request and should be independently restartable.

## 8. Treat bilingual support as product architecture

English and Arabic/RTL are part of the implemented interface rather than an afterthought.

**Reason:** a companion intended for elderly users should speak the user's language and render naturally in that language.

## 9. Use synthetic data for public evaluation

The public demo is documented as fictional.

**Reason:** evaluators can inspect realistic workflows without exposing real personal or medical information.

## 10. Publish limits alongside capabilities

The public case study separates implemented work from real-provider integration, mobile applications and clinical interoperability roadmap items.

**Reason:** trustworthy engineering evidence is more useful than an inflated feature list.