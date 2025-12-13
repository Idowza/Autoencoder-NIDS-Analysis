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

This project uses the **CICIDS2017: Cleaned & Preprocessed** dataset.

- **Source:** [Kaggle - CICIDS2017: Cleaned & Preprocessed](https://www.kaggle.com/datasets/ericanacletoribeiro/cicids2017-cleaned-and-preprocessed)
- **Note:** Due to file size limits, the dataset is not included in this repo. Please download it from the link above.

### Data Context
- **Source Validation:** By using the Ericanacleto version of CICIDS2017, we avoided the common "NaN" and "Infinity" pitfalls of the raw dataset.
- **Class Distribution:** The dataset is composed of ~2.1 million Normal samples and ~425k Attack samples.
  - **Significance:** This 5:1 ratio is a realistic representation of network traffic (mostly normal, some attacks). It confirms that the high Accuracy (99.35%) in the Hybrid model is not just a result of guessing "Normal" every time (which would only yield ~83%), but actually detecting the attacks.

## 📊 Results Summary

### Quantitative Comparison: The Hybrid Victory

The most powerful argument for this project lies in the Recall and the Confusion Matrix.

| Metric | Baseline (Autoencoder Only) | Hybrid (AE + Random Forest) | Improvement |
| :--- | :--- | :--- | :--- |
| **Accuracy** | 91.29% | 99.35% | +8.06% |
| **Precision** | 97.10% | 98.05% | +0.95% |
| **Recall** | 49.91% | 98.11% | +48.2% (Massive) |
| **F1-Score** | 0.6593 | 0.9808 | +0.3215 |

### The Confusion Matrix Deep Dive

- **Baseline Failure:** The baseline model had **42,646 False Negatives** (attacks misclassified as normal). It essentially flipped a coin on detecting attacks (49.9% Recall).
- **Hybrid Success:** The Hybrid model reduced False Negatives to just **1,607**. It successfully detected **98.1%** of the attacks that the baseline missed.

## 🔧 Usage

1. **Clone the repository:**
   ```bash
   git clone https://github.com/Idowza/Autoencoder-NIDS-Analysis.git
   ```

2. **Install dependencies:**
   ```bash
   pip install numpy pandas matplotlib seaborn tensorflow scikit-learn
   ```

3. **Run the analysis:**
   - Open `analysis_realdata.ipynb` in Jupyter Notebook or VS Code.
   - Update the `DATASET_PATH` variable to point to your local copy of the CICIDS2017 dataset.
   - Run the cells to execute the training and evaluation pipeline.

   *Note: `analysis_test.ipynb` is provided for testing purposes with synthetic/sample data.*

## 📈 Visualizations

### Training History
![Autoencoder Training History](Autoencoder%20Training%20History.png)

**Observation:** The Blue line (Training Loss) and Orange line (Validation Loss) drop precipitously in the first 2 epochs and flatten out near zero (1.6×10−5).

**Analysis:** The training history demonstrates that the Autoencoder architecture was highly effective at learning the statistical baseline of 'Normal Traffic'. The rapid convergence and the lack of divergence between training and validation loss indicate that the model did not overfit. It successfully compressed the 52 input features into the 8-dimensional latent space with minimal information loss.

### Reconstruction Error Distribution
![Distribution of Reconstruction Errors](Distribution%20of%20Reconstruction%20Errors%20(Baseline%20Model).png)

**Observation:**
- **Green Region (Normal):** Extremely sharp, narrow spike on the far left (Error ≈ 0.0). This confirms the model perfectly reconstructs normal traffic.
- **Red Region (Attack):** Distributed across the x-axis. While some attacks have high error (right side), a massive portion of the red distribution overlaps directly with the green spike on the left.

**Analysis:** The histogram reveals the critical limitation of the Baseline Unsupervised approach. While the Autoencoder successfully flagged gross anomalies (high reconstruction error on the right), a significant volume of attack traffic produced low reconstruction errors, indistinguishable from normal traffic (the overlapping region on the left). This confirms the 'Reliability Debate' (Alhassan et al.)—that Autoencoders can generalize too well, reconstructing attacks as if they were normal.


## 🏁 Conclusion

This research validates the hypothesis that unsupervised reconstruction error alone is insufficient for high-reliability Network Intrusion Detection on modern datasets like CICIDS2017. The Baseline Autoencoder failed to distinguish nearly 50% of attacks because they statistically resembled normal traffic (low reconstruction error).

However, the Novel Hybrid Model demonstrated that the Autoencoder is an exceptional feature extractor. By discarding the reconstruction error and instead feeding the compressed latent space representations into a Random Forest classifier, the system achieved a near-perfect detection rate (98.11% Recall). This proves that while the magnitude of the error may be similar for some attacks, their location in the high-dimensional latent space is distinct enough for a supervised classifier to separate them from benign traffic.

## 📝 Citation & References

This work is based on research into deep learning for NIDS, specifically analyzing the impact of hidden layers and hybrid architectures.

[1] P. Pavithralakshmi et al., ”Anomaly Detection for Network Traffic Using Autoencoder,” International Journal of Research Publication and Reviews, vol. 6, no. 5, pp. 10168–10173, May 2025.

[2] R. Agrawal, ”Complete Guide to Anomaly Detection with AutoEncoders using Tensorflow,” Data Science Blogathon, Jan 2022.

[3] H. Liao et al., ”A Survey of Deep Learning Technologies for Intrusion Detection in Internet of Things,” IEEE Access, Jan 2024.

[4] S. Selvakumar, M. Sivaanandh, K. Muneeswaran, and B. Lakshmanan, ”Ensemble of feature augmented convolutional neural network and deep autoencoder for efficient detection of network attacks,” Scientific Reports, vol. 15, no. 4267, 2025.

[5] Z. M. Khan, ”Network Intrusion Detection Utilizing Autoencoder Neural Networks,” Communications on Applied Nonlinear Analysis, vol. 31, no. 3s, 2024.

[6] S. Alhassan et al., ”Analyzing Autoencoder-Based Intrusion Detection System Performance: Impact of Hidden Layers,” Journal of Information Security and Cybercrimes Research, vol. 6, no. 2, pp. 105–115, Dec 2023.
