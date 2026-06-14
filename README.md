# Cancer_Clinical_Trial_Measuring_Barriers
# 🔬 CancerGate
### *Measuring the Hidden Barriers in Cancer Clinical Trial Eligibility*

> **"Only 3–5% of cancer patients ever enroll in a clinical trial. We built the data infrastructure to understand why."**

![Python](https://img.shields.io/badge/Python-3.12-blue?style=flat-square&logo=python)
![Pandas](https://img.shields.io/badge/Pandas-2.0-150458?style=flat-square&logo=pandas)
![NLP](https://img.shields.io/badge/NLP-Regex%20%2B%20Pattern%20Mining-green?style=flat-square)
![Dataset](https://img.shields.io/badge/Dataset-927K%2B%20Records-orange?style=flat-square)
![Domain](https://img.shields.io/badge/Domain-Healthcare%20%7C%20Oncology-red?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-lightgrey?style=flat-square)

---

## 📌 Table of Contents

- [Problem Statement](#-problem-statement)
- [Motivation](#-motivation)
- [What Makes This Different](#-what-makes-this-different)
- [Original Metrics Invented](#-original-metrics-invented)
- [Project Architecture](#-project-architecture)
- [Key Findings](#-key-findings)
- [Tech Stack](#-tech-stack)
- [Dataset](#-dataset)
- [Project Structure](#-project-structure)
- [How to Run](#-how-to-run)
- [Results & Achievements](#-results--achievements)
- [Future Scope](#-future-scope)

---

## 🎯 Problem Statement

Cancer clinical trials are the most rigorous pathway to life-saving treatments. Yet the patients these trials are designed to help are often the ones least able to participate in them.

Eligibility criteria — the rules that determine who can and cannot join a trial — are written in dense, unstructured clinical language. No standardised framework exists to measure how restrictive these criteria are, or what percentage of real-world patients they systematically exclude.

**CancerGate answers three questions no existing tool does:**

1. How restrictive is the average cancer clinical trial's eligibility criteria?
2. Which patient groups — by age, treatment history, and health status — are being disproportionately excluded?
3. What percentage of real-world cancer patients could actually qualify for the trials meant to save them?

---

## 💡 Motivation

The inspiration behind CancerGate came from a single, uncomfortable statistic:

> *The average cost of bringing a cancer drug to market exceeds $2.6 billion — yet most trials fail not because the drug doesn't work, but because the trial design itself is flawed.*

A significant driver of trial failure is **poor patient recruitment** — caused directly by overly restrictive eligibility criteria. Trials exclude patients who are too old, who have already received treatment, or who have common comorbidities. The result: trial populations that look nothing like real patients, leading to drugs that work in trials but underperform in the real world.

This project was built to:

- **Quantify** the gap between trial eligibility and real-world patient populations
- **Expose** systemic biases in how eligibility criteria are written
- **Arm** researchers, regulators, and patient advocates with data-driven evidence to demand more inclusive trial design

---

## 🚀 What Makes This Different

Most Kaggle notebooks on clinical trial data stop at counting trial phases or plotting drug frequencies. CancerGate goes several layers deeper:

| What Others Do | What CancerGate Does |
|---|---|
| Count trials by phase | Measure how hard each trial is to enter |
| List top cancer types | Prove which cancers have the most exclusive trials |
| Basic EDA and bar charts | NLP-driven pattern extraction on 927K+ records |
| Use existing metrics | **Invent two original, referenceable metrics** |
| Show what the data contains | Prove what the data means for real patients |

### The Core Differentiator

This project introduces a **custom NLP preprocessing pipeline** designed specifically for clinical eligibility text — handling word-number conversion (`"eighteen"` → `18`), encoded comparison operators (`greater_than`, `less_than`), and domain-specific abbreviations (`ECOG`, `Zubrod`, `SGOT`, `IULN`) that standard NLP tools fail on out of the box.

---

## 📐 Original Metrics Invented

### 1. Eligibility Barrier Score (EBS)
> *A composite score from 0–100 measuring how restrictive a trial's eligibility criteria is.*

The EBS assigns weighted penalty points across six barrier dimensions:

| Barrier Type | Weight | Clinical Rationale |
|---|---|---|
| Age restriction | +20 | Excludes majority elderly cancer population |
| Prior treatment exclusion | +25 | Highest real-world disqualifier |
| Lab value thresholds | +20 | Complex requirements most patients fail |
| Performance status (ECOG/Zubrod) | +15 | Excludes sicker patients |
| Gender restriction | +10 | Cuts eligible pool by ~50% |
| Comorbidity exclusion | +10 | Diabetes, cardiac conditions are common |

**Higher EBS = Harder for a real patient to qualify.**

---

### 2. Patient Reachability Index (PRI)
> *An estimated percentage of real-world cancer patients who could realistically qualify for a given trial.*

The PRI starts at 100% and applies evidence-based multipliers for each barrier identified in the eligibility text:

| Barrier Found | Multiplier | Source |
|---|---|---|
| Prior treatment exclusion | ×0.35 | 65% of patients have had prior treatment |
| Performance status required | ×0.55 | 45% of patients have ECOG > 1 |
| Organ dysfunction excluded | ×0.80 | 20% of patients have organ issues |
| Treatment-naive only | ×0.30 | Only 30% of patients are treatment-naive |
| Gender restricted | ×0.50 | Cuts pool by half |

**Lower PRI = Fewer real patients can get in.**

---

## 🏗 Project Architecture

```
Raw Clinical Text (927K+ records)
           │
           ▼
┌─────────────────────────┐
│   Text Parsing Layer    │  → Extract: Drug | Cancer Type | Criteria
└─────────────────────────┘
           │
           ▼
┌─────────────────────────┐
│   NLP Preprocessing     │  → Lowercase | Symbol removal | Word-to-number
└─────────────────────────┘
           │
           ▼
┌──────────────────────────────────────────────────┐
│              Pattern Extraction Layer             │
│  Age Bias  │  Prior Treatment  │  Lab Values      │
│  ECOG      │  Gender           │  Comorbidity     │
└──────────────────────────────────────────────────┘
           │
           ▼
┌─────────────────────────────────────┐
│         Metric Computation          │
│   Eligibility Barrier Score (EBS)   │
│   Patient Reachability Index (PRI)  │
└─────────────────────────────────────┘
           │
           ▼
┌─────────────────────────┐
│   Visualisation Layer   │  → 8 charts | Dashboard | Key findings
└─────────────────────────┘
```

---

## 📊 Key Findings

> ⚠️ *Findings below are based on analysis of 927,650 parsed eligibility criteria records.*

### Age Bias
- **10,836 criteria** explicitly contain age thresholds
- The average trial age cutoff sits **below the real-world average cancer diagnosis age of 66**
- Elderly patients — who represent the majority of cancer cases — are systematically excluded from the trials designed to treat them

### Prior Treatment Penalty
- A significant proportion of criteria explicitly exclude patients who have received prior chemotherapy, radiation, or immunotherapy
- In the real world, the majority of cancer patients have already undergone at least one line of treatment — making them automatically ineligible for most trials

### Eligibility Barrier Score
- The average EBS across all trials reveals that most cancer trials carry **meaningful eligibility barriers**
- A non-trivial percentage of trials score above 60 — classified as **highly restrictive**

### Patient Reachability Index
- The average PRI across all trials indicates that the **majority of real cancer patients cannot qualify** for the average trial
- Less than 25% reachability is common — meaning 3 out of 4 real patients are excluded before a single medical assessment

---

## 🛠 Tech Stack

| Tool | Purpose |
|---|---|
| `Python 3.12` | Core language |
| `Pandas` | Data loading, parsing, transformation |
| `NumPy` | Numerical operations |
| `re (Regex)` | Pattern extraction from clinical text |
| `Matplotlib` | All chart generation |
| `Seaborn` | Statistical visualisations |
| `WordCloud` | Eligibility language frequency visualisation |

---

## 📂 Dataset

| Property | Detail |
|---|---|
| **Source** | Kaggle — `pratikhiremath/cancer-clinical-trial1` |
| **File** | `labeledEligibilitySample1000000.csv` |
| **Records** | 1,000,000 raw / 927,650 parsed |
| **Format** | Tab-separated, pre-processed clinical NLP text |
| **Content** | Drug interventions, cancer types, eligibility criteria text |
| **Domain** | Oncology — clinical trial eligibility |

> **Note:** The dataset uses word-form numbers (`"eighteen"`) and text-encoded operators (`"greater_than"`) — a non-standard format requiring a custom preprocessing pipeline built as part of this project.

---

## 🗂 Project Structure

```
cancergate/
│
├── cancergate_analysis.py      # Full analysis pipeline
├── README.md                   # This file
│
├── outputs/
│   ├── chart1_age_distribution.png
│   ├── chart2_age_buckets.png
│   ├── chart3_age_by_cancer.png
│   ├── age_bias_detection.png
│   ├── ebs_analysis.png
│   ├── pri_analysis.png
│   ├── ebs_vs_pri.png
│   └── cancergate_dashboard.png
```

---

## ▶️ How to Run

### On Kaggle (Recommended)
1. Go to [kaggle.com](https://kaggle.com) and create a new notebook
2. Add dataset: `pratikhiremath/cancer-clinical-trial1`
3. Upload `cancergate_analysis.py` or paste into notebook cells
4. Click **Run All**

### Locally
```bash
# Clone the repo
git clone https://github.com/yourusername/cancergate.git
cd cancergate

# Install dependencies
pip install pandas numpy matplotlib seaborn wordcloud

# Update the file path in cancergate_analysis.py to your local path
# Then run
python cancergate_analysis.py
```

---

## 🏆 Results & Achievements

### Technical Achievements
- ✅ Built a **custom clinical NLP pipeline** handling word-numbers and encoded operators — not solved by standard NLP libraries
- ✅ Parsed and structured **927,650 unstructured eligibility criteria** records end-to-end
- ✅ Extracted 3 structured fields (drug, cancer type, criteria) from free-form text using regex
- ✅ Detected age thresholds from **10,836 criteria** using 7 distinct linguistic patterns

### Original Contributions
- ✅ Invented the **Eligibility Barrier Score (EBS)** — a referenceable, reproducible metric for trial restrictiveness
- ✅ Invented the **Patient Reachability Index (PRI)** — an evidence-based estimate of real-world patient qualification rates
- ✅ Conducted a **systematic age bias audit** at scale across oncology trials
- ✅ Quantified the **prior treatment penalty** — the most common real-world disqualifier

### Impact
- ✅ Produced a framework that **regulators, sponsors, and patient advocates** can adopt to evaluate trial inclusivity
- ✅ Generated data-driven evidence for a problem previously discussed only anecdotally in clinical literature
- ✅ Delivered a **reproducible, open-source pipeline** applicable to any eligibility criteria dataset

---

## 🔭 Future Scope

- **Phase comparison** — Do Phase III trials have more restrictive EBS than Phase I/II?
- **Temporal analysis** — Are trials becoming more or less inclusive over time?
- **LLM integration** — Use a language model to extract criteria semantics beyond regex
- **Interactive dashboard** — Plotly Dash app for real-time EBS/PRI lookup by cancer type
- **Benchmark dataset** — Publish a cleaned, structured version of this dataset for the research community

---

## 📄 License

This project is licensed under the MIT License. See `LICENSE` for details.

---

## 🙌 Acknowledgements

- Dataset provided by [@pratikhiremath](https://www.kaggle.com/pratikhiremath) on Kaggle
- Clinical statistics referenced from National Cancer Institute (NCI) and published oncology literature

---

<p align="center">
  <i>Built to make cancer trials more transparent, more inclusive, and more impactful.</i><br>
  <i>Because the patients who need trials the most shouldn't be the ones locked out of them.</i>
</p>
