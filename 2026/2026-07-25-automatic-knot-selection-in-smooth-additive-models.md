# Automatic Knot Selection in Smooth Additive Models

## Metadata

- **Authors:** Nicolás Carrizosa, Vanesa Guerrero, María Durbán
- **Publication Year:** 2026
- **Read Date:** 2026-07-25
- **Venue:** arXiv (stat.ML)
- **Paper:** https://arxiv.org/abs/2607.21083
- **Code:** https://github.com/n-carrizo/AKSSAM
- **Topics:** Generalized Additive Models, B-Splines, Nonparametric Regression, Knot Selection, Sparse Models
- **Status:**  Read

---

# Abstract (My Summary)

This paper introduces **AKSSAM (Automatic Knot Selection in Smooth Additive Models)**, a new algorithm for automatically selecting knots in Additive Models (AMs) and Generalized Additive Models (GAMs). Traditional spline methods either manually choose knot locations or use dense spline bases with regularization such as P-splines. AKSSAM instead starts with many knots and automatically removes unnecessary ones while simultaneously tuning the model's penalty parameters. The method extends Adaptive Splines (A-Splines) using an adaptive ridge approximation of the L₀ penalty together with a customized Fellner–Schall optimization scheme. Experiments on synthetic and real-world datasets demonstrate that AKSSAM achieves prediction performance comparable to state-of-the-art approaches while producing significantly smaller and more interpretable spline models.

---

# Problem

Spline regression requires deciding:

- How many knots to use.
- Where to place those knots.

Choosing too many knots increases model complexity and risks overfitting, while too few knots oversmooth the function and miss important patterns. Existing GAM implementations generally avoid explicit knot selection by relying on penalized splines instead.

---

# Motivation

Regularization methods such as P-splines create accurate models but usually retain a large number of basis functions.

Many applications benefit from sparse models because they:

- require less computation,
- are easier to interpret,
- are easier to embed into optimization pipelines,
- reduce storage requirements.

The authors therefore revisit explicit knot selection for modern additive models.

---

# Main Idea

The proposed method extends Adaptive Splines from univariate regression to additive models.

The workflow is:

1. Start with a dense B-spline basis.
2. Use an adaptive ridge approximation of an L₀ penalty to identify unnecessary knots.
3. Automatically estimate penalty parameters using a customized Fellner–Schall algorithm.
4. Remove unnecessary knots.
5. Refit the final sparse model.

The result is a compact spline model with similar predictive performance.

---

# Method

## Base Method

Adaptive Splines (A-Splines)

## Core Components

- B-spline basis functions
- Adaptive ridge approximation
- L₀-based knot selection
- Fellner–Schall optimization
- Alternating optimization
- Penalized likelihood estimation

## Supported Models

- Additive Models (AM)
- Generalized Additive Models (GAM)

## Comparison Baselines

- P-Splines
- GeDS
- Particle Swarm Optimization (PSO)

---

# Results

Across synthetic and real datasets, AKSSAM:

- Achieved prediction accuracy similar to existing methods.
- Used substantially fewer spline basis functions.
- Produced more interpretable models.
- Eliminated expensive grid-search tuning.
- Worked for both Gaussian and non-Gaussian response distributions.

Overall, the main contribution is improving model simplicity while maintaining competitive predictive performance.

---

# Strengths

- Fully automatic knot selection.
- Produces sparse and interpretable models.
- No manual tuning of smoothing parameters.
- Extends Adaptive Splines to GAMs.
- Competitive predictive accuracy.
- Strong mathematical foundation.

---

# Limitations

- Limited to spline-based regression.
- Optimization problem is non-convex.
- Global convergence is not guaranteed.
- Numerical safeguards are sometimes required.
- Computational gains mainly come from the final sparse representation rather than the optimization itself.

---

# Key Takeaways

- Explicit knot selection remains competitive with regularization.
- Sparse spline models improve interpretability.
- Adaptive ridge effectively approximates L₀ optimization.
- Fellner–Schall optimization removes the need for expensive hyperparameter search.
- Smaller spline bases can maintain nearly identical prediction quality.

---

# Interesting Ideas

- Combining knot selection with automatic smoothing parameter estimation.
- Using adaptive ridge to approximate an L₀ penalty.
- Building sparse additive models instead of dense penalized models.
- Extending univariate spline-selection techniques to GAMs.

---

# Questions

- Can AKSSAM be extended to multivariate spline surfaces?
- Could Bayesian optimization replace alternating optimization?
- Would GPU implementations significantly improve runtime?
- Can this framework be combined with neural spline models?

---

# Future Work

The authors suggest:

- Improving optimization stability.
- Extending to more complex spline models.
- Supporting additional response distributions.
- Applying the framework to broader statistical modeling problems.

---

# My Thoughts

This paper focuses more on statistical methodology than machine learning. The main novelty is not inventing a new spline model but modernizing explicit knot selection for generalized additive models. I liked how the authors combined adaptive ridge optimization with the Fellner–Schall algorithm to remove manual hyperparameter tuning. Although the work is niche, it addresses a practical problem in nonparametric regression and demonstrates that sparse models can match the predictive performance of much larger spline representations. It also reinforces the idea that improving optimization and model efficiency can be as valuable as designing entirely new algorithms.

---

# Related Papers

- Adaptive Splines (A-Splines)
- P-Splines
- Generalized Additive Models (GAMs)
- Fellner–Schall Algorithm

---

# Rating

| Category | Rating |
|----------|--------|
| Novelty | ⭐⭐⭐⭐☆ |
| Technical Depth | ⭐⭐⭐⭐⭐ |
| Clarity | ⭐⭐⭐⭐☆ |
| Practical Impact | ⭐⭐⭐⭐☆ |
| Overall | ⭐⭐⭐⭐☆ (8.8/10) |
