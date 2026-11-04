# GateMind AI — Week 2 Evaluation Criteria

**AI Engineering · Topic 2 Lab · Part B**  
**Version:** 1.0 — proposed evaluation plan (no results measured yet)  
**Derived from:** [Week 2 Requirements](week2_requirements.md), Objectives O1–O3.

The table selects the most important requirements for a single-door, sensor-based event-classification prototype. The lecturer's capability buckets are used where applicable: **Domain-specific capability**, **Instruction-following capability** (enforcing defined safety/data-output constraints), and **Cost and latency**. **Generation capability is intentionally not used** because GateMind V1 is not a generative Q&A assistant.

| Objective / Requirement ID | Evaluation criterion | Capability bucket | Measurement method | Metric | Proposed usefulness threshold | Intended user / business outcome |
|---|---|---|---|---|---|---|
| **O1 / FR1 / NFR4** | Event-log completeness | Domain-specific capability | Replay ≥200 sensor/auth events and check common schema, timestamp, ID and availability fields. | Complete event metadata rate | **≥99%** | Reliable event history allows operators to investigate access events without hidden missing records. |
| **O2 / FR3 / NFR2** | Access-event classification quality | Domain-specific capability | Score a frozen, session-separated, human-labelled test set covering the four V1 event classes. Publish per-class confusion matrix. | Macro-F1 and per-class F1 | **Macro-F1 ≥0.80; each class ≥0.65** | Residents get useful categorised alerts instead of only raw credential failures. |
| **O2 / O3 / NFR3** | Single-sensor dropout robustness | Domain-specific capability | On the same held-out test set, remove PIR or face-score modality individually and re-score, recording coverage and uncertain cases. | Absolute macro-F1 loss | **≤0.12** per single nonessential modality dropout | Occasional sensor loss is less likely to produce an invalid event conclusion. |
| **O3 / RAI4** | Benign-scenario false-alert safety | Domain-specific capability | Run ≥60 independently labelled benign sessions; count inappropriate high-risk alerts. | High-risk alerts / benign sessions | **≤5%** | Avoids unnecessary worry or unfair suspicion of visitors. |
| **O3 / NFR1** | Decision responsiveness | Cost and latency | On declared demo hardware, replay ≥100 complete event windows, clock time to visible event result. | p90 post-window latency | **≤2.0 s** | Timely notifications without assuming that sensing-window time is zero. |
| **O1 / RAI1** | Sensitive-data non-disclosure | Instruction-following capability | Inspect ≥20 sensitive-input test cases, exports, persistence and logging paths. | Forbidden sensitive-field leaks | **0** | Better participant privacy and safer public engineering records. |
| **O1 / O2 / RAI2** | Physical-lock safety separation | Instruction-following capability | Verify classifier has no actuator interface; trace ≥30 adverse/benign trial cases. | AI-generated unlock commands | **0** | An inaccurate event label cannot on its own open the lock. |
| **O2 / FR4 / RAI3** | Transparent uncertainty and input availability | Instruction-following capability | Audit ≥100 output records including absent/conflicting channels. | Results containing all mandatory explanation fields | **100%** | Residents know when signals were uncertain rather than trusting a fabricated certainty. |

## How tests will be run (planned)

**Reference set.** Define four operational event labels in the requirements document, collect consented and independently labelled prototype scenarios, and freeze **session-separated** test data before tuning thresholds. Public CASAS/eSense records are *ambient-sensor feasibility data*, not authentic evidence for PIN/fingerprint/face-based door-security labels.

**Comparison.** Run fixed rules, a learned feature-fusion baseline, and a reliability-aware version over exactly the **same** test sessions. Present macro-F1, per-class F1, confusion matrices, false-alerts, time statistics, abstention/uncertainty coverage and a one-modality-at-a-time ablation table. A performance improvement must be measured; it must not be asserted simply because the architecture contains a reliability module.

**Threshold rationale.** Initial values are **provisional engineering acceptance targets**, chosen to distinguish useful from obviously weak prototype behaviour and to support falsifiable evaluation. They are not copied from a published GateMind benchmark or claimed as achieved. Review them after the pilot without secretly changing the held-out acceptance criteria.

**Failure reporting.** A criterion with insufficient independent test examples is reported as **not assessable**. A metric that misses its target is recorded as failed; do not hide it by choosing only favourable windows.

## Trace to later artefacts

- [Requirements specification](week2_requirements.md): FR1–FR7, NFR1–NFR5, RAI1–RAI4.
- [V1 architecture diagram](week2_architecture_v1.png): sensor/credential pipeline, reliability stage, model, output, audit, separated lock authorization, evaluation.
- [Architecture decisions and traceability](week2_architecture_decisions.md): accepted trade-offs and planned evidence for each selected criterion.
