# Acoustic Behavioural Phenotyping (ABP) – Peer-Review Evaluation Repository

[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![npj Digital Medicine Audit](https://img.shields.io/badge/Audit-npj_Digital_Medicine-brightgreen.svg)]()

This repository provides a standalone audit script and anonymised validation data supporting the computational reproducibility of the downstream analytics described in:

> **Digitising the behavioural prodrome of acute decompensation in older adults using continuous ambient acoustic biomarkers**  
> *Pradyumna Thiruvenkatanathan, Anindya Moitra, Guy Spyropoulos* (Earzz Limited, London, UK).

---

## 1. Overview & Intellectual Property Protection

To protect resident privacy in long-term care environments and comply with UK GDPR and the Data Protection Act 2018:
* **No Raw Audio Waveforms:** Raw bedroom audio is sampled in isolated 2-second packets and processed via mathematically non-reversible one-way feature extraction on secure cloud compute infrastructure. In accordance with granted patents (**US Patent 12,293,772 B2; European Patent EP 4141869 B1**), all audio buffers are permanently deleted immediately post-inference without being written to persistent disk storage.
* **Open Analytical Audit Layer:** This repository provides the complete downstream Stage 4 analytical decision-support pipeline (`evaluate_pipeline.py`) to verify out-of-sample sensitivity, proactive evaluation window distributions, nocturnal specificity, and telemetric feature divergence.

---

## 2. Repository Structure

```text
abp-peer-review/
├── README.md                              <- Evaluation documentation and reproducibility guide
├── LICENSE.txt                            <- Academic & peer-review evaluation license (non-commercial)
├── requirements.txt                       <- Minimal standard dependencies (pandas, numpy)
├── evaluate_pipeline.py                   <- Stage 4 decision-support & metric audit script
└── data/
    └── validation_set.csv                 <- Non-reversible validation data slice (Site A audit benchmark)
```

---

## 3. Quick Start & Execution

### Prerequisites
* Python 3.10 or later
* Git

### Installation
Clone this repository and install the standard dependencies:
```bash
git clone https://github.com/earzzAI/abp-peer-review.git
cd abp-peer-review
pip install -r requirements.txt
```

### Run the Evaluation Pipeline
Execute the pipeline across the validation data slice:
```bash
python evaluate_pipeline.py
```

---

## 4. Expected Reproduction Output

Running `python evaluate_pipeline.py` produces the following terminal output:

```text
--- ALGORITHMIC PERFORMANCE SUMMARY ---
Total Recorded Telemetric Fall Incidents: 7
Pre-Analytical Exclusions (Baseline Sufficiency 14/14 Rule): 0
Protocol-Valid Falls with Captured Trajectory Drift: 7
Uncoupled (Non-Fall) Trajectory Departures: 150
Quiescent Baseline Resting Intervals (True Negatives): 1567
---------------------------------------
Benchmark Sensitivity: 100.0%
Nocturnal Specificity (from 2025-12-01): 91.26% (Uncoupled Departure Rate: 8.74%)
Median Proactive Evaluation Window (Δt) for Captured Falls: 9.0 days
```

---

## 5. Data Dictionary: `validation_set.csv`

This repository includes a de-identified slice of the temporal validation dataset (`validation_set.csv`). To protect resident privacy in compliance with UK GDPR and HRA guidelines, the data contains only non-reversible, abstracted trajectory metrics and event flags.

| Column Name | Data Type | Description |
| :--- | :--- | :--- |
| `monitor_id` | `String` | Anonymised unique acoustic sensor identifier corresponding to a specific resident's single-occupancy room. |
| `date` | `Date (YYYY-MM-DD)` | The calendar date corresponding to the morning conclusion (07:00) of the 11-hour nocturnal monitoring window. |
| `prediction_prob` | `Float` | The raw decompensation probability ($P_{i,t} \in [0, 1]$) decoded by the Stage 3 XGBoost ensemble for resident $i$ on `date` $t$ at the morning conclusion (07:00) of the 11-hour nocturnal monitoring window. Operates strictly as an internal trajectory metric evaluated by Stage 4 analytic filtering, rather than a standalone clinical risk score. |
| `burstiness_night_0` | `Float` | The burstiness parameter ($B$) for the last nocturnal window. Quantifies whether acoustic events are evenly spaced ($B \le 0$) or clustered into dense, restless bursts ($B > 0$). Used for observable symptom gating ($S_{\text{agitation}}$). |
| `fraction_core_night_0` | `Float` | The fraction of total nocturnal acoustic activity occurring strictly during the Core Night chronobiological phase (00:00–04:00). Used to evaluate core sleep architecture fragmentation ($S_{\text{sleep}}$). |
| `Target` | `Integer (0, 1)` | Deterministic, protocol-audited binary ground truth. `1` denotes a verified physical fall event, occurring within the next 24 hours of the morning conclusion (07:00) of the 11-hour nocturnal monitoring window. `0` means no fall incident occurred in the next 24 hours of the morning conclusion (07:00) of the nocturnal monitoring window. |

---

## 6. License

This repository (including all software, algorithms, and data files) is proprietary and is made available strictly for the non-commercial purpose of academic peer review, verification and reproducibility. 

For full legal terms, please read the [LICENSE.txt](LICENSE.txt) file in this repository. Copyright &copy; 2026 Earzz Limited. All rights reserved.