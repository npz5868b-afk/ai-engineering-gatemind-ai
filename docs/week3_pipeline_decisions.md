# Week 3 — Pipeline Decisions | GateMind AI

**Project:** Reliability-Aware Multimodal Smart-Lock Access Event Intelligence  
**Part C V1 data pipeline:** time-series preparation of course-provided synthetic CSV.  
**Status:** Google Colab run completed and saved in the notebook (8 successful code-cell outputs). Packaged CSV/JSON sample outputs came from an equivalent prior local test, not a Colab export.

## Requirements traceability

| Topic 2 connection | Week 3 implementation | Later evidence |
|---|---|---|
| O1 / event ingestion requirements | Explicit CSV selection, timestamp parser, sensor identity checks | Ingestion preview, schema, source hash |
| O2 / fusion preparation | Sorted timestamped records, missing readings kept visible | Prepared CSV and lineage file |
| O3 / robustness evaluation | Missingness and chronological sanity checks | Observed missing-value count and test cases; **not** security performance results |
| RAI privacy and transparency | Avoid identifying biometric information; record raw-vs-derived data provenance | Dataset inventory and SHA-256 manifests |

## Part B inspection and comparison

| Candidate | Observation / engineering consequence | Decision |
|---|---|---|
| Preserve source row order | Raw order has 03:00 before 02:00; unsuitable for sequential analysis | **Reject** |
| Parse timestamps and stable-sort | All six timestamps parse; sorting restores chronological order without changing numeric measurements | **Select** |
| Automatically fill missing `energy_kwh` | One row at 04:00 has a missing reading; interpolation would invent data | **Reject** |
| Resample to hourly timestamps | All six hours already exist; automatic resampling unnecessary | **Reject** |

**Chosen configuration:** `timestamp` → `pandas.to_datetime(errors='coerce')` → drop invalid timestamps if any (none here) → stable sort by timestamp/sensor → preserve missing readings → CSV export. No scaling, interpolation, learned features or model training.

## Pipeline architecture and trade-offs

- **ETL vs ELT:** ETL for V1, because the Colab pipeline parses, sorts and validates before storing a prepared file. Trade-off: simpler local batch integrity versus lack of in-place query-on-raw functionality.
- **Batch vs streaming:** Batch for V1. Trade-off: reproducibility, simple SHA-256 lineage and predictable student-lab runs versus no real-time alert handling. Real GateMind deployment may require event-stream processing later.
- **Output contract:** `prepared_timeseries.csv` with a parsed and chronologically sorted `timestamp` column, preserved `sensor_id` and `energy_kwh`, plus processing manifests and source hashes.
- **Source permissions:** Dummy files are course-provided, synthetic and appropriate for classroom practice. Do not present as biometric evidence or publish participant data.
- **Limitation:** Energy telemetry is only a structural proxy for timestamp handling. It lacks lock credentials, camera confidence, PIR, door contact and security-event labels; therefore no conclusion about GateMind AI classification, fairness or safety can be drawn.

## Failure points and controls

| Failure point | Impact | Control / recorded evidence |
|---|---|---|
| F1: Missing or unparsable timestamp | Incorrect event order / silently lost events | Count parse failures, fail on missing source column, record row exclusion |
| F2: Missing sensor measurement | Incorrect fabricated event evidence if imputed blindly | Preserve NaN and report missing count; prohibit arbitrary imputation |
| F3: Wrong source file selected from a ZIP containing several CSVs | Invalid experiment built on unrelated labels | Explicit relative path and schema check before transform |
| F4: Output regenerated with a different input | Unrepeatable experiments | SHA-256 source/output fingerprints and rerun comparison |

## Preparation and Colab execution verification

- Input: 6 rows, 3 columns; missing values: 1.
- Prepared: 6 rows, sorted, 0 excluded invalid timestamps; hashes captured.
- No trained model, classification metrics, runtime latency or deployment performance are claimed.

## Files and storage

- `notebooks/week3_pipeline.ipynb` — executable Colab source.
- `docs/week3_data_inventory.md` — provenance and inspection.
- `docs/week3_pipeline_diagram.png` — data-only architecture with failure points.
- `data/processed/prepared_timeseries.csv` — Part B output.
- `data/week3_manifest.json` — Part B lineage.
- `data/pipeline_v1/timeseries/prepared_timeseries.csv` — Part C pipeline output.
- `data/pipeline_v1/timeseries/pipeline_manifest.json` — Part C pipeline-run provenance.

**Recorded evidence:** The committed notebook preserves real Colab ingestion, preparation, validation, manifest and repeatability outputs. These results concern a synthetic time-series practice dataset, not smart-lock model performance.