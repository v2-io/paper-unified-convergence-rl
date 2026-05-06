## Proof Sketches for Theorem 5.1 and Theorem 7.1(v) ^sec-proof-sketches

**[[#^thm-forgetting-prereq]] (multi-factor forgetting prerequisite) — proof sketch.** Per-element dynamic gives expected one-step correction $\mathbb E[\Delta \delta_{ij}] = -\alpha_{ij}^{\mathrm{ss}} \delta_{ij} + w_{ij}$ where $\alpha_{ij}^{\mathrm{ss}} = \nu_{ij}\iota_{ij}(1-\lambda_{ij})$ — the three factors enter as a product because each is an independent gate (probability of test × signal-fraction × update strength). Diagonal correction gives sector product
$$\boldsymbol\delta_\Sigma^\top \mathbf F(\boldsymbol\delta_\Sigma) \;=\; \sum_{(i,j)} \alpha_{ij}^{\mathrm{ss}} \delta_{ij}^2 \;\ge\; \min_{(i,j)} \alpha_{ij}^{\mathrm{ss}} \cdot \|\boldsymbol\delta_\Sigma\|^2 \;=\; \mathcal T_\Sigma^{\mathrm{bn,ss}} \cdot \|\boldsymbol\delta_\Sigma\|^2.$$
The bound is *tight* — adversarial concentration of $\boldsymbol\delta_\Sigma$ on the bottleneck element saturates it. Khalil ultimate-boundedness [Khalil 2002 §9, Lemma 9.2 / Theorem 9.1] applied to the strategic substate then closes the argument: when $\mathcal T_\Sigma^{\mathrm{bn,ss}} > \rho_\Sigma / R_\Sigma$, the modeled mismatch is ultimately bounded at $R_\Sigma^* = \rho_\Sigma / \mathcal T_\Sigma^{\mathrm{bn,ss}}$. *Sharpness*: when reversed, the bottleneck element admits a disturbance $\boldsymbol w$ concentrated on it with $|w_{i^*j^*}| = \rho_\Sigma$, pushing $\|\boldsymbol\delta_\Sigma\| > R_\Sigma$ within the modeled dynamics. $\square$

**[[#^thm-composition]](v) (trajectory-level dynamic regret) — proof sketch.**

*Step 1 — simulation / performance-difference lemma.* For finite-horizon $N_h$ non-stationary MDPs with bounded reward, the standard performance-difference lemma [Kakade–Langford 2002; Munos 2003; Ross–Bagnell 2010; Azar–Osband–Munos 2017] gives, for any two policies $\pi^*_t, Q_t$ and any initial state $s_0$,
$$V^{\pi^*_t}_t(s_0) - V^{Q_t}_t(s_0) \;\le\; V_{\max}\sum_{h=0}^{N_h-1} \mathbb E_{s_h \sim d^{Q_t}_h(\cdot \mid s_0)}\bigl[\,\mathrm{TV}(\pi^*_t(\cdot \mid s_h),\, Q_t(\cdot \mid s_h))\,\bigr] \;=\; V_{\max}\,N_h\, \overline{\mathrm{TV}}_t,$$
where $d^{Q_t}_h$ is the round-$t$ learner-induced state distribution at horizon step $h$ and $\overline{\mathrm{TV}}_t$ is the occupancy-weighted TV defined in (A5).

*Step 2 — per-state TV identity along the trajectory.* By the deterministic-per-state optimum scope ($\pi^*_t(\cdot \mid s) = \delta_{a^*_t(s)}$), at every visited state $\mathrm{TV}(\pi^*_t(\cdot \mid s), Q_t(\cdot \mid s)) = 1 - Q_t(a^*_t(s) \mid s) = 1 - e^{-K_t(s)}$ — the per-state form of [[#^thm-pointmass-identity]].

*Step 3 — block-sum and Cauchy–Schwarz.* Partition $[1, T]$ at optimum-change events $0 = \tau_0 < \tau_1 < \cdots < \tau_{B_T} \le \tau_{B_T+1} = T$. Within each block $[\tau_i, \tau_{i+1})$ of length $\Delta_i$, (A5) gives $\sum_{t \in \mathrm{block}_i} \mathbb E[\overline{\mathrm{TV}}_t] \le 2c\sqrt{\Delta_i}$. Cauchy–Schwarz across the $B_T + 1$ blocks gives $\sum_i \sqrt{\Delta_i} \le \sqrt{(B_T+1)\,T}$.

*Step 4 — bias term.* [[#^thm-bias-bound]] bounds the per-state, per-step bias from misidentified argmax by $(1 - p_{\mathrm{id}})\log(1/q_0)$. Aggregated linearly over the $N_h$ horizon steps and the $T$ rounds, this contributes $N_h(1 - p_{\mathrm{id}})\log(1/q_0)\cdot T$.

Combining Steps 1–4: $\mathbb E[\mathrm{DynReg}(T)] \le 2cV_{\max} N_h \sqrt{(B_T+1)\,T} + N_h(1-p_{\mathrm{id}})\log(1/q_0)\cdot T$. At $B_T = 0$ and $p_{\mathrm{id}} = 1$ this recovers the stationary-trajectory-regret rate $O(V_{\max} N_h \sqrt{T})$. The Cesàro tracking corollary $\frac{1}{T}\sum_t (V^*_t - V(\pi_t)) = O\!\bigl(N_h \sqrt{(B_T+1)/T}\bigr) \to 0$ when $B_T = o(T)$ and $p_{\mathrm{id}} \to 1$ follows by dividing through by $T$. $\square$

In the bandit special case ($N_h = 1$) under Thompson sampling or UCB as the base learner ([[#^sec-algorithm-sketch]]), the per-block stationary rate sharpens to $O(\log\Delta_i / (\Delta_i \Delta_{\min}))$, summing to $O((B_T+1) \log^2(T/(B_T+1)) / \Delta_{\min})$ across blocks — sharper than $\sqrt{(B_T+1)\,T}$ when $\Delta_{\min}$ is bounded away from zero.
