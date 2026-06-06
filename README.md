# EEG-Based Mental Workload Classification Using Statistical Feature Engineering and Ensemble Machine Learning

## Overview

Mental workload assessment plays a critical role in human-computer interaction, cognitive neuroscience, healthcare, and brain-computer interface applications. This project presents an EEG-based mental workload classification framework that distinguishes between resting-state and mental arithmetic tasks using statistical feature engineering and machine learning techniques.

The proposed system utilizes EEG recordings from the EEGMAT dataset and extracts frequency-domain features from multiple EEG channels. Statistical feature selection using the Wilcoxon Rank-Sum Test is employed to identify discriminative features, followed by classification using several machine learning algorithms.

Among all evaluated models, the Stacking Classifier achieved the highest performance, demonstrating the effectiveness of ensemble learning for EEG-based mental workload detection.

---

## Dataset

### EEGMAT Dataset

Dataset Source:

https://physionet.org/content/eegmat/1.0.0/

Reference:

Zyma I., Tukaev S., Seleznov I., Kiyono K., Popov A., Chernykh M., Shpenkov O.

"Electroencephalograms During Mental Arithmetic Task Performance"

Data, 2019, 4(1), 14.

### Dataset Characteristics

- Total Subjects: 36
- EEG Channels: 19
- Recording Duration: 60 seconds
- Sampling Frequency: 1000 Hz
- EEG Bands:
  - Delta (0.5–4 Hz)
  - Theta (4–8 Hz)
  - Alpha (8–13 Hz)
  - Beta (13–30 Hz)
  - Gamma (30–45 Hz)

### EEG Channels

- Fp1
- Fp2
- F3
- F4
- F7
- F8
- T3
- T4
- C3
- C4
- T5
- T6
- P3
- P4
- O1
- O2
- Fz
- Cz
- Pz

### Classification Classes

| Label | Class |
|---------|---------|
| 0 | Resting State |
| 1 | Mental Arithmetic |

---

## Methodology

### 1. EEG Preprocessing

The EEG recordings were preprocessed before feature extraction.

Preprocessing included:

- Artifact removal using Independent Component Analysis (ICA)
- Noise reduction
- Segmentation into analysis windows
- Preparation for frequency-domain analysis

---

### 2. Feature Extraction

Power Spectral Density (PSD) was estimated using Welch's Method.

For every EEG channel, band powers were extracted from:

- Delta Band
- Theta Band
- Alpha Band
- Beta Band
- Gamma Band

Total Base Features:

95 Features

(19 Channels × 5 Frequency Bands)

---

### 3. Feature Engineering

Additional biologically meaningful features were generated.

#### Absolute Band Power Features

Measure the strength of neural oscillatory activity within a specific frequency band.

Examples:

- Fp1_alpha
- Cz_beta
- Pz_theta

#### Relative Power Features

Measure the contribution of a frequency band relative to total brain activity.

Examples:

- Alpha Relative Power
- Beta Relative Power
- Theta Relative Power

Formula:

Relative Power = Band Power / Total Power

#### Ratio Features

Capture relationships between multiple brain rhythms.

Examples:

- Theta / Alpha Ratio
- Beta / Alpha Ratio
- (Theta + Beta) / Alpha Ratio
- Gamma / Beta Ratio
- Alpha / Delta Ratio
- Beta Ratio

#### Difference Features

Measure dominance of one cognitive state over another.

Examples:

- Beta − Alpha
- Theta − Beta

Total Engineered Features:

More than 300 EEG Features

---

## Statistical Feature Selection

To identify the most discriminative EEG features, a non-parametric statistical test was employed.

### Wilcoxon Rank-Sum Test

The Wilcoxon Rank-Sum Test compares feature distributions between:

- Resting State
- Mental Arithmetic

Features satisfying:

p-value < 0.01

were considered statistically significant.

### Results

Total Significant Features Selected:

252 Features

---

## Top Significant Features

The most discriminative EEG features identified were:

- Cz_beta_rel
- Cz_beta_alpha
- Cz_beta
- Cz_beta_ratio
- Cz_theta_beta_alpha
- P3_beta_rel
- P3_beta
- P3_beta_alpha
- P3_beta_ratio
- P3_theta_beta_alpha
- Fp1_alpha
- Fp1_alpha_delta
- Fp1_alpha_rel
- F4_beta_minus_alpha
- C3_beta_minus_alpha

---

## Biological Interpretation

The selected features are consistent with neuroscience findings regarding cognitive workload.

### Beta Activity

Increased beta activity is associated with:

- Active concentration
- Attention
- Mental effort
- Information processing

### Theta Activity

Theta activity reflects:

- Working memory
- Cognitive control
- Mental calculation processes

### Alpha Activity

Alpha activity generally decreases during:

- Mental workload
- Task engagement
- Focused attention

### Theta-Beta and Beta-Alpha Ratios

These ratios quantify the balance between:

- Cognitive processing
- Relaxation
- Attention

making them effective workload biomarkers.

---

## Machine Learning Models Evaluated

The following models were trained and compared:

1. Logistic Regression
2. Support Vector Machine (SVM)
3. Decision Tree
4. Random Forest
5. AdaBoost
6. XGBoost
7. CatBoost
8. Stacking Classifier

Class imbalance was handled using:

```python
class_weight='balanced'
```

where applicable.

---

## Best Performing Model

### Stacking Classifier

Base Learners:

- Random Forest
- XGBoost
- CatBoost

Meta Learner:

- Logistic Regression

The stacking model combines predictions from multiple strong learners and uses Logistic Regression to make the final decision.

---

## Performance Comparison

| Model | Accuracy | Precision | Recall | F1-Score | ROC-AUC |
|---------|---------|---------|---------|---------|---------|
| Logistic Regression | 75.80% | 41.22% | 80.79% | 54.59% | 83.81% |
| SVM | 85.22% | 55.42% | 91.39% | 69.00% | 94.00% |
| Decision Tree | 83.43% | 54.23% | 50.99% | 52.56% | 70.77% |
| Random Forest | 89.27% | 96.92% | 41.72% | 58.33% | 96.67% |
| AdaBoost | 85.46% | 72.31% | 31.13% | 43.52% | 86.93% |
| XGBoost | 92.97% | 94.23% | 64.90% | 76.86% | 97.51% |
| CatBoost | 90.58% | 92.86% | 51.66% | 66.38% | 96.82% |
| Stacking Classifier | 94.28% | 88.15% | 78.81% | 83.22% | 97.73% |

---

## Evaluation Metrics

The models were evaluated using:

- Accuracy
- Precision
- Recall
- F1-Score
- ROC-AUC Score
- Confusion Matrix
- ROC Curve

---

## Visualizations

The project includes:

### Feature Analysis

- Boxplots of Significant Features
- Line Plots
- Channel-wise Feature Significance

### Statistical Analysis

- Wilcoxon Rank-Sum Test Results
- P-value Analysis

### Model Evaluation

- Confusion Matrices
- ROC Curves
- Performance Comparison Charts

### Correlation Analysis

- Feature Correlation Heatmaps

---

## Technologies Used

### Programming Language

- Python

### Libraries

- NumPy
- Pandas
- SciPy
- Scikit-Learn
- XGBoost
- CatBoost
- Matplotlib
- Seaborn
- OpenPyXL

---

## Repository Structure

```text
EEG-Mental-Workload-Classification
│
├── data
│
├── src
│   ├── preprocess.py
│   ├── feature_extraction.py
│   ├── windowing.py
│   └── build_dataset.py
│
├── notebooks
│
├── results
│   ├── confusion_matrices
│   ├── roc_curves
│   └── visualizations
│
├── report
│   └── IEEE_Paper.pdf
│
├── requirements.txt
│
└── README.md
```

---

## Future Work

Potential extensions include:

- Deep Learning Models
- CNN-Based EEG Classification
- LSTM-Based Temporal Modeling
- Transformer Architectures
- Real-Time Mental Workload Monitoring
- Brain-Computer Interface Integration

---

## Citation

If you use this work, please cite:

```bibtex
@article{zyma2019eegmat,
  title={Electroencephalograms During Mental Arithmetic Task Performance},
  author={Zyma, Igor and Tukaev, Sergii and Seleznov, Ivan and others},
  journal={Data},
  volume={4},
  number={1},
  pages={14},
  year={2019}
}
```

---

## Authors

EEG-Based Mental Workload Classification Project

Developed using statistical feature engineering, neuroscience-inspired EEG biomarkers, and ensemble machine learning techniques.
