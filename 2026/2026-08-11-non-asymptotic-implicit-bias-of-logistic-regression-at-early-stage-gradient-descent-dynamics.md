# 2026-08-11-non-asymptotic-implicit-bias-of-logistic-regression-at-early-stage-gradient-descent-dynamics.md

**Status:** Read

## Summary

This paper studies the **implicit bias of gradient descent in logistic regression**, focusing on what happens during the **early stages of training** rather than only in the asymptotic regime.

For linearly separable data, gradient descent is known to eventually make the parameter direction converge to the **max-margin direction**. However, this asymptotic convergence is extremely slow, with the alignment error decreasing roughly as \(O(1/\log^2 t)\). The paper argues that this asymptotic analysis misses an important practical phenomenon: the parameter direction can become substantially aligned with the max-margin direction **much earlier** than perfect asymptotic convergence would suggest.

The authors decompose the parameter into two components: a **radial component** \(r(t)=\|\mathbf w(t)\|\), which controls the magnitude of the weights, and a **tangential component** \(\mathbf u(t)=\mathbf w(t)/\|\mathbf w(t)\|\), which controls their direction. Their analysis shows that these components behave very differently. The radial component grows slowly, while the tangential component can rapidly move toward the max-margin direction.

The main result establishes a **two-stage early-training dynamic**. First, if the initialization is poorly aligned with the max-margin direction, gradient descent quickly **escapes the bad initialization**. It then enters a **weak-alignment stage**, where the parameter direction approaches the max-margin direction within

\[
O(\exp(\exp(-\delta)))
\]

iterations for a permissible alignment error \(\delta\). This is substantially faster than the time required for asymptotic near-perfect alignment.

The proof works directly with the alignment quantity

\[
V(t)=1-\langle\mathbf u(t),\mathbf u^*\rangle
\]

rather than relying on the asymptotic expansion traditionally used to establish max-margin convergence. A key geometric lemma shows that the exponentially weighted data retain a positive correlation with the max-margin direction during the relevant range of \(V(t)\), enabling the authors to derive the fast alignment bound.

Importantly, the result is **weak alignment**, not perfect alignment. The analysis cannot guarantee arbitrarily small \(\delta\); the achievable accuracy depends on the dataset geometry and margin. The paper also proves a matching lower bound, showing that the doubly exponential time scale is essentially **tight** for the early-stage dynamics.

Overall, the paper provides a more precise picture of gradient descent training: rather than slowly approaching the max-margin solution from the beginning, the dynamics exhibit a rapid directional alignment phase followed by the much slower asymptotic refinement phase. This helps explain why "train longer, generalize better" can coexist with useful max-margin behavior appearing surprisingly early in training. :contentReference[oaicite:0]{index=0}
