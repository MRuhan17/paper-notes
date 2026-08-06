# 0608 - Robust Low-Tubal-Rank Tensor Completion under Cross-Concentrated Sampling

**Authors:** HanQin Cai, Longxiu Huang, Jing Qin, Chengyue Wu

**Paper:** Robust Low-Tubal-Rank Tensor Completion under Cross-Concentrated Sampling

**arXiv:** 2608.03928v2 (5 Aug 2026)

**File:** `0608-robust-low-tubal-rank-tensor-completion-under-cross-concentrated-sampling.md`

**Status:** Read

---

## Summary

This paper studies **robust tensor completion** for third-order tensors that are both **partially observed** and **corrupted by sparse, large-magnitude outliers**. Unlike conventional tensor completion methods that assume uniformly sampled clean data, the authors focus on **Tensor Cross-Concentrated Sampling (t-CCS)**, where observations are available only within selected horizontal and lateral tensor slices.

The authors propose **Robust Iterative t-CUR (R-ItCUR)**, a tensor-native optimization algorithm that extends previous t-CUR completion methods to operate under sparse corruption. Instead of reconstructing the entire tensor during optimization, R-ItCUR stores and updates only the sampled tensor cross, significantly reducing both memory usage and computational cost.

A major contribution is the introduction of a **tube-wise sparsity model**, where entire tensor tubes—not individual entries—are treated as corrupted. This sparsity assumption is preserved under the Fourier transform, making it naturally compatible with the t-SVD framework used for low-tubal-rank modeling.

The optimization procedure consists of four main steps:

1. Partition the sampled tensor cross into three independent blocks.
2. Detect outliers using an adaptive **Welsch correction** function.
3. Perform projected blockwise gradient descent on each block.
4. Enforce the low-tubal-rank constraint through truncated t-SVD on the intersection block.

Experiments on synthetic tensors, cardiac MRI volumes, and 3D seismic datasets show that R-ItCUR consistently achieves higher reconstruction quality than the non-robust ITCURTC method when sparse corruptions are present, while matching its performance on clean data. Compared to Robust-IHT, R-ItCUR performs substantially better under structured t-CCS sampling because it explicitly exploits the cross-concentrated observation geometry.

Overall, the paper demonstrates that combining **cross-concentrated sampling**, **adaptive robust outlier suppression**, and **implicit t-CUR representations** enables scalable and accurate tensor completion even in the presence of severe sparse corruptions.

---

## Key Contributions

- Introduces the first robust low-tubal-rank tensor completion method for **Tensor Cross-Concentrated Sampling (t-CCS)**.
- Proposes **R-ItCUR**, which performs robust completion without reconstructing the full tensor during optimization.
- Introduces **tube-wise sparsity**, preserving sparse corruption structure in the Fourier domain.
- Uses adaptive **Welsch correction** for stable outlier suppression.
- Demonstrates strong performance on synthetic, MRI, and seismic datasets while reducing computational and memory overhead.

---

## Keywords

Tensor Completion, Low-Tubal-Rank, Tensor t-CUR, Cross-Concentrated Sampling, t-SVD, Robust Optimization, Sparse Outliers, Welsch Loss, MRI Reconstruction, Seismic Imaging
