## Proof of the Composition Theorem ^sec-proof-composition

This appendix gives the full proof of Theorem [[#^thm-composition]]. The proof composes the four key lemmas of [[#^sec-mechanism]] (full proofs of those in [[#^sec-key-lemma-proofs]]) with a block decomposition at optimum-change events plus Cauchy–Schwarz across blocks.

**Notation.** Throughout this section: $K_t(s) := D_{\mathrm{KL}}(\delta_{a^*_t(s)} \,\|\, Q_t(\cdot \mid s))$ is the per-state reverse-KL coordinate; $d^{Q_t}_h(\cdot \mid s_0)$ is the round-$t$ learner-induced state distribution at horizon step $h$ starting from $s_0$; $\overline{\mathrm{TV}}_t := \tfrac{1}{N_h} \sum_{h=0}^{N_h-1} \mathbb E_{s_h \sim d^{Q_t}_h}[\operatorname{TV}(\pi^*_t(\cdot \mid s_h), Q_t(\cdot \mid s_h))]$ is the occupancy-weighted per-round coordinate of (A5); $B_T = |\{t : a^*_t \ne a^*_{t-1}\}|$ counts optimum-change events; the partition is $0 = \tau_0 < \tau_1 < \cdots < \tau_{B_T} \le \tau_{B_T+1} = T$ with block $i$ length $\Delta_i := \tau_{i+1} - \tau_i$.

### Proof of (i) — Per-round identity-bounded regret

For deterministic $\pi^*_t = \delta_{a^*_t(s)}$ and any $Q_t(\cdot \mid s)$ with $Q_t(a^*_t(s) \mid s) > 0$ at every visited state (which is (A1)), Key Lemma 1 ([[#^lem-pointmass-identity]], full proof in [[#^sec-proof-key-lemma-1]]) gives the exact identity $\operatorname{TV}(\delta_{a^*_t(s)}, Q_t(\cdot \mid s)) = 1 - e^{-K_t(s)}$. Composed with the per-state TV-regret bounds (full proofs in [[#^sec-proof-key-lemma-1]] and [[#^sec-aux-action-gap]]):
$$\Delta_{\min} \cdot \operatorname{TV}(\delta_{a^*_t(s)}, Q_t(\cdot \mid s)) \;\le\; R(Q_t \mid s) \;\le\; V_{\max} \cdot \operatorname{TV}(\delta_{a^*_t(s)}, Q_t(\cdot \mid s)),$$
substituting the identity yields conclusion (i): $\Delta_{\min}(1 - e^{-K_t(s)}) \le R(Q_t \mid s) \le V_{\max}(1 - e^{-K_t(s)})$. $\square$

### Proof of (ii) — Aggregate mismatch ultimate boundedness

Under (A2), Key Lemma 2 ([[#^lem-forgetting]], full proof in [[#^sec-proof-key-lemma-2]]) applied to the diagonal sector model with $\alpha_{ij}^{\mathrm{ss}} = \nu_{ij} \cdot \iota_{ij} \cdot (1 - \lambda_{ij})$ gives ultimate boundedness of $\|\boldsymbol\delta_\Sigma\|$ within $R_\Sigma^* = \rho_\Sigma / \mathcal T_\Sigma^{\mathrm{bn,ss}}$. $\square$

### Proof of (iii) — KL coordinate bias bound

Key Lemma 4 ([[#^lem-bias-bound]], full proof in [[#^sec-proof-key-lemma-4]]) gives the per-state bias bound directly, under (A3)'s identifiability assumption (which gives $p_{\mathrm{id}}(s)$ for each $s$) and the two-point support condition $Q_t(\cdot \mid s) \ge q_0$ at $a^*_{\mathrm{ag},t}(s)$ and $\tilde a^*_t(s)$. $\square$

### Proof of (iv) — 2$\times$2 corrective-action routing

(A4) states the agent applies the satisfaction-gap / control-regret 2$\times$2 (Section [[#^sec-four-components]] Component 1). Conclusion (iv) is a direct restatement of the 2$\times$2 cell partition. The decomposition is convention-dependent (the values of $\delta_{\mathrm{sat}}$ and $\delta_{\mathrm{regret}}$ depend on the continuation convention; see [[#^sec-aux-conventions]] for monotonicity across C1/C2/C3); the 2$\times$2 *structure* is preserved across all three conventions. $\square$

### Proof of (v) — Trajectory-level cumulative regret

We chain Key Lemmas 1 and 4 with the simulation lemma and a Cauchy–Schwarz block argument. The proof is in four steps; the high-level sketch is in [[#^sec-rate-combination]].

*Step 1: Simulation / performance-difference lemma.* For any two policies $\pi$ and $\pi'$, any state $s$, and finite-horizon non-discounted MDP with horizon $N_h$, the standard performance-difference lemma \cite{kakade-2002-approximately, munos-2003-error, ross-2010-efficient, azar-2017-minimax} gives
$$V^{\pi'}_t(s) - V^{\pi}_t(s) \;=\; \mathbb E\!\left[\sum_{h=0}^{N_h-1} \Bigl(\mathbb E_{a' \sim \pi'(\cdot \mid s_h)}[Q^{\pi'}_h(s_h, a')] - Q^{\pi'}_h(s_h, a_h)\Bigr) \,\Bigm|\, s_0 = s,\ a_h \sim \pi\right].$$
Each per-step bracket is bounded by value range times TV: $\bigl|\mathbb E_{a' \sim \pi'}[Q^{\pi'}_h] - \mathbb E_{a \sim \pi}[Q^{\pi'}_h]\bigr| \le V_{\max} \cdot \operatorname{TV}(\pi'(\cdot \mid s_h), \pi(\cdot \mid s_h))$. Applied at round $t$ with $\pi' = \pi^*_t$, $\pi = Q_t$:
$$V^{\pi^*_t}_t(s_0) - V^{Q_t}_t(s_0) \;\le\; V_{\max} \sum_{h=0}^{N_h-1} \mathbb E_{s_h \sim d^{Q_t}_h(\cdot \mid s_0)}\bigl[\operatorname{TV}(\pi^*_t(\cdot \mid s_h), Q_t(\cdot \mid s_h))\bigr] \;=\; V_{\max} \cdot N_h \cdot \overline{\mathrm{TV}}_t.$$
The horizon factor $N_h$ here is the *linear-in-$N_h$* simulation-lemma penalty — sharper than the $N_h^2$ of TRPO-style worst-state bounds \cite{schulman-2015-trust}, which bound max-state TV rather than occupancy-weighted TV.

*Step 2: Per-state TV identity along the trajectory.* By the deterministic-per-state optimum scope of [[#^thm-composition]] ($\pi^*_t(\cdot \mid s) = \delta_{a^*_t(s)}$), at every visited state Key Lemma 1 gives $\operatorname{TV}(\pi^*_t(\cdot \mid s), Q_t(\cdot \mid s)) = 1 - Q_t(a^*_t(s) \mid s) = 1 - e^{-K_t(s)}$. Summing over the trajectory and taking expectations:
$$\overline{\mathrm{TV}}_t \;=\; \tfrac{1}{N_h} \sum_{h=0}^{N_h-1} \mathbb E_{s_h \sim d^{Q_t}_h}[1 - e^{-K_t(s_h)}].$$
The per-state KL/TV identity is preserved underneath the occupancy aggregation; the per-round coordinate is sharper than Pinsker/BH at every visited state.

*Step 3: Block-sum + Cauchy–Schwarz across blocks.* Within each block $[\tau_i, \tau_{i+1})$ of length $\Delta_i$, (A5)'s restart-on-change identity-tight base learner gives $\sum_{t \in \mathrm{block}_i} \mathbb E[\overline{\mathrm{TV}}_t] \le 2c \sqrt{\Delta_i}$. Summing over the $B_T + 1$ blocks:
$$\sum_{t=1}^T \mathbb E[\overline{\mathrm{TV}}_t] \;=\; \sum_{i=0}^{B_T} \sum_{t \in \mathrm{block}_i} \mathbb E[\overline{\mathrm{TV}}_t] \;\le\; 2c \sum_{i=0}^{B_T} \sqrt{\Delta_i}.$$
Cauchy–Schwarz gives $\sum_{i=0}^{B_T} \sqrt{\Delta_i} \le \sqrt{(B_T + 1) \cdot \sum_i \Delta_i} = \sqrt{(B_T + 1) T}$. Multiplying by $V_{\max} N_h$:
$$\sum_{t=1}^T \mathbb E[V_{\max} N_h \overline{\mathrm{TV}}_t] \;\le\; 2c V_{\max} N_h \sqrt{(B_T + 1) T}.$$
This is the rate term.

*Step 4: Bias term via per-state, per-horizon-step lift of Key Lemma 4.* Key Lemma 4 ([[#^lem-bias-bound]]) applies pointwise at any visited state $s_h$ along a horizon-$N_h$ trajectory: $\mathbb E[\lvert \hat D_t(s_h) - D_t^{\mathrm{true}}(s_h) \rvert] \le (1 - p_{\mathrm{id}}(s_h)) \log(1/q_0)$. Aggregating linearly over the $N_h$ horizon steps and assuming the per-state argmax-correctness floor $p_{\mathrm{id}} := \min_s p_{\mathrm{id}}(s)$, the per-round bias contribution to the trajectory-level value gap is at most $N_h (1 - p_{\mathrm{id}}) \log(1/q_0)$. Summing over $T$ rounds: $N_h (1 - p_{\mathrm{id}}) \log(1/q_0) \cdot T$. This is the bias term.

*Combining.* Adding Steps 1–4:
$$\mathbb E[\mathrm{DynReg}(T)] \;\le\; 2c V_{\max} N_h \sqrt{(B_T + 1) T} \;+\; N_h (1 - p_{\mathrm{id}}) \log(1/q_0) \cdot T,$$
which is conclusion (v). $\square$

**Cesàro tracking corollary.** Dividing through by $T$: $\tfrac{1}{T} \sum_t (V^{\pi^*_t}_t - V^{Q_t}_t) \le 2c V_{\max} N_h \sqrt{(B_T+1)/T} + N_h(1-p_{\mathrm{id}})\log(1/q_0)$. As $T \to \infty$ with $B_T = o(T)$ and $p_{\mathrm{id}} \to 1$, the right side tends to zero — the agent's average value over the trajectory tracks the moving optimum's average value. $\square$

**Bandit case sharpening (Remark).** In the bandit special case ($N_h = 1$, so $\overline{\mathrm{TV}}_t = \mathbb E[1 - e^{-K_t}]$), Thompson sampling and UCB ([[#^sec-algorithm]]) satisfy (A5) with the *stronger logarithmic* per-block rate $\mathbb E[1 - e^{-K_t}] = O(\log t / (t \, \Delta_{\min}))$. Substituting and summing gives cumulative regret $O(V_{\max} (B_T + 1) \log^2(T/(B_T+1)) / \Delta_{\min})$ — sharper than $\sqrt{(B_T + 1) T}$ when $\Delta_{\min}$ is bounded away from zero. The square-root rate is the worst-case Cauchy–Schwarz bound; the logarithmic rate is the typical-case bound for stochastic-bandit base learners. For $N_h > 1$ MDPs, UCRL2 and UCBVI provide the natural occupancy-weighted instantiation ([[#^sec-algorithm]]) with $c$ of order $\tilde O(\sqrt{N_h S A})$, lifting cleanly to $\tilde O(N_h^2 \sqrt{SA(B_T+1)T})$ — same piecewise-stationary $B_T$ family as restart-on-change variants of \cite{cheung-2020-reinforcement}.
