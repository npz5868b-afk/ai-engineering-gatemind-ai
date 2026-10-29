# GateMind AI
### Reliability-Aware Multimodal Smart-Lock Access Event Intelligence

**UTM · AI Engineering · Topic 1 / Week 1**  
**Project type:** FYP-aligned AI System / Application Development  
**Stage:** Scoping and conceptual design — not a deployed or validated security product

> **Research question:** Can reliability-aware multimodal sensor fusion classify smart-lock access events more robustly than fixed rules when inputs are missing or contradictory?

## Overview
GateMind AI explores contextual access-event intelligence for a custom smart-lock prototype. Instead of treating a single successful PIN, fingerprint or camera output as the whole story, it combines authentication results with nearby motion and door-state observations to classify access events. Its **AI contribution is reliability-aware event classification**, not inventing a new face-recognition model.

## Problem Statement
Authentication and physical door activity provide different signals. Missing, noisy and conflicting observations complicate access-event interpretation. GateMind AI investigates whether multimodal fusion can reduce false alerts and improve classification while keeping decision latency measurable.

## Proposed System
```text
Door-side camera (pretrained face / optional spoof scores) ─┐
Fingerprint + PIN authentication events ────────────────────┤
PIR presence / dwell + door-contact state ──────────────────┤
Optional vibration event ───────────────────────────────────┘
                          │
                 Timestamped event logs
                          │
              Feature + availability extraction
                          │
         ┌────────────────┼──────────────────┐
         │                │                  │
      Rule-based       XGBoost        Reliability-aware
      baseline         fusion          fusion variant
         └────────────────┼──────────────────┘
                          │
              Event class + confidence
                          │
             Review / alert / audit log
```
**Conceptual architecture only.** The internal system map required for the group activity will be added separately.

## Candidate Event Classes
Authorized Entry · Visitor Knock · Repeated Failed Attempt · Suspicious Dwell · Tailgating Risk · Force Attempt

These are proposed labels; their operational definitions and feasible sensor evidence must be validated before data collection.

## Initial Related Work / Existing Approaches
| Approach | Role | Limitation to test |
|---|---|---|
| Rule-based access-event monitoring | Interpretable baseline | Brittle thresholds and conflicts |
| [XGBoost (Chen & Guestrin, 2016)](https://doi.org/10.1145/2939672.2939785) | Feature-level fusion | Robustness under missing/noisy inputs |
| [GRU (Cho et al., 2014)](https://arxiv.org/abs/1406.1078) | Optional sequence model | Data volume and latency |

These two papers are **method references**, not evidence of prior smart-lock outcomes. Application-specific related work is pending verification.

## Engineering Gap
Investigate reliable access-event classification under **missing or conflicting sensor signals**, using reproducible baselines and measurable failure scenarios. Novelty claims are provisional until a literature review is completed.

## Objectives
- **O1 — Design:** Integrate face-camera AI outputs, fingerprint/PIN logs, PIR and door-contact events into a consistent event pipeline.
- **O2 — Develop:** Compare rules, XGBoost fusion and reliability-aware fusion; optionally add a temporal GRU.
- **O3 — Evaluate:** Measure per-class F1, false-alarm rate, decision latency, drop-one-sensor ablation and conflict resolution.

## Target Users
Smart-home residents and prototype security-system evaluators.

## Data Sources and Evidence
- Planned: consent-based prototype sensor-event logs (not public).
- [Espressif ESP32 documentation](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/) — technical reference.
- [XGBoost documentation](https://xgboost.readthedocs.io/) — technical reference.

> **Course compliance checkpoint:** Topic 1 asks for 2–3 public **documents as data sources**. The current sensor-event FYP does not yet meet that requirement; confirm the allowed alternative with the lecturer. Do not misrepresent technical documentation as training data.

## Responsible AI and Safety
Privacy-first collection, informed consent, minimal biometric retention, uncertainty-aware alerts and explicit missing-sensor testing. Experimental AI event labels must **not** override the lock's safe credential-based authorization logic.

## Repository Structure
```text
docs/        Week 1 observation, system map, project scope
data/        Dataset documentation and safe synthetic samples
src/         Implementation
notebooks/   Experiments
evaluation/  Metrics, ablations, reports
prompts/     Prompt artifacts, if used
```

## Weekly Progress
| Week | Deliverable | Status |
|---|---|---|
| 1 | Observation sheet, conceptual map, project scope, README | In progress |
| 2 | Refined requirements, architecture and evaluation criteria | Planned |

## Week 1 Checklist
- [x] [Initial individual project scope](docs/week1_project_scope.md)
- [ ] Group observation sheet (`docs/week1_observation_sheet.md`)
- [ ] Group conceptual map (`docs/week1_system_map.png`)
- [ ] Verified evidence and source compliance
- [ ] Submit repository URL to course portal

## Repository Hygiene
Never commit passwords, API keys, private biometric records, personal data or restricted/copyrighted documents. This repository is an engineering record, not proof of a production-ready lock.
