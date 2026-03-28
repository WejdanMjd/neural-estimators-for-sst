# 🌊 Spatio-Temporal Neural Estimator for Sea Surface Temperature
![Julia](https://img.shields.io/badge/Julia-1.12-blue)
![Status](https://img.shields.io/badge/status-in%20progress-orange)

**Author:** Wejdan Alharthi  
**Framework:** NeuralEstimators.jl (Simulation-Based Inference)
**Language:** Julia  

---

## 📌 Overview

This project develops a **simulation-based inference pipeline** for learning parameters of a **spatio-temporal Gaussian Random Field (GRF)** using a **Neural Bayes Estimator (NBE)**.

The model is designed to infer underlying physical parameters from simulated **Sea Surface Temperature (SST)** fields, capturing both **spatial correlation** and **temporal dependence**.

---

## 🌍 Why This Matters

Understanding spatio-temporal dynamics of sea surface temperature is essential for climate modeling and environmental monitoring. This work demonstrates how modern neural inference methods can replace expensive traditional statistical approaches.

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

### 3. Model Training (Upcoming)
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

---

### 💾 Data Management

* **Processed Data:** Included in this repository for direct training.
* **Raw Data:** Excluded from the repository due to its large size.
* **Regeneration:** All data can be regenerated using the provided simulation pipeline in the notebooks.

---

### 🚀 Next Steps

1.  **Train Neural Bayes Estimator:** Utilizing GPU acceleration for efficient training.
2.  **Model Evaluation:** Testing accuracy on the withheld test set to ensure robustness.
3.  **Real-world Application:** Applying the trained model to real SST data (e.g., NOAA ERSST).

---

### 🧪 Technologies

* **Julia:** High-performance scientific computing.
* **NeuralEstimators.jl:** Framework for neural-based inference.
* **Flux.jl:** Deep learning library for Julia.
* **CUDA:** GPU acceleration for training and simulation.

---

## 📚 Supporting and Citation

This project is built using the NeuralEstimators.jl package developed by Matthew Sainsbury-Dale.

If you use this repository in your research or other activities, please consider citing:

```bibtex
@misc{NeuralEstimators.jl,
  title = {{NeuralEstimators.jl}: A Julia package for efficient simulation-based inference using neural networks},
  author = {Sainsbury-Dale, Matthew},
  year = {2026},
  note = {Version 0.2.0},
  howpublished = {\url{https://github.com/msainsburydale/NeuralEstimators.jl}}
}
```

---

### 🎯 Goal

Build a scalable and efficient framework for **spatio-temporal parameter inference** using neural networks, effectively bypassing expensive and complex likelihood computations.

---

### 📎 Notes

This project is inspired by the **NeuralEstimators workshop** and extends its principles to a spatio-temporal setting with specialized handling for temporal dependence.

---

## ⭐ Project Status

This project is actively under development, with upcoming work focusing on model training, evaluation, and real-world SST application.

