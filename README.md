# 🌊 Spatio-Temporal Neural Estimator for Sea Surface Temperature

**Author:** Wejdan Alharthi  
**Framework:** `NeuralEstimators.jl`  
**Language:** Julia  

---

## 📌 Overview

This project develops a **simulation-based inference pipeline** for learning parameters of a **spatio-temporal Gaussian Random Field (GRF)** using a **Neural Bayes Estimator (NBE)**.

The model is designed to infer underlying physical parameters from simulated **Sea Surface Temperature (SST)** fields, capturing both **spatial correlation** and **temporal dependence**.

---

## 🧠 Key Idea

We simulate data from:
* **Spatial correlation** → Matérn covariance ($\nu = 1.5$)  
* **Temporal dependence** → AR(1) process  

To make the data suitable for neural architectures:
> We apply **first-order temporal differencing**, making the data **exchangeable**, enabling the use of a **DeepSet + CNN architecture**.

---

## ⚙️ Pipeline

### 1. Data Generation
* Sample parameters $\theta = (\theta_1, \theta_2)$
* Simulate spatio-temporal fields
* Apply first differences

### 2. Preprocessing
* Handle variable-length time dimension
* Align all samples to fixed length ($T = 9$)

### 3. Training (Next Step)
* DeepSet + CNN architecture
* GPU-based training
* Neural parameter inference

---

## 📊 Dataset

| Split | Samples |
| :--- | :--- |
| **Train** | 10,000 |
| **Validation** | 1,000 |
| **Test** | 500 |

**Sample Dimensions:**
* **Before Preprocessing:** $(21, 21, 1, T)$
* **After Preprocessing:** $(21, 21, 1, 9)$

---

## 📁 Project Structure

```text
NBE/
│
├── data/
│   ├── raw/            # (ignored in repo)
│   └── processed/      # ready for training
│
├── notebooks/
│   └── data_preprocessing.ipynb
│
├── Project.toml
├── Manifest.toml
└── README.md
```
