# 🌊 Spatio-Temporal Neural Estimator for Sea Surface Temperature

![Julia](https://img.shields.io/badge/Julia-1.12-blue)
![Status](https://img.shields.io/badge/status-completed-brightgreen)

Author: Wejdan Majed Alharthi  
Framework: NeuralEstimators.jl (Simulation-Based Inference)  
Backend: Flux.jl + CUDA.jl  
Language: Julia  
---

## 📌 Overview

This project develops a **simulation-based inference pipeline** for learning parameters of a **spatio-temporal Gaussian Random Field (GRF)** using a **Neural Bayes Estimator (NBE)**.

The model infers underlying physical parameters from simulated **Sea Surface Temperature (SST)** fields, capturing both **spatial correlation** and **temporal dependence**.

---

## 🌍 Why This Matters

This project demonstrates how **simulation-based neural inference** can provide scalable and efficient alternatives to traditional likelihood-based methods, especially in complex spatio-temporal settings.

---

## 🧠 Key Idea

We simulate data using:

- **Spatial correlation** → Matérn covariance ($\nu = 1.5$)  
- **Temporal dependence** → AR(1) process  

To make the data compatible with neural architectures:

> We apply **first-order temporal differencing**, making the data **exchangeable**, enabling the use of a **DeepSet + CNN architecture**.

---

## ⚙️ Pipeline

### 1. Data Generation
- Sample parameters $\theta = (\theta_1, \theta_2)$  
- Simulate spatio-temporal fields  
- Apply first-order differencing  

### 2. Preprocessing
- Handle variable temporal length  
- Standardize all samples to fixed length ($T = 9$)  

### 3. Model Training
- DeepSet + CNN architecture  
- GPU-based training (CUDA)  
- Neural parameter estimation  

### 4. Evaluation
- Test-set prediction  
- RMSE, MAE, and bias computation  
- Visualization (true vs predicted)  
- Built-in assessment via `NeuralEstimators.jl`

---

## 📊 Results

The model demonstrates strong generalization performance:

- **Test RMSE ≈ 0.05**
- **Test MAE ≈ 0.03**

### Visualization

The figure below compares true and predicted parameters on the test set:
![Assessment](assets/assessment_results.png)


### Parameter-wise performance:

- **θ₁ (spatial range):**
  - RMSE ≈ 0.018 → very high accuracy  

- **θ₂ (temporal dependence):**
  - RMSE ≈ 0.069 → more challenging due to temporal dynamics  

> The results show that spatial structure is easier to recover than temporal dependence in the current architecture.

---

## 📊 Dataset

| Split | Samples |
| :--- | :--- |
| Train | 10,000 |
| Validation | 1,000 |
| Test | 500 |

**Sample Shape:**
- $(21, 21, 1, 9)$ after preprocessing  

---

## 📁 Project Structure

```text
NBE/
│
├── data/
│   ├── raw/            # (excluded)
│   └── processed/      # training-ready data
│
├── notebooks/
│   ├── data_preprocessing.ipynb
│   └── data_training.ipynb
│
├── assets/
│   └── assessment_results.png   # evaluation visualization
│
├── models/
│   └── nbe_model.jld2          # trained Neural Bayes Estimator
│
├── docs/
│   └── challenges.md           # detailed challenges and solutions
│
├── Project.toml
├── Manifest.toml
└── README.md
```

### 💾 Data Management

* **Processed Data:** Included for direct training to ensure immediate reproducibility.
* **Raw Data:** Excluded from the repository due to its large storage footprint.
* **Full Pipeline:** The provided scripts allow for the complete regeneration of datasets from scratch.

---

### 🧪 Technologies

* **Julia:** The core language used for high-performance scientific computing.
* **NeuralEstimators.jl:** For developing and evaluating neural estimators.
* **Flux.jl:** The primary library for neural network architectures.
* **CUDA.jl + cuDNN:** Leveraged for GPU acceleration and optimized deep learning primitives.
* **CairoMakie & AlgebraOfGraphics:** Used for high-quality, declarative data visualization and plotting.

---

### 📚 Supporting and Citation

This project builds on:
```
@misc{NeuralEstimators.jl,
  title = {{NeuralEstimators.jl}: A Julia package for efficient simulation-based inference using neural networks},
  author = {Sainsbury-Dale, Matthew},
  year = {2026}
}
```

## 🎯 Goal
Build a scalable framework for **spatio-temporal parameter inference** using neural networks, avoiding explicit likelihood computation.

---

## ⚠️ Challenges and Solutions

A detailed discussion of engineering challenges and their solutions is available here:

👉 [View Challenges and Solutions](docs/challenges.md)

---

## ⭐ Project Status
**Completed** (Training + Evaluation + Visualization)

### Future work may include:
* **Improving temporal parameter estimation:** Refining the model's ability to capture time-series dynamics.
* **Applying the model to real SST datasets:** Transitioning from simulations to real-world Sea Surface Temperature data.
