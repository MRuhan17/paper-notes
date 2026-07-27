# Kernelized Linear Attention: Breaking the Capacity Wall with Symmetric Cones

## Metadata

- **Authors:** Ayoub Ghriss, Sourav Chakraborty
- **Publication Year:** 2026
- **Read Date:** 2026-07-26
- **Venue:** arXiv (cs.LG)
- **Paper:** https://arxiv.org/abs/2607.17419
- **Code:** https://github.com/ayghri/kata
- **Topics:** Linear Attention, Transformers, State Space Models, Kernel Methods, Long Context, Associative Memory
- **Status:**  Read

---

# Abstract (My Summary)

This paper introduces **KATA (Kernelized Linear Attention Activations)**, a new framework for designing linear attention feature maps using principles from geometry instead of heuristic engineering. The authors model associative recall as a spherical packing problem and derive attention kernels from self-dual symmetric cones, leading to theoretically justified feature maps with improved memory capacity. The framework introduces PSD (Positive Semi-Definite) feature maps that significantly reduce interference between stored memories while maintaining linear-time inference. Alongside the theoretical analysis, the paper presents optimized Triton kernels that outperform FlashAttention-2 at very long sequence lengths and demonstrates superior associative recall compared with recent linear attention architectures such as Gated DeltaNet.

---

# Problem

Traditional softmax attention provides excellent associative recall but suffers from:

- Quadratic time complexity
- Quadratic memory usage
- Large KV-cache during inference

Linear attention removes these bottlenecks but introduces a new problem:

- Memory collisions caused by storing many key-value pairs in a fixed-size recurrent state.

As context length grows, recall quality deteriorates significantly.

---

# Motivation

Recent architectures such as:

- Mamba
- DeltaNet
- Gated DeltaNet

improve linear attention using gates and state updates, but there is little theoretical understanding of:

- Why certain feature maps work.
- What fundamentally limits recall capacity.
- How feature geometry affects memory interference.

The authors aim to build a principled theory for linear attention rather than relying on empirical design choices.

---

# Main Idea

The key insight is that associative recall can be viewed as a **spherical packing problem**.

Instead of designing feature maps heuristically, KATA derives them from **self-dual homogeneous cones**.

The framework studies three symmetric cone geometries:

- Positive Orthant
- Lorentz Cone
- Positive Semi-Definite (PSD) Cone

Among these, **rank-one PSD features** provide the best trade-off between capacity and interference, enabling many more distinguishable memories without increasing the number of learned parameters.

---

# Method

## Step 1

Replace softmax attention with kernelized linear attention.

---

## Step 2

Represent feature maps inside symmetric cones that guarantee:

- Non-negative attention weights
- Geometric consistency
- Stable normalization

---

## Step 3

Use rank-one PSD feature maps:

\[
\psi(u)=uu^T
\]

which reduce interference by squaring similarities between keys.

---

## Step 4

Analyze memory capacity using:

- Welch Bound
- Rankin Bound
- Spherical Packing Theory
- Signal-to-Noise Ratio analysis

---

## Step 5

Implement optimized Triton kernels for efficient GPU execution.

Two execution modes are provided:

- Quadratic Flash-style kernel
- Linear recurrent state kernel

---

# Architecture

The KATA framework consists of:

- Kernelized feature mapping
- PSD feature construction
- Recurrent linear state update
- Convex output gate
- Chunkwise associative scan
- Triton GPU kernels

Unlike many linear attention variants, the output gate naturally emerges from normalization instead of requiring additional learned parameters. :contentReference[oaicite:0]{index=0}

---

# Theoretical Contributions

The paper proves:

- Linear attention recall is fundamentally limited by memory interference.
- Attention recall can be formulated as a spherical packing problem.
- PSD feature maps reduce interference compared with orthant or Lorentz features.
- Rank-one PSD features achieve much larger associative capacity.
- Memory capacity follows Welch-style interference bounds.

---

# Hardware Optimizations

The authors introduce:

- Flash-KATA
- KATA-SSM
- Chunkwise associative scan
- Triton fused kernels

Performance highlights include:

- Up to **1.6×** FlashAttention-2 forward throughput for the quadratic kernel.
- Around **11×** FlashAttention-2 forward throughput for the linear-state implementation at 131k-token sequences.
- Approximately **2.4×** speedup over a sequential linear-attention chunk implementation using the associative scan. :contentReference[oaicite:1]{index=1}

---

# Experiments

Evaluation includes:

- MQAR (Multi-Query Associative Recall)
- Repeated-key overwrite
- Long-range induction
- 340M-parameter language models
- Long-context recall
- Throughput benchmarks

---

# Results

Main findings include:

- KATA outperforms Gated DeltaNet on several associative recall benchmarks.
- KATA-M1 achieves **0.985 MQAR accuracy** at **16×** the training context while using roughly one-quarter of the KV-cache entries required by softmax attention.
- DeltaKATA-M1 reaches **0.999** accuracy on the largest MQAR setting.
- PSD feature maps retain much stronger recall than previous linear attention variants.
- The proposed kernels scale efficiently to contexts exceeding 100k tokens. :contentReference[oaicite:2]{index=2}

---

# Strengths

- Strong mathematical foundation.
- First-principles design instead of heuristic feature maps.
- Excellent long-context recall.
- Efficient GPU implementation.
- Extensive theoretical analysis.
- Competitive hardware performance.
- Bridges geometry and deep learning.

---

# Limitations

- Heavy mathematical background makes the paper difficult to read.
- Focuses mainly on linear attention rather than general Transformer improvements.
- PSD feature maps require larger recurrent states.
- Additional implementation complexity compared with standard attention.

---

# Key Takeaways

- Geometry strongly influences memory capacity in linear attention.
- Associative recall can be analyzed using spherical packing theory.
- PSD feature maps reduce memory interference.
- Linear attention can approach softmax recall while retaining linear complexity.
- Efficient hardware implementation is essential for practical deployment.

---

# Interesting Ideas

- Viewing memory retrieval as a spherical packing problem.
- Using symmetric cone theory to derive attention kernels.
- Rank-one PSD feature maps.
- Parameter-free convex output gating.
- Associative scan for parallel chunk processing.

---

# Questions

- Can PSD feature maps improve multimodal Transformers?
- How would KATA compare against modern architectures like Kimi Linear or RWKV-8?
- Can higher-order PSD mappings further increase capacity?
- Could similar geometric principles improve retrieval-augmented generation?

---

# Future Work

The authors suggest exploring:

- Complex and quaternionic symmetric cones.
- Better spherical packing constructions.
- More efficient PSD implementations.
- Parallel scan methods for full PSD recurrences.
- Scaling KATA to larger language models.

---

# My Thoughts

This is one of the most theoretically grounded linear-attention papers I've read. Rather than proposing another architectural tweak, it asks a fundamental question: *What actually limits the memory capacity of linear attention?* The geometric interpretation of associative recall as a spherical packing problem is both elegant and convincing, and it provides a principled explanation for why some feature maps outperform others. I also appreciated that the paper complements its theory with optimized Triton kernels and extensive experiments. Although the mathematics is dense, the work advances both the theoretical understanding and practical implementation of efficient attention mechanisms.

---

# Related Papers

- Attention Is All You Need
- Performer
- Linear Transformers
- Mamba
- DeltaNet
- Gated DeltaNet
- FlashAttention
- FlashAttention-2

---

# Rating

| Category | Rating |
|----------|--------|
| Novelty | ⭐⭐⭐⭐⭐ |
| Technical Depth | ⭐⭐⭐⭐⭐ |
| Clarity | ⭐⭐⭐⭐☆ |
| Practical Impact | ⭐⭐⭐⭐⭐ |
| Overall | ⭐⭐⭐⭐⭐ (9.6/10) |
