## Limitations and Conclusion ^sec-limitations-conclusion

### Limitations ^sec-limitations

*Theory-only*: no original experiments; mitigations are [[#^thm-pointmass-identity]]'s airtight two-line calculation and the ProST reduction ([[#^sec-prost-lift]]). *Canonical scope*: deterministic-$\pi^*$ is essential for the *identity* (BH/Pinsker fall back outside; perturbative extension [[#^sec-perturbative-extension]] covers $\epsilon$-greedy and softmax regimes); tied-optimum sketched in [[#^sec-tied-softmax-extensions]]. *Cumulative-regret scope*: [[#^thm-twosided-regret]] is per-state one-step under C1; [[#^thm-composition]](v) lifts to cumulative via variation-budget blocks under (A5). Pointwise $V(\pi_t)\to V^*$ unavailable under genuine non-stationarity; the Cesàro tracking corollary $\frac{1}{T}\sum_t(V^*_t - V(\pi_t)) = O(\sqrt{(B_T+1)/T})$ is the right replacement. Occupancy-measure convergence requires uniform-mixing analysis across $\{P_t\}$ — out of scope. *Partial uniqueness*: [[#^sec-composition-scope]] establishes the metric layer; joint uniqueness across properties 1–3 is open. *Other*: $\rho_\Sigma, R_\Sigma$ are domain quantities; [[#^thm-loop-level2]]'s loop-Level-2 claim depends on directed-separation between $M_t$ and goal state, so coupled-goal architectures (e.g., goal-conditioned LLM policies) need additional machinery [Bruineberg et al.\ 2022].

> [!todo] Pass-5 (a)(v) anonymization hit
> "directed-separation" is reviewer-priming vocab (same risk B-N8 already swept in commit `0aa533f`). Reframe as `architectural-separation property` or `conditional-independence property` before any submission build. Hard-required for anonymized submission. Tracked in `TODO.md`.

### Future work ^sec-future-work

Stochastic-$\pi^*$ extension beyond the perturbative regime (BH/Pinsker fallback). Tied-optimum extension ([[#^sec-tied-softmax-extensions]]). Coupled goal–model architectures via Bayesian inverse-problem stability [Stuart 2010; Sprungk 2019]. Algorithmic instantiation: a practical algorithm with identity-tight base learner ([[#^sec-algorithm-sketch]]), explicit forgetting schedule, regime check, and $2{\times}2$ readout — empirical evaluation deferred to a follow-up paper.

### Conclusion ^sec-conclusion

We assemble four components — two-gap diagnostic, point-mass reverse-KL/TV identity, multi-factor strategic-tempo forgetting prerequisite, closed-loop interventional access — into a unified non-stationary convergence theory. The composition yields cumulative dynamic regret $\mathrm{DynReg}(T) \le 2cV_{\max}\sqrt{(B_T+1)\,T}$ with per-round coordinate sharper than Pinsker/BH; the cumulative rate matches the variation-budget literature as a corollary. The point-mass identity at the deterministic-$\pi^*$ corner of the \citet{bretagnolle-huber-1978-densities} family is the technical anchor — absent, to our knowledge, from the prior RL/non-stationary corpus.

> [!todo] Pass-5 (a)(i) coherence drift
> The cumulative-rate above is the pre-strengthen form. The body's [[#^thm-composition]](v) carries the $N_h$ horizon factor from the spike-N1 strengthen ($N_h\sqrt{(B_T+1)T}$, matching the simulation-lemma penalty). Conclusion / abstract / §1.1 (iv) bullet should propagate. Tracked in `TODO.md`.
