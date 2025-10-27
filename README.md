# ⚛️ DeepONet — Radiation Transport Problems

## Project 1: DeepONet Prediction of Neutron Flux in 1D Slab with Variable Source Distribution

Using **Deep Operator Networks (DeepONet)** via the [DeepXDE](https://github.com/lululxvi/deepxde) library to approximate the **neutron flux distribution** in a one-dimensional slab geometry with an internally varying source distribution.

Reference:
Sahadath, M. H., Cheng, Q., Pan, S., & Ji, W. (2025, April). Deep Operator Network Based Surrogate Model for Neutron Transport Computation. In *Proceedings of International Conference on Mathematics and Computational Methods Applied to Nuclear Science & Engineering (M&C2025), Denver CO, USA*. Available at: https://doi.org/10.13182/xyz-47261

---

### 🧩 Overview

This project develops a **DeepONet-based surrogate model** for solving the **1D steady-state neutron transport equation** under **vacuum boundary conditions**.  
The goal is to learn a mapping from the **source distribution** $Q(x)$ to the corresponding **neutron flux** $\phi(x)$ using operator learning.

We first solve the deterministic $S_N$ transport problem using transport sweep and source iteration technique. Then, we generate a dataset using **Gaussian Random Fields (GRFs)** for diverse source distributions, and finally train a **DeepONet** model to approximate the transport operator.

---

### 🧠 Methodology

1. **Transport Solver**
   - Implements 1D discrete ordinates ($S_N$) method.
   - Uses **diamond-difference scheme** for spatial discretization.
   - Vacuum boundaries at both ends of the slab.
   - Computes cell-averaged scalar flux:
     $\phi_j = \sum_n w_n \psi_{n,j}$

2. **Dataset Generation**
   - Generate 150 GRF-based source samples $Q_i(x)$ and compute corresponding fluxes $\phi_i(x)$ using the solver.
   - Each function is evaluated at 100 equally spaced spatial points.
   - Aligned dataset of shape `(n_samples, n_points)` is used.

3. **DeepONet Model**
   - **Branch network:** takes discretized source distribution $Q(x)$.
   - **Trunk network:** takes position coordinate $x$.
   - Combined through a dot product to predict $\phi(x)$.
   - Implemented with DeepXDE using the PyTorch backend.

---

### 📊 Results

- **Training:** 150 GRF samples  
- **Testing:** 20 independent GRF samples  
- **Training configuration:**
  - Optimizer: Adam  
  - Learning rate: 1e-3 → 1e-5 (two-phase training)  
  - Iterations: 40,000  
  - Loss: Mean Squared Error (MSE)  
- **Performance:**
  - Mean relative L2 error on test set: **0.0033 (0.33%)**
  - The model accurately reproduces the flux profiles from unseen source distributions.

Example prediction plots are available in the Jupyter notebook.

---

