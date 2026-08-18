# 🛡️ Malicious URL Detection Dataset

A comprehensive, curated collection of malicious and benign URL datasets designed for machine learning, deep learning, NLP, and cybersecurity research in phishing, malware, and rogue web domain detection.

[![License](https://img.shields.io/badge/License-Research%20%26%20Education-blue.svg)](LICENSE)
[![Python](https://img.shields.io/badge/Python-3.8%2B-green.svg)](https://www.python.org/)
[![Total URLs](https://img.shields.io/badge/Total%20URLs-649%2C508-orange.svg)](#-dataset-summary-table)
[![Dataset Balance](https://img.shields.io/badge/Balance-50.0%25%20Benign%20%2F%2050.0%25%20Malicious-brightgreen.svg)](#-dataset-summary-table)

---

## 📌 Overview

This repository provides **4 complementary datasets** comprising a total of **649,508 labeled URLs** (324,891 Benign / 324,617 Malicious). 

The collection spans multiple scales and threat distributions—from lightweight benchmarking datasets (2,000 to 10,000 samples) ideal for rapid prototyping and unit testing, to large-scale balanced datasets (630,000+ samples) suited for training production-grade Machine Learning, Deep Learning (LSTM, CNN, Transformers), and LLM security models.

---

## 📊 Dataset Summary Table

| Dataset File | Total Samples | Benign / Clean | Malicious | Balance Ratio | File Size | Key Columns | Header in CSV |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| [`balanced_urls.csv`](file:///Users/khanren/Documents/GitHub/Malicious%20URL%20Detection%20Dataset/Datasets/balanced_urls.csv) | **632,508** | 316,254 | 316,254 | 50.0% / 50.0% | ~44.03 MB | `url`, `label`, `result` | Yes |
| [`Kaggle Dataset.csv`](file:///Users/khanren/Documents/GitHub/Malicious%20URL%20Detection%20Dataset/Datasets/Kaggle%20Dataset.csv) | **10,000** | 5,000 | 5,000 | 50.0% / 50.0% | ~0.84 MB | `url`, `label`, `source` | No |
| [`Dataset II.csv`](file:///Users/khanren/Documents/GitHub/Malicious%20URL%20Detection%20Dataset/Datasets/Dataset%20II.csv) | **5,000** | 2,547 | 2,453 | 50.9% / 49.1% | ~0.30 MB | `URL`, `type` | Yes |
| [`ChatPhishDetector Dataset.csv`](file:///Users/khanren/Documents/GitHub/Malicious%20URL%20Detection%20Dataset/Datasets/ChatPhishDetector%20Dataset.csv) | **2,000** | 1,090 | 910 | 54.5% / 45.5% | ~0.06 MB | `url`, `label` | No |
| **TOTAL (Combined Collection)** | **649,508** | **324,891** | **324,617** | **50.0% / 50.0%** | **~45.23 MB** | — | — |

---

## 📁 Repository Structure

```
.
├── Datasets/
│   ├── ChatPhishDetector Dataset.csv   # 2,000 URLs (Lightweight / Edge testing)
│   ├── Dataset II.csv                  # 5,000 URLs (Benchmark validation set)
│   ├── Kaggle Dataset.csv              # 10,000 URLs (Source-annotated benchmark)
│   └── balanced_urls.csv               # 632,508 URLs (Primary ML/DL training dataset)
└── README.md                           # Project documentation
```

---

## 🔍 Detailed Dataset Breakdown

### 1. `balanced_urls.csv` (Primary Large-Scale Dataset)
- **Description:** The core dataset of the repository. Perfectly balanced (50/50) and pre-processed for training scalable classification models (XGBoost, LightGBM, Random Forest) or neural network architectures (1D CNN, LSTM, CharBERT, Transformers).
- **Columns:**
  - `url` *(string)*: Target URL (e.g., `https://www.google.com`, `http://atualizacaodedados.online`).
  - `label` *(string)*: Categorical label (`benign` or `malicious`).
  - `result` *(integer)*: Binary numeric target (`0` = Benign, `1` = Malicious).
- **Distribution:** 316,254 Benign (50.0%), 316,254 Malicious (50.0%).

### 2. `Kaggle Dataset.csv` (Source-Annotated Benchmark)
- **Description:** Benchmark dataset gathered from Kaggle featuring source annotations from threat intelligence providers (such as `PhishTank`, `URLhaus`).
- **Columns (Inferred - No raw CSV header):**
  - `url` *(string)*: Full URL string.
  - `label` *(string)*: Status identifier (`Clean` or `Malicious`).
  - `source` *(string, optional)*: Origin threat feed tag (e.g., `PhishTank`, `URLhaus`).
- **Distribution:** 5,000 Clean (50.0%), 5,000 Malicious (50.0%).

### 3. `Dataset II.csv` (Secondary Validation Dataset)
- **Description:** Multi-source evaluation dataset suitable for out-of-distribution model validation, cross-dataset testing, and fast experimentation.
- **Columns:**
  - `URL` *(string)*: Target URL path.
  - `type` *(string)*: Class label (`Benign` or `Malicious`).
- **Distribution:** 2,547 Benign (50.9%), 2,453 Malicious (49.1%).

### 4. `ChatPhishDetector Dataset.csv` (Lightweight Test Set)
- **Description:** Highly compact dataset useful for quick unit testing, LLM security prompt evaluation (e.g., ChatPhishDetector agents), and lightweight edge deployment.
- **Columns (Inferred - No raw CSV header):**
  - `url` *(string)*: Domain/URL string.
  - `label` *(string)*: Target label (`Benign` or `Malicious`).
- **Distribution:** 1,090 Benign (54.5%), 910 Malicious (45.5%).

---

## 💻 Code Examples & Data Loading

### 1. Loading Individual Datasets in Python (Pandas)

```python
import pandas as pd

# 1. Load balanced_urls.csv
df_large = pd.read_csv("Datasets/balanced_urls.csv")

# 2. Load Kaggle Dataset.csv (No header in raw file)
df_kaggle = pd.read_csv(
    "Datasets/Kaggle Dataset.csv", 
    header=None, 
    names=["url", "label", "source"],
    encoding="utf-8-sig"
)

# 3. Load Dataset II.csv
df_dataset2 = pd.read_csv("Datasets/Dataset II.csv", encoding="utf-8-sig")

# 4. Load ChatPhishDetector Dataset.csv (No header in raw file)
df_chatphish = pd.read_csv(
    "Datasets/ChatPhishDetector Dataset.csv", 
    header=None, 
    names=["url", "label"],
    encoding="utf-8-sig"
)
```

### 2. Standardized Unified Dataset Loader

This utility function loads and standardizes all 4 CSV files into a single unified DataFrame with consistent `url`, `label` (`0` = Benign, `1` = Malicious), and `source` metadata.

```python
import os
import pandas as pd

def load_all_malicious_url_datasets(datasets_dir="Datasets"):
    dfs = []
    
    # 1. ChatPhishDetector Dataset (No header)
    path = os.path.join(datasets_dir, "ChatPhishDetector Dataset.csv")
    if os.path.exists(path):
        df = pd.read_csv(path, header=None, names=["url", "raw_label"], encoding="utf-8-sig")
        df["label"] = df["raw_label"].apply(lambda x: 1 if str(x).strip().lower() == "malicious" else 0)
        df["source"] = "ChatPhishDetector"
        dfs.append(df[["url", "label", "source"]])
    
    # 2. Dataset II (Has header: URL, type)
    path = os.path.join(datasets_dir, "Dataset II.csv")
    if os.path.exists(path):
        df = pd.read_csv(path, encoding="utf-8-sig")
        df = df.rename(columns={"URL": "url", "type": "raw_label"})
        df["label"] = df["raw_label"].apply(lambda x: 1 if str(x).strip().lower() == "malicious" else 0)
        df["source"] = "Dataset_II"
        dfs.append(df[["url", "label", "source"]])

    # 3. Kaggle Dataset (No header)
    path = os.path.join(datasets_dir, "Kaggle Dataset.csv")
    if os.path.exists(path):
        df = pd.read_csv(path, header=None, names=["url", "raw_label", "threat_source"], encoding="utf-8-sig")
        df["label"] = df["raw_label"].apply(lambda x: 1 if str(x).strip().lower() == "malicious" else 0)
        df["source"] = "Kaggle"
        dfs.append(df[["url", "label", "source"]])

    # 4. balanced_urls.csv (Has header: url, label, result)
    path = os.path.join(datasets_dir, "balanced_urls.csv")
    if os.path.exists(path):
        df = pd.read_csv(path, encoding="utf-8-sig")
        df = df.rename(columns={"result": "label"})
        df["source"] = "Balanced_URLs"
        dfs.append(df[["url", "label", "source"]])
        
    combined_df = pd.concat(dfs, ignore_index=True)
    return combined_df

# Run Unified Loader
df_all = load_all_malicious_url_datasets()
print(f"Successfully loaded {len(df_all):,} URLs!")
print("\nOverall Label Breakdown:")
print(df_all["label"].value_counts().rename(index={0: "Benign (0)", 1: "Malicious (1)"}))
```

---

## 🛠️ Recommended Feature Extraction Pipeline

For classical Machine Learning algorithms (e.g., Random Forest, Gradient Boosting, SVM), key structural and lexical features can be engineered directly from the raw URL string:

| Feature Category | Feature Name | Description & Security Rationale |
| :--- | :--- | :--- |
| **Lexical Metrics** | `url_length` | Total character length of URL (Phishing URLs tend to be longer). |
| | `domain_length` | Length of domain/hostname segment. |
| | `path_length` | Length of URL path directory structure. |
| | `count_dots` (`.`) | Count of dots (multiple dots often indicate complex subdomains). |
| | `count_hyphens` (`-`) | Count of hyphens (frequently used in typosquatting). |
| | `count_at` (`@`) | Presence of `@` symbol (used to confuse browser authority parsers). |
| | `count_question` (`?`) | Count of query parameter strings. |
| | `digit_ratio` | Ratio of numeric digits to overall character length. |
| | `special_char_count` | Count of special characters (`=`, `&`, `%`, `_`, `-`, `?`). |
| **Structural** | `has_ip_address` | Boolean check if domain is a raw IPv4/IPv6 address. |
| | `is_https` | Protocol check (HTTPS vs HTTP). |
| | `subdomain_count` | Number of subdomains extracted via `tldextract`. |
| | `tld_risk_score` | High-risk top-level domain flag (`.xyz`, `.top`, `.tk`, `.bid`, `.online`). |
| **Entropy & NLP** | `shannon_entropy` | Character entropy of domain name (detects DGA / random generation). |
| | `tfidf_char_ngrams` | Character n-gram TF-IDF vectors (n=3 to 5) for sub-word learning. |

---

## 🎯 Key Use Cases

- 🤖 **Machine Learning & Deep Learning:** Training models to predict phishing, malware distribution, and rogue domains.
- 🛡️ **Cyber Threat Intelligence (CTI):** Evaluating domain reputational scores and zero-day threat behavior.
- 💬 **LLM & Agent Safety Benchmarking:** Validating AI security assistants (e.g., ChatPhishDetector) for web link inspection.
- 🔌 **Browser Extension Security:** Building lightweight, real-time phishing detection engines for web browsers.

---

## 📄 License & Attribution

This dataset collection aggregates publicly available security datasets for research and educational purposes. When using these datasets in academic publications or software products, please credit the original data creators and threat intelligence feeds (including UNB CIC, PhishTank, URLhaus, and Kaggle contributors).
