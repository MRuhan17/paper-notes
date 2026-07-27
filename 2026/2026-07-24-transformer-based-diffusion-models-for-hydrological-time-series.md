# Transformer-based Diffusion Models for Hydrological Time Series Probabilistic Imputation and Forecasting

## Metadata

- **Authors:** Ferdinand Bhavsar, Lionel Benoit, Maxime Savatier, Edith Gabriel
- **Publication Year:** 2026
- **Venue:** arXiv (stat.ML)
- **Read Date:** 2026-07-24
- **Topics:** Diffusion Models, Transformers, Time Series, Hydrology, Forecasting, Imputation
- **Status:**  Read

---

# Abstract (My Summary)

This paper proposes an improved Transformer-based Conditional Score-based Diffusion Model (CSDI) for hydrological time series. Unlike previous approaches that focus on either forecasting or missing-value imputation, the proposed architecture performs both tasks using a single probabilistic model. The authors redesign the original CSDI architecture by introducing Residual U-blocks, improved conditioning, Fourier-based temporal encoding, RMSNorm, and optional weather covariates. Experiments on both synthetic and real hydrological datasets demonstrate better forecasting and imputation performance than Gaussian Processes, the original CSDI model, U-Net, NHits, and TSMixer.

---

# Problem

Hydrological time series often contain:

- Missing observations due to sensor failures
- Long temporal dependencies
- Strong correlations across multiple variables
- Large uncertainty
- Highly non-linear behavior

Most existing models solve either forecasting or imputation separately and struggle with uncertainty estimation.

---

# Motivation

Water-resource management depends on accurate and reliable predictions. Missing sensor readings reduce data quality, while deterministic forecasting methods fail to represent uncertainty. A unified probabilistic model capable of both forecasting and imputing missing values would be valuable for environmental monitoring and decision-making.

---

# Main Idea

The authors improve the Conditional Score-based Diffusion Model (CSDI) to better handle complex multivariate hydrological data.

Key improvements include:

- Residual U-blocks for local feature extraction
- Transformer attention for long-range dependencies
- StyleGAN-inspired conditioning mechanism
- Fourier temporal encoding for periodic behavior
- RMSNorm for stable optimization
- Weather covariates for better forecasting

---

# Method

### Base Model

Conditional Score-based Diffusion Model (CSDI)

### Architecture Improvements

- Residual U-blocks
- Transformer backbone
- RMSNorm
- Fourier periodic encoding
- Enhanced conditioning
- Optional weather covariates

### Tasks

- Missing-value imputation
- Time-series forecasting

### Datasets

- Synthetic hydrological dataset
- OPE hydrological monitoring dataset (France)

### Baselines

- Gaussian Processes
- Original CSDI
- U-Net
- NHits
- TSMixer

---

# Results

- Custom CSDI consistently outperformed the original CSDI.
- Better forecasting accuracy than most baseline models.
- Better imputation performance, especially with high missing-data ratios.
- Weather covariates noticeably improved forecasting.
- Successfully captured periodic temporal patterns.
- Gaussian Processes remained competitive for very long missing segments.

---

# Strengths

- Unified architecture for two tasks.
- Strong uncertainty estimation.
- Stable diffusion training.
- Extensive comparison with classical and deep-learning baselines.
- Practical architectural improvements.

---

# Limitations

- Evaluated only on hydrological datasets.
- Computationally expensive due to diffusion sampling.
- Performance decreases for very long missing gaps.
- Forecast quality depends on the availability of external weather covariates.

---

# Key Takeaways

- Diffusion models are effective for probabilistic time-series modeling.
- Architectural changes can significantly outperform the original CSDI.
- Explicit periodic encoding helps capture seasonal patterns.
- External covariates improve forecasting under changing environmental conditions.

---

# Interesting Ideas

- Residual U-blocks inside diffusion transformers.
- StyleGAN-inspired conditioning for time-series generation.
- Fourier temporal encoding for hydrological cycles.
- One model handling both forecasting and imputation.

---

# Questions

- Would linear attention improve efficiency?
- Can this architecture scale to much longer sequences?
- Could the model benefit from FlashAttention?
- How well would it perform on financial or medical time-series data?

---

# Future Work

- Evaluate on additional domains.
- Improve long-range forecasting.
- Better handling of very long missing intervals.
- Reduce computational cost during inference.

---

# My Thoughts

I found the architectural modifications more interesting than the application domain itself. Instead of proposing an entirely new diffusion model, the authors systematically improved CSDI with practical engineering changes that collectively produced strong gains. The idea of combining convolutional blocks for local patterns with transformer attention for global dependencies seems broadly applicable beyond hydrology. It would be interesting to see whether similar improvements transfer to domains such as healthcare, finance, or AI-agent trajectory modeling.

---

# Related Papers

- Conditional Score-based Diffusion Models (CSDI)
- Denoising Diffusion Probabilistic Models (DDPM)
- Attention Is All You Need
- U²-Net
- StyleGAN

---

# Rating

| Category | Rating |
|----------|--------|
| Novelty | ⭐⭐⭐⭐☆ |
| Technical Depth | ⭐⭐⭐⭐⭐ |
| Clarity | ⭐⭐⭐⭐☆ |
| Practical Impact | ⭐⭐⭐⭐☆ |
| Overall | ⭐⭐⭐⭐☆ (8.8/10) |
