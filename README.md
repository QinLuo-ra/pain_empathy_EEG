# pain_empathy_EEG
# Pain Empathy EEG Analysis — ASD vs TD Children

Analysis code for:

> Luo Q, et al. (2025). Dissociable neural stages of pain empathy reveal early theta-linked symptom variation in autism. .

---

## Overview

This repository contains scripts for all EEG preprocessing, time-frequency analysis, functional connectivity, Bayesian regression, and figure generation reported in the manuscript. Analyses were conducted on EEG data from 76 autistic (ASD) and 76 typically developing (TD) children during passive observation of painful and neutral actions.

---

## Software requirements

| Software | Version |
|----------|---------|
| MATLAB | R2024a |
| EEGLAB | v12.01 |
| R | ≥ 4.2 |
| JASP | 0.96 |

R packages required: `BayesFactor`, `ggplot2`, `ggdist` (for raincloud plots). Install with:

```r
install.packages(c("BayesFactor", "ggplot2", "ggdist"))
```

---

## Repository structure

```
├── README.md
├── preprocessing/
│   └── EEG_preprocessing_EEGLAB.m       # Filtering, ICA, epoch rejection
├── ERSP_analysis/
│   ├── compute_theta_ERSP.m             # Theta (3–6 Hz, 180–280 ms) ERSP extraction
│   └── compute_alpha_ERSP.m             # Alpha (7–10 Hz, 400–800 ms) ERSP extraction
├── connectivity/
│   └── compute_dwPLI.m                  # Debiased weighted phase lag index
├── ERP_analysis/
│   └── compute_ERP_components.m         # P2, N2, LPP extraction
├── bayesian_regression/
│   ├── bayesian_regression_theta_ERSP_CARS.R
│   ├── bayesian_regression_alpha_ERSP_CARS.R
│   ├── bayesian_regression_dwPLI_CARS.R
│   └── bayesian_regression_ERP_CARS.R

```

---

## Data

Raw EEG recordings and individual-level clinical data are not publicly available (pediatric clinical participants; ethics approval and informed consent do not permit unrestricted sharing). Access to de-identified derived data may be requested from the corresponding author, subject to institutional ethics approval and a data-use agreement.

The Bayesian regression scripts expect a CSV file with the following columns:

| Column | Description |
|--------|-------------|
| `CARS` | Childhood Autism Rating Scale total score |
| `Age_mo` | Age in months |
| `FSIQ` | Full-Scale IQ (WISC-IV abbreviated) |
| `gender.scale` | Gender (effect-coded: female = −0.5, male = 0.5) |
| `T_fz_df` | Frontal theta ERSP pain − neutral (dB) |
| `T_cz_df` | Central theta ERSP pain − neutral (dB) |
| `A_cz_df` | Central alpha ERSP pain − neutral (dB) |
| `A_pz_df` | Parietal alpha ERSP pain − neutral (dB) |
| `dwPLI_FP_alpha_df` | Fronto-parietal alpha dwPLI pain − neutral |

---

## How to run

**1. EEG preprocessing** (MATLAB + EEGLAB)

```matlab
% Add EEGLAB to path, then run:
run preprocessing/EEG_preprocessing_EEGLAB.m
```

**2. Extract neural indices** (MATLAB)

```matlab
run ERSP_analysis/compute_theta_ERSP.m
run ERSP_analysis/compute_alpha_ERSP.m
run connectivity/compute_dwPLI.m
run ERP_analysis/compute_ERP_components.m
```

**3. Bayesian regression** (R or JASP)

Load the derived data CSV, then run each script in `bayesian_regression/`. The key model for theta ERSP:

```r
source("bayesian_regression/bayesian_regression_theta_ERSP_CARS.R")
```

All models use JZS prior (r = 0.354), BF10, betaBinomial model prior. Set `setSeed = TRUE` for exact replication.

---

## Correspondence

Shuo Zhao — [zhaoshuo09@gmail.com]  
[Shenzhen University]

