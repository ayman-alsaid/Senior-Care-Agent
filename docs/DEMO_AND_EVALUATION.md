# Demo & Evaluation

## Synthetic-data disclosure

The documented public evaluation environment is populated with **fictional data**. No real personal or medical data is represented by the demo dataset described in the project source.

The source records a representative synthetic profile with:

| Demo category | Documented count |
|---|---:|
| Health readings | 21 |
| Medications | 4 |
| Today's medication doses | 5 |
| Memories | 6 |
| Conversations | 10 |
| Alerts | 3 |

The scenarios cover heart rate, blood oxygen, glucose, steps, medication scheduling/adherence, memory prompts, check-ins and alert workflows.

## Why synthetic data matters

A care product demo can look clinically convincing even when it is only exercising software states. This repository therefore states the evaluation boundary explicitly:

**The demo demonstrates product behavior with synthetic scenarios. It is not evidence of real-patient monitoring, clinical efficacy, or validated medical outcomes.**

## Suggested reviewer path

A technical evaluator should focus on:

1. companion continuity and bilingual behavior;
2. medication dose-state transitions and adherence calculations;
3. health-history and export workflows;
4. semantic memory retrieval;
5. alert acknowledgement and escalation state;
6. tenant-isolation behavior;
7. the distinction between implemented mock-provider paths and future richer live AI-provider wiring.

## Public demo

Evaluation endpoint: https://senior-care.agentcraft.info

Availability and deployment state should always be interpreted separately from clinical readiness.