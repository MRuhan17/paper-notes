# Conformal Risk Control for Model-Form Uncertainty in Parametric Non-Intrusive Reduced-Order Models

**Authors:** Edgar Jaber, Rémy Vallot, Thibault Dairay, Mathilde Mougeot

**Status:** Read

---

# Overview

This paper presents a framework for quantifying **model-form uncertainty (MFU)** in **Non-Intrusive Reduced-Order Models (NIROMs)**. The authors combine random perturbations of Proper Orthogonal Decomposition (POD) bases with Gaussian Process (GP) regression and calibrate the resulting uncertainty estimates using **Conformal Risk Control (CRC)**. The objective is to produce reliable prediction intervals that account for both structural errors in the reduced basis and statistical uncertainty in the learned surrogate while providing finite-sample, distribution-free guarantees.

---

# Motivation

Reduced-order models significantly reduce the computational cost of solving parameterized partial differential equations, making them valuable in engineering design, optimization, uncertainty quantification, and digital twins. However, standard NIROMs provide only point predictions and often underestimate prediction uncertainty.

Existing Gaussian Process uncertainty captures only regression uncertainty and ignores errors introduced by basis truncation and model structure. This paper addresses that limitation by explicitly modeling **model-form uncertainty** and combining it with conformal calibration to produce trustworthy uncertainty estimates.

---

# Problem Statement

Given a computationally expensive PDE solver, construct a surrogate model that:

- accurately predicts high-dimensional solution fields,
- quantifies both regression and model-form uncertainty,
- avoids retraining Gaussian Processes during uncertainty propagation,
- provides statistically valid prediction bands with finite-sample guarantees.

---

# Key Concepts

## Non-Intrusive Reduced-Order Models (NIROMs)

NIROMs approximate high-fidelity PDE solutions using:

1. Proper Orthogonal Decomposition (POD) to reduce dimensionality.
2. Gaussian Processes to learn the reduced coordinates.

Unlike intrusive reduced-order methods, NIROMs require only simulation data and treat the numerical solver as a black box.

---

## Proper Orthogonal Decomposition (POD)

A snapshot matrix is constructed from simulation outputs.

Applying Singular Value Decomposition (SVD) yields:

- retained POD modes,
- discarded POD modes.

The retained modes form the reduced basis used for prediction, while discarded modes represent information lost through truncation.

---

## Model-Form Uncertainty (MFU)

Model-form uncertainty refers to prediction errors caused by structural approximations such as:

- truncation rank,
- reduced basis selection,
- surrogate model design.

Unlike regression uncertainty, MFU cannot be eliminated simply by collecting more training data.

---

## Gaussian Process Regression

Each reduced coordinate is modeled independently using a Gaussian Process.

The GP provides:

- posterior mean,
- posterior variance.

The GP variance reflects uncertainty caused by finite training data but does not capture errors introduced by the reduced basis.

---

## Stiefel Manifold Perturbation

Instead of treating the POD basis as fixed, the paper models it as a random object.

Small perturbations are generated on the **Stiefel manifold**, ensuring the basis remains orthonormal.

Importantly, perturbations are directed only toward the discarded POD modes, allowing the induced uncertainty to represent basis truncation error.

---

## First-Order Transport Approximation

Retraining Gaussian Processes for every perturbed basis would be computationally infeasible.

To avoid this, the paper introduces a first-order transport approximation that maps predictions from the reference basis to perturbed bases through a transport matrix.

This decouples basis uncertainty from GP uncertainty while avoiding repeated GP training.

---

## Total Predictive Variance

The predictive covariance is decomposed using the **Law of Total Covariance** into two components:

- regression-induced uncertainty,
- basis-induced (model-form) uncertainty.

This decomposition provides a more realistic estimate of predictive uncertainty than Gaussian Processes alone.

---

## Conformal Risk Control (CRC)

The estimated uncertainty is calibrated using Conformal Risk Control.

Instead of guaranteeing coverage probability, CRC controls the expected coordinate-wise miscoverage of the predicted field.

The calibration introduces a single scaling factor:

- small calibration factor → uncertainty estimate is already accurate,
- large calibration factor → uncertainty estimate significantly underestimates true prediction error.

The calibration factor therefore serves as a quantitative measure of uncertainty quality.

---

# Methodology

The proposed framework consists of six steps:

1. Build the POD reduced basis.
2. Perturb the basis on the Stiefel manifold.
3. Train Gaussian Processes for reduced coordinates.
4. Transport GP predictions to perturbed bases.
5. Compute predictive covariance using the law of total covariance.
6. Apply Conformal Risk Control to obtain calibrated prediction bands.

This workflow produces uncertainty estimates without repeatedly retraining the surrogate model.

---

# Experimental Evaluation

The framework is evaluated on four benchmark problems:

- 2D Poisson equation
- 1D Linear Advection equation
- 1D Burgers equation
- Industrial rubber calendering process

Across all benchmarks:

- CRC consistently achieves the desired coverage guarantees.
- MFU-aware uncertainty requires substantially smaller calibration factors than GP uncertainty alone.
- The proposed uncertainty model is especially effective when basis truncation dominates the prediction error.

The advection problem also highlights a limitation: first-order basis perturbations cannot accurately localize errors when the solution manifold is poorly represented by a linear subspace.

---

# Main Contributions

- Introduces a perturbative framework for estimating model-form uncertainty in Gaussian Process NIROMs.
- Models POD basis uncertainty using random perturbations on the Stiefel manifold.
- Derives a closed-form predictive covariance combining regression and structural uncertainty.
- Eliminates repeated GP retraining through a first-order transport approximation.
- Integrates Conformal Risk Control to obtain finite-sample, distribution-free prediction guarantees.
- Demonstrates effectiveness on both academic PDE benchmarks and an industrial manufacturing application.

---

# Limitations

- Applicable only to linear POD-based reduced-order models.
- First-order approximation assumes relatively small basis perturbations.
- Performance degrades when solution manifolds cannot be represented well by linear subspaces.
- Extension to nonlinear latent representations (e.g., autoencoders) is left for future work.

---

# Key Definitions

| Concept | Definition |
|----------|------------|
| NIROM | Non-Intrusive Reduced-Order Model built from simulation data without modifying the solver. |
| POD | Proper Orthogonal Decomposition used to construct a reduced basis from simulation snapshots. |
| Model-Form Uncertainty | Prediction uncertainty arising from structural approximations rather than finite data. |
| Stiefel Manifold | The set of matrices with orthonormal columns used to preserve valid reduced bases during perturbation. |
| Gaussian Process | Bayesian regression model providing predictive means and variances for reduced coordinates. |
| Transport Approximation | First-order mapping that propagates GP predictions to perturbed bases without retraining. |
| Law of Total Covariance | Decomposition separating regression uncertainty from basis-induced uncertainty. |
| Conformal Risk Control | Distribution-free calibration framework providing guaranteed prediction bands. |
| Calibration Factor | Scalar multiplier measuring how well the estimated uncertainty matches the true prediction error. |

---

# Key Takeaways

- Standard Gaussian Process uncertainty captures only regression uncertainty and ignores structural errors introduced by reduced-order modeling.
- Perturbing the POD basis on the Stiefel manifold provides an efficient way to model basis-induced uncertainty.
- A first-order transport approximation makes uncertainty propagation computationally practical by avoiding repeated GP training.
- Combining basis-induced and regression-induced uncertainty yields more realistic predictive variances.
- Conformal Risk Control calibrates these uncertainty estimates to provide finite-sample guarantees while also serving as an objective measure of uncertainty quality.
- The framework consistently improves uncertainty estimation over Gaussian Process variance alone, particularly when basis truncation is a significant source of prediction error.
