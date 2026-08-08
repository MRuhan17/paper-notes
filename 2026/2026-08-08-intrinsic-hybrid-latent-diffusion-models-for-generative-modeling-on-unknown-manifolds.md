# 2026-08-08-intrinsic-hybrid-latent-diffusion-models-for-generative-modeling-on-unknown-manifolds.md

**Status:** Read

## Summary

This paper introduces **Intrinsic Hybrid Latent Diffusion Model (ILDM)**, a generative framework designed for **data-sparse settings** where the underlying data manifold is unknown. Unlike standard latent diffusion models that treat the latent space as Euclidean, ILDM models it as a **Riemannian manifold**, using a pretrained probabilistic decoder to capture both the geometry of the data and uncertainty in the latent-to-data mapping.

The key idea is a **hybrid diffusion process**. In regions where the decoder is confident, ILDM uses **Riemannian Brownian motion** to respect the learned manifold geometry. When uncertainty becomes high, it switches to **Euclidean diffusion**, avoiding unreliable geometric estimates. The same uncertainty-aware switching is used during the backward sampling process with Riemannian or Euclidean Langevin dynamics.

Since the hybrid forward process has no closed-form transition density, the authors introduce **Approximate Denoising Score Matching (ADSM)**. It estimates the transition distribution from simulated diffusion trajectories and trains a U-Net score network. The framework can also support conditional generation through classifier-free guidance.

Experiments on **COIL-100, cardiac MRI, and MNIST** show that ILDM consistently outperforms standard LDM and image-space diffusion models. For example, ILDM achieves FID/LPIPS scores of **154.12/0.5049** on COIL-100, **162.94/0.5118** on cardiac MRI, and **136.70/0.6816** on MNIST, outperforming the compared baselines in each case.

Overall, the paper shows that incorporating **manifold geometry and mapping uncertainty directly into the diffusion process** can substantially improve generative quality, particularly when training data is limited. :contentReference[oaicite:0]{index=0}
