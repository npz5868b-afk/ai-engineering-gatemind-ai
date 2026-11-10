# GateMind AI
### Reliability-Aware Multimodal Smart-Lock Access Event Intelligence

**UTM · AI Engineering · Topics 1–3 / Weeks 1–3**  
**Project type:** FYP-aligned AI System / Application Development  
**Stage:** Scoping and conceptual design — not a deployed or validated security product

> **Research question:** Can reliability-aware multimodal sensor fusion classify smart-lock access events more robustly than fixed rules when inputs are missing or contradictory?

## Topic 1 Lab — Part A / B / C

The lab is organized into three distinct activities. **Part A and Part B are group work; Part C is individual FYP scoping.** Each student's repository should still store the official deliverable files in `docs/`.

| Part | Activity | Who | Guide | Official deliverable | Status |
|---|---|---|---|---|---|
| **A** | Analyse an existing AI system | Group | [Part A guide](part-a/README.md) | [Observation sheet](docs/week1_observation_sheet.md) | Uploaded — complete |
| **B** | Draw a conceptual AI system map | Group | [Part B guide](part-b/README.md) | [System map PNG](docs/week1_system_map.png) | Uploaded — complete |
| **C** | Scope GateMind AI and set up GitHub | Individual | [Part C guide](part-c/README.md) | [Project scope](docs/week1_project_scope.md), root README and repository URL | Cited scope prepared |

**Important:** Part A and B should analyze the **group's assigned AI system**, which may be ChatGPT, YouTube/Spotify, or Claude—not automatically GateMind AI. Part C uses GateMind AI as directed by the lecturer.

## Overview
GateMind AI explores contextual access-event intelligence for a custom smart-lock prototype. Instead of treating a single successful PIN, fingerprint or camera output as the whole story, it combines authentication results with nearby motion and door-state observations to classify access events. Its **AI contribution is reliability-aware event classification**, not inventing a new face-recognition model.

## Problem Statement
Authentication and physical door activity provide different signals. Missing, noisy and conflicting observations complicate access-event interpretation. GateMind AI investigates whether multimodal fusion can reduce false alerts and improve classification while keeping decision latency measurable.

## Evidence of the Problem

- **Sensor reliability:** Non-intrusive smart-home sensors can produce missed or spurious events. ElHady and Provost (2018) review sensor failure detection and fault-tolerance challenges ([Sensors, 18(7), 1991](https://doi.org/10.3390/s18071991)).
- **Activity interpretation under incomplete signals:** P and Plötz (2024) investigate sensor-based activity recognition in smart homes, including challenges related to noisy, sparse or missing observations ([Sensors, 24(12), 3944](https://doi.org/10.3390/s24123944)).

These papers support the research motivation, but do **not** prove that GateMind AI has already solved these problems. Detailed evidence, boundaries and related work are documented in the [Week 1 project scope](docs/week1_project_scope.md).

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
**Conceptual architecture only.** The separate [Part B group system map](docs/week1_system_map.png) analyzes ChatGPT, not GateMind AI.

## Candidate Event Classes
Authorized Entry · Visitor Knock · Repeated Failed Attempt · Suspicious Dwell · Tailgating Risk · Force Attempt

These are proposed labels; their operational definitions and feasible sensor evidence must be validated before data collection.

## Initial Related Work / Existing Approaches
| Approach | Role | Limitation to test |
|---|---|---|
| Rule-based access-event monitoring | Interpretable baseline | Brittle thresholds and conflicts |
| [XGBoost (Chen & Guestrin, 2016)](https://doi.org/10.1145/2939672.2939785) | Feature-level fusion | Robustness under missing/noisy inputs |
| [GRU (Cho et al., 2014)](https://arxiv.org/abs/1406.1078) | Optional sequence model | Data volume and latency |

Four smart-home/sensor studies, including a behavior-based authentication study and a sensor-fault survey, are catalogued in the [research evidence register](docs/week1_references_and_data_sources.md). XGBoost and GRU are **method references**, not smart-lock outcome studies.

### Smart-home studies and research evidence

- [Amraoui & Zouari (2022)](https://doi.org/10.1007/s10207-021-00571-6) — behavior-based smart-home authentication
- [ElHady & Provost (2018)](https://doi.org/10.3390/s18071991) — systematic survey of sensor failures
- [P & Plötz (2024)](https://doi.org/10.3390/s24123944) — graph-guided activity recognition with sensor robustness
- [Cook et al. (2013)](https://doi.org/10.1109/MC.2012.328) — CASAS smart-home sensor platform

See [Part C scope](docs/week1_project_scope.md) for citations, the precise research gap and study limitations.

## Engineering Gap
Investigate reliable access-event classification under **missing or conflicting sensor signals**, using reproducible baselines and measurable failure scenarios. Novelty claims are provisional until a literature review is completed.

## Objectives
- **O1 — Design:** Integrate face-camera AI outputs, fingerprint/PIN logs, PIR and door-contact events into a consistent event pipeline.
- **O2 — Develop:** Compare rules, XGBoost fusion and reliability-aware fusion; optionally add a temporal GRU.
- **O3 — Evaluate:** Measure per-class F1, false-alarm rate, decision latency, drop-one-sensor ablation and conflict resolution.

## Target Users
Smart-home residents and prototype security-system evaluators.

## Data Sources and Evidence

**Public ambient-event datasets (for sensor-pipeline and robustness feasibility, not complete security-event training):**
- [CASAS Smart Home: Aruba, Cairo, Milan, Tulum](https://doi.org/10.5281/zenodo.17180309) — PIR and door-event data.
- [Smart Home Environment Data Across Four European Countries](https://doi.org/10.5281/zenodo.14243471) — real household PIR and door contacts.
- [eSense PIR Smart Home](https://doi.org/10.5281/zenodo.10223646) — PIR transitions and dataset documentation.

**Public documents for engineering requirements/data interpretation:**
- [NIST SP 800-63B-4](https://doi.org/10.6028/NIST.SP.800-63B-4) — digital authentication guidance.
- [NIST IR 8259A](https://doi.org/10.6028/NIST.IR.8259A) — IoT device cybersecurity baseline.
- [eSense documentation](https://zenodo.org/records/10223646) — sensor-event schema/collection reference.

**Full provenance & academic references:** [Research sources and dataset register](docs/week1_references_and_data_sources.md) and [Data README](data/README.md).

> **Data-source clarification:** Following the lecturer's instruction to continue the FYP, this sensor-based project uses public documents for requirements and data interpretation, public datasets for ambient-event feasibility, and consented prototype logs for the final security-event labels. The public datasets do not contain complete PIN/fingerprint/face-security labels.

## Responsible AI and Safety
Privacy-first collection, informed consent, minimal biometric retention, uncertainty-aware alerts and explicit missing-sensor testing. Experimental AI event labels must **not** override the lock's safe credential-based authorization logic.

## Repository Structure
```text
part-a/      Part A guide (group analysis)
part-b/      Part B guide (group system map)
part-c/      Part C guide (individual FYP scope)
docs/        Week 1–3 deliverables and research evidence
data/        Dataset provenance, prepared outputs and pipeline manifests
src/         Implementation
notebooks/   Colab notebooks and experiments
evaluation/  Metrics, ablations, reports
prompts/     Prompt artifacts, if used
```

## Weekly Progress
| Week | Focus | Deliverable | Status |
|---|---|---|---|
| W1 | Existing AI system analysis, system map and individual FYP scoping | Observation sheet, system map, project scope, README | Prepared; course portal submission pending |
| W2 | Refine application scope, requirements, evaluation and V1 architecture | Requirements, evaluation criteria, ADR, editable architecture | Prepared; portal submission pending |
| W3 | Time-series ingestion, preparation, validation and provenance | Colab notebook, inventory, diagram, decisions, prepared CSV and manifests | Colab execution verified; GitHub artifacts committed |

## Week 2 — Topic 2 Lab

This week's individual FYP-based specification builds on the [Week 1 GateMind project scope](docs/week1_project_scope.md). Proposed requirements and numerical thresholds are **not yet tested or achieved**.

| Part | Scope | Document |
|---|---|---|
| **A** | Project framing; application scope; functional, non-functional, and Responsible AI requirements | [Requirements Specification](docs/week2_requirements.md) |
| **B** | Test criteria; measurement methods; metrics and target thresholds linked to objectives | [Evaluation Criteria](docs/week2_evaluation_criteria.md) |
| **C** | Requirements-driven V1 architecture, architecture decision records and traceability | [V1 PNG Diagram](docs/week2_architecture_v1.png) · [Decisions & Traceability](docs/week2_architecture_decisions.md) · [Editable Draw.io](docs/week2_architecture_v1.drawio) |

**Week 2 deliverables:** All four prescribed files are committed under `docs/`. The requirements contain **proposed, not achieved** evaluation targets. Submit this repository URL through the course portal when instructed.

## Week 3 — Topic 3 Lab: Data Engineering

**Selected pathway:** F — Time-series (course-provided synthetic `energy_readings_demo.csv`). This is a data-engineering practice example, **not** a trained GateMind smart-lock event classifier. The Google Colab notebook includes saved outputs from all eight code cells, including `REPEATABILITY: PASS` and `PROVENANCE: PASS`.

| Lab part | Evidence | Link |
|---|---|---|
| **A — Start with the Data** | Source identity, data-needs traceability and inspection of two representative subsets | [Data Inventory](docs/week3_data_inventory.md) |
| **B — Prepare the Data** | Timestamp parsing and stable sorting, missingness preservation, validated CSV and lineage | [Prepared CSV](data/processed/prepared_timeseries.csv) · [Lineage manifest](data/week3_manifest.json) |
| **C — Executable V1 Pipeline** | Executed notebook, diagram, architecture/stage decisions, repeatability and run outputs | [Notebook](notebooks/week3_pipeline.ipynb) · [Diagram](docs/week3_pipeline_diagram.png) · [Decisions](docs/week3_pipeline_decisions.md) · [Run manifest](data/pipeline_v1/timeseries/pipeline_manifest.json) · [Run output](data/pipeline_v1/timeseries/prepared_timeseries.csv) |

**Execution provenance:** The notebook was genuinely run in Colab. The separately committed prepared CSV and JSON manifests are samples produced by an equivalent earlier local execution with the same source/output fingerprints; they are not represented as downloads from the Colab runtime. No model or physical smart-lock performance is claimed.

## Week 1 Submission Files

- **Part A:** [Observation Sheet](docs/week1_observation_sheet.md)
- **Part B:** [Conceptual AI System Map (PNG)](docs/week1_system_map.png) · [Editable Draw.io file](docs/week1_system_map.drawio) · [Optional engineering notes](docs/week1_system_map_notes.md)
- **Part C:** [Individual Project Scope](docs/week1_project_scope.md) · [Research sources](docs/week1_references_and_data_sources.md)

**Course portal submission:** Use the URL of this repository. The submission itself must be completed in the course portal.

## Repository Hygiene
Never commit passwords, API keys, private biometric records, personal data or restricted/copyrighted documents. This repository is an engineering record, not proof of a production-ready lock.
