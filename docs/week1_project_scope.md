# Week 1 — Project Scoping Template

**Course:** AI Engineering · Topic 1 Lab · Part C (Individual)  
**Project:** GateMind AI (FYP-aligned)  
**Status:** Evidence-backed preliminary project framing, 29 October 2026  
**Purpose:** Semester engineering record. No model training, hardware completion, or evaluation results are claimed at Week 1.

## Project Name
**GateMind AI — Reliability-Aware Multimodal Smart-Lock Access Event Intelligence**

## Domain
Artificial Intelligence; Smart-Home Security; Multimodal Sensor Fusion; Access-Event Classification.

## Problem Statement
Smart-lock authentication verifies credentials, but an individual successful/failed PIN, fingerprint, or face match does not fully describe the physical event unfolding around a doorway. Door-contact and PIR sensors can provide context; however, real-world sensor streams may be noisy, incomplete, or contradictory. Published smart-home research reports challenges from sensor failure and missing observations [R2, R3]. GateMind AI proposes a **laboratory-scale smart-lock event-intelligence prototype** that combines authentication events, face-camera model outputs, motion, and door-state signals. The engineering question is whether **reliability-aware multimodal fusion** can classify access-event sequences more robustly than fixed rules while maintaining acceptable false-alert rates and response latency.

**Research question:** *Compared with a rule-based baseline and ordinary feature fusion, how does reliability-aware fusion affect access-event classification performance when sensor information is incomplete or conflicting?*

## Evidence of the Problem (at least 2)

**Evidence 1 — Smart-home sensor failures have measurable consequences.** ElHady and Provost (2018) systematically review non-intrusive, event-driven sensor failures and fault-tolerance techniques. They describe false-positive/false-negative sensor triggers and note that public datasets often lack fault labels, prompting researchers to inject failures for experiments [R2]. **Relevance:** GateMind AI should explicitly encode availability/confidence and test sensor-dropout scenarios rather than assume ideal inputs. **Boundary:** This survey concerns ambient-assisted-living systems, not an evaluation of this smart-lock prototype.

- Citation: ElHady, N. E., & Provost, J. (2018). *A systematic survey on sensor failure detection and fault-tolerance in ambient assisted living*. **Sensors, 18**(7), 1991. https://doi.org/10.3390/s18071991

**Evidence 2 — Recognizing activities from real smart-home sensors is difficult.** P and Plötz (2024) report noise, sparsity, sensor variability, and dependence on pre-segmented streams in activity recognition. Their graph-guided model studies sensor relationships and missing measurements on CASAS datasets [R3]. **Relevance:** A doorway event classifier needs a transparent time-window and conflict-handling strategy. **Boundary:** Their targets are general daily activities, not GateMind AI's access-security labels.

- Citation: P, S., & Plötz, T. (2024). *Using graphs to perform effective sensor-based human activity recognition in smart homes*. **Sensors, 24**(12), 3944. https://doi.org/10.3390/s24123944

**Evidence 3 — Behavioral context is relevant to smart-home authentication.** Amraoui and Zouari (2022) propose *Duenna*, which evaluates smart-home control actions against learned user behavior. It demonstrates an existing behavioral-anomaly approach to smart-home authorization [R1]. **Relevance:** Contextual security is an established research topic; GateMind should not present behavior-aware authentication as an entirely new idea.

- Citation: Amraoui, N., & Zouari, B. (2022). *Anomalous behavior detection-based approach for authenticating smart home system users*. **International Journal of Information Security, 21**, 611–636. https://doi.org/10.1007/s10207-021-00571-6

## Initial Related Work / Existing Approaches (at least 3)

| Study / existing solution | What was actually studied | Limitation relative to GateMind AI | Source |
|---|---|---|---|
| **[R1] Duenna: behavior-based smart-home authentication** (Amraoui & Zouari, 2022) | Detects anomalous control-app commands based on user cyber/physical behavior and smart-home state. | Focuses on authorization of smart-home commands, **not** reliability-aware classification of door-local sensor and credential event sequences. | https://doi.org/10.1007/s10207-021-00571-6 |
| **[R2] Sensor-fault tolerance systematic review** (ElHady & Provost, 2018) | Surveys failures of non-intrusive binary sensors, fault detection, and fault-tolerant activity recognition. | Documents an existing research problem and evidence gaps; it is a **review**, not a deployable door-access classifier. | https://doi.org/10.3390/s18071991 |
| **[R3] Graph-guided sensor activity recognition** (P & Plötz, 2024) | Studies real smart-home activity recognition, sensor relationships, noise and sensor-dropout robustness. | Targets broad activities with a graph-guided neural approach, rather than a small, explainable smart-lock event taxonomy and a rule-vs-XGBoost reliability-weighted comparison. | https://doi.org/10.3390/s24123944 |
| **[R4] CASAS: A Smart Home in a Box** (Cook et al., 2013) | Describes an installable smart-home sensing platform and the foundation for public ambient-sensor research. | Enables ambient activity studies but does **not** supply fingerprint/PIN/face-confidence observations or GateMind's security-event labels. | https://doi.org/10.1109/MC.2012.328 |

**Initial methodological baselines (not falsely presented as smart-lock studies):**
- Chen, T., & Guestrin, C. (2016). *XGBoost: A scalable tree boosting system*. KDD. https://doi.org/10.1145/2939672.2939785 — candidate tabular event-feature classifier.
- Cho, K., et al. (2014). *Learning phrase representations using RNN encoder–decoder for statistical machine translation*. https://arxiv.org/abs/1406.1078 — origin of GRU-style sequence modeling; optional stretch comparison.

## Engineering Gap / Limitation
Existing studies establish that behavior-aware smart-home security, ambient activity recognition, and sensor-failure robustness already exist [R1–R4]. **This project does not claim to invent multimodal fusion or anomaly detection.** Its proposed, limited-scope contribution is an **end-to-end experimental comparison for smart-lock *access-event* interpretation**: fixed rules vs standard XGBoost feature fusion vs a reliability-aware version, where each event includes feature availability, confidence and temporal context.

The **testable gap** is how these approaches behave when a door-side face result, credential event, or ambient sensor is *missing, delayed, unreliable, or contradictory*; the result should be judged by per-class classification, false alerts, latency and stress tests. The precise novelty relative to prior door-specific systems needs continued review in FYP1.

## Initial Project Objectives (2–3)
- **O1 — Design:** Define an auditable, timestamped event pipeline combining PIN/fingerprint outcomes, pretrained face-camera confidence outputs, PIR movement, door contact, and optional vibration. Mark missing and uncertain channels explicitly.
- **O2 — Develop:** Implement a transparent rule-based baseline and an XGBoost multimodal feature-fusion classifier, then add a reliability-aware variant (availability/confidence features and conflict policies). GRU is optional only if sufficient sequences exist.
- **O3 — Evaluate:** Compare per-class precision/recall/F1, macro-F1, false-alarm rate, decision latency, and performance under drop-one-sensor, injected noise, timing mismatch and conflicting evidence. Report measured values only after experiments.

## Target Users
Residents who need understandable context about door access, and academic evaluators testing a smart-lock prototype. **Not** marketed as a safety-certified residential security system.

## Data Sources (at least 2; public URLs provided)

### A. Actual publicly available **sensor datasets** — for feasibility studies and ambient-feature tests
1. **[D1] CASAS Smart Home (Aruba, Cairo, Milan, Tulum)** — https://doi.org/10.5281/zenodo.17180309 (Zenodo, 2025). Timestamped PIR motion, magnetic door open/close and temperature data; externally annotated activities. **Use:** Prototype time-window parsing, motion/door feature extraction, and ambient activity baselines. **Limitation:** No fingerprint/PIN/face scores or ground-truth security-attempt labels.
2. **[D2] Smart Home Environment Data Across Four European Countries** — https://doi.org/10.5281/zenodo.14243471 (Winterberger et al., 2024 dataset; 2025 accompanying article). Anonymized records from 62 residential participants with PIR and door-contact events, among other environmental variables. **Use:** Explore missing channels, heterogeneous installation layouts and robustness of ambient-only features. **Limitation:** Events are not labeled as authorized entry, forced entry, or authentication failure.
3. **[D3] eSense Smart Home (PIR-based, solo resident)** — https://doi.org/10.5281/zenodo.10223646 (Shirali, Jafari & Ghassemian, 2023). Public PIR-trigger CSVs and room transition event logs; a dataset description PDF is included. **Use:** Optional doorway proximity, transitions and sequence exploration. **Limitation:** PIR only; no contact/credential/face signals or security classes.

**Planned project-specific data:** Laboratory prototype logs (credential success/fail, face-confidence, PIR, door contact and annotated scenario labels), collected **with informed consent** and securely handled. The public datasets are **not** claimed to train the complete GateMind security-event model. A controlled scenario protocol must produce valid labels for the proposed security events; prohibit training/testing leakage between closely related event sequences.

### B. Publicly accessible **documents** — to derive design requirements, data definitions and risk controls
1. **[P1] NIST SP 800-63B-4 — Digital Identity Guidelines: Authentication and Authenticator Management** (2025): https://doi.org/10.6028/NIST.SP.800-63B-4. **Design use:** Authentication concepts, credential management, limitations of biometric authentication. **Scope:** Guidance for digital/online authentication, *not* a physical-door certification standard or event dataset.
2. **[P2] NIST IR 8259A — IoT Device Cybersecurity Capability Core Baseline** (2020): https://doi.org/10.6028/NIST.IR.8259A. **Design use:** Privacy/security requirements for IoT device access, software/data protection and cybersecurity state reporting. **Scope:** Engineering guidance, *not* labeled sensor records.
3. **[P3] eSense Smart Home dataset documentation** (public PDF accompanying [D3]): https://zenodo.org/records/10223646. **Data-documentation use:** Interpret sensor-trigger descriptions, collection configuration and event-log format. **Scope:** Dataset metadata, not an independent second dataset.

**Data-source interpretation for the FYP-aligned assignment:** The lecturer instructed students to continue their FYP in Part C. GateMind is a sensor-based classifier rather than a document-grounded Q&A assistant. The public documents [P1–P3] inform requirements and sensor-data definitions, while the public datasets [D1–D3] support ambient-event feasibility work. Final smart-lock security labels will be created through consented prototype scenarios. These sources have different roles and are not represented as interchangeable training inputs.

### Source-to-feature mapping (proposed; not yet implemented)
| Signal | Public dataset feasibility | Prototype-specific source |
|---|---|---|
| PIR activity / dwell / transitions | D1, D2, D3 | PIR sensor with timestamps |
| Door opened / closed | D1, D2 | Magnetic door contact |
| Face recognition / optional anti-spoof confidence | Not included in D1–D3 | Door-side camera, pretrained model output |
| PIN success/fail | Not included in D1–D3 | Prototype keypad event log |
| Fingerprint success/fail | Not included in D1–D3 | Prototype reader event log |
| Vibration / force indication | Not included in D1–D3 | Optional vibration sensor |

## Similar to EduAdvisor
Both are scoped AI applications that use traceable sources, explicit user needs, a documented pipeline, evaluation evidence and Responsible AI controls.

## Different from EduAdvisor
EduAdvisor uses institutional PDFs as retrieval context to generate question-answer responses. GateMind AI uses timestamped **sensor and authentication events** as classifier inputs; its public documents serve as requirements and dataset documentation rather than retrieval-based answering. The evaluation focuses on event labels, missing modalities, false alerts and response latency.

## Responsible AI Risks (at least 2)
1. **Biometric privacy and consent.** Face-related data and access logs can expose individuals. **Affected:** Participants/residents. **Mitigation:** Collect only consented laboratory data; limit retention, use role-based access and data minimization; prefer derived confidence/embeddings over raw biometric images where feasible.
2. **False alerts and missed events.** Misclassification can alarm residents or fail to surface suspicious activity. **Affected:** Residents/visitors. **Mitigation:** Report per-class confusion, false-alarm rate, calibration/uncertainty and escalation to human review; explicitly label the system as experimental.
3. **Missing/conflicting/spoofed sensors.** A faulty camera/PIR/contact signal can mislead event classification. **Affected:** Residents and system operators. **Mitigation:** Monitor signal health, availability and timestamps; perform failure injection and missing-modality ablation.
4. **Unintended physical action.** An AI label should not independently release the magnetic lock. **Affected:** People and property. **Mitigation:** Keep credential authorization and any physical lock actuator control separate from experimental event-intelligence alerts.

## References (verified source pages)
- [R1] Amraoui, N., & Zouari, B. (2022). *Anomalous behavior detection-based approach for authenticating smart home system users.* International Journal of Information Security, 21, 611–636. https://doi.org/10.1007/s10207-021-00571-6
- [R2] ElHady, N. E., & Provost, J. (2018). *A systematic survey on sensor failure detection and fault-tolerance in ambient assisted living.* Sensors, 18(7), 1991. https://doi.org/10.3390/s18071991
- [R3] P, S., & Plötz, T. (2024). *Using graphs to perform effective sensor-based human activity recognition in smart homes.* Sensors, 24(12), 3944. https://doi.org/10.3390/s24123944
- [R4] Cook, D. J., Crandall, A. S., Thomas, B. L., & Krishnan, N. C. (2013). *CASAS: A smart home in a box.* Computer, 46(7), 62–69. https://doi.org/10.1109/MC.2012.328
- [R5] Winterberger, S., An, D., Biallas, M., & Paice, A. (2025). *Smart home environment data across 4 European countries.* Data in Brief, 60, 111636. https://doi.org/10.1016/j.dib.2025.111636
- [P1] NIST (2025). *SP 800-63B-4: Digital Identity Guidelines — Authentication and Authenticator Management.* https://doi.org/10.6028/NIST.SP.800-63B-4
- [P2] NIST (2020). *IR 8259A: IoT Device Cybersecurity Capability Core Baseline.* https://doi.org/10.6028/NIST.IR.8259A
