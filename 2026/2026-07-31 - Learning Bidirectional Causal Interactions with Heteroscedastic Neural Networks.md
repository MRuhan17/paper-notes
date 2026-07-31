# Learning Bidirectional Causal Interactions with Heteroscedastic Neural Networks

**Date:** 31 July 2026  
**Paper:** arXiv:2607.22313v1  
**Status:** Read

**Authors:** Masahiro Tanaka

---

# Abstract

This paper introduces **SEM-DNN (Simultaneous Equation Deep Neural Network)**, a deep learning framework for estimating **bidirectional causal relationships** from purely observational data. Unlike traditional causal inference methods that require instrumental variables, SEM-DNN identifies causal interactions through **heteroscedasticity**, leveraging feature-dependent changes in structural noise variances. The method jointly learns nonlinear structural mean functions, conditional variance functions, and reciprocal causal coefficients using neural networks trained with a diagonal Gaussian quasi-likelihood. The authors provide theoretical proofs of identification, validate the method through Monte Carlo simulations, and demonstrate its applicability using supermarket scanner data involving simultaneous price-demand interactions.

---

# Why This Paper Matters

Many real-world systems contain variables that influence each other simultaneously.

Examples include:

- Price ↔ Demand
- Advertisers ↔ Users
- Policy ↔ Economic Indicators
- Supply ↔ Consumption
- Biological Feedback Systems

Traditional machine learning models can predict these relationships but cannot determine the direction or magnitude of causal influence because each variable is endogenous to the other.

Econometric methods typically solve this problem using **Instrumental Variables (IVs)**.

However, valid instruments are difficult to find in practice.

This paper proposes an entirely different identification strategy that does **not require instruments**, making it potentially useful in many observational datasets where IV methods are infeasible.

---

# Problem Statement

The authors study two simultaneously determined variables:

y₁ and y₂

Each influences the other at the same time.

The structural equations are

y₁ = γ₁y₂ + f₁(x) + ε₁

y₂ = γ₂y₁ + f₂(x) + ε₂

where

- γ₁ is the causal effect of y₂ on y₁
- γ₂ is the causal effect of y₁ on y₂
- f₁ and f₂ are unknown nonlinear functions
- ε₁ and ε₂ are structural shocks

The objective is to estimate γ₁ and γ₂ from observational data while allowing highly nonlinear relationships.

---

# Why Standard Neural Networks Fail

If we simply train

y₁ ← y₂,x

and

y₂ ← y₁,x

the networks learn only predictive relationships.

Because both variables are jointly determined, the learned coefficients reflect reduced-form associations rather than true structural effects.

Standard DNNs therefore cannot distinguish

- direct causality
- feedback
- confounding
- simultaneity

without additional assumptions.

---

# Main Idea

The central insight is surprisingly elegant.

Suppose we guess values for γ₁ and γ₂.

Using these guesses we compute structural residuals.

If the guessed coefficients are wrong,

the residuals still contain mixtures of both structural shocks and remain correlated.

If the guessed coefficients are correct,

the residual covariance matrix becomes diagonal.

Therefore,

finding the coefficients that diagonalize the residual covariance identifies the causal effects.

This becomes possible because the variances of the structural shocks change differently across the feature space.

The authors call this **conditional covariance diagonalization**.

---

# Identification Through Heteroscedasticity

The identification assumptions are:

## 1. Zero Conditional Mean

Structural shocks satisfy

E(ε|x)=0

meaning the errors contain no systematic information after conditioning on features.

---

## 2. Conditional Independence

The two structural shocks are conditionally uncorrelated.

Therefore

Cov(ε₁,ε₂|x)=0

at the true causal parameters.

---

## 3. Non-Proportional Conditional Variances

The variances

Var(ε₁|x)

and

Var(ε₂|x)

must change differently across the feature space.

If they changed proportionally everywhere,

multiple causal solutions could exist.

When they change differently,

only one pair of causal coefficients diagonalizes the covariance matrix.

This is the fundamental identification result of the paper.

---

# SEM-DNN Architecture

The model jointly learns four neural networks.

## Mean Networks

Two networks estimate

f₁(x)

and

f₂(x)

capturing nonlinear structural effects.

---

## Variance Networks

Two additional neural networks estimate

g₁(x)

and

g₂(x)

which model conditional variances.

Unlike most uncertainty estimation work,

these variance networks provide the identifying signal for causal inference.

---

## Structural Parameters

Two scalar parameters

γ₁

γ₂

are optimized together with all network weights.

Thus the model simultaneously estimates

- nonlinear means
- nonlinear variances
- causal feedback coefficients

within one optimization process.

---

# Loss Function

Training uses a diagonal Gaussian quasi-likelihood consisting of

- residual likelihood
- variance terms
- Jacobian correction for simultaneous equations

The Jacobian term is important because transforming between structural shocks and observed outcomes changes probability density.

The optimization therefore respects the underlying simultaneous equation model rather than treating it as ordinary regression.

---

# Training Stabilization

Jointly learning means and variances is unstable.

Variance networks often collapse toward very small values,

causing exploding gradients.

The paper introduces several stabilization techniques.

## Softplus Variance Parameterization

Guarantees positive variance estimates.

---

## Lower Variance Bound

Prevents variance collapse.

---

## β-NLL Stabilization

Reduces the influence of observations assigned artificially small variances.

The default value is

β = 0.5

---

## Weight Regularization

Regularizes neural weights while leaving causal parameters unpenalized.

---

## Bounded Structural Parameters

Instead of optimizing γ directly,

the model optimizes

γ = γ̄ tanh(γ̃)

ensuring the simultaneous system never becomes singular.

---

# Theoretical Contributions

The strongest contribution of the paper is its mathematical analysis.

The authors prove

## Local Identification

The true causal coefficients maximize the profiled likelihood locally.

---

## Global Identification

Within the stable parameter region,

no incorrect coefficients produce diagonal residual covariance.

---

## Positive Curvature

The profiled objective has positive curvature around the true solution,

making optimization theoretically well behaved.

---

## Neural Compatibility

The authors also prove that neural network parameterization preserves these identification properties despite non-unique neural weights.

---

# Diagnostics

Unlike many ML papers,

SEM-DNN includes practical diagnostics.

These include

## Variance Ratio Analysis

Checks whether heteroscedasticity is strong enough for identification.

---

## Residual Covariance

Measures whether transformed residuals become approximately diagonal.

---

## Variance Calibration

Evaluates whether predicted variances match empirical residuals.

---

## Gradient Concentration

Detects optimization problems caused by inverse variance weighting.

---

## Optimization Sensitivity

Assesses robustness across different random initializations.

---

# Monte Carlo Experiments

The simulation study evaluates SEM-DNN under

- nonlinear functions
- high-dimensional covariates
- non-Gaussian noise
- varying sample sizes
- different signal-to-noise ratios

Sample sizes include

- 5,000
- 10,000
- 20,000

The method is compared against

- Parametric Simultaneous Equation Model (SEM-PAB)
- Kernel-based Simultaneous Equation Model (SEM-Kernel)
- Independent Neural Regression (Single-DNN)

---

# Experimental Results

The simulations show

- SEM-DNN has the lowest bias as sample size increases.
- RMSE decreases consistently with larger datasets.
- Performance improves significantly in highly nonlinear settings.
- Neural approximation outperforms parametric and kernel approaches.
- Single-equation neural networks perform poorly because they ignore simultaneity.

The largest improvements occur when nonlinear nuisance functions become difficult for classical methods to approximate.

---

# Real-World Application

The authors apply SEM-DNN to the Dominick's supermarket scanner dataset.

Objective:

Estimate simultaneous interaction between

- cereal prices
- product sales

The model jointly estimates

Price → Demand

and

Demand → Price

without using external instruments.

The application mainly demonstrates

- estimation
- diagnostics
- optimization workflow

rather than claiming definitive economic conclusions.

---

# Strengths

- Novel causal identification without instrumental variables.
- Strong theoretical foundation.
- Integrates econometrics with deep learning.
- Jointly models means and variances.
- Includes extensive diagnostics.
- Thorough simulation study.
- Real-world empirical validation.

---

# Limitations

- Restricted to two-variable simultaneous systems.
- Strong assumptions about conditional independence.
- Requires meaningful heteroscedasticity.
- Computationally expensive.
- Sensitive to optimization and hyperparameter tuning.
- Not a general causal discovery algorithm.

---

# Key Takeaways

- Introduces SEM-DNN for simultaneous causal estimation.
- Replaces instrumental variables with heteroscedasticity-based identification.
- Learns nonlinear structural equations using neural networks.
- Provides theoretical guarantees for identification.
- Outperforms parametric, kernel, and naive neural baselines in nonlinear environments.
- Demonstrates that variance modeling can be used for causal identification rather than only uncertainty estimation.

---

# Personal Notes

This paper is one of the stronger examples of combining **econometrics with deep learning**. The key novelty is not the neural network architecture itself, but the realization that **heteroscedasticity can act as an identifying signal for bidirectional causality**.

The theoretical sections are significantly stronger than the empirical evaluation, making this paper especially valuable for researchers interested in **causal machine learning**, **structural equation models**, **observational causal inference**, and **machine learning for econometrics**.

While the assumptions may limit applicability in some real-world settings, the paper provides an elegant alternative to instrumental-variable methods and opens an interesting research direction for learning causal feedback systems directly from observational data.
