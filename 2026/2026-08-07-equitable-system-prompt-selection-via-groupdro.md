# 2026-08-07-equitable-system-prompt-selection-via-groupdro.md

**Status:** Read

## Summary

This paper proposes a **fairness-oriented system prompt selection framework** for LLMs. Instead of designing a new prompt, the authors assume a pool of existing high-quality system prompts and formulate prompt selection as a **Constrained Mixed-Strategy GroupDRO** optimization problem. The objective is to minimize the **worst-case response quality** across different user groups (e.g., language, literacy level, phrasing style) while ensuring that the average response quality remains nearly unchanged.

Unlike conventional prompt optimization methods that select a single prompt based on average performance, the proposed method assigns **probabilistic weights to multiple prompts**, allowing complementary prompts to compensate for each other's weaknesses. This is formulated as a linear programming problem, making the approach model-agnostic, computationally efficient, and independent of additional model training.

The framework is evaluated on bilingual **medical (MIRA)** and **consumer-finance** benchmarks using five state-of-the-art LLMs. Across all settings, the proposed constrained mixed strategy consistently improves the worst-performing metric-group pairs while preserving overall response quality. The learned prompt weights also reveal that different prompts specialize in improving different evaluation metrics, demonstrating the value of prompt diversity instead of relying on a single universal prompt.

Overall, the paper shows that **selecting and combining existing prompts intelligently is often more effective than searching for a single optimal prompt**, providing a practical way to improve robustness and fairness in real-world LLM deployments. :contentReference[oaicite:0]{index=0}
