# HR Estimation from Motion-Corrupted PPG (Systematic Review + Benchmark)

Benchmarking open-source algorithms for **heart rate (HR) estimation** from **wrist PPG** under **real-world motion artifacts**, using a standardized evaluation protocol.

> Companion material for the related journal article.

---

## What’s in here

This repository supports a study that:
- **Systematically reviewed** published PPG+accelerometer HR-estimation algorithms (**N = 126**).
- **Benchmarked** all methods with available open-source code (**N = 11**) on a **shared real-world dataset**, using **standardized metrics and a subject-independent evaluation scheme**. 

---

## Reference publication

**Sicbaldi M., Di Florio P., Moscato S., Palmerini L., Silvani A., Chiari L.**  
“Benchmarking of open-source algorithms for heart rate estimation from motion-corrupted photoplethysmography”  
*Computers in Biology and Medicine* 196 (2025): 110808.  
DOI: 10.1016/j.compbiomed.2025.110808 :contentReference[oaicite:2]{index=2}

---

## Dataset

Benchmark dataset: **PPG-DaLiA** (15 healthy subjects; ~36 hours total).  
- Reference HR from **chest ECG** (Respiban Professional) sampled at **700 Hz**  
- Wrist device: **Empatica E4**
  - PPG: **64 Hz**
  - Accelerometer: **32 Hz** :contentReference[oaicite:3]{index=3}

Activities include sitting, stairs, walking, cycling, etc. (i.e., *everyday motion conditions*). :contentReference[oaicite:4]{index=4}

---

## Algorithms benchmarked (open-source)

This benchmark includes:
- **Classical/model-based approaches** (motion-aware spectral tracking, adaptive filtering, etc.)
- **Deep learning approaches** (multi-modal PPG+ACC networks)
- Two strong **beat-detector baselines** that *do not* explicitly correct motion artifacts (included to quantify the benefit of accelerometer-based correction). 

> Full list + brief descriptions appear in the chapter’s Table 3.I. :contentReference[oaicite:6]{index=6}

---

## Evaluation protocol (high-level)

### Windowing
Signals are evaluated in **sliding windows**:
- window length **T = 8 s**
- step **S = 2 s** :contentReference[oaicite:7]{index=7}

### Subject-independent evaluation
Performance is reported in a **subject-independent** manner (e.g., leave-one-subject-out), to better reflect generalization to unseen individuals. :contentReference[oaicite:8]{index=8}

### Metrics (why not just MAE?)
Alongside commonly reported MAE, the benchmark emphasizes metrics that separate:
- **Bias** (median error)
- **Variability** (IQR of error)
- **Monotonic agreement** (Spearman’s ρ vs ECG HR) 

---

## Signal-quality stratification (motion artifacts)

To quantify robustness under motion, the study also stratifies windows by **PPG signal quality** using a published SQI method extended from pulse-level to window-level via majority voting. 

---

## Key findings (headline results)

- **Deep learning algorithms** consistently outperformed classical/model-based approaches, especially in **dynamic activities** with substantial motion artifacts. 
- Among motion-aware methods, **BeliefPPG** achieved the best overall performance:
  - bias: **0.7 ± 0.8 bpm**
  - variability: **4.4 ± 2.0 bpm**
  - Spearman’s ρ: **0.73 ± 0.14** 
- Classical methods were notably less robust in **low-quality PPG windows**, while BeliefPPG remained comparatively stable. 

---

## Repository structure (suggested)

You can adapt this to your actual codebase:

```text
.
├─ data/                      # (optional) dataset pointers / scripts (no raw data)
├─ configs/                   # run configs
├─ src/                       # core pipeline (preprocess, windows, metrics)
├─ algorithms/                # wrappers/adapters for each method
├─ notebooks/                 # exploration + figures
├─ results/                   # generated outputs (excluded from version control if large)
└─ scripts/
   ├─ prepare_dataset.*
   ├─ run_benchmark.*
   └─ make_figures.*
