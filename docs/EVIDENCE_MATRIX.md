# Evidence Matrix

This matrix applies the AgentCraft evidence taxonomy conservatively.

| Claim | Status | Evidence scope / boundary |
|---|---|---|
| Core companion platform exists | **IMPLEMENTED / TESTED** | Agent, health, medication, memory, alert and auth modules documented |
| 115 backend tests pass | **TESTED** | Source-recorded pytest result; software verification only |
| Strict TypeScript has zero errors | **TESTED** | Source-recorded compiler check |
| Production frontend build succeeds | **TESTED** | Source-recorded Vite build + PWA service worker |
| Two adversarial audits performed | **VERIFIED IN PROJECT EVIDENCE** | Findings/remediation described in source |
| Cross-tenant flaw was fixed | **TESTED** | Central ownership layer + regression suite |
| 13 isolation attack attempts fail | **TESTED** | `test_isolation.py` evidence described in source |
| PostgreSQL/pgvector data path exists | **IMPLEMENTED / ACCEPTANCE-CHECKED** | Alembic + eight-point verification script |
| Arabic + RTL support | **IMPLEMENTED / BUILD-VERIFIED** | Source records full Arabic UI/agent support |
| Public demo uses synthetic data | **DISCLOSED EVALUATION CONDITION** | Fictional dataset explicitly documented |
| Real LLM/STT/TTS provider experience is complete | **NOT CLAIMED / NEXT** | Source roadmap places provider wiring next |
| Family mobile app exists | **PLANNED** | React Native app is roadmap work |
| Physician portal / HL7-FHIR exists | **PLANNED** | Care integration is roadmap work |
| System improves clinical outcomes | **NOT YET VALIDATED** | No clinical study evidence supplied |
| System is a certified medical device | **NOT CLAIMED** | No regulatory evidence supplied |
| System is compliant with a named healthcare privacy regime | **NOT CLAIMED** | Requires separate legal/operational evidence |
| Emergency workflow guarantees rescue | **NOT CLAIMED** | Software escalation cannot guarantee external response |

## Review rule

Use the strongest claim the evidence supports — never the strongest claim the product story would benefit from.