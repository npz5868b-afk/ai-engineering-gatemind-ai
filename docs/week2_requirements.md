# GateMind AI — Week 2 Requirements Specification

**AI Engineering · Topic 2 Lab · Part A**  
**Version:** 1.0 (proposed V1 requirements; not implementation results)  
**Project:** GateMind AI — Reliability-Aware Multimodal Smart-Lock Access Event Intelligence  
**Source:** [Week 1 Project Scope](week1_project_scope.md)

## Section 0 — Project Framing & Application Scope Checkpoint

| Framing element | Refined project definition |
|---|---|
| **Problem + evidence** | A single credential result is insufficient to describe a complete doorway event, while ambient sensors can miss or falsely trigger events. A survey of fault tolerance in ambient-assisted-living sensors documents failure modes [R1]. Smart-home sensor activity-recognition work highlights uncertainty associated with sparse, noisy and missing observations [R2]. |
| **Existing approaches (at least 3)** | **1.** Duenna: user-behaviour anomaly detection for smart-home control commands [R3]. **2.** Sensor-fault detection/tolerance approaches surveyed by ElHady & Provost [R1]. **3.** Graph-guided smart-home activity recognition using sensor relationships [R2]. **4.** CASAS smart-home sensing platform as an ambient-event data precedent [R4]. |
| **Engineering gap** | Existing works motivate behaviour-aware security and robust sensing, but the V1 engineering question is whether a small **door-local, multimodal access-event classifier** can explicitly handle missing/conflicting signals and outperform simple rules. This is a **testable practical gap**, not a claim of unprecedented scientific novelty. |
| **O1 — Design** | To design a timestamped and auditable event pipeline for fingerprint/PIN outcomes, derived face-camera AI scores, PIR activity and magnetic door-contact states, including availability and reliability metadata. |
| **O2 — Develop** | To develop and compare a rule-based event baseline with a feature-fusion classifier and a reliability-aware fusion variant, without allowing experimental AI outputs to control unlocking. |
| **O3 — Evaluate** | To evaluate per-class classification performance, false-alerts, response latency, and robustness under missing, noisy and contradictory sensor evidence. |
| **Intended users** | Smart-home residents viewing contextual door-access notifications; academic researchers/evaluators testing a tabletop smart-lock prototype. |
| **Supported tasks** | Capture and synchronise sensor/authentication event logs; assemble short event windows; classify in-scope doorway event scenarios; expose label, confidence/uncertainty and evidence availability; record an audit trail; export de-identified predictions for evaluation. |
| **Information / knowledge domain** | Door-local authentication events and nearby physical activity inferred from the **lock-side** camera's derived confidence, fingerprint/PIN outcomes, PIR, and door-contact signals. Public ambient datasets may help test preprocessing but do not provide complete access-security labels. |
| **Operating context** | Consented laboratory/tabletop single-door prototype with edge/local-first inference where feasible; no house-wide or garden CCTV; removable external camera facing the visitor; magnetic lock only releases when separately authorised. |
| **Important constraints** | One-semester AI Engineering scope aligned to FYP; limited labelled security-event data; missing/delayed/conflicting sensors; CPU/edge resource limits; privacy of biometric data; no storage of plaintext PIN or fingerprint templates; suitable public documents for domain context [P1–P2]. |
| **Explicit out-of-scope tasks** | **No** AI-triggered physical unlock, autonomous policing or high-stakes security decision; no facial identity inference from the ambient public datasets; no identification of unseen people from PIR; no building-wide CCTV; no claims of reliably detecting tailgating/forced entry without additional validated signals. |
| **Evaluation focus** | Can V1 achieve useful access-event classification on **held-out, consented prototype scenarios**, with controlled false-alert rates and acceptable latency, and show measurable resilience when noncritical inputs are missing or conflicting? |

**Initial V1 event labels (to be operationalised before data collection):** (i) *Authorized Entry* — successful valid authentication followed by a door opening within a defined window; (ii) *Repeated Failed Attempt* — multiple failed credential events inside a window; (iii) *Prolonged Presence / Suspicious Dwell* — PIR presence beyond an explicitly recorded threshold without valid entry; and (iv) *Uncertain / Conflicting Evidence* — contradictory or insufficient observations requiring review. “Suspicious” is an **event flag**, not a determination of criminal intent. Visitor Knock, Tailgating Risk, and Force Attempt are deferred pending a doorbell/vibration channel or suitable evidence and labels.

**Dataset boundary:** Public sensor data from CASAS/eSense and other smart-home records supports initial feature engineering and ambient-only feasibility, **not** supervised classification of these four security categories. Ground-truth scenarios must be staged, consented, and split by recording session to reduce leakage.

## Section 1 — Functional Requirements (FR)

Each requirement is an **observable system behaviour**; implementation technology choices are reserved for Part C.

| ID | Requirement — **The system shall…** | Scope boundary — **SHALL NOT** (where relevant) | Linked objective |
|---|---|---|---|
| **FR1** | **The system shall** capture timestamped outcomes of PIN/fingerprint attempts, door-contact changes, PIR movement, and derived camera confidence/availability events using a common event schema. | Store plaintext PINs, fingerprint templates or routine raw face footage as event records. | O1 |
| **FR2** | **The system shall** assemble incoming signals into a defined, documented event window and explicitly mark missing, late or inconsistent channels. | Invent sensor readings to fill missing channels or silently treat absence as a negative observation. | O1 |
| **FR3** | **The system shall** assign each completed V1 event window one of the four documented event categories, or an uncertainty/needs-review outcome. | Claim to identify a criminal, prove tailgating, or classify physical force without validated data/sensors. | O2 |
| **FR4** | **The system shall** present the event label with a confidence/uncertainty indicator, timestamp and evidence availability summary in a resident/evaluator view. | Present uncertain inferences as verified facts or expose underlying biometric details to ordinary users. | O2 |
| **FR5** | **The system shall** flag ambiguous, conflicting or high-risk events for review and retain a minimal audit event with its model/version and decision basis. | Independently trigger a door unlock from an AI event classification or hide a conflicting-input warning. | O1, O2 |
| **FR6** | **The system shall** let authorised evaluators export pseudonymised scenario labels, predictions and time measurements for reproducible comparison across classifier configurations. | Export raw camera frames, personally identifying biometric records or credential secrets. | O3 |
| **FR7** | **The system shall** support running the same held-out event set through a fixed-rule baseline and at least one learned fusion classifier with an identical output-label schema. | Compare models on non-equivalent or leaked test sets while reporting their scores as directly comparable. | O2, O3 |

## Section 2 — Non-Functional Requirements (NFR)

**Thresholds below are preliminary V1 acceptance targets, not observed performance.** They will be reconsidered after collecting a representative pilot dataset. All latency metrics are measured **after a complete event window becomes available**; waiting for the configured sensing window is tracked separately.

| ID | Quality attribute | Requirement | Measurement method | Metric | Proposed threshold | Objective |
|---|---|---|---|---|---|---|
| **NFR1** | Performance / latency | The system shall produce a displayed event classification promptly after the event window closes. | Replay **≥100** windows on the declared demo hardware, with warm-up; time window-ready to dashboard update. | p90 end-to-end post-window latency | **≤ 2.0 s** | O3 |
| **NFR2** | Correctness | The system shall classify supported V1 scenarios with useful overall and per-class accuracy. | Use a held-out, session-separated labelled test set (aim ≥30 independent windows/class); compute confusion matrix. | Macro-F1; minimum per-class F1 | **Macro-F1 ≥0.80** and **each class F1 ≥0.65** | O2, O3 |
| **NFR3** | Robustness | The system shall tolerate loss of one nonessential context modality without severe performance collapse. | Run fixed held-out sessions intact, then remove one modality (e.g. PIR or camera score) at a time; do not suppress essential credential authorisation events. | Absolute macro-F1 decrease (full − ablated) | **≤0.12** per tested single-modality dropout | O2, O3 |
| **NFR4** | Data integrity / reliability | The system shall retain traceable identifiers, timestamps and availability flags for logged events. | Replay **≥200** accepted event records and validate the schema and linked audit entries. | Percentage of accepted events with complete required metadata | **≥99%** | O1 |
| **NFR5** | Maintainability / reproducibility | The system shall provide consistent experiment outputs on deterministic replay under a fixed model and configuration. | Replay one frozen test set twice with identical model/version and random seeds; compare event labels and record IDs. | Event-label agreement | **100%** | O3 |

## Section 3 — Responsible AI Requirements (RAI)

These requirements convert the Week 1 risks into testable controls. Metrics are planned, not already measured.

| ID | Responsible AI risk | Requirement | Measurement method | Metric | Proposed threshold | Objective |
|---|---|---|---|---|---|---|
| **RAI1** | Privacy / biometric exposure | The system shall restrict stored/exported data to consented, minimised event features and exclude plaintext PIN, fingerprint templates and raw face images from routine logs/exports. | Inspect storage schema and exported samples; run at least **20** test cases with sensitive fields in input. | Number of forbidden sensitive-field leaks in persistent records/exports | **0** | O1 |
| **RAI2** | Safety / unintended physical unlocking | The experimental classifier shall be logically separated from the credential-based unlock controller, and no classification label shall cause an unlock command. | Perform **≥30** positive, negative, conflicting and spoof/noise simulation cases; inspect command traces and wiring/API separation. | Model-originated unlock commands | **0** | O1, O2 |
| **RAI3** | Transparency / overclaiming | The system shall show a confidence or uncertainty indicator and clearly identify unavailable/conflicting inputs for each displayed classification. | Audit **≥100** event results, including generated missing-sensor cases. | Coverage of required explanation/availability fields | **100%** | O2 |
| **RAI4** | False alerts / unfair suspicion | The system shall limit high-risk warnings on benign, explicitly labelled scenarios; uncertain cases must not be displayed as proven attacks. | Evaluate **≥60** independently staged benign sessions, using predefined alert mapping. | High-risk false-alert rate among benign sessions | **≤5%** | O3 |

## Proposed Test Protocol and Boundary Conditions

1. Define labeling rules and time-window length **before** collecting or evaluating the final security-event dataset; have a second reviewer spot-check ambiguous windows where possible.
2. Split **by participant/session/time block**, not by randomly shuffled overlapping windows from the same recorded incident.
3. Report sample count and uncertainty alongside every metric; if a class has too few examples, mark its criterion **not assessable** rather than inventing a pass.
4. Evaluate baseline rules, standard learned fusion and reliability-aware fusion on the **same** frozen splits and label taxonomy.
5. Count uncertain cases separately. Do not improve reported accuracy simply by withholding difficult windows without reporting coverage.
6. Preserve physical-lock safeguards and participant privacy irrespective of model score.

## References and Public Documents

- **[R1]** ElHady, N. E., & Provost, J. (2018). *A systematic survey on sensor failure detection and fault-tolerance in ambient assisted living*. Sensors, 18(7), 1991. https://doi.org/10.3390/s18071991
- **[R2]** P, S., & Plötz, T. (2024). *Using graphs to perform effective sensor-based human activity recognition in smart homes*. Sensors, 24(12), 3944. https://doi.org/10.3390/s24123944
- **[R3]** Amraoui, N., & Zouari, B. (2022). *Anomalous behavior detection-based approach for authenticating smart home system users*. International Journal of Information Security, 21, 611–636. https://doi.org/10.1007/s10207-021-00571-6
- **[R4]** Cook, D. J., et al. (2013). *CASAS: A Smart Home in a Box*. IEEE Computer. https://doi.org/10.1109/MC.2012.328
- **[P1]** NIST SP 800-63B-4 (2025), authentication and authenticator management: https://doi.org/10.6028/NIST.SP.800-63B-4
- **[P2]** NIST IR 8259A (2020), IoT device cybersecurity capability baseline: https://doi.org/10.6028/NIST.IR.8259A
- **Public feasibility datasets:** https://doi.org/10.5281/zenodo.17180309 · https://doi.org/10.5281/zenodo.14243471 · https://doi.org/10.5281/zenodo.10223646

**V1 status:** Requirements and thresholds defined for future testing; no implementation, security certification or evaluation outcome is asserted here.
