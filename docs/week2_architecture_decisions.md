# GateMind AI — Week 2 V1 Architecture Decisions and Traceability

**AI Engineering · Topic 2 Lab · Part C**  
**Version:** 1.0 — conceptual decisions, not implemented hardware assertions  
**Companion diagram:** [V1 Conceptual Architecture](week2_architecture_v1.png) · [Editable Draw.io](week2_architecture_v1.drawio)

## C1 — Simplest architecture and its limits

**Starting point:** Resident / door event → Smart-lock AI application → Access-event classifier → Label / review alert.

This is the minimum conceptual pipeline, but **not sufficient** for this project's requirements: it does not show how event data is gathered and time-aligned (FR1–FR2), how sensor uncertainty is represented (FR2/RAI3), how an experiment can be audited (FR5–FR7), or how physical unlocking remains independent of model output (RAI2). The V1 diagram adds only the components needed to cover these requirements.

## C2 — Architecture needs derived from requirements

| Requirement IDs | What does the requirement need? | Architecture response |
|---|---|---|
| **FR1, FR2, NFR4** | Correlate multiple event streams, keep timestamps, flag missing signals. | Sensor/auth-event adapters → event ingestion/time-window builder → reliability/availability stage. |
| **FR3, FR7, NFR2, NFR3** | Classify events and compare model strategies on identical inputs. | Interchangeable event classifier and offline baseline/evaluation harness. |
| **FR4, RAI3** | Show event outcomes with uncertainty and provenance. | Output formatting layer and resident/evaluator timeline. |
| **FR5, FR6, RAI1** | Save enough evidence to review results without storing prohibited personal data. | Privacy filter + local, minimal event/audit storage and restricted export. |
| **RAI2** | Prevent AI labels from directly opening a physical door. | Independent credential authorization and magnetic-lock actuation branch; **no classifier-to-lock edge**. |
| **NFR1** | Timely decision after the configured event window. | Small local pipeline with timestamped stages and a measured UI output point. |
| **RAI4** | Count false high-risk alerts on benign cases. | Output audit events + evaluation harness on labelled test scenarios. |

## C3 — V1 Architecture (diagram interpretation)

The diagram presents **A. Simple conceptual path** and **B. Requirements-driven V1**. A door-side sensor and credential-event interface feeds two explicitly distinct paths:

1. **Event intelligence path:** Ingestion/windowing → availability & conflict assessment → multimodal event classification → uncertainty-aware notification, with minimal audit records and offline evaluation.
2. **Independent door-release path:** Credential authorization → magnetic-lock release (only if authorized). The experimental AI classification has **no control signal** to the physical lock.

This is a high-level **function-based V1** diagram. Labels such as “Event Classifier” and “Reliability Assessment” represent capabilities; XGBoost and precise ESP32 wiring remain implementation choices for later topics. The public research datasets belong primarily to **offline evaluation/preprocessing**, not a live authorization stream.

## C4 — Architecture Decision Records (ADR)

| Decision | Requirement / constraint | Alternatives considered | Selected V1 option | Trade-off accepted |
|---|---|---|---|---|
| **ADR-01: Separate authorization from AI event inference** | RAI2, FR5; prevent unsafe autonomous unlocking. | (A) AI-driven auto-unlock; (B) isolated conventional credential authorization. | **B:** independent credential-to-lock path, model only classifies and alerts. | Less automatic functionality, but a failure in the experimental classifier cannot directly actuate the lock. |
| **ADR-02: Explicit reliability metadata before fusion** | FR2, NFR3, RAI3; sensor dropout and conflict. | (A) Concatenate available values as if complete; (B) preserve availability, timestamps and confidence. | **B:** reliability/availability stage ahead of event classifier. | Added pipeline complexity and some processing overhead, but missing/conflicting information becomes traceable and testable. |
| **ADR-03: Local-first event processing and minimal storage** | RAI1, NFR1, hardware/budget feasibility. | (A) Always stream biometrics to cloud; (B) process derived signals locally when feasible. | **B:** use derived confidence and local minimal audit data; limit raw media retention. | Greater on-device resource pressure and less remote observability, but reduced exposure of personal/biometric data. |
| **ADR-04: Distinct offline evaluation path** | FR6–FR7, NFR2–NFR3, O3. | (A) Report only a live demo; (B) replay frozen labelled sessions through comparable baselines. | **B:** offline evaluation harness with fixed session-separated data. | More experiment setup and provenance tracking, but reproducible metrics and honest baseline comparison. |

## Initial Requirements Traceability Matrix

| Objective | Requirement ID | Evaluation criterion | V1 architecture element | Later evidence (planned, not yet collected) |
|---|---|---|---|---|
| **O1** | FR1, NFR4 | Event-log completeness ≥99%. | Sensors → event ingestion/time-window builder; local audit store. | Schema validation on ≥200 replayed accepted events, completeness report. |
| **O1** | FR2, NFR3 | Single-modality dropout macro-F1 loss ≤0.12. | Reliability/availability & conflict stage. | Frozen-session ablations: PIR missing, face-confidence missing. |
| **O2** | FR3, NFR2 | Macro-F1 ≥0.80 and each class F1 ≥0.65. | Multimodal event-classification capability. | Class-wise precision/recall/F1 and held-out confusion matrix. |
| **O2** | FR4, RAI3 | 100% presence of uncertainty/availability display fields. | Output formatting and resident/evaluator timeline. | Audit of ≥100 outputs including contradictory inputs. |
| **O1, O2** | FR5, RAI2 | Zero classifier-originated unlock commands. | Independent credential authorization → magnetic lock. | Architecture interface check + ≥30 trace-based safety tests. |
| **O1** | RAI1, FR6 | Zero prohibited sensitive-field leaks. | Privacy filter + minimal event/audit storage. | Inspection of logs and exports with ≥20 test cases. |
| **O3** | FR7, NFR5 | 100% replay label agreement. | Offline evaluation harness/model version tracking. | Two deterministic replays of frozen test sessions. |
| **O3** | NFR1 | p90 post-window latency ≤2 s. | Timestamped processing path → dashboard output. | ≥100 window-to-output latency measurements on stated hardware. |
| **O3** | RAI4 | High-risk false-alert rate ≤5% on benign sessions. | Classifier → review/alert output; offline evaluation. | ≥60 separately labelled benign sessions and false-alert report. |

## Review of the V1 design

**Completeness:** Every major component in the diagram maps to a requirement above. **Data boundaries:** Public ambient data is research input for feature exploration, not a substitute for lab-labelled access-security scenarios. **Responsible AI:** The lock-release branch is independent; any uncertain classifier output may only influence the alert/review UI. **Technology neutrality:** The V1 diagram describes functional capabilities; a choice of XGBoost, GRU or firmware is deferred until prototyping. **Simplicity:** No cloud orchestration, household CCTV, complex microservices, or unnecessary foundation-model service is assumed.

**Status:** Architecture, ADR and traceability are planned design artefacts. Technical success and measured thresholds require later implementation and evaluation.
