# Week 1 — Project Scope

> **Status:** Initial FYP-aligned scoping draft. Claims requiring external evidence and the document-source requirement are flagged rather than presented as verified.

## Project Name
GateMind AI — Reliability-Aware Multimodal Smart-Lock Access Event Intelligence

## Domain
Artificial Intelligence / Smart Home Security / Multimodal Sensor Fusion

## Problem Statement
A conventional smart lock may authenticate a PIN, fingerprint or face without interpreting the broader access event. Door-contact and proximity signals offer context, but readings may be missing, noisy or conflicting. GateMind AI proposes a prototype smart lock that combines face-camera AI outputs, fingerprint and PIN events, PIR motion and door-contact readings to classify access events reliably. The engineering challenge is to improve classification under imperfect signals while controlling false alarms and decision latency.

## Evidence of the Problem
1. **Authentication versus event context:** A successful credential check does not necessarily describe all physical activity at a doorway. **Evidence status:** Engineering hypothesis; add a credible access-control study or report before submission.
2. **Imperfect sensor inputs:** Missing or contradictory observations may reduce the reliability of event classifiers. **Evidence status:** To be supported by a verified multimodal sensor-fusion study and tested experimentally.

## Initial Related Work (at least 3)
| Approach | Relevance | Initial limitation | Source |
|---|---|---|---|
| Rule-based access-event monitoring | Interpretable baseline | Handcrafted rules may be brittle under conflict | Project baseline; identify a published smart-lock study |
| XGBoost feature fusion | Learns relationships across sensor-event features | May need explicit missingness/reliability features | Chen & Guestrin (2016), [XGBoost](https://doi.org/10.1145/2939672.2939785) |
| GRU temporal classification (optional) | Models event sequences | Requires sufficient sequences; possible latency overhead | Cho et al. (2014), [RNN Encoder–Decoder](https://arxiv.org/abs/1406.1078) |

**Literature note:** The XGBoost and GRU references establish methods, **not** prior smart-lock results. Verify at least three relevant application studies for a final academic submission.

## Engineering Gap / Limitation
The project investigates whether reliability-aware multimodal fusion improves smart-lock **event classification** over fixed rules under missing, noisy and conflicting inputs. This is a proposed engineering gap; the extent of novelty must be confirmed through literature review.

## Initial Project Objectives
- **O1:** Design a prototype event pipeline integrating face-camera confidence outputs, fingerprint/PIN results, PIR activity and door-contact state.
- **O2:** Implement a rule-based baseline, XGBoost feature fusion and a reliability-aware variant; consider GRU if feasible.
- **O3:** Evaluate per-class F1, false-alarm rate, decision latency, drop-one-sensor robustness and conflict scenarios.

## Target Users
Smart-home residents and researchers/evaluators of prototype access-monitoring systems.

## Data Sources (at least 2)
1. **Prototype event logs** — consent-based PIN/fingerprint outcomes, face-model confidence, PIR and door-contact events. **URL:** Not publicly available; to be collected.
2. **ESP32 technical documentation** — [Espressif ESP-IDF](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/). **Type:** Public engineering reference, not model-training data.
3. **XGBoost technical documentation** — [XGBoost Docs](https://xgboost.readthedocs.io/). **Type:** Public engineering reference, not model-training data.

**Important compliance gap:** The Topic 1 lab asks for **2–3 publicly available documents as data sources**. These technical references do not automatically satisfy a document-grounded data requirement. Ask the lecturer whether a sensor-event FYP is exempt or may substitute an approved public sensor dataset/document collection.

## Similar to EduAdvisor
Both require a scoped AI application, traceable data and engineering assumptions, measurable quality, responsible AI controls and GitHub documentation.

## Different from EduAdvisor
EduAdvisor is document-grounded question answering. GateMind AI is multimodal, time-windowed sensor-event classification in a physical prototype; it does not retrieve answers from university PDFs.

## Responsible AI Risks (at least 2)
1. **Privacy:** Biometric and access-event data may expose personal information. **Mitigation:** Informed consent, minimal retention, access control, and storing scores/embeddings rather than raw media when feasible.
2. **False positives and false negatives:** Alerts can alarm residents unnecessarily or miss suspicious activity. **Mitigation:** Per-class evaluation, false-alarm measurement, confidence-aware review.
3. **Sensor/security failures:** Faulty or manipulated signals may produce incorrect event labels. **Mitigation:** Availability flags, missing-sensor tests, conflict evaluation; keep experimental event labels separate from physical unlock authorization.

## Week 1 Completion Status
- [x] Initial scope drafted
- [ ] Verify problem evidence and at least three application-related sources
- [ ] Confirm public-document data requirement with lecturer
- [ ] Add group observation sheet and system map when available
