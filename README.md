# Simulation-Based Inference for Spatio-Temporal Sea Surface Temperature using Neural Bayes Estimators

![Julia](https://img.shields.io/badge/Julia-1.12-blue)

Likelihood-free inference for spatio-temporal Gaussian random fields using amortized neural Bayes estimators.

## Overview

This repository presents a proof-of-concept application of Neural Bayes Estimators (NBE) for simulation-based inference in a spatio-temporal Gaussian random field model motivated by sea surface temperature (SST) dynamics.

The goal is to infer two model parameters from simulated SST fields: a spatial range parameter controlling spatial dependence, and a temporal dependence parameter controlling AR(1) dynamics. The full pipeline includes data simulation, preprocessing, GPU-based neural estimator training, and parameter recovery assessment using NeuralEstimators.jl.

---

## Statistical Model

We consider a spatio-temporal Gaussian random field model for sea surface temperature with parameter vector

θ = (ρ, ϕ)^T

where:

- ρ controls spatial dependence through a Matérn covariance structure  
- ϕ controls temporal dependence through an AR(1) process

Spatial covariance is modeled using a Matérn kernel (ν = 1.5), while temporal evolution follows an autoregressive process.

To facilitate exchangeable learning with DeepSet-based neural architectures, first-order temporal differencing is applied prior to inference.

---

## Inference Pipeline

### 1. Prior Sampling and Simulation
Parameters θ = (ρ, ϕ) are sampled from prior distributions, and spatio-temporal Gaussian random fields are simulated under a Matérn spatial covariance model and AR(1) temporal dependence.

### 2. Data Preprocessing
First-order temporal differencing is applied to promote exchangeability, and simulated samples are standardized to a fixed temporal length for neural network training.

### 3. Neural Estimator Training
A DeepSet-CNN architecture is trained using Neural Bayes Estimation to amortize parameter inference from simulated fields.

### 4. Parameter Recovery Assessment
Estimator performance is evaluated on held-out test simulations using parameter recovery diagnostics, including RMSE, MAE, and prediction bias.

---

## Results

The estimator demonstrates strong parameter recovery on held-out simulations.

### Parameter Recovery

| Parameter | RMSE | Interpretation |
|---------|------|----------------|
| ρ (spatial range) | 0.018 | strong recovery |
| ϕ (temporal dependence) | 0.069 | more difficult to recover |

Spatial dependence is recovered more accurately than temporal dependence, suggesting that dynamic structure remains the more difficult inferential component under the current architecture.

### Evaluation Metrics
- Test RMSE ≈ 0.05  
- Test MAE ≈ 0.03  
- Bias assessed through parameter recovery diagnostics

### Assessment Visualization
True versus predicted parameter estimates are shown in the assessment figure below.

![Assessment](assets/assessment_results.png)

---
## Limitations

Current limitations include:

- evaluation is based on simulated data only  
- uncertainty calibration is not yet assessed  
- no benchmark against classical inference methods is currently included

Benchmarking against likelihood-based or MCMC alternatives remains future work.

  ---

## Reproducibility

To reproduce the full workflow:

```julia
using Pkg
Pkg.activate(".")
Pkg.instantiate()
```

The repository includes:

- simulation and preprocessing notebooks  
- processed training/validation/test datasets  
- trained estimator checkpoint  
- assessment scripts and visualization outputs

Processed data are included for immediate reproducibility, while raw data can be regenerated using the provided simulation pipeline.

---

## Repository Structure

NBE/
├── data/
├── notebooks/
├── models/
├── assets/
└── docs/

See individual folders for preprocessing, training, and assessment components.

---
## Computational Environment
Julia 1.12  
Flux.jl  
CUDA.jl  
NeuralEstimators.jl
---
## Future Directions

Potential extensions include:

- uncertainty-aware posterior inference using NPE or NRE  
- improved architectures for temporal dependence recovery  
- applications to real sea surface temperature observations  
- extensions to more complex spatio-temporal extreme-value models

---

## Connection to NeuralEstimators.jl

This repository serves as a proof-of-concept application built using NeuralEstimators.jl for spatio-temporal simulation-based inference.

---
## Related Reference

Primary methodological background:

- Sainsbury-Dale, Zammit-Mangion, and Huser (2024), Likelihood-Free Parameter Estimation with Neural Bayes Estimators.

---
## Citation

If you use this repository, please consider citing:

```bibtex
@misc{NeuralEstimators.jl,
  title = {{NeuralEstimators.jl}: A Julia package for efficient simulation-based inference using neural networks},
  author = {Sainsbury-Dale, Matthew},
  year = {2026}
}
```

and the methodological reference:

```bibtex
@article{SainsburyDale2024,
  author = {Sainsbury-Dale, Matthew and Zammit-Mangion, Andrew and Huser, Raphael},
  title = {Likelihood-Free Parameter Estimation with Neural Bayes Estimators},
  journal = {The American Statistician},
  year = {2024},
  doi = {10.1080/00031305.2023.2249522}
}
```

---

## Acknowledgements

This work builds on the NeuralEstimators.jl framework developed by Matthew Sainsbury-Dale and collaborators.

---

This repository is an ongoing research prototype and proof-of-concept for simulation-based spatio-temporal inference.
