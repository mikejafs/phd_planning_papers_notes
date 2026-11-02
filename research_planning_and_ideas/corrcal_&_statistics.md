# CorrCal: Statistical and Numerical Background

Reading notes on how CorrCal's likelihood is built and why its optimization reduces to Ax = b problems solvable by conjugate gradient.

## Summary

- **Sky model:** the sky is a Gaussian random field, marginalized out of the likelihood.
- **Covariance:** C = N + G(ΔΔᵀ + ΣΣᵀ)Gᴴ
  - N: noise
  - Δ: redundant-baseline (diffuse sky) component
  - Σ: point-source component
  - G: per-visibility gain products
- **Objective:** minimize dᴴC⁻¹d over the gains, optionally plus log det C.
- **Cheap inverse:** the sky terms are low rank, so the Woodbury identity gives C⁻¹ cheaply.
- **Link to Ax = b:** the objective is non-quadratic in the gains, and connects to linear solves in two ways:
  1. **Nonlinear CG** applies the logic of linear CG to a general function.
  2. **Gauss-Newton** linearization produces the normal equations (JᴴC⁻¹J) δg = JᴴC⁻¹r at each step.

**Reading order:** Shewchuk → Rasmussen & Williams → Bishop → Boyd & Vandenberghe → Sievers 2017

## Saved references

### Shewchuk, *An Introduction to the Conjugate Gradient Method Without the Agonizing Pain*

The bridge between minimizing a quadratic form and solving Ax = b.

| Section | Content |
|---|---|
| §3 | ½xᵀAx − bᵀx is minimized exactly where Ax = b |
| §7–9 | Derivation of linear CG and its convergence |
| §12 | Preconditioning |
| §13 | CG on the normal equations (the Gauss-Newton case) |
| §14 | Nonlinear CG, used by the original CorrCal code through scipy |

### Rasmussen & Williams, *Gaussian Processes for Machine Learning*

Same statistical structure as CorrCal: a latent Gaussian signal marginalized into the covariance, with parameters fit by maximizing the marginal likelihood.

| Section | Content |
|---|---|
| Ch. 2 | Gaussian model; predictive and marginal distributions |
| Ch. 5 (esp. §5.4) | Marginal likelihood as quadratic term plus log det; gradients with respect to covariance parameters (analog of the gain gradients) |
| App. A.3 | Matrix identities: Woodbury and the determinant lemma, used for the low-rank inverse |

### Bishop, *Pattern Recognition and Machine Learning*

Statistics foundation for linear-Gaussian models.

| Section | Content |
|---|---|
| §2.3 (esp. §2.3.3) | Integrating out a linear-Gaussian latent variable adds its covariance to the noise. The core move in CorrCal |
| §3.1.1 | Maximum likelihood under Gaussian noise reduces to least-squares normal equations |
| §3.5 | Maximizing the evidence over hyperparameters |

### Boyd & Vandenberghe, *Convex Optimization*

Estimation and numerical linear algebra.

| Section | Content |
|---|---|
| §7.1 | ML estimation for linear measurements with Gaussian noise; why the estimate is a prewhitened least-squares problem |
| App. C | Numerical linear algebra: matrix inversion lemma, block elimination for low-rank structure |

## Not yet saved

- [ ] **Sievers 2017** (arXiv:1701.01860): the primary CorrCal paper
- [ ] **Liu et al. 2010** (arXiv:1001.5268): logcal and lincal as linear least squares
- [ ] **Tegmark 1997** (astro-ph/9611130): CMB map-making as a GLS problem solved with preconditioned CG
- [ ] **Hogg, Bovy & Lang 2010** (arXiv:1008.4686): GLS normal equations
- [ ] **Kay, *Fundamentals of Statistical Signal Processing: Estimation Theory***, ch. 3 and 15: Fisher information and Cramér-Rao bounds on the gains (if needed later)