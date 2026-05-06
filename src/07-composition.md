## Composition Theorem ^sec-composition

The four components compose. Taken together, they give a non-stationarity-aware convergence theory with three properties no existing strand has individually:

1. **Handles non-stationarity** via the strategic tempo and forgetting prerequisite (Component 3).
2. **Has explicit metric structure on policy space** via the point-mass reverse-KL/TV identity (Component 2).
3. **Is learnable on-policy** via closed-loop interventional access (Component 4).

The two-gap diagnostic (Component 1) is the connective tissue that routes regret-signal interpretation: it tells the agent *which* corrective action the regret signal warrants, and therefore *which* of the three properties must be invoked at any given moment.

### Composition theorem (joint guarantees with conditional cumulative reduction) ^sec-composition-statement

Let $K_t := D_{\mathrm{KL}}(\pi^*_t \,\|\, Q_t)$ denote the per-round reverse-KL coordinate.

> [!theorem] Joint guarantees + conditional blockwise regret reduction ^thm-composition
> Let $(\mathcal S, \mathcal A, P_t, r_t, N_h)$ be a non-stationary MDP with bounded reward, finite action space, and for each round $t$, deterministic optimum policy $\pi^*_t = \delta_{a^*_t}$ with action gap $\Delta_{\min} > 0$. Suppose the agent operates on a singular causal trajectory in the sense of [[#^sec-setup]] and updates its policy with per-element exponential forgetting at rates $\{\lambda_{ij}\}$. If
>
> **(A1) Metric on policy space.** $Q_t(a^*_t) > 0$ at every round.
>
> **(A2) Multi-factor forgetting prerequisite (Model (S) of [[#^sec-sector-model]]).** The bottleneck strategic tempo $\mathcal T_\Sigma^{\mathrm{bn,ss}} := \min_{(i,j) \in E} \nu_{ij} \cdot \iota_{ij} \cdot (1 - \lambda_{ij})$ exceeds $\rho_\Sigma / R_\Sigma$.
>
> **(A3) Identifiability regime / [[#^thm-loop-level2]] conditions.** The action mechanism is known and (C1)–(C3) of [[#^thm-loop-level2]] hold; the agent operates in Regime A or Regime B with argmax-correctness probability $p_{\mathrm{id}}$.
>
> **(A4) Two-gap diagnostic.** The agent applies the satisfaction-gap / control-regret $2{\times}2$ to route corrective action.
>
> **(A5) Piecewise-stationary block structure with identity-tight base learner — occupancy-weighted form (for conclusion (v)).** The time axis is partitioned into $B_T + 1$ intervals on which the MDP is stationary and $\pi^*_t(\cdot \mid s) = \delta_{a^*_t(s)}$ is fixed per state. Within each interval of length $L$, define the occupancy-weighted per-round coordinate $\overline{\mathrm{TV}}_t := \frac{1}{N_h}\sum_{h=0}^{N_h-1} \mathbb E_{s_h \sim d^{Q_t}_h}[\,\mathrm{TV}(\pi^*_t(\cdot\mid s_h),\, Q_t(\cdot\mid s_h))\,]$, where $d^{Q_t}_h$ is the round-$t$ learner-induced state distribution at horizon step $h$. The base learner satisfies $\sum_{t=1}^{L} \mathbb E[\overline{\mathrm{TV}}_t] \le 2c\sqrt L$ within each interval. Either the learner is restarted at block boundaries, or (A5) is taken as a local guarantee under within-block carryover. Trajectory-level non-stationary RL bases (UCRL2 \cite{auer-2010-nearoptimal}, UCBVI \cite{azar-2017-minimax}, optimistic-PSRL) natively give cumulative trajectory-level regret of this $\sqrt L$ shape; the per-state TV identity coordinate $1 - e^{-K_t(s)}$ — strictly sharper than Pinsker — is preserved underneath the occupancy aggregation.
>
> Then:
>
> (i) Per-round regret is two-sided identity-bounded:
> $$\Delta_{\min}\bigl(1 - e^{-D_{\mathrm{KL}}(\pi^*_t \,\|\, Q_t)}\bigr) \;\le\; R(Q_t) \;\le\; V_{\max}\bigl(1 - e^{-D_{\mathrm{KL}}(\pi^*_t \,\|\, Q_t)}\bigr).$$
>
> (ii) Aggregate mismatch $\boldsymbol\delta_\Sigma$ is ultimately bounded under non-stationarity, with steady-state bound $R_\Sigma^* = \rho_\Sigma / \mathcal T_\Sigma^{\mathrm{bn,ss}}$.
>
> (iii) The KL coordinate $D_{\mathrm{KL}}(\delta_{a^*_t} \,\|\, Q_t) = -\log Q_t(a^*_t)$ is computable directly from $Q_t$ (the agent's policy, internally available) and a chosen $a^*_t$. The agent uses its identified optimum $a^*_{\mathrm{ag},t}$; let $p_{\mathrm{id}} := \Pr[a^*_{\mathrm{ag},t} = \tilde a^*_t]$ denote the argmax-correctness probability against the true optimum $\tilde a^*_t$. Under $Q_t(a) \ge q_0$ on both $a^*_{\mathrm{ag},t}$ and $\tilde a^*_t$,
> $$\mathbb E\bigl[\bigl|\hat D_t - D_t^{\mathrm{true}}\bigr|\bigr] \;\le\; (1 - p_{\mathrm{id}})\log(1/q_0)$$
> where $\hat D_t := -\log Q_t(a^*_{\mathrm{ag},t})$ and $D_t^{\mathrm{true}} := -\log Q_t(\tilde a^*_t)$. Bias scales linearly in misidentification mass $1 - p_{\mathrm{id}}$ (Regime A, $p_{\mathrm{id}} \to 1$: zero bias; Regime B, $p_{\mathrm{id}} \in (0, 1)$: bias controlled; Regime C, $p_{\mathrm{id}} \to 0$: bias up to $\log(1/q_0)$). The argmax-correctness $p_{\mathrm{id}}$ relates to per-edge identifiability $\iota$ via the policy DAG structure; in the simple-bandit case $p_{\mathrm{id}} = \iota$. Proof in [[#^sec-bias-bound]].
>
> (iv) The $2{\times}2$ cell containing $(\delta_{\mathrm{sat}}, \delta_{\mathrm{regret}})$ identifies the corrective action class: revise policy (regret-driven), revise model/policy-class/horizon (capability-driven), or revise objective (last resort).
>
> (v) *Trajectory-level blockwise dynamic regret (in expectation, conditional on (A5)).* Under (A5), with $K_t(s) := D_{\mathrm{KL}}(\delta_{a^*_t(s)} \,\|\, Q_t(\cdot \mid s))$ the per-state reverse-KL coordinate,
> $$\mathbb E[\mathrm{DynReg}(T)] \;:=\; \mathbb E\!\left[\sum_{t=1}^T \bigl(V^{\pi^*_t}_t - V^{Q_t}_t\bigr)\right] \;\le\; 2c\,V_{\max}\,N_h\,\sqrt{(B_T+1)\,T} \;+\; N_h\,(1 - p_{\mathrm{id}})\log(1/q_0)\cdot T,$$
> where $B_T := |\{t : a^*_t \ne a^*_{t-1}\}|$ counts optimum-change events along the realized trajectory and the partition into $B_T + 1$ piecewise-stationary blocks is given by (A5). The first term is the trajectory-level identity-coordinate rate; the second is the Regime-B/C bias contribution from per-state argmax misidentification (vanishes in Regime A, where $p_{\mathrm{id}} \to 1$ under [[#^thm-loop-level2]]'s identifiability conditions). The per-round identity coordinate (i) is sharper than Pinsker/BH bounds; the $N_h$ horizon factor is the standard simulation-lemma penalty (linear in $N_h$, sharper than the $N_h^2$ of TRPO-style worst-state bounds \cite{schulman-2015-trust}) and matches the structural shape of near-optimal non-stationary MDP rates \cite{mao-2021-nearoptimal}. **Scope.** The reduction is over piecewise-stationary blocks (the partition of (A5)) rather than continuous variation, and does not derive (A5) — it composes (A5) with the per-round identity via the performance-difference / simulation lemma \cite{kakade-2002-approximately, munos-2003-error, ross-2010-efficient, azar-2017-minimax} to produce the trajectory-level rate. Detailed proof: [[#^sec-proof-sketches]].

**Bundle-of-guarantees framing.** Conclusions (i)–(iv) jointly hold under (A1)–(A4); conclusion (v) further requires (A5). The cumulative-rate inequality (v) uses the metric coordinate (i) and the base-learner assumption (A5); the strategic-tempo threshold (A2)/(ii) supports persistence not the algebraic regret rate; the causal access (A3)/(iii) supports learnability of the bias-controlled KL coordinate not the rate itself; the diagnostic (A4)/(iv) routes interpretation. The composition is a *bundle of compatible guarantees*, not a single integrated proof in which every component is necessary for the rate.

The proof: [[#^thm-twosided-regret]] gives (i); [[#^thm-forgetting-prereq]] gives (ii); [[#^thm-bias-bound]] gives (iii); the $2{\times}2$ of [[#^sec-two-gap-diagnostic]] gives (iv); for (v), the simulation lemma bounds the per-round value gap by $V_{\max} N_h \overline{\mathrm{TV}}_t$ along the learner's trajectory; partition $[1, T]$ at optimum-change events into $B_T + 1$ blocks; (A5) gives per-block $\sum_t \mathbb E[\overline{\mathrm{TV}}_t] \le 2c\sqrt{\Delta_i}$; Cauchy–Schwarz across blocks gives $\sum_i \sqrt{\Delta_i} \le \sqrt{(B_T+1)\,T}$; multiplication by $V_{\max} N_h$ closes the rate term, and [[#^thm-bias-bound]] aggregated over the $N_h$ horizon steps closes the bias term. Detailed proof of (v) in [[#^sec-proof-sketches]].

**Remarks.**

- In the bandit special case ($N_h = 1$, so $\overline{\mathrm{TV}}_t = \mathbb E[1 - e^{-K_t}]$), Thompson sampling or UCB as base learner satisfies (A5) with the stronger logarithmic rate $\mathbb E[1 - e^{-K_t}] = O(\log t / (t\Delta_{\min}))$ per stationary block ([[#^sec-algorithm-sketch]]), giving cumulative regret $O(V_{\max} (B_T+1) \log^2(T/(B_T+1)) / \Delta_{\min})$ — sharper than $\sqrt{(B_T+1)\,T}$ when $\Delta_{\min}$ is bounded away from zero. The square-root rate is the worst-case Cauchy–Schwarz bound; the logarithmic rate is the typical-case bound for stochastic-bandit base learners. For $N_h > 1$, UCRL2 \cite{auer-2010-nearoptimal} / UCBVI \cite{azar-2017-minimax} provide the natural occupancy-weighted instantiation ([[#^sec-algorithm-sketch]]), with $c$ of order $\tilde O(\sqrt{N_h S A})$ and the lifted cumulative rate $\tilde O(N_h^2 \sqrt{SA(B_T+1)T})$ matching \cite{mao-2021-nearoptimal} up to log factors.

- Pointwise convergence $V(\pi_t) \to V^*$ is structurally unavailable under genuine non-stationarity (the target is itself moving). The right replacement is the *Cesàro tracking statement* $\frac{1}{T}\sum_t (V^*_t - V(\pi_t)) = O\!\bigl(\sqrt{(B_T+1)/T}\bigr) \to 0$ when $B_T = o(T)$, which is a corollary of (v) under (A5).

### Scope and partial uniqueness ^sec-composition-scope

The composition is *assembly + a derived rate*: (i)–(iv) have published or directly-derived ancestors; (v)'s rate matches \cite{cheung-2020-reinforcement, wei-luo-2021-blackbox} as a corollary, but the route through the per-round identity is new. The composition holds in [[#^sec-setup]]'s canonical scope (deterministic $\pi^*$, bounded value, isolated optimum, singular trajectory, finite horizon); each axis degrades cleanly outside it (stochastic $\pi^*$ → BH one-sided; $V_{\max} = \infty$ → upper bound trivial, $\Delta_{\min}$-lower survives; $\Delta_{\min} = 0$ → upper survives; non-singular trajectories break loop-Level-2). At the *metric layer*, reverse-KL is uniquely determined up to positive scaling under chain-rule additivity \cite{hobson-1969-new-theorem, csiszar-1991-why-ls-maxent} ([[#^thm-chain-rule-uniqueness]]) — any theory satisfying property 2 plus chain-rule must use reverse-KL. We do *not* claim joint uniqueness across properties 1–3; alternative correction architectures and interventional-access taxonomies exist in principle, and a joint-uniqueness theorem would require further axioms (singular-trajectory, sector-boundedness) — future work.
