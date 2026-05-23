# Endovascular Treatment for Medium and Distal Vessel Occlusion Strokes

[![R Version](https://img.shields.io/badge/R-%E2%89%A54.4.2-blue.svg)](https://cran.r-project.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Overview
This repository contains the complete analytical pipeline and replication code for the manuscript: **"Endovascular Treatment for Medium and Distal Vessel Occlusion Strokes: A Systematic Review and Frequentist–Bayesian Meta-Analysis of Randomised Controlled Trials."** The scripts provided here execute a dual-methodology synthesis (Frequentist and Bayesian) of summary data from dedicated randomized controlled trials (RCTs) comparing Endovascular Treatment (EVT) versus Best Medical Treatment (BMT) for MDVO stroke.

### Evaluated Clinical Endpoints
1. **Functional Independence** (Modified Rankin Scale 0–2 at 90 days)
2. **Symptomatic Intracranial Hemorrhage** (sICH)
3. **All-Cause Mortality** (ACM at 90 days)

---

## Methodological Highlights

### Frequentist Framework
* **Estimator:** Restricted Maximum Likelihood (REML) for between-study heterogeneity ($\tau^2$).
* **Variance Adjustment:** Hartung-Knapp-Sidik-Jonkman (HK) adjustment to account for small-study effects ($k=3$).
* **Sensitivity:** Built-in comparisons of $\tau^2$ estimates across REML, DerSimonian–Laird (DL), and Paule–Mandel (PM) estimators.
* **Outputs:** Odds Ratios (OR), 95% Confidence Intervals, Prediction Intervals, and Leave-one-out sensitivity analyses.

### Bayesian Framework
* **Prior Selection:** Utilizes objective, empirical priors mapped to the specific clinical nature of the endpoints using the `TurnerEtAlPrior` function (e.g., mapping sICH to "adverse events" and mRS to "quality of life / functioning").
* **Sensitivity Analyses:** Compares the informative Turner prior against weakly informative Half-Normal priors.
* **Evidence Quantification:** Calculates exact Savage-Dickey density ratios to generate Bayes Factors ($BF_{10}$ and $BF_{01}$), providing robust quantification of evidence for the null versus alternative hypotheses.
* **Outputs:** Posterior distributions, 95% Credible Intervals (CrI), and cumulative density function (CDF) probabilities for clinically meaningful benefit/harm thresholds.

---

## Repository Structure

* `Frequentist_Analysis.R` - Executes the REML/HK frequentist models and generates dynamic forest plots.
* `Bayesian_Analysis.R` - Executes the Bayesian random-effects models, posterior probability calculations, and distribution visualizations.
* `Data_Template.xlsx` - (Example) The required structure for the summary data input.

---

## Prerequisites and Installation

The scripts are highly automated and feature built-in package management. Upon running the scripts, the `pacman` package will automatically install and load the necessary dependencies. 

**Core Dependencies:**
* `meta` (Locked to version 8.3-0 for reproducibility)
* `bayesmeta`
* `BayesianPriorsTool` (Installed via GitHub)
* `tidyverse` (Data wrangling and `ggplot2`)
* `rstudioapi` & `readxl` (Interactive data loading)

*Note: Ensure you have an active internet connection on the first run so the script can fetch any missing packages from CRAN and GitHub.*

---

## Usage Instructions

Both the Frequentist and Bayesian scripts utilize a "Single-Switch Control Panel" to automate analysis across different endpoints.

**Step 1: Run the Setup Chunk**
Execute blocks `#001` and `#002` to configure the environment and load dependencies.

**Step 2: Import Data and Select Output**
Execute blocks `#003.1` through `#003.3`. 
* A prompt will appear asking you to select your Excel data file.
* A second prompt will ask you to designate a folder where all generated high-resolution plots (.png) will be saved.

**Step 3: Define the Active Outcome**
Navigate to block `#004`. Change the `active_outcome` string to the endpoint you wish to analyze. The script will automatically route the correct data sheet, plot labels, and clinical directionality (e.g., ensuring OR > 1 correctly favors EVT for mRS, but favors BMT for mortality).

```R
# Set ONLY this value before running the analysis pipeline
active_outcome <- "sich"   # options: "mrs", "acm", "sich"
