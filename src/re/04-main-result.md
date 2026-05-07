## Main Result ^sec-main-result

We present the four components first as motivation, then state the formal composition theorem, then unpack what its conclusions mean.

### The four components ^sec-four-components

Each component supplies a *distinct epistemic property* that the others cannot deliver. The composition is necessary for the joint properties (rate + persistence + learnability + corrective-action routing), not for the rate alone. The component-by-component failure-mode analysis is in [[#^sec-necessity]] below.

**Component 1: Two-gap diagnostic — corrective-action routing.** Let $\Pi$ be the policy class and $V_O^{\min}$ the trajectory-value threshold above which the objective is met. Define the *satisfaction gap* $\delta_{\mathrm{sat}} := V_O^{\min} - \sup_{\pi \in \Pi} V_O(M_t, \pi; N_h)$ (goal-feasibility: positive means unmet under the best available policy) and the *control regret* $\delta_{\mathrm{regret}} := \sup_{\pi \in \Pi} V_O(M_t, \pi; N_h) - V_O(M_t, \pi_{\mathrm{current}}; N_h) \ge 0$ (policy-quality). The 2$\times$2 cross routes four regimes — *Success* / *Strategy problem* (revise the policy) / *Capability limit* (revise model, policy class, or horizon) / *Both* — along an axis *orthogonal* to the exploration-vs-adaptation decompositions of \cite{li-zhao-zhou-2024-dynamic-regret, fei-2020-dynamic, stradi-2024-learning}.

**Component 2: Point-mass reverse-KL/TV identity — metric layer on policy space.** Under deterministic $\pi^* = \delta_{a^*}$, the reverse-KL and total variation collapse to an *exact identity*
$$\operatorname{TV}(\pi^*, Q) \;=\; 1 - e^{-D_{\mathrm{KL}}(\pi^* \,\|\, Q)},$$
yielding the two-sided regret characterization $\Delta_{\min}(1 - e^{-K}) \le R(Q) \le V_{\max}(1 - e^{-K})$ — *Lipschitz-equivalent* with constants $\Delta_{\min}/V_{\max}$ and $1$, and *coordinate-optimal among bounds depending only on TV*. The identity sits *strictly below* both the Pinsker $\sqrt{D_{\mathrm{KL}}/2}$ and Bretagnolle–Huber $\sqrt{1 - e^{-D_{\mathrm{KL}}}}$ envelopes at this corner ([[#^sec-pinsker-numerics]]); the deterministic-$\pi^*$ scope extends perturbatively to $\epsilon$-greedy and softmax-regularized optima with quantified corrections ([[#^sec-key-lemma-1]] + [[#^sec-perturbative]]).

**Component 3: Multi-factor strategic tempo with forgetting prerequisite — non-stationarity persistence.** A multi-factor strategic tempo composed per revisable policy element of an observation rate $\nu$, an identifiability coefficient $\iota \in [0,1]$, and an update gain $1 - \lambda$ yields a structural survival inequality
$$\mathcal T_\Sigma^{\mathrm{bn,ss}} \;:=\; \min_{(i,j) \in E} \nu_{ij} \cdot \iota_{ij} \cdot (1 - \lambda_{ij}) \;>\; \rho_\Sigma / R_\Sigma,$$
the *bottleneck* (per-element minimum) being the right aggregator since adversarial disturbance concentrates on the weakest element. A structural-class theorem on the *gain-decay* class $\mathcal A_{\mathrm{decay}}$ (count-accumulating Bayesian without forgetting, observation-aggregating without restart, gradient-based with vanishing step) shows every member eventually violates the threshold for any positive disturbance rate ([[#^sec-aux-decay-class]]); finite-gain mechanisms (constant-step SA, sliding-window, bounded-memory, block-restart) face *bidirectional* ceilings instead. This *lifts* the tempo result of \citet{lee-2023-prost-tempo} from hyperparameter optimization to a structural-threshold claim. Full Model (Σ) and derivation in [[#^sec-key-lemma-2]].

**Component 4: Closed-loop interventional access — on-policy learnability.** Under (C1) *positivity* ($\pi_t(a \mid H_t) \ge p_{\min} > 0$), (C2) *sequential ignorability* ($H_t$ d-separates $a_t$ from $o_{t+1}$ in the mutilated graph $G_{\overline{a_t}}$), and (C3) *known action mechanism*, the loop's data-generating process is *interventional* — the conditional $P(o_{t+1} \mid a_t, H_t)$ coincides with the do-distribution $P(o_{t+1} \mid \mathrm{do}(a_t), H_t)$ on the realized-action support ([[#^sec-key-lemma-3]]). This makes Component 2's regret bound *learnable on-policy* in regimes with sufficient identifiability, with bias from misidentification quantified by $1 - p_{\mathrm{id}}$ ([[#^sec-key-lemma-4]]). Three regimes \cite{bareinboim-correa-ibeling-icard-2022-pearl-hierarchy} partition usable strength: A (intervention-rich, $\iota \approx 1$), B (partial), C (observation-only).

### The composition theorem ^thm-composition

> [!theorem] Composition: joint guarantees with conditional cumulative reduction ^thm-composition
> Let $(\mathcal S, \mathcal A, P_t, r_t, N_h)$ be a non-stationary MDP with bounded reward, finite action space, and for each round $t$ a deterministic optimum policy $\pi^*_t = \delta_{a^*_t}$ with action gap $\Delta_{\min} > 0$. Suppose the agent operates on a singular causal trajectory in the sense of [[#^sec-preliminaries]] and updates its policy with per-element exponential forgetting at rates $\{\lambda_{ij}\}$. Let $K_t(s) := D_{\mathrm{KL}}(\delta_{a^*_t(s)} \,\|\, Q_t(\cdot \mid s))$ be the per-state reverse-KL coordinate (in the bandit case $N_h=1$ the state argument is suppressed). If
>
> **(A1) Metric extension.** The identity $\operatorname{TV}(\delta_{a^*_t(s)}, Q_t(\cdot \mid s)) = 1 - e^{-K_t(s)}$ of [[#^lem-pointmass-identity]] is read in the extended real sense: when $Q_t(a^*_t(s) \mid s) = 0$, $K_t(s) = +\infty$ and $1 - e^{-K_t(s)} = 1$ recovers the trivial bound $R(Q_t \mid s) \le V_{\max}$. No pointwise positivity is required; vanilla deterministic UCB / UCBVI / UCRL2 deployed at the optimism-argmax are in scope without modification.
>
> **(A2) Multi-factor forgetting prerequisite.** Within Model (Σ) of [[#^sec-key-lemma-2]], the bottleneck strategic tempo $\mathcal T_\Sigma^{\mathrm{bn,ss}} := \min_{(i,j) \in E} \nu_{ij} \cdot \iota_{ij} \cdot (1 - \lambda_{ij}) > \rho_\Sigma / R_\Sigma$.
>
> **(A3) Identifiability regime.** The action mechanism is known and (C1)–(C3) of [[#^sec-key-lemma-3]] hold; the agent operates in Regime A or Regime B with per-state argmax-correctness probability $p_{\mathrm{id}}(s)$, with floor $p_{\mathrm{id}} := \min_s p_{\mathrm{id}}(s)$.
>
> **(A4) Two-gap diagnostic.** The agent applies the satisfaction-gap / control-regret 2$\times$2 to route corrective action.
>
> **(A5) Piecewise-stationary block structure with restart-on-change identity-tight base learner.** The time axis partitions into $B_T + 1$ stationary intervals on which the MDP is stationary and $\pi^*_t(\cdot \mid s) = \delta_{a^*_t(s)}$ is fixed per state. The base learner is *restarted* at each block boundary; within each restarted interval of length $L$ it satisfies $\sum_{t=1}^L \mathbb E[\overline{\mathrm{TV}}_t] \le 2c \sqrt L$, where $\overline{\mathrm{TV}}_t := \tfrac{1}{N_h} \sum_{h=0}^{N_h-1} \mathbb E_{s_h \sim d^{Q_t}_h}\bigl[\operatorname{TV}(\pi^*_t(\cdot \mid s_h), Q_t(\cdot \mid s_h))\bigr]$ is the occupancy-weighted per-round coordinate. (A non-restarting carryover variant is plausible but requires bounding cross-block occupancy shift; see Remark in [[#^sec-mechanism]].)
>
> Then:
>
> **(i) Per-round regret is two-sided identity-bounded.** At every visited state, $\Delta_{\min}\bigl(1 - e^{-K_t(s)}\bigr) \le R(Q_t \mid s) \le V_{\max}\bigl(1 - e^{-K_t(s)}\bigr)$.
>
> **(ii) Aggregate strategic mismatch is ultimately bounded.** Under (A2), the modeled mismatch $\|\boldsymbol\delta_\Sigma\|$ is ultimately bounded within $R_\Sigma^* = \rho_\Sigma / \mathcal T_\Sigma^{\mathrm{bn,ss}}$.
>
> **(iii) The KL coordinate is computable with controlled bias.** $-\log Q_t(a^*_{\mathrm{ag},t}(s) \mid s)$ is computable directly from $Q_t$. Per-state bias: $\mathbb E[\lvert \hat D_t(s) - D_t^{\mathrm{true}}(s) \rvert] \le (1 - p_{\mathrm{id}}(s)) \log(1/q_0)$ under $Q_t(\cdot\mid s) \ge q_0$ at the two argmax candidates. Vanishes in Regime A.
>
> **(iv) The 2$\times$2 cell identifies the corrective-action class.** $(\delta_{\mathrm{sat}}, \delta_{\mathrm{regret}})$ routes to revise-policy / revise-model-class-or-horizon / revise-objective.
>
> **(v) Trajectory-level blockwise dynamic regret.** Under (A5),
> $$\mathbb E[\mathrm{DynReg}(T)] \;:=\; \mathbb E\!\left[\sum_{t=1}^T \bigl(V^{\pi^*_t}_t - V^{Q_t}_t\bigr)\right] \;\le\; 2c\,V_{\max}\,N_h \sqrt{(B_T+1) \, T} \;+\; V_{\max}\,N_h\,(1 - p_{\mathrm{id}}) \cdot T.$$

The proof composes the per-round identity (i) with the simulation lemma \cite{kakade-2002-approximately, munos-2003-error, ross-2010-efficient, azar-2017-minimax}, a block decomposition at optimum-change events, Cauchy–Schwarz across the $B_T + 1$ blocks, and the bias bound from (iii). Full proof in [[#^sec-proof-composition]]; the four key lemmas are surfaced in [[#^sec-mechanism]].

### Unpacking the conclusions ^sec-unpacking

[[#^thm-composition]] asserts a non-stationarity-aware convergence theory with three jointly-novel properties: *non-stationarity persistence* (via (ii)/(A2)), *explicit metric structure on policy space* (via (i)/Component 2), *on-policy learnability* (via (iii)/(A3)). Conclusion (iv) is the connective tissue; conclusion (v) the cumulative rate from composing per-round metric with base-learner stationary rate.

*The per-round coordinate is exact, not an inequality.* Under deterministic $\pi^*$, the identity-bound (i) is $\Delta_{\min}/V_{\max}$-Lipschitz-equivalent to regret with both endpoints achieved on extremal value landscapes — *coordinate-optimal among bounds depending only on TV*. Strictly sharper than Pinsker $\sqrt{D_{\mathrm{KL}}/2}$ (exact, not approximate) and Bretagnolle–Huber $\sqrt{1 - e^{-D_{\mathrm{KL}}}}$ at this corner ($x < \sqrt x$ on $(0,1)$).

*Cumulative rate: piecewise-stationary $B_T$ family by direct aggregation, continuous-variation $V_T$ family by MASTER wrapping.* The lifted UCRL2/UCBVI rate $\tilde O(N_h^2\sqrt{SA(B_T+1)T})$ ([[#^sec-algorithm]]) sits in the restart-on-change family of \cite{cheung-2020-reinforcement}; via \cite{wei-luo-2021-blackbox}'s MASTER black-box, the same identity-routed base learner achieves Best-of-Both-Worlds $\tilde O(\min\{\sqrt{(B_T+1) T},\,(V_T+1)^{1/3}T^{2/3}\})$ automatically — matching \cite{mao-2021-nearoptimal}'s near-optimal $V_T$ exponent (parameter ($B_T$ vs $V_T$), $T$-scaling, $S,A$-scaling differ from Mao's RestartQ-UCB; full development in [[#^sec-conclusion]] + (A5')-BoBW Remark in [[#^sec-proof-composition]]).

*The bias term in (v) is a value-coordinate misidentification penalty.* The second term $V_{\max} N_h (1 - p_{\mathrm{id}}) \cdot T$ is the expected value gap between true and agent-identified optima, $V_{\max}$-bounded on the misidentification event of probability $1 - p_{\mathrm{id}}$ aggregated over $N_h$ horizon steps. Vanishes in Regime A; in Regime B the dominant rate term is the metric one when $1 - p_{\mathrm{id}}$ is small relative to $\sqrt{(B_T+1)/T}$; in Regime C saturates at the trivial $V_{\max} N_h \cdot T$ envelope ([[#^sec-algorithm]] flags Regime C out-of-scope). *Strict assumption-set weakening:* (v) does not require Lemma 4's two-point support $Q_t \ge q_0$ — that condition is needed only for conclusion (iii)'s diagnostic computability.

*The bundle is compatible, not integrated.* (v)'s rate goes through (A5) and (i) alone — Component 2 supplies the metric, (A5) the base-learner regret, the $\sqrt{(B_T+1) T}$ aggregation falls out. The other components carry the *other* guarantees: Component 1 the routing (iv); Component 3 the persistence (ii) (without it, $K_t$ may itself drift unboundedly under $\mathcal A_{\mathrm{decay}}$); Component 4 the on-policy learnability via (iii) (without it, (v) becomes vacuous in Regime C). The horizon factor $N_h$ in (v) is the linear-in-$N_h$ simulation-lemma penalty, sharper than the $N_h^2$ of TRPO-style worst-state bounds \cite{schulman-2015-trust}. Component-by-component failure-mode analysis in [[#^sec-necessity]].
