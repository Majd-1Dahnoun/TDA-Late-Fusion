# TDA-Late-Fusion

Augmenting Convolutional Neural Networks with Topological Data Analysis for Mammographic Microcalcification Classification.

## Overview
This repository contains the experimental pipeline and evaluation code for the **CM3070 Final Project** (BSc Computer Science, University of London / Goldsmiths).

The project investigates whether multimodal feature fusion of **Cubical Persistent Homology (TDA)** and **EfficientNetB0 (CNN)** improves microcalcification classification on the **CBIS-DDSM** dataset. Evaluated across a four-model ablation study (M1–M4), the results demonstrate that late-stage offline TDA fusion introduces representational noise that degrades overall discrimination and creates a structural bias penalizing benign localization.

## Repository Structure
```
TDA-Late-Fusion/
├── data/                  # CBIS-DDSM data directory
├── manifests/             # Preprocessed patient-stratified split manifests
├── notebooks/             # Main execution notebook (CM3070_Ablation_Code.ipynb)
├── reports/               # Output metrics, ROC plots, and Grad-CAM maps
├── environment.yml        # Conda environment specification (Python 3.10)
├── .gitignore
└── README.md
```
## Model Configurations
M1 (Custom CNN): 3 block convolutional baseline trained from scratch.

M2 (EfficientNetB0 Baseline): Transfer learning model using pre-trained ImageNet weights and Global Average Pooling.

M3 (TDA Fusion): Concatenation of the 1,280 D CNN embedding with an 800 D domain normalized GUDHI persistence image, feeding an L2-regularized lambda = 0.01 64 unit bottleneck head.

M4 (Null Input Control): Identical architecture to M3, replacing the 800 D TDA input with an all zero vector to isolate head capacity effects from topological information.

## Key Results

Multi Seed Performance (189 Test ROIs; Mean ± Std):

M2 (EfficientNetB0 Baseline): AUC 0.7578 ± 0.0109 | Sensitivity 0.7706 ± 0.0865 | Specificity 0.5952 ± 0.0849 | F1 0.6523 ± 0.0193

M4 (Null Input Control): AUC 0.7246 ± 0.0225 | Sensitivity 0.7013 ± 0.0909 | Specificity 0.5923 ± 0.1116 | F1 0.6109 ± 0.0357

M3 (TDA Fusion): AUC 0.7050 ± 0.0141 | Sensitivity 0.7879 ± 0.0738 | Specificity 0.5119 ± 0.1035 | F1 0.6307 ± 0.0057

M1 (From scratch CNN): AUC 0.6529 ± 0.0060 | Sensitivity 0.7186 ± 0.0865 | Specificity 0.4613 ± 0.0979 | F1 0.5733 ± 0.0174

Interpretability (75th Percentile Grad-CAM mIoU):

Malignant Subset: M3 (0.2679) > M2 (0.2440) > M4 (0.2109)

Benign Subset: M4 (0.4058) > M2 (0.3706) > M3 (0.3259)

All Lesions: M4 (0.3264) > M2 (0.3190) > M3 (0.3023)

## Setup & Reproduction

Clone the repository:
git clone https://github.com/Majd-1Dahnoun/TDA-Late-Fusion.git
cd TDA-Late-Fusion

Create and activate the environment:
conda env create -f environment.yml
conda activate cm3070-env

Data Acquisition:
Download the CBIS-DDSM Calcification dataset from The Cancer Imaging Archive (TCIA).
Place the extracted scan folders and metadata files (calc_case_description_train_set.csv, calc_case_description_test_set.csv) inside data/.   
Run notebooks/CM3070_Ablation_Code.ipynb.

Academic Context
Author: Majd Dahnoun

Degree: BSc Computer Science (Artificial Intelligence and Machine Learning), University of London / Goldsmiths

Module: CM3070 Final Project
