# A Unified Convergence Theory for Non-Stationary Reinforcement Learning

## Abstract

Non-stationary reinforcement learning lacks a unified convergence theory: variation-budget regret bounds, sliding-window methods, tempo-and-forgetting analyses, two-term decompositions, and causal-RL frameworks each address fragments without composing. We present a unified theory through the composition of four structural elements: (i) a two-gap diagnostic separating goal-feasibility from policy-quality; (ii) a point-mass reverse-KL/TV identity under deterministic optimum, $\mathrm{TV}(\delta_{a^*}, Q) = 1 - e^{-D_{\mathrm{KL}}}$, that is *exact* and strictly improves both Pinsker and the Bretagnolle–Huber inequality at this corner; (iii) a multi-factor strategic tempo $\mathcal{T}_\Sigma = \min_{(i,j)} \nu_{ij} \cdot \iota_{ij} \cdot (1-\lambda_{ij})$ with a structural forgetting prerequisite $\mathcal{T}_\Sigma > \rho_\Sigma/R_\Sigma$ as a survival inequality; (iv) closed-loop interventional access making regret bounds learnable from on-policy data via the Pearl causal hierarchy. Composing these gives a cumulative dynamic regret theorem with rate $O(V_{\max}\sqrt{(B_T+1)\,T})$ where $B_T$ counts optimum-change events. We further establish a structural failure class: every count-accumulating agent (any update with $n_{\mathrm{eff}}(t) \to \infty$) eventually violates the forgetting prerequisite for any positive disturbance rate, while non-accumulating mechanisms admit bidirectional thresholds (constant-step stochastic approximation, sliding windows, bounded memory, block restart). The point-mass identity is coordinate-optimal among bounds depending only on total variation, chain-rule compositional to multi-step trajectory regret as behavior-cloning loss against the optimal trajectory, and load-bearing for the cumulative theorem. The deterministic-optimum scope extends perturbatively to $\epsilon$-stochastic and softmax-regularized policies with $O(\epsilon\log(1/\epsilon))$ and $O(\exp(-\Delta_{\min}/\tau))$ corrections respectively. Theory-only; Lemma 5.2 makes the ProST reduction rigorous at the sector-parameter level, while their published experiments motivate the strategic-tempo component.

---

## 1  Introduction

Non-stationary reinforcement learning has matured along four largely separate tracks: **variation-budget dynamic regret** [Cheung-Simchi-Levi-Zhu 2020; Wei-Luo 2021; Mao-Zhang-Zhu-Simchi-Levi-Başar 2021; Gajane-Ortner-Auer 2019]; **two-term regret decompositions** along an exploration-vs-adaptation axis [Long-Fei Li-Zhao-Zhou 2024; Fei-Yang-Wang-Xie 2020; Stradi-Lunghi-Castiglioni-Marchesi-Gatti 2024]; **tempo and forgetting analyses** making update-timing or discount rate explicit convergence variables [Lee et al.\ 2023 ProST; Lee et al.\ 2024; Touati-Vincent 2020; Russac-Vernade-Cappé 2019; Garivier-Moulines 2008]; and **causal / interventional access** [Junzhe Zhang-Bareinboim 2016, 2022; Lu-Meisami-Tewari 2021, 2022; Wang-Yang-Wang 2021 DOVI; Junzhe Zhang 2020], stationary throughout. The information-theoretic regret literature [Russo-Van Roy 2014a, 2014b; Lu-Van Roy 2019; Min-Russo 2023] cuts across these tracks using entropy, mutual information, information ratio, Pinsker, or Hellinger; an *exact* reverse-KL / TV identity at the deterministic-optimum corner — strictly improving both Pinsker and Bretagnolle--Huber [1978] there — is absent from this corpus.

No published framework composes the four tracks. In particular, no framework combines (a) a regret decomposition along the *goal-feasibility-vs-policy-quality* axis (distinct from the exploration-vs-adaptation decompositions); (b) an exact point-mass reverse-KL/TV regret identity under deterministic optimum, strictly tighter than the BH inequality at this corner; (c) a structural survival inequality threading policy-revision rate against environment-side disturbance; and (d) a closed-loop causal-access argument making the regret bound *learnable* on-policy.

### 1.1  Contribution

The contribution is "a novel combination of existing techniques" (NeurIPS Theory Track) routing four components into a single non-stationary convergence theory. **(i) Point-mass reverse-KL/TV identity (§4).** Under deterministic $\pi^* = \delta_{a^*}$, $D_{\mathrm{KL}}(\pi^* \,\|\, Q) = -\log Q(a^*) = -\log(1 - \operatorname{TV}(\pi^*, Q))$ — an exact identity, strictly improving the BH inequality $\operatorname{TV} \le \sqrt{1 - e^{-D_{\mathrm{KL}}}}$ at this corner since $x < \sqrt{x}$ on $(0, 1)$. Composed with the textbook TV-regret bound, this yields the two-sided characterization $\Delta_{\min}(1 - e^{-D_{\mathrm{KL}}}) \le R(Q) \le V_{\max}(1 - e^{-D_{\mathrm{KL}}})$, strictly improving the Pinsker form $V_{\max}\sqrt{D_{\mathrm{KL}}/2}$ (which is vacuous for $D_{\mathrm{KL}} > 2$). The identity is load-bearing in three ways (§4.4): coordinate-optimality among TV-only bounds; chain-rule compositionality (multi-step KL = behavior-cloning loss); cumulative-regret connection. The deterministic-$\pi^*$ scope extends *perturbatively* to $\epsilon$-greedy and softmax-regularized optima with $O(\epsilon\log(1/\epsilon))$ and $O(\exp(-\Delta_{\min}/\tau))$ corrections (§4.6).

**(ii) Two-gap diagnostic (§3).** Separating $\delta_{\mathrm{sat}}$ (goal-feasibility) from $\delta_{\mathrm{regret}}$ (policy-quality) routes four regimes — success, strategy problem, capability limit, both — to distinct corrective actions, along an axis orthogonal to the exploration-vs-adaptation decompositions.

**(iii) Multi-factor strategic tempo with forgetting prerequisite (§5).** A structural survival inequality $\mathcal T_\Sigma^{\mathrm{bn,ss}} := \min_{(i,j)} \nu_{ij} \iota_{ij} (1-\lambda_{ij}) > \rho_\Sigma / R_\Sigma$ in which observation-rate, identifiability, and discount-rate are independently load-bearing per element. Recovers the single-factor $(1-\lambda) > \rho_\Sigma / R_\Sigma$ under Beta-Bernoulli normalization. A structural-class theorem covers count-accumulating updates $\mathcal A_{\mathrm{accum}}$ universally; non-accumulating mechanisms (constant-step SA, sliding-window, bounded-memory, block-restart) face bidirectional ceilings (§5.3.1). Lifts [Lee et al.\ 2023] from hyperparameter to structural threshold; strategic analog of the adaptive-control persistence condition $\alpha > \rho/R$ [Khalil 2002].

**(iv) Closed-loop interventional access (§6).** Grounded in [Bareinboim-Correa-Ibeling-Icard 2022], the agent's action $a_t$ causally precedes $o_{t+1}$, so loop data is Pearl Level-2 by construction — making the bound *learnable* on-policy under sufficient identifiability. Three regimes (A/B/C) partition usable strength of identification.

**Composition (§7).** The four together yield a non-stationary convergence theory with three properties no existing strand has jointly: handles non-stationarity (iii); explicit metric structure on policy space (i); learnable from on-policy data (iv). A variation-budget block argument over the per-round identity gives cumulative dynamic regret $\mathrm{DynReg}(T) \le 2cV_{\max}\sqrt{(B_T+1)\,T}$ — per-round coordinate sharper than Pinsker/BH; cumulative rate matches the variation-budget literature [Cheung et al.\ 2020; Wei-Luo 2021] as a corollary.

### 1.2  Scope and limitations

Theory-only. The identity is a two-line direct calculation (§4); Lemma 5.2 makes the ProST reduction rigorous at the steady-state sector-parameter level (§8). The composition theorem holds in the canonical scope of §2: deterministic $\pi^*$, bounded value range, isolated optimum ($\Delta_{\min} > 0$), singular causal trajectory. Deterministic-$\pi^*$ extends perturbatively to $\epsilon$-stochastic and softmax-regularized optima (§4.6); the tied-optimum extension (Appendix A.5) is a degraded one-sided form.

---

## 2  Setup

**Markov decision process.** A finite-horizon non-stationary MDP $(\mathcal S, \mathcal A, P_t, r_t, N_h)$ with state space $\mathcal S$, finite action space $\mathcal A$, time-indexed transition kernels $P_t(\cdot \mid s, a)$ and reward functions $r_t(s, a)$ allowed to vary across rounds $t$, and finite planning horizon $N_h$. Standard non-stationary-RL boundedness assumptions [Cheung et al.\ 2020]: per-step reward in $[0, 1]$, total cumulative variation in transition and reward bounded by a *variation budget*
$$V_T \;:=\; \sum_{t=1}^{T-1} \max\!\Bigl\{ \sup_{s,a} \|P_{t+1}(\cdot \mid s, a) - P_t(\cdot \mid s, a)\|_{\mathrm{TV}},\ \ \sup_{s,a} |r_{t+1}(s, a) - r_t(s, a)| \Bigr\}.$$

**Two variation regimes.** Theorem 7.1(v) is stated for the *piecewise-stationary* specialization: $B_T + 1$ stationary blocks separated by optimum-change events, with $B_T := |\{t : a^*_t \ne a^*_{t-1}\}|$. $B_T$ and $V_T$ are distinct in general (piecewise-stationary abrupt magnitude-$\delta$: $V_T \asymp \delta B_T$; continuous optimum-preserving: $V_T > 0$, $B_T = 0$; tiny optimum-flipping: $B_T \gg V_T / \delta_{\min}$). The rate $O(V_{\max}\sqrt{(B_T+1)T})$ is not directly comparable to the variation-budget rate $\widetilde O(V_T^{1/3} T^{2/3})$ [Cheung-Simchi-Levi-Zhu 2020; Wei-Luo 2021] in continuous regimes; they coincide up to constants under abrupt-magnitude-$\delta$. Generalization to continuous variation is future work.

**Policy distribution.** Round-$t$ policy $Q_t(\cdot \mid s)$; we write $Q$ when state and round are understood. Under canonical scope, $\pi^*(\cdot \mid M_t, s) = \delta_{a^*(s)}$ where $a^*(s) := \arg\max_a Q^\pi(s, a)$.

**Value object.**
$$V_O(M_t, \pi;\, N_h) \;=\; \mathbb E[V_O(\tau_{t:t+N_h}) \mid M_t, \pi], \qquad Q_O(M_t, a;\, \pi_{\mathrm{cont}}, N_h) \;=\; \mathbb E[V_O(\tau) \mid M_t,\, do(a_t = a),\, a_{t+1:} \sim \pi_{\mathrm{cont}}].$$
The $do(\cdot)$ [Pearl 2009] flags intervention on $a_t$ rather than conditioning. Default continuation: **one-step improvement** $\pi_{\mathrm{cont}} = \pi_{\mathrm{current}}$ — most conservative, comparable across rounds, fixed-point-free; receding-horizon and Bellman alternatives in Appendix A.1.

**Bounded value range.** $V_{\max}(M_t) := \max_a Q_O(M_t, a) - \min_a Q_O(M_t, a)$, finite under bounded reward, finite horizon.

**Action gap (isolated optimum).** $\Delta(a) := Q_O(a^*) - Q_O(a) \in [0, V_{\max}]$, $\Delta_{\min} := \min_{a \neq a^*} \Delta(a) > 0$ whenever the optimum is isolated.

**Strategic regret.** $R(Q) := Q_O(a^*) - \mathbb E_{a \sim Q}[Q_O(a)] = \sum_{a \neq a^*} Q(a) \Delta(a)$; three classical forms ($\mathbb E_{\pi^*}[V] - \mathbb E_Q[V]$, $V(a^*) - \mathbb E_Q[V]$, $\mathbb E_{\pi^*}[V - V_Q]$) coincide under deterministic $\pi^*$.

**Singular trajectory.** Single, non-forkable causal trajectory: $a_t$ causally precedes $o_{t+1}$, $M_t$'s sufficiency defined relative to *this* trajectory. This grounds the Pearl $do$-operator above and the closed-loop interventional-access argument of §6.


---

## 3  Component 1 — The Two-Gap Diagnostic

### 3.1  The two gaps

Let $\Pi$ denote the policy class available to the agent and let $V_O^{\min}$ denote the threshold trajectory value above which the objective is considered "met" — a property of the objective, set by the application domain (for constraints: all satisfied; for utility maximization: a minimum acceptable value).

**Objective attainability.**
$$A_O(M_t;\, \Pi, N_h) \;:=\; \sup_{\pi \in \Pi} V_O(M_t, \pi;\, N_h).$$
$A_O$ is the best achievable value given the agent's beliefs, available policies, and horizon.

**Satisfaction gap** (goal-feasibility diagnostic):
$$\delta_{\mathrm{sat}} \;:=\; V_O^{\min} - A_O(M_t;\, \Pi, N_h).$$
$\delta_{\mathrm{sat}} > 0$ means the objective is *unmet* under the best available policy. The signal does not automatically mean the goal is wrong: it means the goal is unmet *given* $(M_t, \Pi, N_h)$.

**Control regret** (policy-quality diagnostic):
$$\delta_{\mathrm{regret}} \;:=\; A_O(M_t;\, \Pi, N_h) - V_O(M_t, \pi_{\mathrm{current}};\, N_h) \;\ge\; 0.$$
$\delta_{\mathrm{regret}}$ measures the gap between the best available policy improvement and the agent's current policy. It is non-negative by the supremum definition of $A_O$.

The two quantities answer different diagnostic questions: $\delta_{\mathrm{sat}}$ asks "is the goal unattainable from here?"; $\delta_{\mathrm{regret}}$ asks "is the current policy suboptimal?" These can be true simultaneously, neither, or in either combination.

### 3.2  The $2{\times}2$ disambiguation

The $2{\times}2$ cross of the two gaps is the load-bearing diagnostic:

| | $\delta_{\mathrm{sat}} \le 0$ (attainable) | $\delta_{\mathrm{sat}} > 0$ (unmet) |
|---|---|---|
| $\delta_{\mathrm{regret}} \approx 0$ (near-optimal policy) | **Success**: goal achievable, policy good | **Capability limit**: optimally pursuing an unmet goal |
| $\delta_{\mathrm{regret}} \gg 0$ (suboptimal policy) | **Strategy problem**: goal achievable, policy poor | **Both**: goal hard *and* policy weak |

Each cell prescribes a different corrective action:

- **Success.** No corrective action.
- **Strategy problem.** Revise the policy. The signal is informative about *which* policy revision: the strategic-calibration residual decomposes $\delta_{\mathrm{regret}}$ over the policy's structural elements (Appendix A).
- **Capability limit.** $\delta_{\mathrm{regret}} \approx 0$ rules out *the policy* as the source of failure. The remaining candidates are the model ($M_t$ may be wrong about feasibility), the policy class ($\Pi$ may be too narrow), the horizon ($N_h$ may be too short), or the objective itself (revising $O$ as last resort). The order matters: $M_t$ first, then $\Pi$ and $N_h$, then $O$.
- **Both.** Revise the policy first (cheaper, more likely to be the issue), then re-evaluate $\delta_{\mathrm{sat}}$.

### 3.3  Distinction from existing two-term decompositions

The closest neighbor is [Long-Fei Li-Zhao-Zhou 2024]'s decomposition $\mathrm{DynReg}_K = C_{\mathrm{exp}} + C_{\mathrm{adapt}}$ (exploration / confidence-set vs.\ adaptation under non-stationarity); [Fei-Yang-Wang-Xie 2020] (POWER, POWER++) and [Stradi et al.\ 2024] (CMDP) share the shape. These decompose along an *uncertainty-source axis*; our $2{\times}2$ decomposes along a *goal-feasibility-vs-policy-quality axis*. The two are orthogonal: any of our four cells can host either a $C_{\mathrm{exp}}$ or $C_{\mathrm{adapt}}$ term; the diagnostic content is complementary.

Constraint-feasibility theory [Yang-Zheng-Tomizuka-Liu-Li 2024] uses "feasibility" for constraint-region feasibility (state stays in constraint set); our $\delta_{\mathrm{sat}}$ is goal feasibility (objective threshold attainable from $M_t$). Satisficing literature [Hajiabolhassan-Ortner 2025; Y. Zhang-Zhu-Xie 2026] uses "satisficing" for "any policy $\ge \beta$ acceptable"; our $\delta_{\mathrm{sat}}$ signals goal-relative infeasibility, not policy-relative satisficing. Vocabulary overlaps are genuine and warrant disambiguation per deployment.

### 3.4  Convention dependence

$\delta_{\mathrm{sat}}, \delta_{\mathrm{regret}}$ depend on the continuation convention $\pi_{\mathrm{cont}}$. Default is one-step improvement (C1, §2); receding-horizon (C2) and Bellman (C3) tighten the diagnostic at increasing cost. The monotonicity $\delta_{\mathrm{sat}}^{\mathrm B} \le \delta_{\mathrm{sat}}^{\mathrm{RH}} \le \delta_{\mathrm{sat}}^{(1)}$ (with reversed inequality on $\delta_{\mathrm{regret}}$) is proved in Appendix A.1. The $2{\times}2$ structure is preserved across all three; only the inferential force of "infeasible" vs.\ "locally stuck" varies. Analyses must state the convention.

---

## 4  Component 2 — A Point-Mass Reverse-KL/TV Identity for Action-Distribution Regret

### 4.1  Total-variation regret bound (textbook setup)

A bounded-value, deterministic-$\pi^*$ argument gives the textbook total-variation regret bound. From the regret expression $R(Q) = \sum_{a \neq a^*} Q(a) \Delta(a)$ and $\Delta(a) \le V_{\max}$,
$$R(Q) \;\le\; V_{\max} \sum_{a \neq a^*} Q(a) \;=\; V_{\max}\bigl(1 - Q(a^*)\bigr).$$
Under deterministic $\pi^* = \delta_{a^*}$, the total variation between $\pi^*$ and $Q$ is
$$\operatorname{TV}(\pi^*, Q) \;=\; \tfrac12 \sum_a |\pi^*(a) - Q(a)| \;=\; 1 - Q(a^*).$$
So
$$\boxed{\,R(Q) \;\le\; V_{\max} \cdot \operatorname{TV}(\pi^*, Q)\,} \qquad \text{(tight under deterministic } \pi^* + \text{ extremal value landscape).}$$
The bound is tight when $\Delta(a) = V_{\max}$ for all $a \neq a^*$; for typical landscapes it is loose by a factor $\mathbb E_{Q}[\Delta \mid a \neq a^*] / V_{\max} \in (0, 1]$. The matching lower bound via the action gap is:
$$R(Q) \;\ge\; \Delta_{\min} \cdot \operatorname{TV}(\pi^*, Q).$$

### 4.2  Point-mass reverse-KL/TV identity (deterministic $\pi^*$)

Under our canonical scope — deterministic optimum $\pi^* = \delta_{a^*}$ — reverse Kullback--Leibler divergence and total variation between $\pi^*$ and any policy $Q$ are related by an *exact identity*, not by an inequality.

**Theorem 4.1 (Point-mass reverse-KL/TV identity).** *For deterministic $\pi^* = \delta_{a^*}$ and any policy $Q$ with $Q(a^*) > 0$,*
$$\boxed{\;D_{\mathrm{KL}}(\pi^* \,\|\, Q) \;=\; -\log Q(a^*) \;=\; -\log\bigl(1 - \operatorname{TV}(\pi^*, Q)\bigr),\;}$$
*equivalently $\operatorname{TV}(\pi^*, Q) = 1 - e^{-D_{\mathrm{KL}}(\pi^* \,\|\, Q)}$.*

*Proof.* The reverse-KL collapses under the point-mass $\pi^*$:
$$D_{\mathrm{KL}}(\delta_{a^*} \,\|\, Q) \;=\; \sum_a \delta_{a^*}(a) \log\frac{\delta_{a^*}(a)}{Q(a)} \;=\; \log\frac{1}{Q(a^*)} \;=\; -\log Q(a^*),$$
where the convention $0 \log 0 = 0$ disposes of the $a \neq a^*$ terms. Combined with $\operatorname{TV}(\delta_{a^*}, Q) = 1 - Q(a^*)$ from §4.1, $-\log Q(a^*) = -\log(1 - \operatorname{TV})$. Solving for TV gives the equivalent form. $\square$

The identity is elementary — a direct two-line calculation under the point-mass assumption. It is *not* the Bretagnolle--Huber inequality at equality; the relationship between the two is the subject of §4.3.

### 4.3  Relationship to the Bretagnolle--Huber inequality (adjacent context)

The classical Bretagnolle--Huber inequality [Bretagnolle-Huber 1978; Tsybakov 2009 §2.4; Sason-Verdú 2016] holds in general,
$$\operatorname{TV}(P, Q) \;\le\; \sqrt{1 - e^{-D_{\mathrm{KL}}(P \,\|\, Q)}},$$
and is sharper than Pinsker for moderate-to-large $D_{\mathrm{KL}}$. Substituting the point-mass identity $\operatorname{TV}(\pi^*, Q) = 1 - e^{-D_{\mathrm{KL}}}$ of Theorem 4.1 into the BH right-hand side gives
$$1 - e^{-D_{\mathrm{KL}}(\pi^* \,\|\, Q)} \;\le\; \sqrt{1 - e^{-D_{\mathrm{KL}}(\pi^* \,\|\, Q)}},$$
which holds for all $D_{\mathrm{KL}} \in [0, \infty)$ (since $1 - e^{-D_{\mathrm{KL}}} \in [0, 1]$ and $x \le \sqrt{x}$ on this interval) but is *strict* for $D_{\mathrm{KL}} \in (0, \infty)$. So at the deterministic-$\pi^*$ corner, the BH inequality is *loose*: our identity supplies the *exact* TV value, which lies strictly below the BH upper envelope. Theorem 4.1 thus identifies the BH inequality as a strictly looser instrument at this special point — the two stand in a "general bound vs.\ exact value at a corner" relationship, with our identity strictly improving BH on this slice. We position BH here as the lineage the identity sits within rather than as the source it instantiates.

The identity's load-bearing consequence is that the bound it composes with the TV-regret inequality (§4.4) is *exact* in the deterministic-$\pi^*$ regime, not merely tighter-than-Pinsker.

### 4.4  Two-sided regret bound and Lipschitz equivalence

Composing Theorem 4.1 with the TV-regret bound of §4.1 and its action-gap lower bound:

**Theorem 4.2 (Two-sided point-mass regret bound).** *Under bounded value range $V_{\max}$, deterministic optimum $\pi^*$, isolated optimum so $\Delta_{\min} > 0$, and any policy $Q$ with $Q(a^*) > 0$,*
$$\boxed{\;\Delta_{\min}\bigl(1 - e^{-D_{\mathrm{KL}}(\pi^* \,\|\, Q)}\bigr) \;\le\; R(Q) \;\le\; V_{\max}\bigl(1 - e^{-D_{\mathrm{KL}}(\pi^* \,\|\, Q)}\bigr).\;}$$

*Proof.* From the TV-regret bound $R(Q) \le V_{\max} \cdot \operatorname{TV}(\pi^*, Q)$ and the point-mass identity $\operatorname{TV}(\pi^*, Q) = 1 - e^{-D_{\mathrm{KL}}}$:
$$R(Q) \;\le\; V_{\max}\bigl(1 - e^{-D_{\mathrm{KL}}(\pi^* \,\|\, Q)}\bigr).$$
The lower bound follows analogously from $R(Q) \ge \Delta_{\min} \cdot \operatorname{TV}$. $\square$

**Lipschitz equivalence and coordinate-optimality.** Theorem 4.2 gives $\Delta_{\min}/V_{\max} \le R(Q) / [V_{\max}(1 - e^{-D_{\mathrm{KL}}})] \le 1$ — Lipschitz equivalence with constants tight on extremal ($\Delta(a) = V_{\max}$) and uniformly-bad ($\Delta(a) = \Delta_{\min}$) landscapes respectively. The identity coordinate is therefore *coordinate-optimal among TV-only bounds*: any tighter bound on $R(Q)$ requires information beyond TV (the value-landscape spread).

**Scope.** Theorem 4.2 is per-state, one-step-improvement regret under fixed $M_t$ and C1 (§2); §7 invokes it per round and combines with a variation-budget block argument for cumulative dynamic regret.

**Multi-step chain-rule compositionality.** $D_{\mathrm{KL}}(\pi^*_{1:T} \,\|\, Q_{1:T}) = -\sum_t \log Q_t(a^*_t \mid h_t)$ — the negative log-likelihood of the optimal trajectory, equivalently the *behavior-cloning loss against optimal-trajectory data*. Per-step identity coordinates compose additively via the chain rule.

### 4.5  Strict improvement over Pinsker

Pinsker [Tsybakov 2009 §2.4; Cover-Thomas 2006 §11.6] gives $R(Q) \le V_{\max}\sqrt{D_{\mathrm{KL}}/2}$ without assuming deterministic $\pi^*$. Under the canonical scope, Theorem 4.2 strictly improves it: (i) *linear vs.\ square-root* — $1 - e^{-D_{\mathrm{KL}}} < \sqrt{D_{\mathrm{KL}}/2}$ for $D_{\mathrm{KL}} > 0$; (ii) *Pinsker becomes vacuous for $D_{\mathrm{KL}} > 2$* (exceeds the trivial $V_{\max}$ envelope), while $V_{\max}(1 - e^{-D_{\mathrm{KL}}})$ saturates at $V_{\max}$. The same comparison against BH gives $V_{\max}(1 - e^{-D_{\mathrm{KL}}}) < V_{\max}\sqrt{1 - e^{-D_{\mathrm{KL}}}}$ on $(0, \infty)$. Worked numerical comparison: Appendix B.

### 4.6  Perturbative extension to $\epsilon$-stochastic optima; where Pinsker / BH are the right tool

Deterministic $\pi^*$ is canonical for finite-MDP RL with isolated optima [Lattimore-Szepesvári 2020]; tied-optimum extensions (Appendix A.5). The deterministic regime is the unperturbed limit, not a hard wall:

**Theorem 4.3 (Perturbative identity for $\epsilon$-stochastic optima).** *For $\epsilon$-greedy $\pi^*_\epsilon$ and any policy $Q$ with full-support lower bound $Q(a) \ge q_0 > 0$ for all $a \in \mathcal A$,*
$$\boxed{\;\operatorname{TV}(\pi^*_\epsilon, Q) \;=\; 1 - e^{-D_{\mathrm{KL}}(\pi^*_\epsilon \,\|\, Q)} + O\!\left(\epsilon \log(1/\epsilon)\right).\;}$$
*The correction vanishes uniformly as $\epsilon \to 0$ and is sub-linear in $\epsilon$ (slower than $\epsilon$ but vanishing). For softmax-regularized $\pi^*_\tau \propto \exp(Q_O/\tau)$ with temperature $\tau$, the correction is $O(\exp(-\Delta_{\min}/\tau))$ — exponentially small in $1/\tau$.*

Derivation in Appendix A.6. The two-sided regret bound of Theorem 4.2 transfers with the same correction order. **Outside the perturbative regime** — genuinely high-entropy optima, tied-optimum, hard-exploration — the BH inequality $\operatorname{TV} \le \sqrt{1 - e^{-D_{\mathrm{KL}}}}$ is the relevant general bound; Pinsker is the textbook fallback.

### 4.7  Direction of the divergence is forced

Reverse-KL is forced: forward-KL $D_{\mathrm{KL}}(Q \,\|\, \pi^*) = +\infty$ whenever $Q$ has off-optimum mass (since $\pi^*(a) = 0$ for $a \neq a^*$). Within the reverse direction, reverse-KL is *uniquely* selected by the chain-rule additivity axiom [Hobson 1969; Csiszár 1991] — the only smooth $f$-divergence decomposing additively over factorizations (Appendix C). Detailed direction-forcing argument and admissible-divergence comparison: Appendix A.2-A.3.

### 4.8  Position within the information-theoretic RL literature

The information-theoretic regret line [Russo-Van Roy 2014a, 2014b; Lu-Van Roy 2019; Min-Russo 2023; Lattimore-György 2021; Kakade-Krishnamurthy-Lowrey-Ohnishi-Sun 2020] uses entropy / mutual information / information ratio / Pinsker / Hellinger as the connective tissue between regret and policy-space divergence; none deploys the Bretagnolle--Huber inequality, let alone its point-mass exact specialization. A structured prior-art retrieval (63 papers; ~75% abstract-level coverage; Appendix D) confirms BH and Theorem 4.1 are absent from the RL/non-stationary corpus.


---

## 5  Component 3 — Strategic Tempo and the Forgetting Prerequisite

### 5.1  Strategic tempo: aggregate and bottleneck forms

The agent's rate of *useful* policy revision is a structural quantity, not a tunable hyperparameter. In a non-stationary environment, this rate must keep pace with the rate at which the environment invalidates the agent's existing policy. We make this precise.

Consider a structured policy with $|E|$ revisable elements (in our analysis, edges of an internal causal model the agent maintains over policy components; in [Lee et al.\ 2023]'s ProST, sub-policies indexed by a tempo schedule). Index by $(i, j)$. For each element, three quantities are relevant:
- $\nu_{ij}$ — the effective observation rate at which the agent receives evidence about element $(i, j)$.
- $\iota_{ij} \in [0, 1]$ — an identifiability coefficient: the fraction of the evidence stream that genuinely identifies the element's causal effect, rather than reflecting confounded association.
- $\eta_{\mathrm{edge}, ij}$ — the per-element update gain (how much the agent revises element $(i, j)$ per unit of evidence).

**Strategic tempo (aggregate / throughput form).**
$$\mathcal T_\Sigma^{\mathrm{agg}} \;:=\; \sum_{(i,j) \in E} \nu_{ij} \cdot \iota_{ij} \cdot \eta_{\mathrm{edge}, ij}.$$
The per-element product factors three distinct considerations: how often evidence arrives ($\nu$), how *identifiable* the element is from that evidence ($\iota$), and how informative each piece of evidence is for revising the element ($\eta$). Each is an independent gate on per-step expected correction: switching any one to zero kills the per-element correction rate. The $\iota$ factor captures the regime distinction of Section 6 — in intervention-rich regimes (Regime A; software, controlled experiments) $\iota \approx 1$; in observation-only regimes (Regime C) $\iota \approx 0$.

**Strategic tempo (bottleneck / threshold form).** The aggregate sum measures total throughput across the policy DAG. The quantity that determines structural persistence is the per-element minimum:
$$\mathcal T_\Sigma^{\mathrm{bn}} \;:=\; \min_{(i,j) \in E} \big(\nu_{ij} \cdot \iota_{ij} \cdot \eta_{\mathrm{edge}, ij}\big).$$
The two satisfy $\mathcal T_\Sigma^{\mathrm{bn}} \le \mathcal T_\Sigma^{\mathrm{agg}}/|E| \le \mathcal T_\Sigma^{\mathrm{agg}}$. The bottleneck enters the persistence threshold (§5.3); the aggregate enters the necessary-condition $\mathcal T_\Sigma^{\mathrm{agg}} > |E| \rho_\Sigma/R_\Sigma$ and the channel-capacity / sustained-information-rate floor.

The structural parallel with adaptive (epistemic) tempo $\mathcal T = \sum_k \nu^{(k)} \eta^{(k)*}$ from adaptive control [Khalil 2002] is exact for the aggregate form at $\iota \equiv 1$. The $\iota$ factor and the bottleneck aggregator are what distinguish strategic from epistemic tempo: an agent with high observation rate and high update gain still has zero strategic tempo at any element whose causal effect is unidentifiable, and the bottleneck of the whole DAG is then zero.

**Why bottleneck, not sum, in the threshold.** A Lyapunov derivation under adversarial disturbance (Appendix A.7 / proof of Theorem 5.1) requires the inequality $\sum_{(i,j)} \alpha_{ij} \delta_{ij}^2 \ge c \cdot \|\boldsymbol\delta_\Sigma\|^2$ to hold for all $\boldsymbol\delta_\Sigma$. The largest $c$ for which this holds uniformly is $\min_{(i,j)} \alpha_{ij}$, not $\sum_{(i,j)} \alpha_{ij}$ — adversarial concentration of $\boldsymbol\delta_\Sigma$ on the weakest element saturates the bound at the bottleneck. The same effect makes per-dimension persistence conditions sharper than scalar conditions in adaptive control [Anderson 1985]: scalar tempo can overstate the available margin because the worst-case disturbance concentrates on the slowest-correcting dimension.

### 5.2  Persistence condition for strategy

Adaptive control's persistence condition is $\alpha > \rho / R$ — correction rate $\alpha$ exceeding disturbance-to-reserve ratio — a Lyapunov-derived ultimate-boundedness criterion under sector-bounded correction and bounded disturbance [Khalil 2002 Ch.\ 4, 9; Khasminskii 2012; Anderson-Moore 1979; Anderson 1985; Kreisselmeier 1986]. We instantiate this template on the *strategic substate*: $\delta_\Sigma$ = policy-mismatch state (gap between current policy and best available revision), $\rho_\Sigma$ = environment's policy-invalidation rate, $R_\Sigma$ = strategic reserve. Under sector-bounded strategic correction with parameter $\alpha_\Sigma$,
$$\alpha_\Sigma \;>\; \rho_\Sigma / R_\Sigma. \tag{P}$$
The form is inherited; what is new is the strategic-substate instantiation and the forgetting-prerequisite specialization (§5.3).

### 5.3  The forgetting prerequisite and the structural class $\mathcal A_{\mathrm{accum}}$

Condition (P) is an *instantaneous* check at the current operating point of the agent. For Bayesian-style policy updates (e.g., Beta-Bernoulli edge updates in a structured policy DAG, or any update mechanism whose effective sample size grows with experience), the per-element sector parameter has the form
$$\alpha_{ij} \;=\; \nu_{ij} \cdot \iota_{ij} \cdot \eta_{\mathrm{edge}, ij}, \qquad \eta_{\mathrm{edge}, ij} \;=\; 1 / (n_{ij} + 1),$$
with $n_{ij}$ the accumulated experience at element $(i,j)$. Define the structural class
$$\mathcal A_{\mathrm{accum}} \;:=\; \big\{\text{updates with } n_{\mathrm{eff}}(t) \to \infty \text{ as } t \to \infty\big\}$$
— count-accumulating Bayesian updates without forgetting; bounded-memory schemes with growing memory; observation-aggregating schemes without restart; *and gradient-based methods with vanishing step sizes* (e.g., $\eta_t = c/t^\beta$ for $\beta \in (0, 1]$, the Robbins–Monro regime), where the per-element correction rate $\alpha_{ij}^{(t)} \propto \eta_t$ decays to zero asymptotically. *For any fixed $(\rho_\Sigma, R_\Sigma)$ with $\rho_\Sigma > 0$, every agent in $\mathcal A_{\mathrm{accum}}$ eventually violates condition (P)* — at every element, the per-element correction rate decays to zero, so the bottleneck $\mathcal T_\Sigma^{\mathrm{bn}}$ decays to zero with experience.

This is a *structural failure of the class $\mathcal A_{\mathrm{accum}}$*, not a tuning problem. The agent's prior calibration cannot help: at any finite calibration level, the correction rate decays below threshold once enough experience accumulates. The class is broader than the Bayesian / sample-counting picture suggests — any update mechanism whose effective rate $\alpha_{ij}^{(t)}$ tends to zero asymptotically falls in $\mathcal A_{\mathrm{accum}}$, including the canonical Robbins–Monro stochastic approximation regime. Mechanisms outside $\mathcal A_{\mathrm{accum}}$ — constant-step-size stochastic approximation, sliding-window updates, bounded-memory learners, block-restart schemes — escape this asymptotic decay by maintaining a finite correction-rate ceiling, but then face a *bidirectional threshold* (§5.3.1).

The canonical fix is exponential forgetting: at each step, shrink per-element pseudo-counts by a factor $\lambda_{ij} \in (0, 1)$ before incorporating new evidence,
$$\alpha_k \mapsto \lambda_{ij} \alpha_k + y_k, \qquad \beta_k \mapsto \lambda_{ij} \beta_k + (1 - y_k).$$
The per-element effective sample size stabilizes at $n_{\mathrm{eff}, ij} \approx 1/(1-\lambda_{ij})$, giving steady-state per-element sector parameter
$$\alpha_{ij}^{\mathrm{ss}} \;\approx\; \nu_{ij} \cdot \iota_{ij} \cdot (1 - \lambda_{ij}).$$

**Theorem 5.1 (Multi-factor forgetting prerequisite — sharp threshold within the sector-Lyapunov model).** *Under policy-update-with-forgetting dynamics with per-element forgetting rate $\lambda_{ij} \in (0,1)$, regime-adjusted identifiability $\iota_{ij} \in [0,1]$, and topology-determined per-element observation rate $\nu_{ij} \in [0,1]$, and conditions (T1)–(T3) of [Khalil 2002 §9] applied to the strategic substate, ultimate boundedness of the modeled aggregate mismatch $\boldsymbol\delta_\Sigma$ is achieved when*
$$\boxed{\;\mathcal T_\Sigma^{\mathrm{bn,ss}} \;:=\; \min_{(i,j) \in E} \big(\nu_{ij} \cdot \iota_{ij} \cdot (1 - \lambda_{ij})\big) \;>\; \rho_\Sigma / R_\Sigma,\;}$$
*with steady-state bound $R_\Sigma^* = \rho_\Sigma / \mathcal T_\Sigma^{\mathrm{bn,ss}}$. The threshold is sharp within the modeled regime: when reversed, an adversarial disturbance concentrating on the bottleneck element drives $\|\boldsymbol\delta_\Sigma\| > R_\Sigma$. (Proof sketch: Appendix A.7.)*

The right-hand side is environment-side (disturbance rate, reserve); the left-hand side is a bottleneck over per-element products of observation rate, identifiability, and discount rate. The theorem is a sufficient-and-sharp statement *inside* the sector-Lyapunov model; it is silent about correction architectures outside this model (alternative non-sector-bounded revisions, or boundedness produced by mechanisms other than the modeled $\alpha_\Sigma$-correction).

**Corollary 5.1a (Beta-Bernoulli normalization — recovers the single-factor form).** *Under $\nu_{ij} = \iota_{ij} = 1$ for all elements and uniform $\lambda_{ij} = \lambda$, $\mathcal T_\Sigma^{\mathrm{bn,ss}} = 1-\lambda$, recovering the textbook persistence condition $(1-\lambda) > \rho_\Sigma/R_\Sigma$.*

**Corollary 5.1b (Aggregate necessary condition).** *Under uniform domain parameters, $\mathcal T_\Sigma^{\mathrm{agg,ss}} > |E| \cdot \rho_\Sigma/R_\Sigma$ is necessary (but not sufficient) for the bottleneck condition — a cheap fail-fast pre-check.*

**Worked-topology corollaries** (each surfacing a different factor as binding; full Beta-Bernoulli derivations in §A.8):
- *Single edge.* $\mathcal T_\Sigma^{\mathrm{bn,ss}} = \nu \iota (1-\lambda)$ — all three factors load-bearing.
- *AND-chain depth-$d$ with observable intermediates.* $\mathcal T_\Sigma^{\mathrm{bn,ss}} = \min_k \prod_{j<k}\theta_j (1-\lambda_k)$ — *depth-gated*: each downstream edge inherits attenuation from the success probability of upstream edges.
- *OR-node $\varepsilon$-greedy.* $\mathcal T_\Sigma^{\mathrm{bn,ss}} = \min\bigl((1-\varepsilon)(1-\lambda_1),\ \varepsilon(1-\lambda_2)\bigr)$ — *exploration-gated*: pure greedy ($\varepsilon = 0$) collapses the bottleneck on the unexplored arm.
- *Any single Regime-C edge.* $\iota_{ij} = 0$ at any single element $\Rightarrow \mathcal T_\Sigma^{\mathrm{bn,ss}} = 0$. **Regime-C contamination is structurally fatal**: an agent with even one fully-confounded element cannot meet the threshold by forgetting alone.

### 5.3.1  Bidirectional thresholds for non-accumulating mechanisms

Mechanisms outside $\mathcal A_{\mathrm{accum}}$ stabilize the sector parameter $\alpha_\Sigma$ at a finite ceiling. Within the sector-Lyapunov reduction, the forgetting prerequisite then holds *iff* that ceiling exceeds $\rho_\Sigma/R_\Sigma$:

| Mechanism | $\alpha_\Sigma^{\mathrm{ss}}$ | Prerequisite holds iff |
|---|---|---|
| Exponential forgetting with $\lambda$ | $1 - \lambda$ | $1 - \lambda > \rho_\Sigma/R_\Sigma$ |
| Constant-step SA with rate $\eta$ | $\eta$ | $\eta > \rho_\Sigma/R_\Sigma$ |
| Sliding-window size $W$ | $1/W$ | $W < R_\Sigma/\rho_\Sigma$ |
| Bounded-memory size $K$ | $1/K$ | $K < R_\Sigma/\rho_\Sigma$ |
| Block-restart with period $T_R$ | $1/T_R$ | $T_R < R_\Sigma/\rho_\Sigma$ |

For each row, the threshold is *bidirectional* and sharp within the sector-Lyapunov reduction (the converse-direction failure is also within the model). The structural-class theorem of §5.3 covers $\mathcal A_{\mathrm{accum}}$ universally; the thresholds above cover the standard non-accumulating mechanisms in the literature individually. A mechanism that is *neither* count-accumulating nor of one of the five tabulated forms (e.g., persistent-exploration paired with count-accumulating updates on the exploit arm) inherits the $\mathcal A_{\mathrm{accum}}$ verdict on the accumulating component: persistent exploration alone does not escape the asymptotic-decay class.

### 5.4  Structural, not heuristic

The prerequisite is a *structural threshold*, not a hyperparameter:
- *Every factor load-bearing per element.* $\nu_{ij}, \iota_{ij}, (1-\lambda_{ij})$ enter multiplicatively; zeroing any collapses the bottleneck. Not interchangeable: low identifiability cannot be compensated by faster forgetting.
- *Asymptotic failure of count-accumulators.* $\lambda_{ij} = 1$ at any element gives $\eta_{\mathrm{edge}, ij} \to 0$ — the structural-class theorem (§5.3) covers $\mathcal A_{\mathrm{accum}}$ universally.
- *Sharpness within the model.* Below threshold, adversarial disturbance on the bottleneck drives $\|\boldsymbol\delta_\Sigma\| > R_\Sigma$; above, it bounds at $R_\Sigma^* = \rho_\Sigma / \mathcal T_\Sigma^{\mathrm{bn,ss}}$. Stabilization by out-of-model mechanisms is not excluded.
- *Aggregate vs.\ bottleneck.* $\mathcal T_\Sigma^{\mathrm{agg,ss}}$ governs throughput / channel capacity; $\mathcal T_\Sigma^{\mathrm{bn,ss}}$ governs survival. Both assessed; only bottleneck enters (P).
- *No prior-learning subsidy.* The threshold is on steady-state operation, not initial accuracy.

### 5.5  Lifting [Lee et al.\ 2023] from hyperparameter to structural threshold

[Lee et al. 2023] (the ProST framework, NeurIPS 2023) is the closest published neighbor. ProST defines an *agent tempo* — the schedule of policy update times $\{t_1, \dots, t_K\}$ — and computes the schedule that minimizes the dynamic regret upper bound under non-stationarity. The companion paper [Lee et al. 2024] (Pausing Policy Learning, ICML 2024) shows that *non-zero policy hold duration* yields sharper dynamic regret. Together, they establish tempo as a convergence-relevant variable in non-stationary RL.

Our forgetting prerequisite *lifts* the ProST move along two axes simultaneously: (a) *single-factor → multi-factor*: ProST's tempo is a single scalar (update frequency); ours is a per-element bottleneck over $\nu \cdot \iota \cdot (1-\lambda)$, with each factor independently load-bearing. ProST's schedule recovers as the special case $|E| = 1$, $\nu = \iota = 1$. (b) *Hyperparameter-optimization → structural-survival inequality*: ProST asks *given* an environment, *what tempo schedule* minimizes regret? The forgetting prerequisite asks *given* an environment with disturbance $\rho_\Sigma$ and reserve $R_\Sigma$, *what is the minimal bottleneck below which no schedule persists?* The two questions are complementary; ProST's optimal-schedule result is silent about the *threshold* below which no schedule works.

Concretely, in the Lee et al.\ ProST setup, holding policy fixed between updates is a *block-restart* mechanism. The sector-level equivalence is rigorous:

**Lemma 5.2 (ProST-forgetting equivalence at the sector parameter level).** *Under ProST's block-restart update with $K$ uniform updates over $T$ rounds (block length $\Delta = T/K$), the steady-state sector parameter satisfies $\alpha_\Sigma^{\mathrm{ss}} = K/T$. Within the sector-Lyapunov reduction, this is the same steady-state sector parameter as exponential forgetting with $1 - \lambda_{\mathrm{eff}} = K/T$. The two mechanisms are not equivalent at the cycle-by-cycle level — block-restart freezes between updates while exponential forgetting smoothly discounts — but coincide at the level of the steady-state ultimate-boundedness threshold.*

Applied to ProST: when $K/T > \rho_\Sigma / R_\Sigma$, ProST's schedule satisfies the forgetting prerequisite and the modeled mismatch dynamic remains ultimately bounded, consistent with ProST's sublinear dynamic-regret upper bound. When $K/T < \rho_\Sigma / R_\Sigma$, the schedule fails the prerequisite *within the sector-Lyapunov reduction*, and the modeled mismatch is not ultimately bounded — suggesting an analogous threshold for ProST-style dynamic regret. We do not claim that *every* RL algorithm's dynamic regret diverges in this regime; the suggestion holds for algorithms whose policy-revision dynamics fall under the sector-Lyapunov model. Section 8 develops the reduction in detail.

### 5.6  Distinction from sliding-window and weighted-LSVI forgetting

Forgetting mechanisms appear throughout non-stationary RL — [Garivier-Moulines 2008] (sliding-window UCB); [Touati-Vincent 2020] (OPT-WLSVI exponential weighting); [Russac-Vernade-Cappé 2019] (weighted linear bandits); [Cheung-Simchi-Levi-Zhu 2020] (sliding-window with confidence widening) — each as an *algorithmic mechanism* with a tunable parameter optimizing dynamic regret. The prerequisite reframes this machinery: $(1-\lambda) > \rho_\Sigma/R_\Sigma$ as *structural survival condition*, with environment-side parameters on the RHS. The reframe matters: dynamic-regret-optimization leaves open whether *some* schedule satisfies the prerequisite *every* environment; the threshold form shows that within the sector-Lyapunov reduction it does not — for $\rho_\Sigma / R_\Sigma \ge 1$, no $\lambda \in (0, 1)$ suffices, making existence of such failure regimes explicit. Out-of-model stabilization is not addressed.

---

## 6  Component 4 — Closed-Loop Interventional Access

### 6.1  Pearl's causal hierarchy

Pearl's causal hierarchy [Pearl 2009; Bareinboim-Correa-Ibeling-Icard 2022] partitions causal queries into three levels:

- **Level 1 (associational):** $P(Y \mid X)$ — what is the distribution of $Y$ given that we *observed* $X$?
- **Level 2 (interventional):** $P(Y \mid do(X))$ — what is the distribution of $Y$ given that we *intervened to set* $X$?
- **Level 3 (counterfactual):** $P(Y_X \mid X', Y')$ — what would $Y$ have been *had* $X$ been set, given that we observed $X'$ and $Y'$?

The **causal-hierarchy theorem** [Bareinboim et al.\ 2022, Theorem 1] establishes that, under broad conditions, the levels are *strictly nested*: Level 2 queries are not in general identifiable from Level 1 data alone, and Level 3 queries are not in general identifiable from Level 2 data alone. Causal claims at Level 2 require interventional data (or strong structural assumptions enabling identification from observation, e.g., back-door admissible adjustment sets [Pearl 2009]).

### 6.2  The loop generates interventional data

**Theorem 6.1 (Loop-as-Level-2-engine).** *In a feedback loop, the agent's action $a_t$ causally precedes the next observation $o_{t+1}$, so the conditional mismatch $\delta_t \mid a_t$ is interventional in character: it tells the agent how the environment responded to its specific intervention $a_t$, relative to the model's prediction.*

By temporal ordering and the singular-trajectory commitment of §2, $a_t$ is a cause of $o_{t+1}$, not a correlate; replaying a saved $M_t$ against a different event stream is not equivalent to intervening. The loop generates Level 2 data by construction, independent of the agent's internal architecture (a Q-learning or transformer-based agent still operates in an action-perception loop with Level 2 data generation). Whether the agent *exploits* this data for Level 2 reasoning depends on the update mechanism.

### 6.3  Interventional *data* is not identified *do*-estimates

Action-generated data is Level 2 in *character*, but a clean estimate of $P(o \mid do(a), \Omega_t)$ requires overcoming four typical obstacles: coverage (diverse actions tried), within-step confounding, delay (consequences past $t+1$), and partial observability. We honor this distinction: Theorem 4.2 uses $D_{\mathrm{KL}}(\pi^* \,\|\, Q)$ with $\pi^*$ computed under $M_t$; the KL coordinate $-\log Q(a^*)$ is computable directly from the policy, but the meaning of $a^*$ matching the true optimum depends on causal identification strength from loop data. Three regimes [Bareinboim et al.\ 2022 taxonomy]:

- **Regime A (intervention-rich, $\iota \approx 1$).** Software tests, controlled labs. $do$-effects identified cleanly; bound realizable on-policy.
- **Regime B (partial, $\iota \in (0, 1)$).** Mixed observation-intervention. Bound holds for the model the agent identifies; bias $\propto 1 - \iota$.
- **Regime C (observation-only, $\iota \approx 0$).** Passive monitoring. Bound provable analytically but not realizable on-policy; observer-on-sub-agent extensions can recover identifiability in subcases.

### 6.4  Closing the gap: the bound as learnable

Components 2 and 4 jointly make the bound *learnable*: §4's point-mass identity supplies the analytic regret-KL relationship; §6's loop-Level-2 access supplies the data substrate from which $\pi^*$ — and therefore the bound's RHS — can be empirically estimated under sufficient identifiability. Without 2: data without metric structure on policy space. Without 4: metric structure provable but not usable on-policy.

### 6.5  Distinction from active inference and causal-RL precursors

Action-perception-loop frameworks — active inference [Friston-FitzGerald-Rigoli-Schwartenbeck-Pezzulo 2017; Parr-Pezzulo 2022], control-as-inference [Levine 2018], cybernetics [Wiener 1948; Conant-Ashby 1970] — implicitly use the action-causes-observation observation. Our distinctive moves:

- *Bareinboim-hierarchy connection.* Active inference / cybernetics rest on Bayesian-network (Level 1) generative models; we invoke the causal-hierarchy theorem to position the policy DAG as causal with $do$-conditioning in $Q_O$ (§2).
- *Regime-indexed identifiability (A/B/C).* AI literature treats causal identifiability uniformly within modeling assumptions; we surface the regime split at framework level.
- *Scope honesty.* We distinguish "data generated under intervention" from "cleanly identified $do$-estimates"; [Bruineberg-Dolega-Dewhurst-Baltieri 2022] documents that the active-inference literature sometimes elides this.

The causal-RL line [Junzhe Zhang-Bareinboim 2016, 2022; Lu-Meisami-Tewari 2021, 2022; Wang-Yang-Wang 2021 DOVI; Junzhe Zhang 2020] is the direct ancestor for regime-indexed identifiability and on-policy interventional access; all are stationary-MDP. Composition with non-stationarity is, to our knowledge, novel.

---

## 7  Composition Theorem

The four components compose. Taken together, they give a non-stationarity-aware convergence theory with three properties no existing strand has individually:

1. **Handles non-stationarity** via the strategic tempo and forgetting prerequisite (Component 3).
2. **Has explicit metric structure on policy space** via the point-mass reverse-KL/TV identity (Component 2).
3. **Is learnable on-policy** via closed-loop interventional access (Component 4).

The two-gap diagnostic (Component 1) is the connective tissue that routes regret-signal interpretation: it tells the agent *which* corrective action the regret signal warrants, and therefore *which* of the three properties must be invoked at any given moment.

### 7.1  Composition theorem (cumulative dynamic regret form)

**Theorem 7.1 (Composition with cumulative dynamic regret under non-stationarity).** *Let $(\mathcal S, \mathcal A, P_t, r_t, N_h)$ be a non-stationary MDP with bounded reward, finite action space, deterministic optimum policy $\pi^*_t$ at each round, and isolated optima with action gap $\Delta_{\min} > 0$. Suppose the agent operates on a singular causal trajectory in the sense of Section 2 and updates its policy with per-element exponential forgetting at rates $\{\lambda_{ij}\}$. If*

> **(A1) Metric on policy space.** *The agent's policy $Q_t$ satisfies the canonical scope of Section 4: $Q_t(a^*) > 0$ at every round.*

> **(A2) Multi-factor forgetting prerequisite.** *The bottleneck strategic tempo $\mathcal T_\Sigma^{\mathrm{bn,ss}} := \min_{(i,j) \in E} \nu_{ij} \cdot \iota_{ij} \cdot (1 - \lambda_{ij})$ exceeds the disturbance-to-reserve ratio $\rho_\Sigma / R_\Sigma$. (Reduces to $1 - \lambda > \rho_\Sigma/R_\Sigma$ under uniform-Regime-A normalization.)*

> **(A3) Identifiability regime.** *The agent operates in Regime A or Regime B with identifiability coefficient $\iota$.*

> **(A4) Two-gap diagnostic.** *The agent applies the satisfaction-gap / control-regret $2{\times}2$ to route corrective action.*

> **(A5) Identity-tight base learner (for conclusion (v)).** *In each stationary block between optimum-change events, the base learner achieves per-round regret coordinate $\mathbb E[1 - e^{-K_t}] \le c \cdot t^{-1/2}$ for a constant $c$ independent of the block (Appendix E surveys Thompson sampling and UCB as instances).*

*Then:*

*(i) Per-round regret is two-sided identity-bounded:*
$$\Delta_{\min}\bigl(1 - e^{-D_{\mathrm{KL}}(\pi^*_t \,\|\, Q_t)}\bigr) \;\le\; R(Q_t) \;\le\; V_{\max}\bigl(1 - e^{-D_{\mathrm{KL}}(\pi^*_t \,\|\, Q_t)}\bigr).$$

*(ii) Aggregate mismatch $\boldsymbol\delta_\Sigma$ is ultimately bounded under non-stationarity, with steady-state bound $R_\Sigma^* = \rho_\Sigma / \mathcal T_\Sigma^{\mathrm{bn,ss}}$.*

*(iii) The KL coordinate $D_{\mathrm{KL}}(\pi^*_t \,\|\, Q_t)$ is estimable from on-policy data via the empirical visit-frequency estimator $\hat D := -\log \hat Q_n(a^*_{\mathrm{ag}})$ satisfying, on the event $\{Q(a^*) \ge q_0\}$,*
$$\mathbb E[|\hat D - D_{\mathrm{KL,true}}|] \;\le\; (1-\iota)\log(1/q_0) \;+\; q_0^{-1}\sqrt{\log 2 / (2n)};$$
*the variance term decays at standard $1/\sqrt n$; the bias term scales linearly in $1 - \iota$ (Regime A, $\iota = 1$: zero bias; Regime B, $\iota \in (0,1)$: bias controlled by conflated-edge mass; Regime C, $\iota = 0$: bias up to $\log(1/q_0)$). Proof in Appendix F.*

*(iv) The $2{\times}2$ cell containing $(\delta_{\mathrm{sat}}, \delta_{\mathrm{regret}})$ identifies the corrective action class: revise policy (regret-driven), revise model/policy-class/horizon (capability-driven), or revise objective (last resort).*

*(v) Cumulative dynamic regret obeys*
$$\mathrm{DynReg}(T) \;:=\; \sum_{t=1}^T R_t(Q_t) \;\le\; 2c\,V_{\max}\,\sqrt{(B_T+1)\,T},$$
*where $B_T := |\{t : a^*_t \ne a^*_{t-1}\}|$ counts optimum-change events (so the time axis is partitioned into $B_T + 1$ stationary blocks). The per-round coordinate is sharper than Pinsker / BH; the cumulative rate matches the variation-budget literature [Cheung et al.\ 2020; Wei--Luo 2021] as a corollary, not as a new rate.*

The proof composes four component theorems with one variation-budget block argument: Theorem 4.2 gives (i); Theorem 5.1 gives (ii); the empirical-estimator analysis of Appendix F gives (iii); the $2{\times}2$ disambiguation of Section 3 gives (iv); for (v), partition $[1, T]$ into $B_T + 1$ stationary blocks at the optimum-change events; in each block the per-round identity bound combines with (A5) and Cauchy–Schwarz across blocks, $\sum_{i=0}^{B_T} \sqrt{\Delta_i} \le \sqrt{(B_T+1)\,T}$ where $\Delta_i$ is the $i$-th block length. Detailed proof of (v) in Appendix G.

**Remarks.**

- Under Thompson sampling or UCB as the base learner, (A5) holds with a stronger logarithmic rate $\mathbb E[1 - e^{-K_t}] = O(\log t / (t\Delta_{\min}))$ in each stationary block (Appendix E), giving cumulative regret $O(V_{\max} (B_T+1) \log^2(T/(B_T+1)) / \Delta_{\min})$ — sharper than $\sqrt{(B_T+1)\,T}$ when $\Delta_{\min}$ is bounded away from zero. The square-root rate is the worst-case Cauchy–Schwarz bound; the logarithmic rate is the typical-case bound for stochastic-bandit base learners.

- Pointwise convergence $V(\pi_t) \to V^*$ is structurally unavailable under genuine non-stationarity (the target is itself moving). The right replacement is the *Cesàro tracking statement* $\frac{1}{T}\sum_t (V^*_t - V(\pi_t)) = O\!\bigl(\sqrt{(B_T+1)/T}\bigr) \to 0$ when $B_T = o(T)$, which is a corollary of (v) under (A5).

### 7.2  Assembly plus a derived cumulative-regret rate

The composition is *assembly + a derived rate*. Conclusions (i)–(iv) have published or directly-derived ancestors; conclusion (v) — $\mathrm{DynReg}(T) \le 2cV_{\max}\sqrt{(B_T+1)\,T}$ — is derived by composing the per-round identity (i) with the variation-budget block argument under (A5). The cumulative rate matches [Cheung et al.\ 2020; Wei-Luo 2021] as a corollary; the route through the per-round identity is new. Each prior strand has internal reasons not to need the others (dynamic-regret treats metric structure as orthogonal; info-theoretic treats non-stationarity as orthogonal; causal-RL treats non-stationarity as out-of-scope); composition cuts orthogonally, with each strand's machinery load-bearing for one of properties 1–3. The two-gap diagnostic makes the joint statement actionable rather than merely true.

### 7.3  Honest scope

The composition holds in the canonical scope of §2 (deterministic $\pi^*$, bounded value, isolated optimum, singular trajectory, finite horizon) and degrades cleanly on each axis: stochastic $\pi^*$ → BH inequality (one-sided); unbounded value → upper-bound trivial, lower bound via $\Delta_{\min}$ survives; non-isolated optima ($\Delta_{\min} = 0$) → lower bound lost, upper preserved; non-singular trajectories break the loop-Level-2 argument.

**Partial uniqueness (Component 2).** Reverse-KL is uniquely determined up to positive scaling under the chain-rule additivity axiom [Hobson 1969; Csiszár 1991] (Theorem C.1) — any non-stationary convergence theory satisfying property 2 plus chain-rule additivity must use reverse-KL as its policy-space metric. **No joint uniqueness.** Properties 1–3 do not force our specific composition: alternative correction architectures (outside the sector-Lyapunov model) and alternative interventional-access taxonomies exist in principle; full joint uniqueness would require further axioms (singular-trajectory commitment, sector-boundedness) beyond chain-rule additivity. Future work.


---

## 8  Worked Example: Conceptual Reduction to ProST [Lee et al.\ 2023]

[Lee et al.\ 2023]'s ProST parameterizes a non-stationary MDP's policy update by a tempo schedule $\{t_1, \dots, t_K\}$ — policy held fixed between update times — and optimizes the schedule against a dynamic-regret upper bound. We reduce ProST to a special case of Theorem 7.1; the reduction is rigorous at the steady-state sector-parameter level (Lemma 5.2) and conceptual at the cycle-by-cycle level.

**Forgetting rate $\leftrightarrow$ schedule density.** $K$ updates over $T$ rounds gives effective discount $\lambda_{\mathrm{eff}} \approx 1 - K/T$, i.e., forgetting rate $1 - \lambda_{\mathrm{eff}} \approx K/T$. **Prerequisite at non-vacuous bound.** ProST's sublinear dynamic-regret regime corresponds to $1 - \lambda_{\mathrm{eff}} > \rho_\Sigma / R_\Sigma$, i.e., (A2); below this threshold ProST's bound is vacuous. **Regime A** ($\iota \approx 1$) under full state observability; (A1), (A3) hold. **$2{\times}2$** trivializes ($\delta_{\mathrm{sat}} = 0$): only the strategy-problem cell is exercised — the cell ProST optimizes against. **Empirical grounding.** ProST's experiments confirm that optimal schedule density tracks environmental change rate, consistent with the §5.5 threshold; this *motivates* Component 3 rather than directly testing the sector-Lyapunov ultimate-boundedness conclusion.

**Variation-budget instantiation.** [Cheung-Simchi-Levi-Zhu 2020]'s SWUCRL2-CW corresponds to fixed $1-\lambda = 1/W$; [Wei-Luo 2021]'s black-box reduction tunes $W$ adaptively; [Mao et al.\ 2021]'s RestartQ-UCB approximates abrupt forgetting at restart times. Under our framework, each maps to $(1-\lambda) > \rho_\Sigma / R_\Sigma$ with $\rho_\Sigma$ identified up to a constant proportional to $V_T / T$ — conceptual reduction, not exact algorithmic identification. **Not yet covered:** causal-RL composition [Junzhe Zhang-Bareinboim 2022; Lu-Meisami-Tewari 2021] (Component 4); satisficing [Hajiabolhassan-Ortner 2025; Y. Zhang-Zhu-Xie 2026] (Component 1 with $\delta_{\mathrm{sat}} > 0$); future work.

---

## 9  Related Work

The four strands of prior art map onto our four components; none composes all four.

| Strand | Closest neighbors | Our distinction |
|---|---|---|
| 1. Dynamic regret under drift | [Cheung-Simchi-Levi-Zhu 2020; Wei-Luo 2021; Mao-Zhang-Zhu-Simchi-Levi-Başar 2021; Cheung-Simchi-Levi-Zhu 2022; Gajane-Ortner-Auer 2019] | Recovered as instances under the forgetting prerequisite (§5.5, §8.3); novelty is the *threshold form* — environment regimes where no $\lambda \in (0,1)$ stabilizes the sector-Lyapunov mismatch — which dynamic-regret-optimization analyses do not surface. |
| 2. Two-term decompositions | [Long-Fei Li-Zhao-Zhou 2024; Fei-Yang-Wang-Xie 2020; Stradi-Lunghi-Castiglioni-Marchesi-Gatti 2024]; [Yang-Zheng-Tomizuka-Liu-Li 2024] (constraint-feasibility) | Same shape, different axis: ours is goal-feasibility vs.\ policy-quality; theirs is uncertainty-source (or constraint-region feasibility for Yang et al.). Neither subsumes the other. |
| 3. Tempo and forgetting | [Lee et al.\ 2023 ProST; Lee et al.\ 2024; Touati-Vincent 2020; Russac-Vernade-Cappé 2019; Garivier-Moulines 2008] | Lifts ProST's single-factor tempo schedule to a multi-factor structural threshold (§5.5); recasts forgetting from tunable hyperparameter to survival inequality. |
| 4. Causal / interventional access | [Junzhe Zhang-Bareinboim 2022, 2016; Lu-Meisami-Tewari 2021, 2022; Wang-Yang-Wang 2021 DOVI; Junzhe Zhang 2020; Schulte-Poupart 2025] | Causal-RL line is stationary throughout; we compose with non-stationarity. |
| Cross-cutting (info-theoretic regret) | [Russo-Van Roy 2014a, 2014b; Lu-Van Roy 2019; Min-Russo 2023; Lattimore-György 2021; Canonne 2022] | Uses entropy / mutual information / Pinsker / Hellinger; the BH inequality and its point-mass exact corner (Theorem 4.1) are absent from this corpus (Appendix D). |
| Adjacent (satisficing / feasibility) | [Hajiabolhassan-Ortner 2025; Y. Zhang-Zhu-Xie 2026] | Vocabulary overlap on "satisficing"; their axis is "any policy $\ge \beta$ acceptable," ours is "goal unmet from $M_t$" (§3). |
| Contemporaneous (post-March 2026) | [Gerogiannis-Huang-Veeravalli 2026 DARLING; Y. Zhang-Zhu-Xie 2026] | Adjacent; neither composes the four components. |

---

## 10  Limitations and Conclusion

### 10.1  Limitations

**Theory-only.** No original experiments; mitigations are the two-line identity (Theorem 4.1, mathematically airtight), the ProST reduction (§8) for empirical grounding, and the honest-assembly framing of Theorem 7.1.

**Canonical scope.** Deterministic-$\pi^*$ is essential for the *identity*; under stochastic $\pi^*$ the BH inequality $\operatorname{TV} \le \sqrt{1 - e^{-D_{\mathrm{KL}}}}$ replaces it (one-sided regret bound). The perturbative extension of §4.6 covers $\epsilon$-greedy and softmax regimes; genuinely high-entropy optima fall back on BH/Pinsker. Tied-optimum sketched in Appendix A.5.

**Cumulative-regret scope.** Theorem 4.2 is per-state, one-step-improvement under C1; Theorem 7.1(v) lifts to $\mathrm{DynReg}(T) \le 2cV_{\max}\sqrt{(B_T+1)\,T}$ via the variation-budget block argument and (A5). The $B_T+1$ block count is structurally necessary (recovers $O(V_{\max}\sqrt T)$ at $B_T = 0$). Pointwise $V(\pi_t) \to V^*$ is unavailable under non-stationarity; the Cesàro tracking corollary $\frac{1}{T}\sum_t (V^*_t - V(\pi_t)) = O(\sqrt{(B_T+1)/T}) \to 0$ when $B_T = o(T)$ is the right replacement. Occupancy-measure convergence requires uniform-mixing analysis across $\{P_t\}$ — out of scope.

**Partial uniqueness; no joint uniqueness.** No existing framework composes all four components in the non-stationary regime (Appendix D). At the *metric layer*, reverse-KL is uniquely determined up to positive scaling under the chain-rule additivity axiom [Hobson 1969; Csiszár 1991] (Theorem C.1; §7.3). We do *not* claim joint uniqueness for the full composition; the structural-inequality and learnable-from-data layers admit alternative routes in principle. A full joint-uniqueness theorem would require further structural axioms (singular-trajectory commitment, sector-boundedness) — future work.

**Other.** $\rho_\Sigma, R_\Sigma$ are domain quantities (we give the threshold form, not the numerical value). Theorem 6.1's loop-Level-2 claim depends on a directed-separation property between $M_t$ and goal state; coupled-goal architectures (e.g., goal-conditioned LLM policies) require additional machinery, out of scope [Bruineberg et al.\ 2022].

### 10.2  Future work

Stochastic-$\pi^*$ extension beyond the perturbative regime (BH/Pinsker fallback). Tied-optimum extension (Appendix A.5). Coupled goal–model architectures via Bayesian inverse-problem stability [Stuart 2010; Sprungk 2019]. Algorithmic instantiation: a practical algorithm with identity-tight base learner (Appendix E), explicit forgetting schedule, regime check, and $2{\times}2$ readout — empirical evaluation deferred to a follow-up paper.

### 10.3  Conclusion

We assemble four components — two-gap diagnostic, point-mass reverse-KL/TV identity, multi-factor strategic-tempo forgetting prerequisite, closed-loop interventional access — into a unified non-stationary convergence theory. The composition yields cumulative dynamic regret $\mathrm{DynReg}(T) \le 2cV_{\max}\sqrt{(B_T+1)\,T}$ with per-round coordinate sharper than Pinsker/BH; the cumulative rate matches the variation-budget literature as a corollary. The point-mass identity at the deterministic-$\pi^*$ corner of the Bretagnolle--Huber [1978] family is the technical anchor — absent, to our knowledge, from the prior RL/non-stationary corpus.


---

## References

*Running list, alphabetical. To be cleaned and uniformized for camera-ready; flagged citations with **(verify)** are in the prior-art retrieval but warrant a final cross-check.*

- Abbasi-Yadkori, Y., György, A., Lazic, N. (2022). A new look at dynamic regret for non-stationary stochastic bandits. *J.\ Mach.\ Learn.\ Res.*
- Aczél, J., Daróczy, Z. (1975). *On Measures of Information and Their Characterizations*. Academic Press.
- Agarwal, A., Kakade, S., Lee, J., Mahajan, G. (2021). On the theory of policy gradient methods. *J.\ Mach.\ Learn.\ Res.*
- Amari, S., Nagaoka, H. (2000). *Methods of Information Geometry*. AMS.
- Anderson, B. D. O. (1985). Adaptive systems, lack of persistence of excitation, and bursting phenomena. *Automatica* 21(3):247–258.
- Anderson, B. D. O., Moore, J. B. (1979). *Optimal Filtering*. Prentice-Hall.
- Bareinboim, E., Correa, J., Ibeling, D., Icard, T. (2022). On Pearl's hierarchy and the foundations of causal inference. In *Probabilistic and Causal Inference: The Works of Judea Pearl*, ACM.
- Besbes, O., Gur, Y., Zeevi, A. (2013). Non-stationary stochastic optimization. *Operations Research*.
- Bhatia, K., Sridharan, K. (2020). Online learning with dynamics: a minimax perspective. *NeurIPS*. arXiv:2012.01705.
- Bretagnolle, J., Huber, C. (1978). Estimation des densités: risque minimax. *Séminaire de Probabilités XII*, Springer LNM 649.
- Bruineberg, J., Dolega, K., Dewhurst, J., Baltieri, M. (2022). The Emperor's new Markov blankets. *Behavioral and Brain Sciences*.
- Canonne, C. (2022). A short note on an inequality between KL and TV. *arXiv:2202.07198*.
- Cheung, W. C., Simchi-Levi, D., Zhu, R. (2022). Hedging the drift: learning to optimize under non-stationarity. *Management Science* 68(3):1696–1713; SSRN 2018, arXiv:1903.01461 (2019).
- Cheung, W. C., Simchi-Levi, D., Zhu, R. (2020). Reinforcement learning for non-stationary Markov decision processes: the blessing of (more) optimism. *arXiv:2006.14389*.
- Conant, R., Ashby, W. R. (1970). Every good regulator of a system must be a model of that system. *Int.\ J.\ Systems Sci.*
- Cover, T., Thomas, J. (2006). *Elements of Information Theory*. Wiley.
- Csiszár, I. (1991). Why least squares and maximum entropy? An axiomatic approach to inference for linear inverse problems. *Annals of Statistics* 19(4):2032–2066.
- Dick, T., György, A., Szepesvári, C. (2014). Online learning in MDPs with changing cost sequences. *ICML*.
- Fei, Y., Yang, Z., Wang, Z., Xie, Q. (2020). Dynamic regret of policy optimization in non-stationary environments. *arXiv:2007.00148*.
- Foster, D., Rakhlin, A., Sekhari, A., Sridharan, K. (2022). On the complexity of adversarial decision making. *arXiv:2206.13063*.
- Friston, K., FitzGerald, T., Rigoli, F., Schwartenbeck, P., Pezzulo, G. (2017). Active inference: a process theory. *Neural Computation* 29.
- Gajane, P., Ortner, R., Auer, P. (2019). Variational regret bounds for reinforcement learning. *UAI*.
- Garivier, A., Moulines, E. (2008). On upper-confidence bound policies for non-stationary bandit problems. *arXiv:0805.3415*.
- Gerogiannis, A., Huang, Y.-H., Veeravalli, V. (2026). DARLING: Detection augmented reinforcement learning with non-stationary guarantees. *(contemporaneous; April 2026)*.
- Hajiabolhassan, H., Ortner, R. (2025). Online regret bounds for satisficing in Markov decision processes. *Mathematics of Operations Research*.
- Hobson, A. (1969). A new theorem of information theory. *J.\ Stat.\ Phys.* 1(3):383–391.
- Sprungk, B. (2019). On the local Lipschitz stability of Bayesian inverse problems. *Inverse Problems* 36(5). arXiv:1906.07120.
- Junzhe Zhang. (2020). Designing optimal dynamic treatment regimes: a causal RL approach. *ICML*.
- Junzhe Zhang, Bareinboim, E. (2016). MDPs with unobserved confounders: a causal approach. (Tech.\ report; Columbia.)
- Junzhe Zhang, Bareinboim, E. (2022). Online RL for mixed policy scopes. *NeurIPS*.
- Kakade, S., Krishnamurthy, A., Lowrey, K., Ohnishi, M., Sun, W. (2020). Information-theoretic regret bounds for online nonlinear control. *NeurIPS*. arXiv:2006.12466.
- Khalil, H. (2002). *Nonlinear Systems* (3rd ed.). Prentice Hall.
- Khasminskii, R. (2012). *Stochastic Stability of Differential Equations*. Springer.
- Kreisselmeier, G. (1986). Stable adaptive regulation of arbitrary nth-order plants. *IEEE Trans. Automatic Control* AC-31:299–305.
- Lattimore, T., György, A. (2021). Mirror descent and the information ratio. *COLT* (PMLR vol 134); arXiv:2009.12228 (2020).
- Lattimore, T., Szepesvári, C. (2020). *Bandit Algorithms*. Cambridge.
- Lee, H., Ding, Y., Lee, J., Jin, M., Lavaei, J., Sojoudi, S. (2023). Tempo adaptation in non-stationary RL (ProST). *NeurIPS 36*.
- Lee, H., Jin, M., Lavaei, J., Sojoudi, S. (2024). Pausing policy learning in non-stationary RL. *ICML*.
- Levine, S. (2018). Reinforcement learning and control as probabilistic inference. *arXiv:1805.00909*.
- Liese, F., Vajda, I. (1987). *Convex Statistical Distances*. Teubner.
- Long-Fei Li, Zhao, P., Zhou, Z.-H. (2024). Dynamic regret of adversarial MDPs with unknown transition and linear function approximation. *AAAI*.
- Lu, X., Van Roy, B. (2019). Information-theoretic confidence bounds for reinforcement learning. *NeurIPS*.
- Lu, Y., Meisami, A., Tewari, A. (2021). Causal MDPs: learning good interventions efficiently. *arXiv:2102.07663*.
- Lu, Y., Meisami, A., Tewari, A. (2022). Efficient RL with prior causal knowledge. *CLEaR*.
- Mao, W., Zhang, K., Zhu, R., Simchi-Levi, D., Başar, T. (2021). Near-optimal model-free RL in non-stationary episodic MDPs. *ICML*.
- Min, S., Russo, D. (2023). Information-theoretic analysis of nonstationary bandit learning. *ICML*.
- Parr, T., Pezzulo, G., Friston, K. J. (2022). *Active Inference: The Free Energy Principle in Mind, Brain, and Behavior*. MIT Press.
- Pearl, J. (2009). *Causality: Models, Reasoning, and Inference* (2nd ed.). Cambridge.
- Russac, Y., Vernade, C., Cappé, O. (2019). Weighted linear bandits for non-stationary environments. *NeurIPS*.
- Russo, D., Van Roy, B. (2014a). An information-theoretic analysis of Thompson sampling. *J.\ Mach.\ Learn.\ Res.*
- Russo, D., Van Roy, B. (2014b). Learning to optimize via information-directed sampling. *Operations Research*.
- Sanov, I. N. (1957). On the probability of large deviations of random variables. *Mat.\ Sb.* 42(84):11–44.
- Sason, I., Verdú, S. (2016). $f$-divergence inequalities. *IEEE Trans.\ Info.\ Theory*.
- Schulte, O., Poupart, P. (2025). When should reinforcement learning use causal reasoning? *Trans.\ Mach.\ Learn.\ Res.*
- Shore, J., Johnson, R. (1980). Axiomatic derivation of the principle of maximum entropy and the principle of minimum cross-entropy. *IEEE Trans.\ Info.\ Theory* 26(1).
- Stradi, F. E., Lunghi, A., Castiglioni, M., Marchesi, A., Gatti, N. (2024). Learning CMDPs with non-stationary rewards and constraints. *arXiv:2405.14372*.
- Stuart, A. (2010). Inverse problems: a Bayesian perspective. *Acta Numerica*.
- Touati, A., Vincent, P. (2020). Efficient learning in non-stationary linear MDPs (OPT-WLSVI). *arXiv:2010.12870*.
- Tsybakov, A. (2009). *Introduction to Nonparametric Estimation*. Springer.
- Wang, L., Yang, Z., Wang, Z. (2021). Provably efficient causal RL with confounded observational data (DOVI). *NeurIPS*; arXiv:2006.12311 (2020).
- Wei, C.-Y., Luo, H. (2021). Non-stationary RL without prior knowledge: an optimal black-box approach. *arXiv:2102.05406*.
- Wiener, N. (1948). *Cybernetics*. MIT Press.
- Y. Zhang, Zhu, R., Xie, Q. (2026). On the peril of (even a little) non-stationarity in satisficing regret minimization. *(contemporaneous; March 2026)*.
- Yang, Y., Zheng, Z., Tomizuka, M., Liu, C., Li, S. (2024). The Feasibility Theory of Constrained Reinforcement Learning: A Tutorial Study. arXiv:2404.10064.
- Zhao, P., Wang, Y.-X., Zhou, Z.-H. (2021). Non-stationary online learning with memory and non-stochastic control. *AISTATS*.


---

## Appendix A — Supporting Material for the Main Components

### A.1  Convention hierarchy: C1, C2, C3

Three named continuation conventions form a hierarchy of increasing diagnostic power and computational cost.

**C1 — One-step improvement (canonical default).** $\pi_{\mathrm{cont}} = \pi_{\mathrm{current}}$. Each action is evaluated assuming current behavior continues afterward. No fixed-point computation, no global-optimality assumption. Cheapest; weakest diagnostic.

**C2 — Receding-horizon ($N_r$-step replanning).** At each future step, re-optimize over a horizon of $N_r$ steps using the model available at that step:
$$\pi_{\mathrm{RH}}(M_\tau) \;=\; \arg\max_\pi V_O(M_\tau, \pi;\, N_r)\quad \text{applied at each } \tau.$$
Captures multi-step recovery: a goal that appears unattainable under frozen continuation may be reachable with replanning. Moderate cost; moderate diagnostic.

**C3 — Bellman.** $\pi_{\mathrm{cont}} = \pi^*$ where $\pi^* = \arg\max_\pi V_O(M_t, \pi; N_h)$. The continuation is the optimal policy — a fixed-point equation. Strongest diagnostic; most expensive.

**Monotonicity.** For any model $M_t$, horizon $N_h$, and policy class $\Pi$:
$$A_O^{(1)}(M_t;\, \Pi, N_h) \;\le\; A_O^{\mathrm{RH}}(M_t;\, \Pi, N_r, N_h) \;\le\; A_O^{\mathrm{B}}(M_t;\, \Pi, N_h).$$
*Proof.* C1 freezes continuation at $\pi_{\mathrm{current}}$ (possibly suboptimal); C2 re-optimizes periodically, so $\pi_{\mathrm{RH}} \succeq \pi_{\mathrm{current}}$ at each future step; C3 uses the globally optimal $\pi^*$. A weakly better continuation yields a weakly higher expected trajectory value; taking the supremum over the first action preserves the ordering. $\square$

**Corollary.** $\delta_{\mathrm{sat}}^{\mathrm B} \le \delta_{\mathrm{sat}}^{\mathrm{RH}} \le \delta_{\mathrm{sat}}^{(1)}$ (since $\delta_{\mathrm{sat}} = V_O^{\min} - A_O$, higher $A_O$ means lower $\delta_{\mathrm{sat}}$). And $\delta_{\mathrm{regret}}^{(1)} \le \delta_{\mathrm{regret}}^{\mathrm{RH}} \le \delta_{\mathrm{regret}}^{\mathrm B}$ (since $\delta_{\mathrm{regret}} = A_O - V_O(M_t, \pi_{\mathrm{current}}; N_h)$, higher $A_O$ means higher $\delta_{\mathrm{regret}}$).

C1 is the most conservative diagnostic (most likely to diagnose "locally unattainable"); C3 is the most accurate (least false "unattainable" diagnoses). The $2{\times}2$ diagnostic structure is preserved under all three; only the inferential force varies.

### A.2  Admissible-divergence family for the regret bound

The reverse-KL direction is forced by the deterministic-$\pi^*$ regime (Section 4.6). Within the direction-forced family, multiple smooth $f$-divergences yield valid regret bounds:

| Divergence | Bound on $R(Q)$ | Tightness | Finite under det.\ $\pi^*$? |
|---|---|---|---|
| $\operatorname{TV}(\pi^*, Q)$ | $V_{\max} \cdot \operatorname{TV}$ | Tight (extremal $V$) | Yes |
| $D_{\mathrm{KL}}(\pi^* \,\|\, Q)$ via Pinsker | $V_{\max} \sqrt{D_{\mathrm{KL}}/2}$ | Loose by $\sqrt{\cdot}$ | Yes |
| $D_{\mathrm{KL}}(\pi^* \,\|\, Q)$ via point-mass identity | $V_{\max}(1 - e^{-D_{\mathrm{KL}}})$ | Tight (this paper) | Yes |
| $\chi^2(\pi^* \,\|\, Q)$ (Le Cam) | $V_{\max} \cdot \tfrac12 \sqrt{\chi^2}$ | Looser than Pinsker | $\chi^2 = 1/Q(a^*) - 1$ |
| $D_\alpha(\pi^* \,\|\, Q)$ (Rényi, $\alpha \ge 1$) | Various | Interpolates | Yes for $\alpha \ge 1$ |
| $D_{\mathrm{KL}}(Q \,\|\, \pi^*)$ (forward-KL) | $+\infty$ | Vacuous | No |

Reverse-KL is selected uniquely within the direction-forced family by the chain-rule axiom (Appendix C). The point-mass identity supplies the *exact* bound on reverse-KL under deterministic $\pi^*$; the table reflects different bound shapes on the same divergence (with the BH inequality $V_{\max}\sqrt{1 - e^{-D_{\mathrm{KL}}}}$ as the looser general form to which the identity reduces outside the deterministic-$\pi^*$ corner).

### A.3  Direction-forcing argument

For deterministic $\pi^* = \delta_{a^*}$ and any $Q$ with $Q(a) > 0$ for some $a \neq a^*$:
$$D_{\mathrm{KL}}(Q \,\|\, \pi^*) \;=\; \sum_a Q(a) \log\frac{Q(a)}{\pi^*(a)} \;=\; \sum_{a \neq a^*} Q(a) \log\frac{Q(a)}{0} \;=\; +\infty.$$
A bound "$R \le +\infty$" is vacuous. The reverse direction $D_{\mathrm{KL}}(\pi^* \,\|\, Q)$ is finite (and equal to $-\log Q(a^*)$) whenever $Q(a^*) > 0$. The asymmetry is forced by the regime: regret is a one-sided quantity (contributes only from $Q$'s off-optimum mass; $\pi^*$ has no support off $a^*$); divergences whose role is to bound this one-sided quantity must themselves be one-sided. Symmetric divergences (squared Hellinger, Jensen-Shannon, symmetrized KL) introduce a vacuous symmetric term.

### A.4  Action-gap matching lower bound

For any $Q$ with $\Delta_{\min} = \min_{a \neq a^*} \Delta(a) > 0$:
$$R(Q) \;=\; \sum_{a \neq a^*} Q(a) \Delta(a) \;\ge\; \Delta_{\min} \sum_{a \neq a^*} Q(a) \;=\; \Delta_{\min} \cdot (1 - Q(a^*)) \;=\; \Delta_{\min} \cdot \operatorname{TV}(\pi^*, Q).$$
By the point-mass identity (Theorem 4.1), $\operatorname{TV}(\pi^*, Q) = 1 - e^{-D_{\mathrm{KL}}(\pi^* \,\|\, Q)}$, giving the matching lower bound of Theorem 4.2.

The lower bound is tight when sub-optimal actions are uniformly bad ($\Delta_{\min} = \max_{a \neq a^*} \Delta(a)$). For typical landscapes the gap between upper and lower bound is $V_{\max} - \Delta_{\min}$, controlled by the *spread* of action gaps.

### A.5  Tied-optimum and softmax-smoothed extensions

**Tied-optimum.** If $\pi^*$ has support on a tied-optimum set $\mathcal A^* = \{a : Q_O(a) = Q_O(a^*)\}$ with uniform mass, reverse-KL is finite whenever $Q$ covers $\mathcal A^*$. The regret bound extends:
$$R(Q) \;=\; \sum_{a \notin \mathcal A^*} Q(a) \Delta(a) \;\le\; V_{\max} \cdot \mathbb P_Q(a \notin \mathcal A^*).$$
Pinsker applies unchanged. A multi-action analog of the point-mass identity holds with $\pi^*(a^*) = 1/|\mathcal A^*|$ replacing the point-mass form, yielding $D_{\mathrm{KL}}(\pi^* \,\|\, Q) = -\sum_{a \in \mathcal A^*} (1/|\mathcal A^*|) \log(|\mathcal A^*| Q(a))$, a multi-action analog of $-\log Q(a^*)$. The two-sided bound becomes one-sided in general; the upper bound holds with the modified KL form.

**Softmax-smoothed $\pi^*$ with temperature $\tau \to 0$.** Replace $\pi^* = \delta_{a^*}$ with $\pi^*_\tau \propto \exp(Q_O / \tau)$. As $\tau \to 0$, $\pi^*_\tau \to \delta_{a^*}$. For finite $\tau$, the *perturbative* identity of Theorem 4.3 applies: $\operatorname{TV}(\pi^*_\tau, Q) = 1 - e^{-D_{\mathrm{KL}}(\pi^*_\tau \,\|\, Q)} + O(\exp(-\Delta_{\min}/\tau))$ — the correction is exponentially small in $1/\tau$. The two-sided regret bound transfers with the same correction order. (Derivation in §A.6.) Outside the perturbative regime — for genuinely high-entropy optima — the BH inequality $\operatorname{TV} \le \sqrt{1 - e^{-D_{\mathrm{KL}}}}$ becomes the relevant general bound; Pinsker also applies, with a one-sided regret form.

### A.6  Perturbative identity for $\epsilon$-stochastic and softmax-regularized optima

We establish Theorem 4.3.

**$\epsilon$-greedy stochastic optimum.** Let $\pi^*_\epsilon(a^*) = 1-\epsilon$, $\pi^*_\epsilon(a) = \epsilon/(|\mathcal A|-1)$ for $a \neq a^*$. For any policy $Q$ with full-support lower bound $Q(a) \ge q_0 > 0$ for all $a \in \mathcal A$ (necessary because $\pi^*_\epsilon$ has $\epsilon/(|\mathcal A|-1)$ mass on every action — without $Q(a) > 0$ on the full support, $D_{\mathrm{KL}}(\pi^*_\epsilon \,\|\, Q) = +\infty$), the reverse-KL admits the expansion
\begin{align*}
D_{\mathrm{KL}}(\pi^*_\epsilon \,\|\, Q) &= (1-\epsilon)\log\frac{1-\epsilon}{Q(a^*)} + \frac{\epsilon}{|\mathcal A|-1}\sum_{a \neq a^*}\log\frac{\epsilon/(|\mathcal A|-1)}{Q(a)} \\
&= -\log Q(a^*) + \epsilon\log\epsilon + O(\epsilon),
\end{align*}
where the $O(\epsilon)$ term collects bounded contributions from $-\epsilon\log Q(a^*)$ and the $\epsilon$-mass log-ratio terms over $a \neq a^*$. The total variation between $\pi^*_\epsilon$ and $Q$ is
$$\operatorname{TV}(\pi^*_\epsilon, Q) = \tfrac12 \sum_a |\pi^*_\epsilon(a) - Q(a)|.$$
Under the natural alignment ($Q(a^*) > $ all other $Q(a)$, $Q$ broadly aligned with $\pi^*_\epsilon$ on the suboptimal mass), this reduces to $\operatorname{TV}(\pi^*_\epsilon, Q) = (1-\epsilon) - Q(a^*) + O(\epsilon) = (1 - Q(a^*)) - \epsilon + O(\epsilon)$. Substituting the unperturbed identity $1 - Q(a^*) = 1 - e^{-D_{\mathrm{KL}}(\pi^*_0 \,\|\, Q)}$ and combining the expansions:
$$\operatorname{TV}(\pi^*_\epsilon, Q) - \bigl[1 - e^{-D_{\mathrm{KL}}(\pi^*_\epsilon \,\|\, Q)}\bigr] = -\epsilon - Q(a^*) \cdot \epsilon\log\epsilon + O(\epsilon^2) = O(\epsilon \log(1/\epsilon)),$$
where the dominant correction term is $-Q(a^*) \cdot \epsilon\log\epsilon$ since $|\epsilon\log\epsilon| \gg \epsilon$ for $\epsilon \to 0$. This establishes the $\epsilon$-greedy form of Theorem 4.3. The correction is sub-linear in $\epsilon$ but vanishes uniformly as $\epsilon \to 0$. $\square$

**Softmax-regularized optimum.** Let $\pi^*_\tau(a) \propto \exp(Q_O(a)/\tau)$. Under isolated optimum with action gap $\Delta_{\min}$, the off-optimum mass concentrates exponentially in $1/\tau$:
$$1 - \pi^*_\tau(a^*) = \frac{\sum_{a \neq a^*} \exp(-(Q_O(a^*) - Q_O(a))/\tau)}{1 + \sum_{a \neq a^*} \exp(-(Q_O(a^*) - Q_O(a))/\tau)} \le (|\mathcal A|-1)\exp(-\Delta_{\min}/\tau).$$
Treating $\pi^*_\tau$ as $\pi^*_\epsilon$ with effective $\epsilon = O(\exp(-\Delta_{\min}/\tau))$, the $\epsilon$-greedy expansion above applies with $\epsilon\log(1/\epsilon) = O((\Delta_{\min}/\tau)\exp(-\Delta_{\min}/\tau)) = O(\exp(-\Delta_{\min}/\tau))$ (the $1/\tau$ factor is absorbed by the exponential). $\square$

**Two-sided regret bound under perturbation.** Composing Theorem 4.3 with the TV-regret bounds of §4.1:
$$\Delta_{\min}\bigl(1 - e^{-D_{\mathrm{KL}}(\pi^*_\epsilon \,\|\, Q)}\bigr) - O(\epsilon\log(1/\epsilon)) \;\le\; R(Q) \;\le\; V_{\max}\bigl(1 - e^{-D_{\mathrm{KL}}(\pi^*_\epsilon \,\|\, Q)}\bigr) + O(\epsilon\log(1/\epsilon)).$$
The two-sided bound transfers from Theorem 4.2 with the same correction order; the regret-vs-identity-coordinate Lipschitz equivalence holds up to perturbative correction.

### A.7  Strategic-tempo consistency across canonical topologies

The bottleneck strategic tempo $\mathcal T_\Sigma^{\mathrm{bn,ss}}$ of Theorem 5.1 is verified — and the resulting per-topology threshold derived — across four canonical topologies under Beta-Bernoulli edge updates with per-element forgetting:

- **B.1 — Single edge.** $\mathcal T_\Sigma^{\mathrm{bn,ss}} = \nu \cdot \iota \cdot (1-\lambda)$. All three factors load-bearing; setting any one to zero collapses the bottleneck.
- **B.2 — Two-edge AND chain, observable intermediate.** $\mathcal T_\Sigma^{\mathrm{bn,ss}} = \min\bigl(\nu_1(1-\lambda_1),\ \nu_1\theta_1(1-\lambda_2)\bigr)$. Edge 2's effective observation rate is gated by edge 1's success probability $\theta_1$ — *depth-gated attenuation*. For depth-$d$ chains, the bottleneck is $\min_k \prod_{j<k}\theta_j (1-\lambda_k)$.
- **B.3 — Two-edge AND chain, unobservable intermediate.** Per-edge tempo ill-defined; plan-level bottleneck $\mathcal T_{\Sigma, \mathrm{plan}}^{\mathrm{bn,ss}} = \nu(1-\lambda_\Phi)$ over a single tracked plan-level quantity $\hat\Phi = p_1 p_2$.
- **B.4 — Two-arm OR node, $\varepsilon$-greedy.** $\mathcal T_\Sigma^{\mathrm{bn,ss}} = \min\bigl((1-\varepsilon)(1-\lambda_1),\ \varepsilon(1-\lambda_2)\bigr)$. Action selection controls rate allocation; pure greedy ($\varepsilon = 0$) collapses the unexplored arm's bottleneck — *exploration-gated*.

The structural decomposition: AND-chains exhibit *depth-gated* geometric attenuation $\nu_k = \nu \prod_{j < k} \theta_j$; OR-nodes exhibit *exploration-gated* allocation. Mixed AND/OR DAGs interleave both. Each topology surfaces a different factor as the binding bottleneck.

### A.8  Loop-as-causal-engine: three deployment modes

The Pearl-$do$ structure of closed-loop interventional access manifests at distinct layers with semantically distinct interventional mechanisms:

- **Mode 1 — agent-self-intervention.** The agent performs $do$-actions on its own action space as part of its ordinary adaptive loop. Intervention is on the agent's own action; target is the environment's response. This is Theorem 6.1's primary content.
- **Mode 2 — observer-on-sub-agent.** An observer external to a composite performs $do$-interventions on one sub-agent's action space; target is another sub-agent's response. Reveals cross-coupling structure that component-marginal observation cannot identify.
- **Mode 3 — observer-on-agent-input** (sketched, future work). An observer intervenes on the agent's observation channel; target is the agent's subsequent policy. Useful for offline architectural-class diagnosis.

Modes share the Pearl-$do$ structure but differ in who intervenes on what. The unification is at the pattern level; the mechanism is semantically distinct per layer.

---

## Appendix B — Pinsker vs.\ Point-Mass Identity Numerical Comparison

We compare $V_{\max}(1 - e^{-D_{\mathrm{KL}}})$ (point-mass identity bound, exact under deterministic $\pi^*$ per Theorem 4.2) with $V_{\max}\sqrt{D_{\mathrm{KL}}/2}$ (Pinsker bound, also valid under deterministic $\pi^*$ but loose), $V_{\max}\sqrt{1 - e^{-D_{\mathrm{KL}}}}$ (Bretagnolle--Huber inequality, the general bound to which the identity strictly improves at this corner), and the trivial envelope $V_{\max}$. Set $V_{\max} = 1$ for normalization.

| $D_{\mathrm{KL}}$ | $1 - e^{-D_{\mathrm{KL}}}$ (identity) | $\sqrt{1 - e^{-D_{\mathrm{KL}}}}$ (BH) | $\sqrt{D_{\mathrm{KL}}/2}$ (Pinsker) | $\min(\sqrt{D_{\mathrm{KL}}/2}, 1)$ | Pinsker / identity ratio |
|---|---|---|---|---|---|
| 0.01 | 0.00995 | 0.0998 | 0.0707 | 0.0707 | 7.10× |
| 0.1 | 0.0952 | 0.308 | 0.224 | 0.224 | 2.35× |
| 0.5 | 0.393 | 0.627 | 0.500 | 0.500 | 1.27× |
| 1.0 | 0.632 | 0.795 | 0.707 | 0.707 | 1.12× |
| 2.0 | 0.865 | 0.930 | 1.000 | 1.000 | 1.16× (Pinsker = trivial) |
| 4.0 | 0.982 | 0.991 | 1.414 | 1.000 | 1.02× (Pinsker vacuous) |
| 10.0 | 0.99995 | 0.99998 | 2.236 | 1.000 | 1.00× (Pinsker fully vacuous) |

The point-mass identity bound is uniformly tighter than Pinsker, by a factor of $7\times$ at $D_{\mathrm{KL}} = 0.01$ and converging to $1$ as $D_{\mathrm{KL}}$ grows large (where both saturate at $V_{\max}$). It is also uniformly tighter than the BH inequality $\sqrt{1 - e^{-D_{\mathrm{KL}}}}$ — the relation $x < \sqrt{x}$ on $(0, 1)$ — although both saturate together at $V_{\max}$ for large $D_{\mathrm{KL}}$, where BH stays informative while Pinsker becomes vacuous (exceeds the trivial $V_{\max}$ envelope) for $D_{\mathrm{KL}} > 2$.

The matching lower bound $\Delta_{\min}(1 - e^{-D_{\mathrm{KL}}})$ has the same shape, scaled by $\Delta_{\min}/V_{\max}$. In the extremal value landscape ($\Delta_{\min} = V_{\max}$), the upper and lower bounds coincide and the regret is *identified exactly* by the KL coordinate. For typical landscapes, regret and KL are Lipschitz-equivalent with constants $\Delta_{\min}/V_{\max}$ and $1$.

---

## Appendix C — Chain-Rule Uniqueness of Reverse-KL

**Theorem C.1 (Hobson 1969; Csiszár 1991).** *Let $D_f(P \,\|\, Q) = \sum_x Q(x) f(P(x)/Q(x))$ be a smooth $f$-divergence with $f$ convex and $f(1) = 0$. The chain rule*
$$D_f(P_{XY} \,\|\, Q_{XY}) \;=\; D_f(P_X \,\|\, Q_X) \;+\; \mathbb E_{P_X}\!\left[D_f(P_{Y|X} \,\|\, Q_{Y|X})\right]$$
*holds for all joint distributions if and only if $f(t) = c \cdot t \log t$ for some $c > 0$ — i.e., $D_f$ is reverse-KL up to positive scaling.*

*Proof sketch.* Writing $r_x = P(x)/Q(x)$ and $s_{y|x} = P(y|x)/Q(y|x)$, the chain rule reduces to the functional equation $f(rs) = f(r) + r f(s)$ for all $r, s > 0$. With $f(1) = 0$ and convexity, the unique solution is $f(t) = c \cdot t \log t$ for $c > 0$ [Aczél-Daróczy 1975 §4]. $\square$

**References.** Hobson 1969 ("A new theorem of information theory," *J.\ Stat.\ Phys.*); Csiszár 1991 ("Why least squares and maximum entropy?" *Annals of Statistics*; Theorem 3 corollary, Theorem 5); Shore-Johnson 1980 ("Axiomatic derivation of the principle of maximum entropy," *IEEE Trans.\ Info.\ Theory*, system-independence axiom); Sanov 1957 (large-deviation rate function); Aczél-Daróczy 1975 (functional-equation machinery).

These references give *structurally equivalent reformulations* of the same axiom. The Cauchy functional equation each reduces to is the common content. No known uniqueness route outside the independence-on-sub-problems family exists.

**Why other family members fail the chain rule.** Concrete counterexample for $\chi^2$: take $Q_X$ uniform on $\{x_1, x_2\}$, $P_X = (3/4, 1/4)$, $Q(y|x)$ uniform, $P(y|x) = (3/4, 1/4)$. Direct calculation gives $\chi^2(P_{XY} \,\|\, Q_{XY}) = 9/16$ while $\chi^2(P_X \,\|\, Q_X) + \mathbb E_{P_X}[\chi^2(P_{Y|X} \,\|\, Q_{Y|X})] = 1/4 + 1/4 = 8/16$. Non-additive. Rényi-$\alpha$ for $\alpha \neq 1$ fails analogously; squared Hellinger likewise fails.

---

## Appendix D — Prior-Art Retrieval Summary

We conducted a structured prior-art retrieval (via Undermind) with the goal of identifying any framework composing all four target elements: (1) goal-feasibility-vs-policy-quality two-gap diagnostic; (2) an exact reverse-KL/TV regret identity (point-mass) under deterministic-$\pi^*$, or — more loosely — Bretagnolle--Huber-style use in regret analysis; (3) strategic-tempo / forgetting prerequisite tying convergence to rate of useful policy revision; (4) closed-loop interventional access making regret bounds learnable from on-policy data.

**Result: 63 papers retrieved, abstract-level coverage estimated 75%, no direct anticipation.** The landscape splits into four largely separate strands corresponding to our four components, with no published framework composing all four. Retrieval queries used the structured prior-art tool Undermind; the four-strand cite-and-distinguish in §9 surfaces the closest neighbor in each strand.

**Strongest negative signal: the Bretagnolle--Huber inequality is absent from the retrieved RL/non-stationary corpus.** The information-theoretic regret literature uses entropy, mutual information, information ratio, Pinsker, or Hellinger uniformly. A fortiori, the *exact identity at the BH inequality's deterministic-$\pi^*$ corner* — the point-mass reverse-KL/TV identity of Theorem 4.1, which strictly improves the BH bound at this point — is also absent and is, to our knowledge, novel.

**Closest neighbors per strand:** [Cheung-Simchi-Levi-Zhu 2020] (Strand 1); [Long-Fei Li-Zhao-Zhou 2024] (Strand 2 — closest two-term decomposition, different axis); [Lee et al.\ 2023 ProST] (Strand 3 — closest tempo result, different form); [Zhang-Bareinboim 2022] (Strand 4 — closest causal-RL, stationary only).

---

## Appendix E — Sketch of an Algorithm Achieving Theorem 7.1

A practical algorithm achieving the joint guarantees of Theorem 7.1 requires:

**(a) Base learner achieving identity-tight per-round coordinate (up to log factors).** In the stationary deterministic-$\pi^*$ regime, Thompson sampling [Russo–Van Roy 2014a] and UCB [Lattimore–Szepesvári 2020] both achieve the per-round regret coordinate
$$\mathbb E\!\left[1 - e^{-K_t}\right] \;=\; \mathbb E[1 - Q_t(a^*)] \;=\; O\!\left(\frac{\log t}{t \,\Delta_{\min}}\right),$$
matching the identity form $V_{\max}(1 - e^{-D_{\mathrm{KL}}})$ of Theorem 4.2 up to constants and a $\log t$ factor. This satisfies (A5) of Theorem 7.1 with the stronger logarithmic rate. Applied between optimum-change events in the composition of Theorem 7.1(v), this yields cumulative dynamic regret $O(V_{\max} (B_T+1) \log^2(T/(B_T+1)) / \Delta_{\min})$, sharper than the Cauchy–Schwarz $\sqrt{(B_T+1)\,T}$ bound when $\Delta_{\min}$ is bounded away from zero. Information-directed sampling [Russo–Van Roy 2014b] requires a different analysis here because $H(\pi^*) = 0$ collapses its information ratio; we leave the IDS analysis as future work.

**(b) Multi-factor forgetting schedule.** Choose per-element discount rates $\{\lambda_{ij}\}$ such that the bottleneck $\mathcal T_\Sigma^{\mathrm{bn,ss}} = \min_{(i,j)} \nu_{ij}\iota_{ij}(1-\lambda_{ij})$ exceeds $\rho_\Sigma / R_\Sigma$. When $\rho_\Sigma$ is unknown, the [Wei–Luo 2021] black-box reduction gives an adaptive choice (applied per element if heterogeneous); when $\rho_\Sigma$ is known via a variation-budget [Cheung et al.\ 2020], the choice is direct. As a fail-fast pre-check, $\mathcal T_\Sigma^{\mathrm{agg,ss}} > |E| \cdot \rho_\Sigma / R_\Sigma$ is necessary.

**(c) Identifiability check.** Test whether the loop data is in Regime A (full identifiability), Regime B (partial), or Regime C (none). In Regime A, no adjustment is needed. In Regime B, apply [Wang-Yang-Wang 2020 DOVI]-style confounding-bias adjustment. In Regime C, the bound is provable but not realizable; algorithm flags the regime as out-of-scope.

**(d) Two-gap diagnostic readout.** Compute $\delta_{\mathrm{sat}}$ (requires $A_O$, intractable in general; estimable via the policy-class supremum over recent rounds) and $\delta_{\mathrm{regret}}$ (requires $V_O$ at current policy; tractable in simulation). Route to corrective action class.

The algorithm is sketch-grade; full instantiation and empirical evaluation are deferred to a follow-up paper.

---

## Appendix F — Estimator and Bias Bound for the KL Coordinate

We establish conclusion (iii) of Theorem 7.1 — the KL-coordinate is estimable from on-policy data with bias scaling linearly in $1 - \iota$.

**Estimator.** Define the empirical visit-frequency estimator
$$\hat Q_n(a^*_{\mathrm{ag}}) \;:=\; \frac{1}{n}\sum_{k=1}^n \mathbf 1\!\left[A_k = a^*_{\mathrm{ag}}\right], \qquad \hat D_{\mathrm{KL}} \;:=\; -\log \hat Q_n(a^*_{\mathrm{ag}}),$$
where $a^*_{\mathrm{ag}}$ is the agent-identified optimum from $n$ on-policy rollouts.

**Concentration.** By Hoeffding's inequality, for any $\delta \in (0, 1)$,
$$\Pr\!\left[\left|\hat Q_n(a^*_{\mathrm{ag}}) - Q(a^*_{\mathrm{ag}})\right| \le \sqrt{\log(2/\delta)/(2n)}\right] \;\ge\; 1 - \delta.$$

**KL-coordinate variance bound.** On the event $\{Q(a^*), \hat Q_n(a^*) \ge q_0\}$, by the mean-value theorem applied to $-\log$,
$$\left|\hat D_{\mathrm{KL}} - D_{\mathrm{KL}}(\delta_{a^*_{\mathrm{ag}}} \,\|\, Q)\right| \;\le\; \frac{|\hat Q_n(a^*_{\mathrm{ag}}) - Q(a^*_{\mathrm{ag}})|}{q_0} \;\le\; q_0^{-1}\sqrt{\log(2/\delta)/(2n)}.$$

**Bias decomposition under partial identifiability.** Under partial identifiability $\iota \in (0, 1)$, the agent identifies the true optimum $\tilde a^*$ (under intervention) only with probability $\iota$. By definition, $\Pr[a^*_{\mathrm{ag}} \ne \tilde a^*] = 1 - \iota$. The agent's estimable KL is $D_{\mathrm{KL}}(\delta_{a^*_{\mathrm{ag}}} \,\|\, Q) = -\log Q(a^*_{\mathrm{ag}})$; the true KL is $D_{\mathrm{KL}}(\delta_{\tilde a^*} \,\|\, Q) = -\log Q(\tilde a^*)$. Decomposing,
$$\mathbb E\!\left[\left|\hat D - D_{\mathrm{KL,true}}\right|\right] \;\le\; \mathbb E\!\left[\left|\log Q(a^*_{\mathrm{ag}}) - \log Q(\tilde a^*)\right|\right] + \mathbb E[\text{variance term}].$$
The first (bias) term is bounded by an indicator on misidentification times a worst-case ratio:
$$\mathbb E\!\left[\left|\log Q(a^*_{\mathrm{ag}}) - \log Q(\tilde a^*)\right|\right] \;\le\; \Pr[a^*_{\mathrm{ag}} \ne \tilde a^*] \cdot \log(1/q_0) \;=\; (1 - \iota)\log(1/q_0).$$

**Combined bound.** On the high-probability event of the Hoeffding concentration:
$$\boxed{\;\mathbb E\!\left[\left|\hat D - D_{\mathrm{KL,true}}\right|\right] \;\le\; (1-\iota)\log(1/q_0) \;+\; q_0^{-1}\sqrt{\log 2 / (2n)}.\;}$$

The variance term decays at the standard $1/\sqrt n$ rate; the bias term scales linearly in $1 - \iota$. This rigorously establishes the $1 - \iota$ scaling claimed in conclusion (iii) of Theorem 7.1: Regime A ($\iota = 1$) gives zero bias; Regime B ($\iota \in (0, 1)$) gives bias controlled by conflated-edge mass $1 - \iota$; Regime C ($\iota = 0$) gives bias up to $\log(1/q_0)$, consistent with the loop-Level-2 analysis of §6 (Regime-C data is interventional in character but does not yield identified $do$-estimates).

---

## Appendix G — Proof Sketches for Theorem 5.1 and Theorem 7.1(v)

**Theorem 5.1 (multi-factor forgetting prerequisite) — proof sketch.** Per-element dynamic gives expected one-step correction $\mathbb E[\Delta \delta_{ij}] = -\alpha_{ij}^{\mathrm{ss}} \delta_{ij} + w_{ij}$ where $\alpha_{ij}^{\mathrm{ss}} = \nu_{ij}\iota_{ij}(1-\lambda_{ij})$ — the three factors enter as a product because each is an independent gate (probability of test × signal-fraction × update strength). Diagonal correction gives sector product
$$\boldsymbol\delta_\Sigma^\top \mathbf F(\boldsymbol\delta_\Sigma) \;=\; \sum_{(i,j)} \alpha_{ij}^{\mathrm{ss}} \delta_{ij}^2 \;\ge\; \min_{(i,j)} \alpha_{ij}^{\mathrm{ss}} \cdot \|\boldsymbol\delta_\Sigma\|^2 \;=\; \mathcal T_\Sigma^{\mathrm{bn,ss}} \cdot \|\boldsymbol\delta_\Sigma\|^2.$$
The bound is *tight* — adversarial concentration of $\boldsymbol\delta_\Sigma$ on the bottleneck element saturates it. Khalil ultimate-boundedness [Khalil 2002 §9, Lemma 9.2 / Theorem 9.1] applied to the strategic substate then closes the argument: when $\mathcal T_\Sigma^{\mathrm{bn,ss}} > \rho_\Sigma / R_\Sigma$, the modeled mismatch is ultimately bounded at $R_\Sigma^* = \rho_\Sigma / \mathcal T_\Sigma^{\mathrm{bn,ss}}$. *Sharpness*: when reversed, the bottleneck element admits a disturbance $\boldsymbol w$ concentrated on it with $|w_{i^*j^*}| = \rho_\Sigma$, pushing $\|\boldsymbol\delta_\Sigma\| > R_\Sigma$ within the modeled dynamics. $\square$

**Theorem 7.1(v) (cumulative dynamic regret) — proof sketch.** Partition $[1, T]$ at the optimum-change events $0 = \tau_0 < \tau_1 < \cdots < \tau_{B_T} \le \tau_{B_T+1} = T$. Within each block $[\tau_i, \tau_{i+1})$ of length $\Delta_i := \tau_{i+1} - \tau_i$, the optimum $a^*_t$ is constant, so by the per-round identity (Theorem 4.2) and (A5),
$$\sum_{t=\tau_i}^{\tau_{i+1}-1} \mathbb E[1 - e^{-K_t}] \;\le\; \sum_{t=1}^{\Delta_i} c \cdot t^{-1/2} \;\le\; 2c\sqrt{\Delta_i}.$$
Summing across blocks and applying Cauchy–Schwarz:
$$\sum_{t=1}^T \mathbb E[1 - e^{-K_t}] \;\le\; 2c \sum_{i=0}^{B_T} \sqrt{\Delta_i} \;\le\; 2c\sqrt{(B_T + 1)\,T},$$
using $\sum_i \Delta_i = T$ and Cauchy–Schwarz across $B_T + 1$ blocks. Multiplying by $V_{\max}$ via Theorem 4.2 gives $\mathrm{DynReg}(T) \le 2cV_{\max}\sqrt{(B_T+1)\,T}$. At $B_T = 0$ this recovers the stationary-base-regret rate $O(V_{\max}\sqrt T)$ rather than collapsing to zero. The Cesàro tracking corollary $\frac{1}{T}\sum_t (V^*_t - V(\pi_t)) = O\!\bigl(\sqrt{(B_T+1)/T}\bigr) \to 0$ when $B_T = o(T)$ follows by dividing through by $T$. $\square$

Under Thompson sampling or UCB as the base learner (Appendix E), the per-block stationary rate sharpens to $O(\log\Delta_i / (\Delta_i \Delta_{\min}))$, summing to $O((B_T+1) \log^2(T/(B_T+1)) / \Delta_{\min})$ across blocks — sharper than $\sqrt{(B_T+1)\,T}$ when $\Delta_{\min}$ is bounded away from zero.

