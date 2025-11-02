# README — Apache Beam Data Engineering Exercise (CMPE-255 • FA25 • Sec 49)

**Student:** _<Your Name>_  
**Course:** CMPE-255 (Data Mining), Section 49 — Fall 2025  
**Assignment:** Apache Beam Data Engineering Exercise (100 pts)  
**Notebook:** `FA25_CMPE255_Beam_Exercise_Py312_OK.ipynb`

---

## 🧠 What I did

I completed the Apache Beam data engineering exercise in **Google Colab**. In this notebook, I demonstrate:

- **Pipeline I/O:** `ReadFromText`, `WriteToText`
- **Elementwise transforms:** `Map`, `Filter`
- **ParDo (DoFn)** with a custom function
- **Composite transform** (`PTransform`)
- **Partition** (even/odd and hot/ok)
- **Windowing** on **event time** (10-second fixed windows) with `Count.PerKey` and `Mean.PerKey`
- **Bonus ML inference** inside a Beam pipeline (scikit-learn Logistic Regression) using a robust custom `DoFn`

All inputs are generated inside the notebook. Outputs are written under `/content/output/`.

---

## ✅ What I’m submitting

- The Colab notebook: **FA25_CMPE255_Beam_Exercise_Py312_OK.ipynb**.
- This **README**.
- A short **walk-through video** where I explain inputs, outputs, and how each Beam feature is used.

---

## ▶️ How to run (Colab)

1. Open **colab.research.google.com** → **File → Upload** → select the notebook.
2. Run the **Install** cell (Python 3.12 friendly). If prompted, **Restart runtime**.
3. Run all the remaining cells **top to bottom**.
4. Use the “Quick Peek / Diagnostics” cell to print sample outputs for the video.

**Environment used:** Python 3.12, Apache Beam 2.61.0, scikit-learn (Colab default), DirectRunner.

---

## 📁 Inputs I created

- `/content/data/numbers.txt` — integers −10..20 (used for Map, Filter, Partition).
- `/content/data/events.jsonl` — 90 JSON lines with `(device_id, ts, temp_f)` across 3 devices.

---

## 🔧 Pipelines I built

1) **Map + Filter + Partition + I/O** → /output/evens*.txt, /output/odds*.txt  
2) **ParDo + Composite Transform + side input** (`ParseEnrich`, 90th percentile safe) → /output/clean_events*.jsonl  
3) **Windowing (event time)** FixedWindows(10s) with Count/Mean → /output/window_counts*.csv, /output/window_means*.csv  
4) **Partition (hot/ok)** on enriched events → /output/hot*.jsonl, /output/ok*.jsonl  
5) **Bonus ML inference** inside Beam (custom DoFn) → /output/iris_preds*.jsonl

---

## 🧩 Rubric mapping

- Pipeline I/O, Map, Filter, ParDo, Partition, Windowing, Composite Transform, and optional ML all demonstrated.

---

## 📚 References

- Beam Playground & interactive docs: https://beam.apache.org/get-started/try-beam-playground/  
- Interactive overview: https://beam.apache.org/get-started/an-interactive-overview-of-beam/  
- Colab getting started: https://colab.research.google.com/github/apache/beam/blob/master/examples/notebooks/interactive-overview/getting-started.ipynb  
- Beam ML overview & sklearn inference: https://beam.apache.org/documentation/transforms/python/elementwise/runinference-sklearn/

