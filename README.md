# Autoencoder-NIDS-Analysis

## 📌 Project Overview

This repository contains the source code and analysis for a graduate-level research project in EE 672: Emerging Threats & Defense in Cybersecurity.

The project investigates the "capability gap" in traditional signature-based Intrusion Detection Systems (IDS). It implements and compares two deep learning approaches to detect malicious network traffic (such as DoS and MITM attacks) within the CICIDS2017 benchmark dataset.

## 🚀 Key Features & Methodology

### 1. Baseline Model: Unsupervised Anomaly Detection

- **Architecture:** Deep Autoencoder (DAE) implemented in TensorFlow/Keras.
- **Method:** The model is trained exclusively on benign traffic to learn a baseline of normal behavior.
- **Detection Logic:** Attacks are identified as outliers based on high Reconstruction Error (MSE).

### 2. Novelty: Hybrid Feature Extraction Model

- **Architecture:** The trained Autoencoder is repurposed as a feature extractor.
- **Method:** The "bottleneck" layer (latent space) is used to compress high-dimensional network traffic into a dense representation.
- **Detection Logic:** These compressed features are fed into a lightweight Random Forest Classifier to improve detection precision and reduce false positives compared to the baseline.

## 🛠️ Tech Stack

- **Language:** Python 3.x
- **Deep Learning:** TensorFlow, Keras
- **Data Manipulation:** Pandas, NumPy
- **Machine Learning:** Scikit-learn
- **Visualization:** Matplotlib, Seaborn

## 📂 Dataset

This project uses the CICIDS2017 dataset provided by the Canadian Institute for Cybersecurity.

- **Note:** Due to file size limits, the raw dataset is not included in this repo.
- **Download:** You can access the dataset [here](https://www.unb.ca/cic/datasets/ids-2017.html).

## 📊 Results Summary

- **Baseline (Reconstruction Error):** Effective at identifying gross anomalies but prone to higher false positive rates on complex benign traffic.
- **Hybrid (Novelty):** Demonstrated improved separation between classes, leveraging the latent space representations to achieve higher F1-Scores.

## 🔧 Usage

1. **Clone the repository:**
   ```bash
   git clone https://github.com/Idowza/Autoencoder-NIDS-Analysis.git
   ```

2. **Install dependencies:**
   ```bash
   pip install numpy pandas tensorflow scikit-learn matplotlib seaborn
   ```

3. **Run the Jupyter Notebook:**
   ```bash
   jupyter notebook analysis.ipynb
   ```

## 📝 Citation & References

This work is based on research into deep learning for NIDS, specifically analyzing the impact of hidden layers and hybrid architectures.

- P. Pavithralakshmi et al., "Anomaly Detection for Network Traffic Using Autoencoder," International Journal of Research Publication and Reviews, 2025.
- S. Alhassan et al., "Analyzing Autoencoder-Based Intrusion Detection System Performance," Journal of Information Security and Cybercrimes Research, 2023.
