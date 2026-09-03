# Limitations

Senior Care Agent is intentionally presented with explicit boundaries.

## Current product boundaries

- The source roadmap places richer **real LLM / STT / TTS provider wiring** in the next phase. Provider abstractions and deterministic mocks should not be described as proof of a completed live-provider experience.
- The **family React Native application** is planned, not shipped evidence.
- The **physician portal, weekly PDF reports and HL7/FHIR export** are planned, not shipped evidence.
- The public evaluation dataset is synthetic.

## Medical and safety boundaries

The current evidence does not support claims of:

- diagnosis;
- treatment recommendation accuracy;
- clinical efficacy;
- improved morbidity or mortality;
- medical-device certification;
- guaranteed emergency intervention;
- replacement of physicians, caregivers or family members.

Health trends and risk assessment are software support features and require appropriate human interpretation.

## Security boundaries

Two adversarial audits and isolation regression tests are meaningful evidence, but they are not proof of complete security. No claim of formal security certification or exhaustive penetration testing is made here.

## Deployment and integration boundaries

Wearable and notification behavior depends on deployment-specific integrations, credentials, devices, networks and external services. A software workflow can initiate escalation; it cannot guarantee third-party delivery or response.

## Human-centered boundary

Continuous assistance can become intrusive if deployed without appropriate consent, settings and governance. Sensitive monitoring capabilities require deployment-specific privacy choices. The project architecture includes settings and sensitivity concepts, but population-specific policy validation is outside the supplied evidence.

## Evaluation boundary

The live demo is useful for engineering/product evaluation. It should not be interpreted as a clinical trial or production patient-care deployment.