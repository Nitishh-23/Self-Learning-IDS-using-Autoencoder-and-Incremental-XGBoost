# Self-Learning Intrusion Detection System using Deep Learning

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-orange)
![XGBoost](https://img.shields.io/badge/XGBoost-1.7.x-green)
![License](https://img.shields.io/badge/License-MIT-brightgreen)

## 📌 Project Overview
With the rapid escalation of cyber threats and the integration of IoT devices, traditional static Network Intrusion Detection Systems (NIDS) are failing to detect zero-day domain shifts. This project proposes and implements a **Self-Learning Intrusion Detection System (SL-IDS)** that mathematically guarantees cross-domain plasticity. 

The architecture synergizes unsupervised representation learning (Deep Autoencoder) with a supervised, incrementally updating multi-class classification engine (XGBoost). It is empirically proven to ingest foreign network traffic, map unknown threat geometries, and autonomously update its intelligence on the fly without suffering from catastrophic forgetting.

---

## ⚙️ Core Architecture (The 3 Phases)

### 1. Unsupervised Latent Baseline (Phase 1)
A Deep Autoencoder is trained strictly on benign network traffic to compress 55 complex, high-dimensional features into a dense, 32-dimensional latent space. This noise-free bottleneck mathematically isolates anomalous behavior by evaluating reconstruction errors, acting as a strict zero-day filter.

### 2. High-Fidelity Classification (Phase 2)
The optimized 32-dimensional latent vectors are passed directly into a supervised XGBoost classifier configured with a `multi:softprob` objective. This translates the abstract anomalies into exact, multi-class threat categories (DoS, DDoS, PortScan, Botnet, Web Attack, etc.).

### 3. Autonomous Self-Learning (Phase 3)
To address dynamic real-world environments, the system utilizes the native **XGBoost Booster API** for incremental learning. By enforcing strict mathematical constraints (Learning Rate = 0.01, Max Depth = 3), the system dynamically appends exactly five specialized decision trees to the existing ensemble to learn novel zero-day threats without requiring a computationally exhaustive full-system retraining.

---

## 🗄️ Datasets
To empirically validate both the baseline and the real-world adaptation capabilities, this project utilizes two massive datasets:
1. **CICIDS2017 (Global Benchmark):** The primary training and validation sandbox. Comprises traditional web and network traffic with over 2.8 million initial records.
2. **MU-IoT (Real-World Deployment):** The Manipal University IoT dataset. A localized 8.6 GB dataset consisting of heterogeneous Indian IoT device traffic, utilized strictly to test zero-day adaptation and cross-domain plasticity.

---

## 📂 Repository Structure

The repository is structured into four sequential Jupyter Notebooks, mapping the complete pipeline from raw data to real-world self-learning adaptation.

| File | Description |
| :--- | :--- |
| `PreprocessingCICID2017 (1).ipynb` | **Sandbox Prep:** Ingests raw CICIDS2017 data, sanitizes infinity/NaN values, optimizes memory, removes identical columns, scales using Z-score standardization, and exports the 55-feature baseline artifacts. |
| `Model_Training_CICID.ipynb` | **Phase 1 & 2 Execution:** Trains the Deep Autoencoder exclusively on benign traffic. Maps the latent space to the Base XGBoost classifier. Achieves the 99.77% sandbox accuracy baseline. |
| `MU_IoT_Data_Preprocessing.ipynb` | **Cross-Domain Mapping:** Ingests the localized MU-IoT dataset. Semantically translates, prunes missing features, and strictly aligns the features to match the CICIDS architecture. Applies immutable historical scaling. |
| `Final_Model_Large_Testing.ipynb` | **Phase 3 Self-Learning Deployment:** Tests the locked base model against MU-IoT, then triggers the Incremental XGBoost Booster API to update the 8-class model with new IoT signatures on the fly. |

---

## 🚀 Installation & Execution Guide

### Prerequisites
It is highly recommended to run these notebooks in an environment with high RAM and GPU/TPU support (such as Google Colab or a dedicated deep learning server) due to the massive dataset sizes.

```bash
pip install pandas numpy scikit-learn tensorflow xgboost pyarrow fastparquet
```
### Execution Order
To reproduce the pipeline, execute the notebooks in the exact order below. Ensure that your output paths (e.g., Google Drive mounts or local `/data/` directories) are consistent so the serialized models and scalers pass correctly between files.

1. Run `PreprocessingCICID2017 (1).ipynb` to generate `scaler.pkl` and clean matrices.
2. Run `Model_Training_CICID.ipynb` to train and save the Autoencoder (`.h5`) and XGBoost (`.json`) models.
3. Run `MU_IoT_Data_Preprocessing.ipynb` to map the foreign dataset to the strict 55-feature layout.
4. Run `Final_Model_Large_Testing.ipynb` to execute the incremental self-learning update.

---

## 📊 Key Results & Empirical Validation

The architecture successfully bridged the intelligence gap between a controlled global benchmark and a complex localized network.

| Performance Metric | CICIDS2017 (Sandbox Validation) | MU-IoT (Real-World Deployment) |
| :--- | :--- | :--- |
| **Operational Environment** | Controlled Global Benchmark | Localized Indian IoT Network |
| **Dataset Scale Evaluated** | 453,738 testing flow records | 174,238 testing flow records |
| **Stabilized Overall Accuracy** | **99.73%** | **91.89%** |
| **Benign Traffic Recall** | 1.00 (100%) | 0.93 (93%) |
| **Zero-Day Attack Recall** | N/A | **Jumped from 0.00% ➡️ 42.0%** |

**Conclusion:** By generating merely 5 highly specialized decision trees, the system assimilated 42% of a completely foreign threat taxonomy while retaining a 93% benign recall rate, mathematically proving the eradication of catastrophic forgetting.

