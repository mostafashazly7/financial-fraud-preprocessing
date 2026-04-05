<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0d1117,50:0a2a1a,100:0d1117&height=200&section=header&text=Financial%20Fraud%20Preprocessing&fontSize=36&fontColor=7ee787&animation=fadeIn&fontAlignY=38&desc=End-to-End%20Pipeline%20for%202M%2B%20Transaction%20Records&descAlignY=58&descSize=16&descColor=8b949e"/>

<img src="https://img.shields.io/badge/Records-2%2C000%2C000%2B-7ee787?style=for-the-badge&labelColor=0d1117"/>
<img src="https://img.shields.io/badge/Dataset%20Size-360MB%2B-f0883e?style=for-the-badge&labelColor=0d1117"/>
<img src="https://img.shields.io/badge/Storage-Git%20LFS-58a6ff?style=for-the-badge&labelColor=0d1117"/>
<img src="https://img.shields.io/badge/Status-Preprocessing%20Complete-7ee787?style=for-the-badge&labelColor=0d1117"/>

</div>

---

## 📌 Problem Statement

Financial fraud causes billions in losses each year. Detecting it with machine learning requires clean, normalized, model-ready data — but real-world financial datasets are messy, massive, and highly imbalanced. This project solves that first critical step.

This repository implements a complete **data preprocessing pipeline** for a large-scale financial transaction dataset containing over **2,000,000 records**, transforming raw messy data into a clean, normalized CSV ready to train classification models — all while managing a 360MB+ file using **Git LFS**.

---

## 🗂️ Dataset Overview

| Property | Details |
|---|---|
| Source | Large-Scale Financial Fraud Dataset |
| Total Records | 2,000,000+ transactions |
| Raw File Size | ~360 MB |
| Storage Method | Git LFS (Large File Storage) |
| Output File | `fraud_final_preprocessed.csv` |

### Raw Feature Schema

| Feature | Type | Description |
|---|---|---|
| `type` | Categorical | Transaction type: CASH-IN, CASH-OUT, TRANSFER, PAYMENT, DEBIT |
| `amount` | Float | Transaction amount |
| `oldbalanceOrg` | Float | Origin account balance before transaction |
| `newbalanceOrig` | Float | Origin account balance after transaction |
| `oldbalanceDest` | Float | Destination balance before transaction |
| `newbalanceDest` | Float | Destination balance after transaction |
| `isFraud` | Binary | Ground truth label (1 = Fraud, 0 = Legitimate) |
| `isFlaggedFraud` | Binary | Rule-based flag by existing system |

---

## ⚙️ Preprocessing Pipeline

```text
Raw CSV (360MB+, 2M+ rows) | ▼
┌──────────────────────────────────────────────────────────────────┐
| Step 1 — Data Cleaning                                           |
| • Drop missing / null values                                     |
| • Remove inconsistent records                                    |
| • Fix data type mismatches                                       |
| • Memory optimization for high-speed CSV processing              |
└────────────────────────────────┬─────────────────────────────────┘
                                 | ▼
┌────────────────────────────────┴─────────────────────────────────┐
| Step 2 — Feature Encoding                                        |
| • One-Hot Encoding on `type`                                     |
|   CASH-IN  → [1, 0, 0, 0, 0]                                     |
|   CASH-OUT → [0, 1, 0, 0, 0]                                     |
|   TRANSFER → [0, 0, 1, 0, 0]                                     |
|   PAYMENT  → [0, 0, 0, 1, 0]                                     |
|   DEBIT    → [0, 0, 0, 0, 1]                                     |
└────────────────────────────────┬─────────────────────────────────┘
                                 | ▼
┌────────────────────────────────┴─────────────────────────────────┐
| Step 3 — Feature Scaling                                         |
| • Min-Max Scaling on: amount, oldbalanceOrg, newbalanceOrig,     |
|   oldbalanceDest, newbalanceDest                                 |
| • All values mapped to [0, 1]                                    |
| • Fraud patterns preserved                                       |
└────────────────────────────────┬─────────────────────────────────┘
                                 | ▼
┌────────────────────────────────┴─────────────────────────────────┐
| Step 4 — Output                                                  |
| fraud_final_preprocessed.csv | Clean · Normalized · ML-Ready ✅  |
└──────────────────────────────────────────────────────────────────┘
```

---

## 🔬 Key Technical Decisions

### Why One-Hot Encoding for Transaction Type?
The `type` column contains 5 distinct string categories. Label encoding would imply a false ordinal relationship (as if TRANSFER > PAYMENT numerically). One-Hot Encoding creates 5 binary columns — one per type — letting the model treat each transaction type independently without introducing bias.

### Why Min-Max Scaling for Balances?
Financial amounts vary enormously — from a few dollars to millions. Without normalization, large-magnitude features dominate gradient-based learning. Min-Max scaling maps every value to **[0, 1]** without distorting the distribution, which is critical for preserving the fraud signal hidden in extreme balance discrepancies.

### Why Git LFS?
The raw dataset (~360MB) and output file exceed GitHub's 100MB file limit. Git LFS (Large File Storage) stores the actual binary content separately while keeping a lightweight pointer in the repository — enabling full version control on large files without bloating the repo history.

---

## 📊 Pipeline Results

| Metric | Value |
|---|---|
| Records processed | 2,000,000+ |
| Null values handled | ✅ Cleaned |
| Categorical features encoded | 1 column → 5 binary columns (One-Hot) |
| Numerical features scaled | 5 columns normalized to [0, 1] |
| Output file | `fraud_final_preprocessed.csv` |
| Ready for ML training | ✅ Yes |

---


## 📁 Repository Structure

```text
financial-fraud-preprocessing/
|
├── fraud_preprocessed.ipynb      # Full preprocessing pipeline
├── fraud_final_preprocessed.csv  # ✅ Final clean output (via Git LFS)
├── dataset.pdf                   # Dataset documentation
|
├── Large-Scale Financial Fraud Dataset/
|   └── improved_fraud_dataset.csv  # Raw dataset (via Git LFS)
|
├── .gitattributes                # Git LFS tracking configuration
├── .gitignore
└── README.md


---

## 🚀 How to Run

### Prerequisites

You **must** have Git LFS installed to download the dataset files.
```bash
# Install Git LFS (one-time setup)
git lfs install
```

### 1. Clone the Repository
```bash
git clone https://github.com/mostafashazly7/financial-fraud-preprocessing.git
cd financial-fraud-preprocessing
```

### 2. Pull the Large Dataset Files
```bash
# This downloads the actual CSV files tracked by Git LFS
git lfs pull
```

### 3. Install Dependencies
```bash
pip install pandas scikit-learn matplotlib
```

### 4. Run the Notebook
```bash
jupyter notebook fraud_preprocessed.ipynb
```

> ✅ The output `fraud_final_preprocessed.csv` is already included in the repo via Git LFS — you can use it directly for model training without running the notebook.

---

## 🛠️ Tech Stack

<div align="center">
  <img src="https://img.shields.io/badge/Python-3.x-3776ab?style=flat-square&logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/Pandas-Data%20Wrangling-150458?style=flat-square&logo=pandas&logoColor=white"/>
  <img src="https://img.shields.io/badge/Scikit--Learn-Encoding%20%26%20Scaling-f7931e?style=flat-square&logo=scikit-learn&logoColor=white"/>
  <img src="https://img.shields.io/badge/Matplotlib-Visualization-11557c?style=flat-square&logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/Git%20LFS-Large%20File%20Storage-f05032?style=flat-square&logo=git&logoColor=white"/>
</div>

---

## 🔭 What's Next

The `fraud_final_preprocessed.csv` output is ready to feed directly into classification models:

- [ ] Train a **Random Forest** or **XGBoost** classifier on the preprocessed data
- [ ] Handle class imbalance with **SMOTE** or `class_weight='balanced'`
- [ ] Evaluate with **Precision, Recall, F1, and AUC-ROC** — accuracy alone is misleading on imbalanced data
- [ ] Engineer new features: balance error rate `(newbalanceOrig + amount - oldbalanceOrg)`
- [ ] Deploy as a real-time scoring API with **Flask** or **FastAPI**

---

## 👤 Author

**Mostafa Shazly** · [LinkedIn](https://linkedin.com/in/mostafa-shazly-148945314) · [GitHub](https://github.com/mostafashazly7)

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0d1117,50:0a2a1a,100:0d1117&height=100&section=footer"/>


---

## ⭐ Support
If you like this project, don't forget to ⭐ the repo!
