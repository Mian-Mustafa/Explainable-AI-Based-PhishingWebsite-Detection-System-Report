# Explainable AI-Based Phishing Website Detection System

**A Human-Centred Machine Learning and SHAP-Based Framework for Non-Technical Users**

---

| Field | Details |
|---|---|
| **Author** | Ghulam Mustafa (F2023266976) |
| **Degree** | BS Computer Science |
| **Course** | Human–Computer Interaction |
| **Supervisor** | Ms. Iqra Khalil |
| **Institution** | University of Management and Technology, School of Systems and Technology |
| **Submission** | June 2026 |

---

## Abstract

Phishing websites continue to be a dangerous cybersecurity threat, mimicking trusted services to steal credentials and personal data. While machine learning can detect phishing patterns with high accuracy, most classifiers are black-box systems that provide little interpretable output — meaning a non-technical user may ignore or distrust an unexplained warning.

This study develops a **human-centred, explainable-AI framework** for phishing website detection. Six supervised classifiers were trained on the Web Page Phishing Detection dataset (11,430 balanced instances, 87 features). **XGBoost** achieved the best performance: **96.63% accuracy**, **96.65% F1-score**, and **99.44% ROC-AUC**. Global feature importance and SHAP analysis identified the most influential signals, which were then translated into plain-language warnings within a proposed interface mock-up. An eight-item Likert instrument and structured response workbook were designed to support future empirical usability testing.

**Keywords:** cybersecurity · explainable AI · human–computer interaction · machine learning · phishing detection · SHAP · XGBoost

---

## Table of Contents

- [Research Overview](#research-overview)
- [Repository Structure](#repository-structure)
- [Dataset](#dataset)
- [Methodology](#methodology)
- [Results](#results)
- [Explainability Analysis](#explainability-analysis)
- [Human-Centred Interface](#human-centred-interface)
- [Key Contributions](#key-contributions)
- [Requirements](#requirements)
- [How to Reproduce](#how-to-reproduce)
- [Generated Outputs](#generated-outputs)
- [Citation](#citation)

---

## Research Overview

| Area | Details |
|---|---|
| **Research domain** | Machine learning, Explainable AI, Cybersecurity, Human–Computer Interaction |
| **Dataset** | `dataset_B_05_2020.csv` — 11,430 instances, 87 features |
| **Class balance** | 5,715 legitimate / 5,715 phishing (perfectly balanced) |
| **Models compared** | Logistic Regression, Decision Tree, Random Forest, Extra Trees, Gradient Boosting, XGBoost |
| **Best model** | XGBoost — Accuracy 96.63%, F1 96.65%, ROC-AUC 99.44% |
| **Explainability** | Global feature importance + SHAP (global & local) |
| **Interface** | Human-centred warning mock-up with plain-language output |
| **HCI evaluation** | 8-item Likert questionnaire + structured response workbook |

---

## Repository Structure

```
Explainable-AI-Based-PhishingWebsite-Detection-System-Report/
│
├── DataSet/
│   └── Web page phishing detection/
│       ├── dataset_B_05_2020.csv          # Main CSV dataset (11,430 × 89)
│       ├── dataset_A_05_2020/             # Pickle-format dataset partitions (p1–p10)
│       └── scripts/
│           ├── feature_extractor.py       # Master feature extraction pipeline
│           ├── url_features.py            # URL-based feature computation
│           ├── content_features.py        # HTML/content-based features
│           ├── external_features.py       # Reputation-based features
│           └── allbrands.txt              # Brand name reference list
│
├── Colab Code and Output/
│   ├── HCI_Paper.ipynb                    # Main Google Colab notebook
│   └── Web_Phishing_Outputs/             # All generated artefacts (plots, CSVs, model)
│
├── results/                               # Exported copies of key experiment outputs
│   ├── best_phishing_detection_model.pkl  # Serialised XGBoost model
│   ├── model_performance_results.csv      # Accuracy/F1/AUC for all 6 models
│   ├── cross_validation_results.csv       # 5-fold CV F1 scores (top 3 models)
│   ├── feature_importance_results.csv     # XGBoost feature importance scores
│   ├── feature_list.csv                   # Full list of 87 features used
│   ├── dataset_summary.csv                # Descriptive statistics for all features
│   └── research_experiment_summary.txt    # High-level experiment summary
│
├── Latex Code/
│   ├── main.tex                           # Full LaTeX source of the research paper
│   └── figures/                           # All 21 figures embedded in the paper
│
├── Explainable AI-Based PhishingWebsite Detection System Report/
│   └── Explainable AI-Based Phishing Website Detection.pdf   # Final PDF report
│
├── Project Presentation/
│   └── Explainable_AI_Phishing_Detection_HCI.pptx            # Presentation slides
│
├── Survey Questionnaire/
│   └── HCI_Questionnaire_Responses_Data_Sheet.xlsx           # Likert response workbook
│
├── Project Requirement/
│   └── HCI_Project_Brief_UMT.pdf
│
└── Turnitin Report/
    ├── Phishing_Paper-AI Turnitin Report.pdf
    └── Phishing_Paper-Plagarism Report.pdf
```

---

## Dataset

The study uses the publicly available **Web Page Phishing Detection** dataset (`dataset_B_05_2020.csv`).

| Property | Value |
|---|---|
| Total instances | 11,430 |
| Legitimate websites | 5,715 (50%) |
| Phishing websites | 5,715 (50%) |
| Total features | 89 columns (87 features + URL + label) |
| Feature types | Numeric / binary |
| Target encoding | `0` = Legitimate, `1` = Phishing |

### Feature Categories

| Category | Examples |
|---|---|
| **URL structure** | `length_url`, `nb_dots`, `nb_hyphens`, `nb_at`, `https_token`, `ratio_digits_url`, `shortening_service`, `prefix_suffix` |
| **Hostname** | `length_hostname`, `ip`, `nb_subdomains`, `abnormal_subdomain`, `random_domain`, `punycode`, `port` |
| **Page content** | `nb_hyperlinks`, `ratio_intHyperlinks`, `ratio_extHyperlinks`, `login_form`, `external_favicon`, `iframe`, `popup_window`, `empty_title` |
| **External reputation** | `google_index`, `page_rank`, `web_traffic`, `dns_record`, `domain_age`, `domain_registration_length`, `whois_registered_domain` |
| **Brand signals** | `domain_in_brand`, `brand_in_subdomain`, `brand_in_path`, `phish_hints`, `suspecious_tld`, `statistical_report` |

---

## Methodology

### Pipeline Overview

```
Raw Dataset (11,430 × 87)
        │
        ▼
  Preprocessing & EDA
  (class balance check, descriptive statistics)
        │
        ▼
  80 / 20 Stratified Train–Test Split
        │
        ▼
  Model Training (6 classifiers)
  Logistic Regression │ Decision Tree │ Random Forest
  Extra Trees │ Gradient Boosting │ XGBoost
        │
        ▼
  Evaluation
  Accuracy · Precision · Recall · F1-Score · ROC-AUC
  5-Fold Stratified Cross-Validation
        │
        ▼
  Explainability (Best Model — XGBoost)
  Global Feature Importance · SHAP Summary Plot · Local SHAP
        │
        ▼
  Human-Centred Interface
  Plain-Language Warning Mock-up
        │
        ▼
  HCI Evaluation Instrument
  8-Item Likert Questionnaire + Response Workbook
```

### Experimental Setup

- **Train/test split:** 80:20, stratified by class
- **Cross-validation:** 5-fold stratified CV (top 3 models)
- **Evaluation metrics:** Accuracy, Precision (phishing), Recall (phishing), F1-score (phishing), ROC-AUC
- **Explainability:** `sklearn` feature importance + `shap` TreeExplainer

---

## Results

### Model Performance Comparison

| Model | Accuracy | Precision | Recall | F1-Score | ROC-AUC |
|---|---|---|---|---|---|
| **XGBoost** | **0.9663** | **0.9619** | **0.9711** | **0.9665** | **0.9944** |
| Extra Trees | 0.9646 | 0.9642 | 0.9650 | 0.9646 | 0.9930 |
| Random Forest | 0.9619 | 0.9583 | 0.9659 | 0.9621 | 0.9936 |
| Gradient Boosting | 0.9532 | 0.9497 | 0.9571 | 0.9534 | 0.9905 |
| Logistic Regression | 0.9361 | 0.9392 | 0.9326 | 0.9359 | 0.9814 |
| Decision Tree | 0.9340 | 0.9291 | 0.9396 | 0.9343 | 0.9340 |

> All metrics are reported on the held-out 20% test set. Precision, Recall, and F1 are reported for the phishing class.

### Cross-Validation Results (5-Fold Stratified)

| Model | Mean CV F1 | Std CV F1 |
|---|---|---|
| **XGBoost** | **0.96906** | **0.00202** |
| Random Forest | 0.96705 | 0.00461 |
| Extra Trees | 0.96341 | 0.00455 |

XGBoost shows the highest mean F1-score with the lowest variance, confirming robust generalisation.

---

## Explainability Analysis

### Top 10 Most Important Features (XGBoost)

| Rank | Feature | Importance Score |
|---|---|---|
| 1 | `google_index` | 0.5145 |
| 2 | `page_rank` | 0.0340 |
| 3 | `nb_qm` | 0.0302 |
| 4 | `nb_www` | 0.0297 |
| 5 | `nb_hyperlinks` | 0.0292 |
| 6 | `ratio_digits_host` | 0.0205 |
| 7 | `dns_record` | 0.0189 |
| 8 | `phish_hints` | 0.0177 |
| 9 | `suspecious_tld` | 0.0152 |
| 10 | `length_words_raw` | 0.0114 |

`google_index` alone accounts for **~51.4%** of the model's total feature importance, indicating that Google's indexing status is the single strongest signal separating phishing from legitimate pages.

### SHAP Analysis

Global SHAP analysis was applied using `shap.TreeExplainer` on the XGBoost model to:
- Quantify the direction and magnitude of each feature's contribution to individual predictions
- Identify which features push a prediction towards "phishing" vs. "legitimate"
- Generate beeswarm summary plots for interpretability communication

These explanations form the foundation of the plain-language user warnings in the proposed interface.

---

## Human-Centred Interface

A warning mock-up was designed to translate model outputs into actionable, user-friendly warnings. The design follows HCI best practices:

- **Plain language** — avoids technical jargon (e.g., "This site is not listed on Google" instead of "google\_index = 0")
- **Visual hierarchy** — colour-coded risk levels (red/amber/green) with accessible contrast ratios
- **Actionable guidance** — explicit next steps ("Do not enter your password", "Leave this page")
- **Explanation transparency** — top contributing signals displayed in non-technical terms
- **Trust calibration** — confidence score communicated alongside the warning

> Note: The interface is a design prototype/mock-up. No browser extension or live detector was implemented or deployed as part of this study.

---

## Key Contributions

| # | Contribution | Description |
|---|---|---|
| 1 | **Predictive modelling** | Six supervised classifiers trained, tuned, and benchmarked on 87 phishing features |
| 2 | **Explainability analysis** | Global feature importance and SHAP (global + local) applied to the best model |
| 3 | **User-centred warning design** | Interface mock-up translating XAI outputs into plain-language warnings |
| 4 | **HCI evaluation instrument** | 8-item Likert questionnaire and structured response workbook for future user studies |

---

## Requirements

The experiment was run in **Google Colab**. Core dependencies:

```
python >= 3.9
pandas
numpy
scikit-learn
xgboost
shap
matplotlib
seaborn
joblib
openpyxl
```

Install all at once:

```bash
pip install pandas numpy scikit-learn xgboost shap matplotlib seaborn joblib openpyxl
```

---

## How to Reproduce

1. **Clone the repository**

   ```bash
   git clone <repository-url>
   cd Explainable-AI-Based-PhishingWebsite-Detection-System-Report
   ```

2. **Open the notebook**

   Upload `Colab Code and Output/HCI_Paper.ipynb` to [Google Colab](https://colab.research.google.com/) and mount your Google Drive, or run locally with Jupyter.

3. **Set the dataset path**

   Place `DataSet/Web page phishing detection/dataset_B_05_2020.csv` in the expected path, or update the path variable at the top of the notebook.

4. **Run all cells**

   Execute all cells in order. The notebook will:
   - Load and preprocess the dataset
   - Train and evaluate all six classifiers
   - Generate all performance plots and CSV exports
   - Run SHAP analysis on the XGBoost model
   - Save the best model as `best_phishing_detection_model.pkl`

5. **Load the pre-trained model** (optional)

   ```python
   import joblib
   model = joblib.load("results/best_phishing_detection_model.pkl")
   ```

---

## Generated Outputs

All experiment outputs are available in the `results/` directory and `Colab Code and Output/Web_Phishing_Outputs/`:

| File | Description |
|---|---|
| `best_phishing_detection_model.pkl` | Serialised XGBoost classifier |
| `model_performance_results.csv` | Accuracy, F1, AUC for all 6 models |
| `cross_validation_results.csv` | 5-fold CV F1 mean and standard deviation |
| `feature_importance_results.csv` | Per-feature XGBoost importance scores |
| `feature_list.csv` | Full list of 87 input features |
| `dataset_summary.csv` | Descriptive statistics for all 89 columns |
| `research_experiment_summary.txt` | Concise experiment summary |
| `accuracy_comparison.png` | Bar chart comparing model accuracy |
| `f1_score_comparison.png` | Bar chart comparing F1-scores |
| `roc_auc_comparison.png` | Bar chart comparing ROC-AUC scores |
| `confusion_matrix_best_model.png` | Confusion matrix for XGBoost |
| `roc_curve_best_model.png` | ROC curve for XGBoost |
| `precision_recall_curve_best_model.png` | Precision–Recall curve for XGBoost |
| `top_20_feature_importance.png` | Top-20 XGBoost feature importance bar chart |
| `shap_summary_plot.png` | SHAP beeswarm summary plot |

---

## Citation

If you use this work, please cite:

```
Mustafa, G. (2026). Explainable AI-Based Phishing Website Detection:
A Human-Centred Machine Learning and SHAP-Based Framework for Non-Technical Users.
Human–Computer Interaction Project Report, BS Computer Science,
University of Management and Technology, Lahore, Pakistan.
```

---

## Ethical Statement

- The dataset used (`dataset_B_05_2020.csv`) is publicly available and contains no personally identifiable information.
- The HCI evaluation instrument was designed for future user studies; the response values in `HCI_Questionnaire_Responses_Data_Sheet.xlsx` are **synthetic demonstration data** and do not represent empirical evidence of usability, trust, or user acceptance.
- No browser extension, live phishing detector, or real-user study was conducted as part of this work.

---

*University of Management and Technology — School of Systems and Technology — June 2026*
