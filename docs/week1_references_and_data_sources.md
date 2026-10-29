# GateMind AI — Research Sources and Dataset Provenance

**Prepared for:** Topic 1 Lab, Part C | **Date:** 29 October 2026  
All URLs below point to verified public landing pages or DOI records. No claim is made that their full datasets or article PDFs have been downloaded, inspected row by row, or used for training.

## Core Related Work

| ID | Full bibliographic citation | Verified contribution | Relation / limitation for GateMind |
|---|---|---|---|
| R1 | Amraoui, N., & Zouari, B. (2022). *Anomalous behavior detection-based approach for authenticating smart home system users.* International Journal of Information Security, 21, 611–636. https://doi.org/10.1007/s10207-021-00571-6 | Duenna performs behavioral anomaly detection on smart-home user/device commands. | Closest security-related predecessor; different from a door-local multi-input access-event classification problem. |
| R2 | ElHady, N. E., & Provost, J. (2018). *A systematic survey on sensor failure detection and fault-tolerance in ambient assisted living.* Sensors, 18(7), 1991. https://doi.org/10.3390/s18071991 | Reviews false sensor triggers, failure detection, fault-tolerant recognition, and limitations of public failure labels. | Motivates explicit reliability/availability features and injected failure tests; not a specific lock classifier. |
| R3 | P, S., & Plötz, T. (2024). *Using graphs to perform effective sensor-based human activity recognition in smart homes.* Sensors, 24(12), 3944. https://doi.org/10.3390/s24123944 | Graph-guided smart-home activity recognition; examines missing sensor readings. | Demonstrates prior sensor dependency modeling; GateMind proposes a smaller interpretable comparative study for security events. |
| R4 | Cook, D. J., Crandall, A. S., Thomas, B. L., & Krishnan, N. C. (2013). *CASAS: A smart home in a box.* Computer, 46(7), 62–69. https://doi.org/10.1109/MC.2012.328 | Smart-home sensing platform and activity-study foundation. | Supports public ambient sensor work, not PIN/face security labels. |
| R5 | Winterberger, S., An, D., Biallas, M., & Paice, A. (2025). *Smart home environment data across 4 European countries.* Data in Brief, 60, 111636. https://doi.org/10.1016/j.dib.2025.111636 | Documents real residential PIR/door event data with heterogeneous deployments. | Dataset companion evidence; it is not access-security ground truth. |

### Method References (secondary)
- Chen, T., & Guestrin, C. (2016). *XGBoost: A scalable tree boosting system*. Proceedings of KDD. https://doi.org/10.1145/2939672.2939785
- Cho, K., et al. (2014). *Learning phrase representations using RNN encoder–decoder for statistical machine translation*. https://arxiv.org/abs/1406.1078

These are methods, not smart-lock application studies.

## Public Sensor Datasets

| ID | Dataset | DOI / URL | What is described | Supported use | Cannot support |
|---|---|---|---|---|---|
| D1 | CASAS Smart Home: Aruba, Cairo, Milan, Tulum (2025 release) | https://doi.org/10.5281/zenodo.17180309 | PIR motion, door contact, temperature, timestamps, activity annotation | Parsing, ambient time-window features, ambient-only proof of concept | Face/fingerprint/PIN confidence, unauthorized-entry labels |
| D2 | Smart Home environment data across 4 European countries (2024 release) | https://doi.org/10.5281/zenodo.14243471 | PIR, door-contact events, environment readings, participant/time/location fields | Transfer feasibility and sensor-missingness tests | Verified security-attempt labels; full biometrics |
| D3 | eSense: PIR-based solo-resident smart home (2023) | https://doi.org/10.5281/zenodo.10223646 | PIR raw-event CSVs, transition logs, explanatory PDF | Doorway-motion/transition features; schema interpretation | Door-contact events, PIN/fingerprint/face outputs |

**Access notes:** D1's curated archive is about 33 MB. D2 offers a separate `event_data.zip` (~6.8 MB) instead of requiring the much larger monthly environmental archive. D3 offers multiple CSVs and a PDF explanation. Verify actual column schemas, licensing details and permissible uses before importing or redistributing.

## Public Documents (not falsely relabeled as sensor data)

| ID | Document | DOI / URL | Proposed engineering role | Boundary |
|---|---|---|---|---|
| P1 | NIST SP 800-63B-4 (2025), Authentication and Authenticator Management | https://doi.org/10.6028/NIST.SP.800-63B-4 | Credential/authenticator design concepts and privacy-aware security rationale | For digital authentication, not physical-door lock certification |
| P2 | NIST IR 8259A (2020), IoT Device Cybersecurity Capability Core Baseline | https://doi.org/10.6028/NIST.IR.8259A | IoT protection, device security state and interface-access requirements | Requirements reference, not a labeled training dataset |
| P3 | eSense Smart Home dataset's accompanying technical PDF | https://zenodo.org/records/10223646 | Actual data schema and PIR-trigger collection methodology | Documentation of D3, not an extra independent dataset |

### Lecturer Approval Needed
The Week 1 Topic 1 template asks for 2–3 **publicly available documents as data sources**. GateMind AI is not a document-grounded assistant. **Proposed adaptation:** use the three public documents as authoritative engineering input and provenance; use public sensor datasets D1–D3 for algorithm prototyping; use consented lab-generated events for the final supervised security-event labels. Seek explicit lecturer agreement that this satisfies the source requirement. If denied, adapt the scope rather than claim documents were model-training samples.

## Proposed evaluation integrity

1. Record source, license, sensor mapping, transformation, consent, and timestamp normalization.
2. Do not map everyday activity labels directly to 'force attempt'/'tailgating' without verified labeling.
3. Separate real ambient-only experiments from prototype security-event experiments in metrics.
4. Split chronologically or by recorded session/participant to avoid leakage across correlated event windows.
5. Inject missing signals, contradictory readings and latency in the **held-out** data with a documented protocol.
6. Report macro-F1, per-class F1, false alarm rate, latency and missing-channel ablation; never invent measured performance.
7. Do not publish raw biometric records or restricted data in this public GitHub repository.

**Source review:** Public abstracts, DOI metadata and repository descriptions checked on 29 October 2026. Full paper/dataset ingestion remains a later implementation task.
