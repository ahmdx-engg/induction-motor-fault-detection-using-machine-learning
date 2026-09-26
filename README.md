# Induction Motor Fault Detection using Machine Learning (MATLAB)

A MATLAB signal processing and machine learning pipeline that detects three of the most common induction motor faults — **bearing faults**, **stator winding faults**, and **broken rotor bars** — from vibration and current signals, and classifies each into its specific fault type.

Each fault category lives in its own folder with its own README covering that category's data and code in detail. **This README is the entry point**: what the project does, how the three parts relate, and where to look next.

---

## Table of Contents

- [Overview](#overview)
- [Repository Structure](#repository-structure)
- [Fault Categories](#fault-categories)
- [Common Pipeline](#common-pipeline)
- [Results Summary](#results-summary)
- [Requirements](#requirements)
- [How to Run](#how-to-run)
- [Project Report and Demo](#project-report-and-demo)
- [Notes for Reproducing on Your Own Machine](#notes-for-reproducing-on-your-own-machine)
- [Team](#team)
- [References](#references)

---

## Overview

Over 90% of industrial electric drives are induction motors, and unplanned failures are costly. This project builds a **predictive maintenance pipeline** that:

1. Processes raw vibration and current signals
2. Extracts diagnostic features using MATLAB's **Diagnostic Feature Designer**
3. Trains classifiers (evaluated in MATLAB's **Classification Learner**) to separate healthy operation from specific fault types

...across three distinct fault mechanisms.

---

## Repository Structure

```
.
├── bearing-fault-detection/                  # Inner/outer race and roller element bearing faults (KNN, CSV features)
├── rotor-fault-detection/                    # Broken rotor bar detection (Diagnostic Feature Designer pipeline)
├── winding-fault-detection/                  # Stator inter-turn short circuit detection
├── project-report.pdf                        # Full write-up: datasets, methodology, results per fault type
├── Project demonstration by Ahmed Razi.mp4   # Short video walkthrough
└── README.md                                 # You are here
```

Each subfolder has its own `Readme.md` describing its specific dataset, scripts, and usage. See [Fault Categories](#fault-categories) below for a summary and links.

---

## Fault Categories

### 🔧 Bearing Faults
**[`bearing-fault-detection/`](./bearing-fault-detection)**

Detects inner race, outer race, and roller element faults from vibration data, based on the IMS bearing run-to-failure dataset (4 bearings, 2000 RPM, 20 kHz sampling, 1-second snapshots). Features are extracted per file, merged into a single table, and classified with KNN.

📄 See [`bearing-fault-detection/Readme.md`](./bearing-fault-detection/Readme.md) for exact data import and prediction commands.

### ⚙️ Broken Rotor Bar Faults
**[`rotor-fault-detection/`](./rotor-fault-detection)**

Detects 0 to 4 broken rotor bars from three-phase current and radial vibration signals, across multiple load levels. Uses a `fileEnsembleDatastore` to organize raw experiment files, then a band-pass filter (900–1300 Hz) and envelope analysis feed the Diagnostic Feature Designer pipeline (`diagnosticFeatures.m`) before classification.

📄 See [`rotor-fault-detection/Readme.md`](./rotor-fault-detection/Readme.md).

### ⚡ Stator Winding Faults
**[`winding-fault-detection/`](./winding-fault-detection)**

Detects inter-turn short circuit faults at different severity and load levels from three-phase current signals (2,240 recorded conditions across 6 defect classes and 3 load levels).

📄 See [`winding-fault-detection/Readme.md`](./winding-fault-detection/Readme.md).

---

## Common Pipeline

All three fault categories follow the same general workflow:

| Stage | Description |
|---|---|
| **1. Data organization** | Raw recordings are grouped (via `fileEnsembleDatastore` or merged tables) and tagged with their health condition. |
| **2. Signal processing** | A band-pass filter isolates the fault-relevant frequency band, followed by envelope analysis; the Wavelet Toolbox is used for scalogram visualization. |
| **3. Feature extraction** | MATLAB's Diagnostic Feature Designer app auto-generates candidate features and ranks them with one-way ANOVA. |
| **4. Model training** | Top features are exported to the Classification Learner app, where multiple models (KNN, decision trees, SVM, ensembles) are trained with cross-validation and compared. |
| **5. Evaluation** | The best model is assessed with a confusion matrix on held-out test data. |

---

## Results Summary

> Figures below are quoted from `project-report.pdf` per fault category. If you rerun the pipelines, use your own confusion matrix output as the source of truth, and update this table (and each subfolder's README) accordingly.

| Fault Category | Best Model | Reported Accuracy |
|---|---|---|
| Bearing (inner/outer race, roller element) | KNN | ~95.2–95.7% |
| Stator winding (inter-turn short circuit) | Decision Tree / KNN | ~95% |
| Broken rotor bar | Decision Tree | ~96% |

See `project-report.pdf` for full confusion matrices and discussion.

---

## Requirements

- MATLAB R2023a or newer
- Signal Processing Toolbox
- Wavelet Toolbox
- Statistics and Machine Learning Toolbox
- Predictive Maintenance Toolbox

---

## How to Run

Each fault category is run independently. **Open the relevant subfolder's README first**, since the exact scripts and file names differ per category.

**Bearing:**
Open `bearing-fault-detection/Readme.md`, import the listed CSVs, merge them, and run through Classification Learner as described — or load `KNN_Model_Session.mat` directly to reuse the already-trained model.

**Rotor:**
Open `rotor-fault-detection/Readme.md` and run `razi.m` to build the ensemble datastore, then `diagnosticFeatures.m` for feature extraction.

**Winding:**
Open `winding-fault-detection/Readme.md`. `trainedmodel.mat` and `testdata.mat` let you reproduce predictions without retraining, or use `test.m` directly.

---

## Notes for Reproducing on Your Own Machine

- Some scripts (e.g., `Files_Import.m`, code snippets in the sub-READMEs) use hardcoded Windows paths such as `D:\EM_Project...`. Update these to your own dataset location before running.
- The pretrained models (`KNN_Model_Session.mat`, `trainedmodel.mat`) let you skip straight to prediction without retraining, if you just want to see the pipeline work end to end.
- Large raw datasets (e.g., the IMS bearing test-to-failure files under `bearing-fault-detection/test-data/`) are experiment recordings referenced by the scripts — see that folder's own README for what each file represents.

---


