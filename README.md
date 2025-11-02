README — Apache Beam Data Engineering Exercise (CMPE-255 • FA25 • Sec 49)

Student: <Your Name>
Course: CMPE-255 (Data Mining), Section 49 — Fall 2025
Assignment: Apache Beam Data Engineering Exercise (100 pts)
Notebook: FA25_CMPE255_Beam_Exercise_Py312_OK.ipynb

----------------------------------------------------------------------
WHAT I DID (FIRST-PERSON SUMMARY)
----------------------------------------------------------------------
I completed the Apache Beam data engineering exercise in Google Colab. In my
notebook I demonstrate:
- Pipeline I/O: ReadFromText, WriteToText
- Elementwise transforms: Map, Filter
- ParDo (custom DoFn)
- Composite transform (a reusable PTransform)
- Partition (even/odd and hot/ok)
- Windowing on event time (10-second FixedWindows) with Count.PerKey and
  Mean.PerKey
- Bonus ML inference inside a Beam pipeline using a small scikit-learn model
  with a robust custom DoFn

All inputs are generated inside the notebook. All outputs are written under
/content/output/ so they’re easy to verify.

----------------------------------------------------------------------
WHAT I SUBMITTED
----------------------------------------------------------------------
- Colab notebook: FA25_CMPE255_Beam_Exercise_Py312_OK.ipynb
- This README
- Short walkthrough video explaining inputs, outputs, and how each Beam feature
  is used

----------------------------------------------------------------------
HOW TO RUN (COLAB)
----------------------------------------------------------------------
1) Open colab.research.google.com → File → Upload → select the notebook.
2) Run the Install cell (Python 3.12 friendly). If Colab prompts to Restart
   runtime, I accept it, then continue.
3) Run the remaining cells top to bottom.
4) Use the final “Quick Peek / Diagnostics” cell to print sample outputs for the
   video.

Environment I used:
- Python 3.12
- Apache Beam 2.61.0 (Py 3.12 compatible)
- scikit-learn (Colab default)
- Runner: DirectRunner (local execution in Colab)

----------------------------------------------------------------------
INPUTS I CREATED
----------------------------------------------------------------------
- /content/data/numbers.txt — integers −10..20 (for Map, Filter, Partition)
- /content/data/events.jsonl — 90 JSON lines with (device_id, ts, temp_f) across
  3 devices (for ParDo, composite transform, windowing, and second partition)

----------------------------------------------------------------------
PIPELINES I BUILT (AND WHY)
----------------------------------------------------------------------
1) Map + Filter + Partition + I/O
   - Read numbers, Map string→int, Filter non-negatives, Partition even/odd
   - Write: /content/output/evens-*.txt, /content/output/odds-*.txt
   - Why: demonstrates basic elementwise transforms and Beam file I/O.

2) ParDo + Composite Transform (+ side input)
   - Composite PTransform “ParseEnrich”: parse JSON → °F→°C (ParDo) → drop
     unrealistic temps → add status (“hot” if ≥30°C else “ok”)
   - Safe 90th percentile side input (handles empty collections) → flag “high90”
   - Write: /content/output/clean_events-*.jsonl
   - Why: shows ParDo, reusable PTransform, and side inputs.

3) Windowing on event time
   - Attach event timestamps from “ts” via DoFn
   - FixedWindows(10s), per device Count and Mean
   - Each output line includes window start and end
   - Write: /content/output/window_counts-*.csv, /content/output/window_means-*.csv
   - Why: demonstrates event-time semantics and per-key aggregations.

4) Partition again on enriched events
   - Partition by status into HOT and OK streams
   - Write: /content/output/hot-*.jsonl, /content/output/ok-*.jsonl
   - Why: second Partition example on structured events (not just ints).

5) Bonus: ML inference inside Beam
   - Train a tiny Logistic Regression on Iris (offline), save with joblib
   - Inference via custom DoFn that loads model in setup() and predicts
   - Write: /content/output/iris_preds-*.jsonl
   - Why: shows how inference can be embedded inside a Beam pipeline.

----------------------------------------------------------------------
OUTPUT ARTIFACTS
----------------------------------------------------------------------
- evens-00000-of-00001.txt, odds-00000-of-00001.txt
- clean_events-00000-of-00001.jsonl
- hot-00000-of-00001.jsonl, ok-00000-of-00001.jsonl
- window_counts-00000-of-00001.csv, window_means-00000-of-00001.csv
- iris_preds-00000-of-00001.jsonl

A peek/diagnostics cell prints sample lines from each file.

----------------------------------------------------------------------
RUBRIC MAPPING (WHAT I DEMONSTRATE)
----------------------------------------------------------------------
- Pipeline I/O: ReadFromText, WriteToText
- Map: type conversion, formatting, keying, enrichment
- Filter: non-negative integers, realistic temperature range
- ParDo: °F→°C conversion; adding event timestamps; window formatting
- Partition: even/odd; hot/ok
- Windowing: event-time FixedWindows(10s) with Count and Mean per key
- Composite Transform: reusable ParseEnrich PTransform
- Bonus (ML): predictions inside a Beam pipeline

----------------------------------------------------------------------
MY VIDEO TALKING POINTS
----------------------------------------------------------------------
1) Intro: what I’m demonstrating (Beam primitives + event-time windowing)
2) Inputs: show numbers.txt and events.jsonl
3) Map/Filter/Partition: show evens/odds outputs
4) Composite + ParDo + side input: walk ParseEnrich and high90 flag
5) Windowing: event time vs processing time; show CSV with window boundaries
6) HOT/OK partition: show example records
7) ML inference: show iris_preds predictions
8) Close: summarize how this meets the rubric

----------------------------------------------------------------------
TROUBLESHOOTING I HANDLED
----------------------------------------------------------------------
- Python 3.12 compatibility: pinned Apache Beam 2.61.0
- Jupyter “-f …kernel…json” warnings: used PipelineOptions(flags=[], save_main_session=True)
- Percentile side input: safe guard for empty lists
- RunInference FS in Colab: used a custom DoFn predictor to avoid FS handler issues

----------------------------------------------------------------------
REFERENCES
----------------------------------------------------------------------
- Beam Playground: https://beam.apache.org/get-started/try-beam-playground/
- Interactive Overview: https://beam.apache.org/get-started/an-interactive-overview-of-beam/
- Getting Started Colab: https://colab.research.google.com/github/apache/beam/blob/master/examples/notebooks/interactive-overview/getting-started.ipynb
- Beam ML + scikit-learn: https://beam.apache.org/documentation/transforms/python/elementwise/runinference-sklearn/
- Extra tutorial: https://www.macrometa.com/event-stream-processing/apache-beam-tutorial

----------------------------------------------------------------------
ACADEMIC HONESTY
----------------------------------------------------------------------
This notebook and README are my own work for CMPE-255 FA25. I cited resources
above and described how I used them.
