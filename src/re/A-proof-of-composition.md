## Proof of the Composition Theorem ^sec-proof-composition

This appendix gives the full proof of [[#^thm-composition]]. The proof composes the four key lemmas of [[#^sec-mechanism]] (full proofs of those in [[#^sec-key-lemma-proofs]]) with a block decomposition at optimum-change events plus Cauchy–Schwarz across blocks.

**Notation.** Throughout this section: $K_t(s) := D_{\mathrm{KL}}(\delta_{a^*_t(s)} \,\|\, Q_t(\cdot \mid s))$ is the per-state reverse-KL coordinate; $d^{Q_t}_h(\cdot \mid s_0)$ is the round-$t$ learner-induced state distribution at horizon step $h$ starting from $s_0$; $\overline{\mathrm{TV}}_t := \tfrac{1}{N_h} \sum_{h=0}^{N_h-1} \mathbb E_{s_h \sim d^{Q_t}_h}[\operatorname{TV}(\pi^*_t(\cdot \mid s_h), Q_t(\cdot \mid s_h))]$ is the occupancy-weighted per-round coordinate of (A5); $B_T = |\{t : (P_t, r_t) \ne (P_{t-1}, r_{t-1})\}|$ counts piecewise-stationary segment boundaries (per [[#^sec-preliminaries]]); the partition is $0 = \tau_0 < \tau_1 < \cdots < \tau_{B_T} \le \tau_{B_T+1} = T$ with block $i$ length $\Delta_i := \tau_{i+1} - \tau_i$.

### Proof of (i) — Per-round identity-bounded regret

For deterministic $\pi^*_t = \delta_{a^*_t(s)}$ and any $Q_t(\cdot \mid s)$, Key Lemma 1 ([[#^lem-pointmass-identity]], full proof in [[#^sec-proof-key-lemma-1]]) gives the exact identity $\operatorname{TV}(\delta_{a^*_t(s)}, Q_t(\cdot \mid s)) = 1 - e^{-K_t(s)}$ in the extended real sense (when $Q_t(a^*_t(s) \mid s) = 0$, $K_t(s) = +\infty$ and both sides equal $1$, recovering the trivial bound). Composed with the per-state TV-regret bounds (full proofs in [[#^sec-proof-key-lemma-1]] and [[#^sec-aux-action-gap]]):
$$\Delta_{\min} \cdot \operatorname{TV}(\delta_{a^*_t(s)}, Q_t(\cdot \mid s)) \;\le\; R(Q_t \mid s) \;\le\; V_{\max} \cdot \operatorname{TV}(\delta_{a^*_t(s)}, Q_t(\cdot \mid s)),$$
substituting the identity yields conclusion (i): $\Delta_{\min}(1 - e^{-K_t(s)}) \le R(Q_t \mid s) \le V_{\max}(1 - e^{-K_t(s)})$. $\square$

### Proof of (ii) — Aggregate mismatch ultimate boundedness

Under (A2), Key Lemma 2 ([[#^lem-forgetting]], full proof in [[#^sec-proof-key-lemma-2]]) applied to the diagonal sector model with $\alpha_{ij}^{\mathrm{ss}} = \nu_{ij} \cdot \iota_{ij} \cdot (1 - \lambda_{ij})$ gives ultimate boundedness of $\|\boldsymbol\delta_\Sigma\|$ within $R_\Sigma^* = \rho_\Sigma / \mathcal T_\Sigma^{\mathrm{bn,ss}}$. $\square$

### Proof of (iii) — KL coordinate bias bound

Key Lemma 4 ([[#^lem-bias-bound]], full proof in [[#^sec-proof-key-lemma-4]]) gives the per-state bias bound directly, under (A3)'s identifiability assumption (which gives $p_{\mathrm{id}}(s)$ for each $s$) and the two-point support condition $Q_t(\cdot \mid s) \ge q_0$ at $a^*_{\mathrm{ag},t}(s)$ and $\tilde a^*_t(s)$. $\square$

### Proof of (iv) — 2$\times$2 corrective-action routing

(A4) states the agent applies the satisfaction-gap / control-regret 2$\times$2 ([[#^sec-four-components]] Component 1). Conclusion (iv) is a direct restatement of the 2$\times$2 cell partition. The decomposition is convention-dependent (the values of $\delta_{\mathrm{sat}}$ and $\delta_{\mathrm{regret}}$ depend on the continuation convention; see [[#^sec-aux-conventions]] for monotonicity across C1/C2/C3); the 2$\times$2 *structure* is preserved across all three conventions. $\square$

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

*Step 4: Misidentification penalty via simulation lemma at the identified-vs-true-optimum gap.* Steps 1–3 implicitly compared the agent's policy $Q_t$ to the *agent's identified-optimum policy* $\pi^*_{\mathrm{ag},t}(\cdot \mid s) := \delta_{a^*_{\mathrm{ag},t}(s)}$ (since (A5)'s base-learner guarantee is what the agent achieves on its identified objective). The trajectory-level dynamic regret in (v) compares to the *true* optimum $\tilde\pi^*_t(\cdot \mid s) := \delta_{\tilde a^*_t(s)}$. Decompose:
$$V^{\tilde\pi^*_t}_t(s_0) - V^{Q_t}_t(s_0) \;=\; \bigl[V^{\tilde\pi^*_t}_t(s_0) - V^{\pi^*_{\mathrm{ag},t}}_t(s_0)\bigr] \;+\; \bigl[V^{\pi^*_{\mathrm{ag},t}}_t(s_0) - V^{Q_t}_t(s_0)\bigr].$$
Steps 1–3 bound the second bracket. The first bracket is the misidentification penalty. Apply the simulation lemma to the two point-mass policies $(\tilde\pi^*_t, \pi^*_{\mathrm{ag},t})$: each per-step bracket reduces to
$$Q^{\tilde\pi^*_t}_h(s_h, \tilde a^*_t(s_h)) - Q^{\tilde\pi^*_t}_h(s_h, a^*_{\mathrm{ag},t}(s_h)) \;\in\; [0,\, V_{\max}] \cdot \mathbf 1[\tilde a^*_t(s_h) \ne a^*_{\mathrm{ag},t}(s_h)].$$
Lower bound by optimality of $\tilde a^*$; upper bound by the cumulative-Q range with the indicator on the misidentification event. Taking expectations, with the per-state floor $p_{\mathrm{id}} := \min_s p_{\mathrm{id}}(s)$:
$$\mathbb E\!\bigl[V^{\tilde\pi^*_t}_t(s_0) - V^{\pi^*_{\mathrm{ag},t}}_t(s_0)\bigr] \;\le\; V_{\max} \cdot N_h \cdot (1 - p_{\mathrm{id}}).$$
Summing over $T$ rounds: $V_{\max} \cdot N_h \cdot (1 - p_{\mathrm{id}}) \cdot T$. This is the bias term, in *value coordinates* throughout — no detour through the KL coordinate. Key Lemma 4's KL-readout bound is the right tool for conclusion (iii)'s diagnostic computability; for the value-coordinate misidentification penalty here, the indicator decomposition is direct and tighter (the support condition $Q_t \ge q_0$ at the two argmax candidates is *not* required for this term — only conclusion (iii) needs it).

*Combining.* Adding Steps 1–4:
$$\mathbb E[\mathrm{DynReg}(T)] \;\le\; 2c\, V_{\max}\, N_h \sqrt{(B_T + 1)\, T} \;+\; V_{\max}\, N_h\, (1 - p_{\mathrm{id}})\, T,$$
which is conclusion (v). $\square$

**Cesàro tracking corollary.** Dividing through by $T$: $\tfrac{1}{T} \sum_t (V^{\tilde\pi^*_t}_t - V^{Q_t}_t) \le 2c V_{\max} N_h \sqrt{(B_T+1)/T} + V_{\max} N_h(1-p_{\mathrm{id}})$. As $T \to \infty$ with $B_T = o(T)$ and $p_{\mathrm{id}} \to 1$ (Regime A), the right side tends to zero — the agent's average value over the trajectory tracks the moving optimum's average value. In Regime B the second term is constant; tracking is only up to a $V_{\max}(1-p_{\mathrm{id}})$ residual. $\square$

**Bandit case sharpening (Remark).** In the bandit special case ($N_h = 1$, so $\overline{\mathrm{TV}}_t = \mathbb E[1 - e^{-K_t}]$), Thompson sampling and UCB ([[#^sec-algorithm]]) satisfy (A5) with the *stronger logarithmic* per-block rate. By \citet{lattimore-2020-bandit} Theorem 7.1 (or \citealp{auer-cesa-bianchi-fischer-2002-finitetime}), $\mathbb E[N_a(L)] = O(\log L / \Delta_a^2)$ per suboptimal arm $a$; aggregating over arms gives $\mathbb E[1 - e^{-K_t}] = \mathbb E[1 - Q_t(a^*)] = O(\log t / (t\, \Delta_{\min}^2))$ per stationary block. Substituting and summing gives cumulative regret $O(V_{\max} (B_T + 1) \log(T/(B_T+1)) / \Delta_{\min}^2)$ — sharper than the worst-case $\sqrt{(B_T + 1) T}$ when $\Delta_{\min}$ is bounded away from zero. The framework's $V_{\max} \cdot \operatorname{TV}$ chain is structurally one factor of $\Delta_{\min}$ looser than direct gap-aware UCB analysis (which gives $O(\log T / \Delta_{\min})$ cumulative regret per block by weighting each suboptimal pull by $\Delta(a)$ rather than $V_{\max}$); this is a feature of the unification, not a bug. For $N_h > 1$ MDPs, UCRL2 and UCBVI provide the natural occupancy-weighted instantiation ([[#^sec-algorithm]]) with $c$ of order $\tilde O(\sqrt{N_h S A})$, lifting cleanly to $\tilde O(N_h^2 \sqrt{SA(B_T+1)T})$ — same piecewise-stationary $B_T$ family as restart-on-change variants of \cite{cheung-2020-reinforcement}.

**Best-of-Both-Worlds non-stationarity extension (Remark on (A5')).** Conclusion (v) above uses the restart-on-change form of (A5). The per-round identity route is *stationarity-agnostic* — Steps 1, 2, 4 above hold per round regardless of cross-round dynamics; only Step 3's block-Cauchy–Schwarz aggregator is piecewise-stationary-specific. Replacing (A5) by

> **(A5')** *MASTER-wrapped base learner.* The base learner is the \citet{wei-luo-2021-blackbox} MASTER black-box wrapping of any base algorithm satisfying their Assumption 1 (a near-stationary regret guarantee with $C(t) = c_1 t^p + c_2$ for $p \in [1/2, 1)$, plus an auxiliary-quantity output condition; UCRL2, UCBVI, and Thompson sampling all qualify), giving stationary regret $\sum_{t \in W} \mathbb E[\overline{\mathrm{TV}}_t] \le 2c\sqrt{|W|}$ within each MASTER-selected window $W$.

substitutes Step 3's block-Cauchy–Schwarz aggregator with MASTER's adaptive-window aggregator (parallel base-learner instances at exponentially-spaced window sizes $W_k = 2^k$, online meta-learner over the $\log T$ instances). The wrapping is mechanical: the per-round identity (Steps 1–2) is preserved unchanged at each round; the misidentification penalty (Step 4) is round-additive and rides through; only the aggregator changes. The resulting rate, by Wei-Luo Theorem 2 applied with our framework's $C(t) = 2c V_{\max} N_h \sqrt t$ at the boundary $p = 1/2$, is
$$\mathbb E[\mathrm{DynReg}(T)] \;\le\; \tilde O\!\left(V_{\max}\, N_h \cdot \min\Big\{\sqrt{(B_T+1) T},\ (V_T+1)^{1/3} T^{2/3}\Big\}\right) \;+\; V_{\max}\, N_h\, (1 - p_{\mathrm{id}})\, T,$$
*automatically* — without prior knowledge of $B_T$ or $V_T$. The $V_T^{1/3} T^{2/3}$ exponent matches \cite{mao-2021-nearoptimal}'s near-optimal RestartQ-UCB rate and is information-theoretically near-optimal: the \cite{besbes-gur-zeevi-2014-stochastic} lower bound $\Omega(K^{1/3} V_T^{1/3} T^{2/3})$ for non-stationary stochastic bandits applies *also at the deterministic-π* corner* (their construction is a Bernoulli MAB with dynamic oracle playing the per-round argmax — deterministic-per-round optimum, identical to our canonical scope), so the per-round identity sharpens *constants and per-round form*, not the V_T exponent. The two regimes are *not* structurally distinct: they share the per-round coordinate $1 - e^{-K_t}$ and differ only in the aggregation strategy. $\square$

### Component-by-component failure-mode analysis ^sec-necessity

The headline §4.3 unpacking observes that the four components compose into a *compatible bundle*: the rate (v) goes through (A5) and (i) alone, while the joint properties — rate plus persistence plus learnability plus corrective-action routing — require all four. This appendix unpacks the failure modes component-by-component for the natural reviewer question "why exactly four?".

**Without Component 2 (point-mass identity).** The per-round bound is Pinsker $V_{\max}\sqrt{D_{\mathrm{KL}}/2}$ or BH $V_{\max}\sqrt{1 - e^{-D_{\mathrm{KL}}}}$ — both strictly looser at this corner; Pinsker is vacuous for $D_{\mathrm{KL}} > 2$. The metric layer loses *exactness*, and the "behavior-cloning loss against optimal trajectory" interpretation of the cumulative coordinate vanishes.

**Without Component 3 (strategic tempo).** The agent has no persistence guarantee. By the structural-class theorem on $\mathcal A_{\mathrm{decay}}$ ([[#^sec-aux-decay-class]]), every count-accumulating update without forgetting eventually violates the threshold for any positive disturbance; the modeled mismatch $\|\boldsymbol\delta_\Sigma\|$ grows unboundedly. The per-round bound (i) still holds, but $K_t(s)$ may itself drift — there is no guarantee the per-round coordinate stays bounded.

**Without Component 4 (loop-Level-2).** The bound's RHS is computable from $Q_t$, but its interpretation as regret-against-true-optimum requires identifying $a^*_t(s)$ — an interventional query under the environment's transition kernel. In Regime C ($\iota \approx 0$), the misidentification penalty saturates at $V_{\max}$ per state per step, contributing $V_{\max} N_h \cdot T$ to cumulative regret — the rate (v) becomes vacuous.

**Without Component 1 (two-gap diagnostic).** The regret signal $\delta_{\mathrm{regret}}$ alone does not distinguish "policy revision is the right move" from "the goal is unattainable from $M_t$." An agent might persist in policy revision when the model, policy class, or horizon is the actual problem (Capability-limit cell), or revise the goal when policy revision would have sufficed.

So while the rate (v) goes through (A5) and (i) alone, the *bundle* — rate plus persistence plus learnability plus corrective-action routing — requires all four. Each drop is a concrete loss of an epistemic property that the field treats as separate.
