# Time, Not Task Count: Cost-Aware and Uncertainty-Quantified Optimisation of Digital Handwriting Protocols for Alzheimer's Disease

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Python 3.11+](https://img.shields.io/badge/Python-3.11+-green.svg)](https://www.python.org/)
[![Dataset: DARWIN](https://img.shields.io/badge/Dataset-DARWIN%20(UCI%20732)-orange.svg)](https://archive.ics.uci.edu/dataset/732/darwin)

> **Authors:** Wirapong Chansanam · Khon Kaen University  
> **Status:** Under review  
> **Keywords:** Alzheimer's disease · Digital handwriting · Cost-aware optimisation · Conformal prediction · Nested cross-validation

---

## Overview

This repository contains all source code, data, figures, and supplementary results for the manuscript:

> Chansanam, W. (*in review*). *Time, Not Task Count: Cost-Aware and Uncertainty-Quantified Optimisation of Digital Handwriting Protocols for Alzheimer's Disease.*

The study presents a cost-aware, time-explicit machine learning framework for optimising digital handwriting screening protocols for Alzheimer's disease (AD). Rather than minimising the *number* of tasks, we minimise realised **writing time** while preserving diagnostic discrimination. Three research questions are addressed:

| RQ | Question | Section |
|---|---|---|
| RQ1 | To what extent do evaluation shortcuts (hyperparameter tuning and task-selection leakage) inflate AUROC estimates? | Section D |
| RQ2 | Which protocol minimises writing time while remaining non-inferior in AUROC to the full 25-task reference? | Section A |
| RQ3 | Do reduced protocols provide reliable individual-level uncertainty estimates and clinically useful net benefit under realistic screening prevalences? | Section E |

**Key result:** The recommended Greedy-cost protocol (11 tasks) achieves AUROC = 0.943 ± 0.013 with ΔAUROC = +0.0001 ± 0.0100 relative to the full 25-task protocol, while requiring only **44.9% of the full writing time** (5 × 5-fold repeated nested cross-validation, DARWIN dataset, N = 174).

---

## Repository Structure

```
Digital_Handwriting_Protocols/
│
├── README.md                          # This file
├── LICENSE                            # MIT License
├── requirements.txt                   # Python dependencies
│
├── analysis/
│   └── Phase1_DARWIN_CostAware_Uncertainty.ipynb   # Main analysis notebook
│
├── data/
│   └── data.csv                       # DARWIN dataset (UCI ML Repository ID 732)
│
├── figures/                           # Publication-ready figures (400 dpi, no embedded titles)
│   ├── Figure_1_AUROC_variants_classifiers.png
│   ├── Figure_2_Bayesian_posterior_AUROC_diff.png
│   ├── Figure_3_Pareto_front_AUROC_time.png
│   ├── Figure_4_AUROC_by_protocol_size.png
│   ├── Figure_5_Task_selection_frequency_heatmap.png
│   ├── Figure_6_Reliability_calibration_diagrams.png
│   ├── Figure_7_Conformal_coverage_referral.png
│   ├── Figure_8_Decision_curves_prevalence.png
│   └── Supplementary_S1_Accuracy_coverage_tradeoff.png
│
└── results/                           # Supplementary result tables
    ├── TableS1_leakage_vs_coverage.csv
    ├── TableS2_consensus.csv
    ├── TableS3_consensus_freq.csv
    └── Phase1_results.xlsx            # All tables (D1–D3, A1–A3, E1–E4, S1–S3)
```

---

## Dataset

### DARWIN (Diagnosis AlzheimeR WIth haNdwriting)

| Attribute | Value |
|---|---|
| **Source** | UCI Machine Learning Repository |
| **ID** | 732 |
| **URL** | https://archive.ics.uci.edu/dataset/732/darwin |
| **Participants** | 174 (89 AD, 85 Healthy Controls) |
| **Tasks** | 25 standardised handwriting tasks |
| **Features** | 450 (18 kinematic/temporal/pressure features × 25 tasks) |
| **Key feature** | `total_time{t}` = pen-activity duration per task (milliseconds) |
| **Original citation** | Cilia et al. (2022). *Engineering Applications of Artificial Intelligence, 111*, 104822. |

The file `data/data.csv` is a copy of the original DARWIN dataset obtained from the UCI ML Repository. It is redistributed here under the [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/) licence as specified by the original data contributors. **Please cite the original dataset paper (Cilia et al., 2022) when using this data.**

---

## Analysis Notebook

The main analysis is in `analysis/Phase1_DARWIN_CostAware_Uncertainty.ipynb`.

### Notebook Sections

| Section | Description | Key Outputs |
|---|---|---|
| **0. Setup** | Imports, configuration, caching utilities | — |
| **1. Data Loading** | Load DARWIN, compute task time shares | — |
| **2. Base Functions** | Nested CV helpers, model builders | — |
| **D. Optimism Decomposition** | Fixed / Nested / Best-of-grid variants; Bayesian t-test | Tables D1–D3, Figures 1–2 |
| **A. Cost-Aware Selection** | Greedy-cost, Greedy-AUROC, Mean-F protocols | Tables A1–A3, Figures 3–5 |
| **E. Calibration & Conformal** | Platt scaling, conformal prediction, DCA | Tables E1–E4, Figures 6–8 |
| **S (Synthesis)** | Excel workbook, manuscript draft | `Phase1_results.xlsx` |
| **Phase 1b** | Consensus protocol (stability selection) | Tables S1–S3 |

### Key Configuration

```python
FAST        = False   # Full mode (True = fast test mode)
USE_CACHE   = True    # Load from cache if available
SEED_BASE   = 2026    # Random seed base
N_REPEATS_A = 5       # Repeats for protocol selection (Sections A & Phase 1b)
N_REPEATS_D = 10      # Repeats for optimism decomposition (Section D)
N_REPEATS_E = 5       # Repeats for calibration / conformal (Section E)
N_FOLDS     = 5       # Outer CV folds
N_INNER     = 5       # Inner CV folds
DELTA_NI    = 0.01    # Non-inferiority margin (AUROC)
```

---

## Figures

All figures are generated at **400 dpi** with no embedded "Fig. X" titles (titles are provided as captions in the manuscript). The figures are located in `figures/`.

| File | Description | Corresponds to |
|---|---|---|
| `Figure_1_AUROC_variants_classifiers.png` | Out-of-fold AUROC across evaluation variants and classifiers | Figure 1 |
| `Figure_2_Bayesian_posterior_AUROC_diff.png` | Bayesian posterior of pairwise AUROC differences | Figure 2 |
| `Figure_3_Pareto_front_AUROC_time.png` | Discrimination–time frontier (Pareto front) | Figure 3 |
| `Figure_4_AUROC_by_protocol_size.png` | AUROC as a function of protocol size | Figure 4 |
| `Figure_5_Task_selection_frequency_heatmap.png` | Task-selection frequency heatmap (Greedy-cost) | Figure 5 |
| `Figure_6_Reliability_calibration_diagrams.png` | Reliability diagrams (calibration) | Figure 6 |
| `Figure_7_Conformal_coverage_referral.png` | Conformal prediction coverage and referral rates | Figure 7 |
| `Figure_8_Decision_curves_prevalence.png` | Decision curves under screening prevalences | Figure 8 |
| `Supplementary_S1_Accuracy_coverage_tradeoff.png` | Accuracy–coverage trade-off (selective prediction) | Fig. S1 |

---

## Installation

### Requirements

- Python 3.11+
- See `requirements.txt` for full dependency list

```bash
pip install -r requirements.txt
```

### Running the Notebook

```bash
# Option 1: Jupyter Notebook
jupyter notebook analysis/Phase1_DARWIN_CostAware_Uncertainty.ipynb

# Option 2: Google Colab (recommended for full run)
# Upload the notebook and data.csv, mount Google Drive for outputs
```

> **Estimated runtime (full mode):** 40–90 minutes on CPU (Section A greedy search is the bottleneck). Results are cached in `outputs/cache/` so re-runs load instantly.

> **Fast test mode:** Set `FAST = True` for an 8–12 minute run to verify all cells execute correctly before the full analysis.

---

## Reproducibility

All random operations use deterministic seeds:

```python
SEED_BASE = 2026
# Each repeat r uses seed = SEED_BASE + r
```

Cached outputs (`.pkl` files) are stored in `outputs/cache/` (not included in this repository due to file size; they are regenerated on first run). The cache naming scheme encodes the configuration:

- `A_r{N_REPEATS_A}_m{MAX_TASKS}_full.pkl` — Protocol selection results
- `C_{strategy}_{m}_B{B_CONS}_r{N_REPEATS_A}_full.pkl` — Consensus protocol results
- `E_{strategy}_{m}_v2_full.pkl` — Calibration/conformal results

**Software versions used in the manuscript:**

| Package | Version |
|---|---|
| Python | 3.11.15 |
| scikit-learn | 1.8.0 |
| XGBoost | 3.2.0 |
| statsmodels | 0.14.6 |
| numpy | 2.0.2 |
| pandas | 2.3.3 |
| scipy | 1.13.1 |
| joblib | 1.5.3 |

---

## Data and Code Availability

> **The following statement is provided for use in the Data Availability / Code Availability section of the research manuscript:**

The DARWIN digital handwriting dataset used in this study is publicly available from the UCI Machine Learning Repository (ID 732) at [https://archive.ics.uci.edu/dataset/732/darwin](https://archive.ics.uci.edu/dataset/732/darwin) and is cited as Cilia et al. (2022). A copy of the dataset is also included in this repository (`data/data.csv`) to facilitate exact reproducibility.

All source code for data preprocessing, feature extraction, nested cross-validation, task selection, calibration, conformal prediction, decision curve analysis, and figure generation is available in the Jupyter Notebook `analysis/Phase1_DARWIN_CostAware_Uncertainty.ipynb`. Publication-quality figures (400 dpi, no embedded titles) are provided in the `figures/` directory. Supplementary result tables (Tables S1–S3) and the consolidated results workbook (`Phase1_results.xlsx`) are available in the `results/` directory.

All code and data are deposited at: **https://github.com/wirapong/Digital_Handwriting_Protocols**

---

## Citation

If you use this code, data, or figures in your research, please cite:

```bibtex
@article{chansanam2026time,
  title   = {Time, Not Task Count: Cost-Aware and Uncertainty-Quantified 
             Optimisation of Digital Handwriting Protocols for 
             Alzheimer's Disease},
  author  = {Chansanam, Wirapong},
  journal = {(under review)},
  year    = {2026},
  url     = {https://github.com/wirapong/Digital_Handwriting_Protocols}
}
```

Also cite the original DARWIN dataset:

```bibtex
@article{cilia2022diagnosing,
  title     = {Diagnosing {Alzheimer's} disease from on-line handwriting: 
               A novel dataset and performance benchmarking},
  author    = {Cilia, Nicoletta D. and De Gregorio, Giovanni and 
               De Stefano, Claudio and Fontanella, Francesco and 
               Marcelli, Angelo and Parziale, Antonio},
  journal   = {Engineering Applications of Artificial Intelligence},
  volume    = {111},
  pages     = {104822},
  year      = {2022},
  publisher = {Elsevier},
  doi       = {10.1016/j.engappai.2022.104822}
}
```

---

## Methodological References

Key methodological papers implemented or referenced in this study:

| Method | Reference |
|---|---|
| Bayesian correlated t-test | Corani & Benavoli (2015). *Machine Learning, 100*(2–3), 285–304. |
| Corrected resampled t-test | Nadeau & Bengio (2003). *Machine Learning, 52*(3), 239–281. |
| Feature selection stability (Φ) | Nogueira, Sechidis & Brown (2018). *JMLR, 18*, 1–54. |
| Conformal prediction | Barber, Candès, Ramdas & Tibshirani (2023). *Annals of Statistics, 51*(2), 816–845. |
| Stability selection | Meinshausen & Bühlmann (2010). *JRSS-B, 72*(4), 417–473. |
| Decision curve analysis | Vickers & Elkin (2006). *Medical Decision Making, 26*(6), 565–574. |

---

## Contact

**Wirapong Chansanam**  
Department of Computer Science, Khon Kaen University  
Email: wirach@kku.ac.th  
GitHub: [@wirapong](https://github.com/wirapong)

---

*Last updated: October 2026*
