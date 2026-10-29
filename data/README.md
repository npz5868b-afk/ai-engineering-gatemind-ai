# Data Sources and Provenance — GateMind AI

This folder is reserved for **non-sensitive, license-permitted data and dataset documentation**. Do not upload raw biometric files, photos of participants, PINs, fingerprint templates, access credentials or confidential activity logs to a public repository.

## Public datasets for ambient-event feasibility

| ID | Source | Sensors/features | Intended initial experiment | Limit |
|---|---|---|---|---|
| D1 | [CASAS 4-home, Zenodo](https://doi.org/10.5281/zenodo.17180309) | PIR, magnetic door contact, timestamps, activity labels | Windowing and ambient sensor features | No security-event labels or auth logs |
| D2 | [Four-country smart-home data, Zenodo](https://doi.org/10.5281/zenodo.14243471) | PIR, door-contact events, heterogeneous deployments | Ambient signal availability/robustness study | No face/fingerprint/PIN labels |
| D3 | [eSense PIR, Zenodo](https://doi.org/10.5281/zenodo.10223646) | PIR and movement transitions | Doorway dwell/proximity time-series | PIR-only; no door-contact or credential data |

**None of the public datasets supports complete supervised GateMind access-security classification on its own.** Final event-class labels must come from properly defined consented prototype scenarios.

## Public engineering documents
- [NIST SP 800-63B-4](https://doi.org/10.6028/NIST.SP.800-63B-4) — digital authentication reference.
- [NIST IR 8259A](https://doi.org/10.6028/NIST.IR.8259A) — IoT device security reference.
- [eSense documentation, with D3](https://zenodo.org/records/10223646) — data collection and event schema reference.

**Academic requirement:** The lecturer must approve whether these documents, used to inform requirements rather than train a document-grounded model, meet the specified document-source criterion.

## Proposed acquisition plan (not yet executed)
1. Inspect dataset licenses, data dictionaries, and columns.
2. Select an ambient-only baseline from D1, and use D2/D3 for targeted validation where compatible.
3. Prototype event schemas: `timestamp`, `sensor_id`, `channel`, `value`, `available`, `confidence` (when provided).
4. Define new labels and scenarios for smart-lock credentials + camera outputs + magnetic contact + PIR.
5. Collect informed consent and avoid storing direct biometric identifiers.
6. Generate separate **real**, **synthetic**, and **lab-collected** experiment partitions; never merge without provenance.
7. Evaluate on held-out sessions with missingness/conflict stress tests.

For citations and limitations, see [the research evidence register](../docs/week1_references_and_data_sources.md).
