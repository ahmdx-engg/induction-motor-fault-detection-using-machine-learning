Induction Motor Fault Detection using Machine Learning
MATLAB ML

A MATLAB signal processing and machine learning pipeline that detects three of the most common induction motor faults, namely bearing faults, stator winding faults, and broken rotor bars, from vibration and current signals, and classifies each into its specific fault type.

This repository accompanies the paper "Detection of Faults in an Induction Motor using Signal Processing and Machine Learning" (project-report.pdf), and includes a short demo video (Project demonstration by Ahmed Razi.mp4).

Each fault category lives in its own folder with its own README covering that category's data and code in detail. This README is the entry point: what the project does, how the three parts relate, and where to look next.

Table of Contents
Overview
Repository Structure
Fault Categories
Common Pipeline
Results Summary
Requirements
How to Run
Project Report and Demo
Notes for Reproducing on Your Own Machine
Team
References
Overview
Over 90% of industrial electric drives are induction motors, and unplanned failures are costly. This project builds a predictive maintenance pipeline. It processes raw vibration and current signals, extracts diagnostic features with MATLAB's Diagnostic Feature Designer, and trains classifiers (evaluated in MATLAB's Classification Learner) to separate healthy operation from specific fault types, for three different fault mechanisms.

Repository Structure
.
├── bearing-fault-detection/     Inner/outer race and roller element bearing faults (KNN, CSV features)
├── rotor-fault-detection/       Broken rotor bar detection (Diagnostic Feature Designer pipeline)
├── winding-fault-detection/     Stator inter turn short circuit detection
├── project-report.pdf           Full write up: datasets, methodology, results per fault type
├── Project demonstration by Ahmed Razi.mp4   Short video walkthrough
└── README.md                    You are here
Each subfolder has its own Readme.md describing its specific dataset, scripts and usage. See Fault Categories below for a summary and links.

Fault Categories
1. Bearing Faults bearing-fault-detection/
Detects inner race, outer race, and roller element faults from vibration data (based on the IMS bearing run to failure dataset: 4 bearings, 2000 RPM, 20 kHz sampling, 1 second snapshots). Features are extracted per file, merged into a single table, and classified with KNN. See bearing-fault-detection/Readme.md for the exact data import and prediction commands.

2. Broken Rotor Bar Faults rotor-fault-detection/
Detects 0 to 4 broken rotor bars from three phase current and radial vibration signals, across multiple load levels. Uses a fileEnsembleDatastore to organize raw experiment files, then a band pass filter (900 to 1300 Hz) and envelope analysis feed the Diagnostic Feature Designer pipeline (diagnosticFeatures.m) before classification. See rotor-fault-detection/Readme.md.

3. Stator Winding Faults winding-fault-detection/
Detects inter turn short circuit faults at different severity and load levels from three phase current signals (2240 recorded conditions across 6 defect classes and 3 load levels). See winding-fault-detection/Readme.md.

Common Pipeline
All three fault categories follow the same general workflow:

Data organization: raw recordings are grouped (via fileEnsembleDatastore or merged tables) and tagged with their health condition.
Signal processing: a band pass filter isolates the fault relevant frequency band, followed by envelope analysis; the Wavelet Toolbox is used for scalogram visualization.
Feature extraction: MATLAB's Diagnostic Feature Designer app auto generates candidate features and ranks them with one way ANOVA.
Model training: top features are exported to the Classification Learner app, where multiple models (KNN, decision trees, SVM, ensembles) are trained with cross validation and compared.
Evaluation: the best model is assessed with a confusion matrix on held out test data.
Results Summary
(from project-report.pdf; see that document for full confusion matrices and discussion)

Fault Category	Best Model	Reported Accuracy
Bearing (inner/outer race, roller element)	KNN	about 95.2 to 95.7%
Stator winding (inter turn short circuit)	Decision tree / KNN	about 95%
Broken rotor bar	Decision tree	about 96%
Note: figures above are quoted from the project report per fault category. If you rerun the pipelines, use your own confusion matrix output as the source of truth, and update this table (and each subfolder's README) accordingly.

Requirements
MATLAB R2023a or newer
Signal Processing Toolbox
Wavelet Toolbox
Statistics and Machine Learning Toolbox
Predictive Maintenance Toolbox
How to Run
Each fault category is run independently. Open the relevant subfolder's README first, since the exact scripts and file names differ per category:

Bearing: open bearing-fault-detection/Readme.md, import the listed CSVs, merge them, and run through Classification Learner as described, or load KNN_Model_Session.mat directly to reuse the already trained model.
Rotor: open rotor-fault-detection/Readme.md and run razi.m to build the ensemble datastore, then diagnosticFeatures.m for feature extraction.
Winding: open winding-fault-detection/Readme.md. trainedmodel.mat and testdata.mat let you reproduce predictions without retraining, or use test.m directly.
Project Report and Demo
Full write up: project-report.pdf, covering dataset descriptions, experimental setup, feature extraction details and per fault conclusions for all three categories.
Demo video: Project demonstration by Ahmed Razi.mp4, a short walkthrough of the project in action.
Notes for Reproducing on Your Own Machine
Some scripts (for example Files_Import.m, the sub README code snippets) use hardcoded Windows paths such as D:\EM_Project\.... Update these to your own dataset location before running.
The pretrained models (KNN_Model_Session.mat, trainedmodel.mat) let you skip straight to prediction without retraining, if you just want to see the pipeline work end to end.
Large raw datasets (for example the IMS bearing test to failure files under bearing-fault-detection/test-data/) are experiment recordings referenced by the scripts; see that folder's own README for what each file represents.
