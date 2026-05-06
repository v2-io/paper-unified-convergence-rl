# A Unified Convergence Theory for Non-Stationary Reinforcement Learning

## Abstract

Non-stationary reinforcement learning lacks a unified convergence theory: variation-budget regret bounds, sliding-window methods, tempo-and-forgetting analyses, two-term decompositions, and causal-RL frameworks each address fragments without composing. We present a unified theory through the composition of four structural elements: (i) a two-gap diagnostic separating goal-feasibility from policy-quality; (ii) a point-mass reverse-KL/TV identity under deterministic optimum, $\mathrm{TV}(\delta_{a^*}, Q) = 1 - e^{-D_{\mathrm{KL}}}$, that is *exact* and strictly improves both Pinsker and the Bretagnolle–Huber inequality at this corner — and BH is itself absent from the retrieved RL/non-stationary corpus, making the identity novel *a fortiori*; (iii) a multi-factor strategic tempo $\mathcal{T}_\Sigma = \min_{(i,j)} \nu_{ij} \cdot \iota_{ij} \cdot (1-\lambda_{ij})$ with a structural forgetting prerequisite $\mathcal{T}_\Sigma > \rho_\Sigma/R_\Sigma$ as a survival inequality; (iv) closed-loop interventional access making regret bounds learnable from on-policy data via the Pearl causal hierarchy. Composing these gives a cumulative dynamic regret theorem with rate $O(V_{\max}\sqrt{B_T \cdot T})$ where $B_T$ counts optimum-change events. We further establish a structural failure class: every count-accumulating agent (any update with $n_{\mathrm{eff}}(t) \to \infty$) eventually violates the forgetting prerequisite for any positive disturbance rate, while non-accumulating mechanisms admit bidirectional thresholds (constant-step stochastic approximation, sliding windows, bounded memory, block restart). The point-mass identity is coordinate-optimal among bounds depending only on total variation, chain-rule compositional to multi-step trajectory regret as behavior-cloning loss against the optimal trajectory, and load-bearing for the cumulative theorem. The deterministic-optimum scope extends perturbatively to $\epsilon$-stochastic and softmax-regularized policies with $O(\epsilon\log(1/\epsilon))$ and $O(\exp(-\Delta_{\min}/\tau))$ corrections respectively. Theory-only; Lemma 5.2 makes the ProST reduction rigorous at the sector-parameter level, while their published experiments motivate the strategic-tempo component.

---

## 1  Introduction

Non-stationary reinforcement learning has matured along four largely separate research tracks. Each has produced rigorous results; none has been composed with the others into a single convergence theory.

The four tracks are visible in the recent literature as parallel lineages. **Variation-budget dynamic regret under drift** [Cheung-Simchi-Levi-Zhu 2020; Wei-Luo 2021; Mao-Zhang-Zhu-Simchi-Levi-Başar 2021; Gajane-Ortner-Auer 2019] gives sublinear dynamic regret under bounded total variation in reward and transition dynamics. **Two-term regret decompositions** [Long-Fei Li-Zhao-Zhou 2024; Fei-Yang-Wang-Xie 2020; Stradi-Lunghi-Castiglioni-Marchesi-Gatti 2024] split the dynamic regret along an *exploration-vs-adaptation* axis, isolating the cost of confidence-set construction from the cost of tracking a moving optimum. **Tempo and forgetting analyses** [Lee et al. 2023 ProST; Lee et al. 2024; Touati-Vincent 2020; Russac-Vernade-Cappé 2019; Garivier-Moulines 2008] make policy-update timing or evidence-discount rate an explicit convergence variable. **Causal and interventional access** [Zhang-Bareinboim 2016, 2022; Lu-Meisami-Tewari 2021, 2022; Wang-Yang-Wang 2020 DOVI; Junzhe Zhang 2020] uses causal-graph structure to sharpen sample complexity, but operates in stationary settings only.

The information-theoretic regret literature [Russo-Van Roy 2014a, 2014b; Lu-Van Roy 2019; Min-Russo 2023] traverses these tracks at a different layer, using entropy, mutual information, information ratio, Pinsker, or Hellinger to bound regret. Notably absent from that literature, as we document below, is an *exact* reverse-KL / total-variation identity at the deterministic-optimum corner: a sharp action-distribution regret coordinate that strictly improves both Pinsker and the Bretagnolle--Huber [Bretagnolle-Huber 1978] inequality at this point. The Bretagnolle--Huber inequality is itself absent from the retrieved RL/non-stationary corpus; the *exact identity at its point-mass corner* — which we establish — is a fortiori absent.

These four tracks share an evident common ancestor — the dynamic-regret analysis of online MDPs — but no published framework composes them. In particular, no framework combines (a) a regret decomposition along the *goal-feasibility-vs-policy-quality* axis (distinct from the exploration-vs-adaptation decompositions of [Long-Fei Li et al.\ 2024; Fei et al.\ 2020]); (b) an exact point-mass reverse-KL/TV regret identity under deterministic optimum, strictly tighter than the Bretagnolle--Huber inequality at this corner; (c) a structural survival inequality threading rate-of-policy-revision against environment-side disturbance; and (d) a closed-loop causal-access argument that makes the regret bound *learnable* on-policy rather than merely provable analytically.

### 1.1  Contribution

This paper assembles the composition. The contribution has the shape — recognized by the NeurIPS Theory Track guidelines as a valid form of originality — of "a novel combination of existing techniques [where] the reasoning behind this combination is well-articulated." Three specific moves carry the paper:

**(i) Point-mass reverse-KL/TV regret identity (Section 4).** Under deterministic optimum policy $\pi^* = \delta_{a^*}$, an elementary calculation gives the *exact identity* $D_{\mathrm{KL}}(\pi^* \,\|\, Q) = -\log Q(a^*) = -\log(1 - \operatorname{TV}(\pi^*, Q))$, equivalently $\operatorname{TV}(\pi^*, Q) = 1 - e^{-D_{\mathrm{KL}}(\pi^* \,\|\, Q)}$. The classical Bretagnolle--Huber inequality $\operatorname{TV}(P, Q) \le \sqrt{1 - e^{-D_{\mathrm{KL}}(P \,\|\, Q)}}$ [Bretagnolle-Huber 1978] is the general bound that this identity sits within; under deterministic $\pi^*$ the identity *strictly improves* it, since $1 - e^{-D_{\mathrm{KL}}} \le \sqrt{1 - e^{-D_{\mathrm{KL}}}}$ holds strictly for $D_{\mathrm{KL}} \in (0, \infty)$. Composing the identity with the textbook total-variation regret bound yields a tight two-sided regret characterization
$$\Delta_{\min}\bigl(1 - e^{-D_{\mathrm{KL}}(\pi^* \,\|\, Q)}\bigr) \;\le\; R(Q) \;\le\; V_{\max}\bigl(1 - e^{-D_{\mathrm{KL}}(\pi^* \,\|\, Q)}\bigr)$$
with $\Delta_{\min}$ the action-gap and $V_{\max}$ the value range. This bound *strictly improves* the Pinsker form $V_{\max}\sqrt{D_{\mathrm{KL}}/2}$: the identity-based form is tight on the upper side, has a matching lower bound, is informative for all $D_{\mathrm{KL}} > 0$, and remains nontrivial when $D_{\mathrm{KL}} > 2$ (where Pinsker is vacuous against the trivial $V_{\max}$ envelope). To our knowledge, neither the Bretagnolle--Huber inequality nor its point-mass exact identity has been deployed in the RL or non-stationary-RL literature; the identity, which sits strictly below the BH bound at this corner, is sharper than what BH itself would supply.

The identity is two-line as a calculation but *load-bearing* in three ways (§4.4): (1) *coordinate-optimality* — both endpoints of the Lipschitz envelope are achieved on extremal value landscapes, so the identity coordinate $\operatorname{TV} = 1 - e^{-D_{\mathrm{KL}}}$ is optimal among bounds depending only on TV; (2) *chain-rule compositionality* — the multi-step KL $D_{\mathrm{KL}}(\pi^*_{1:T} \,\|\, Q_{1:T}) = -\sum_t \log Q_t(a^*_t \mid h_t)$ is the negative log-likelihood of the optimal trajectory under the agent's joint policy, equivalently the behavior-cloning loss against optimal-trajectory data; (3) *cumulative-regret connection* — composition with a variation-budget block argument yields cumulative dynamic regret $\mathrm{DynReg}(T) \le 2cV_{\max}\sqrt{B_T \cdot T}$ (§7.1, conclusion (v)). The deterministic-$\pi^*$ scope is not a hard wall: the identity extends *perturbatively* to $\epsilon$-greedy and softmax-regularized optima with quantified $O(\epsilon\log(1/\epsilon))$ and $O(\exp(-\Delta_{\min}/\tau))$ corrections respectively (§4.6), recovering exact equality at $\epsilon = 0$ / $\tau \to 0$.

**(ii) Two-gap diagnostic with goal-feasibility-vs-policy-quality axis (Section 3).** We separate $\delta_{\mathrm{sat}}$ (the *satisfaction gap*: the goal is unmet under the best available one-step policy improvement given current model and horizon) from $\delta_{\mathrm{regret}}$ (the *control regret*: the gap between the best available policy improvement and the agent's current policy). The 2$\times$2 disambiguation of these two gaps routes four regimes — *success*, *strategy problem*, *capability limit*, *both* — to four distinct corrective actions. This decomposition runs along a structural axis distinct from the exploration-vs-adaptation decompositions of [Long-Fei Li et al.\ 2024] and [Fei et al.\ 2020]: their decomposition isolates uncertainty *sources* (transition uncertainty vs.\ non-stationarity); ours isolates *what is wrong* (the goal vs.\ the policy).

**(iii) Strategic tempo with forgetting prerequisite (Section 5).** We define a *multi-factor* strategic tempo $\mathcal T_\Sigma$ — composed per revisable policy element of an observation rate $\nu$, an identifiability coefficient $\iota$, and an update gain $\eta$ — and derive a structural survival inequality
$$\mathcal T_\Sigma^{\mathrm{bn,ss}} \;:=\; \min_{(i,j) \in E} \nu_{ij} \cdot \iota_{ij} \cdot (1-\lambda_{ij}) \;>\; \rho_\Sigma / R_\Sigma$$
in which all three factors are multiplicatively load-bearing per element, with the *bottleneck* (per-element minimum) as aggregator. The familiar single-factor form $(1-\lambda) > \rho_\Sigma / R_\Sigma$ is recovered as a corollary under uniform-Regime-A Beta-Bernoulli normalization. A structural-class theorem identifies the count-accumulating class $\mathcal A_{\mathrm{accum}}$ (Bayesian updates without forgetting, observation-aggregating schemes without restart) as one in which every agent eventually violates the threshold, while non-accumulating mechanisms (constant-step SA, sliding-window, bounded-memory, block-restart) face *bidirectional* ceilings (§5.3.1). This *lifts* the tempo result of [Lee et al.\ 2023] from a single-factor hyperparameter-optimization claim to a multi-factor structural-threshold claim. The forgetting prerequisite is the strategic analog of the persistence condition in adaptive control [Khalil 2002], with $(1-\lambda)$ playing the role of adaptive tempo per element.

**(iv) Closed-loop interventional access (Section 6).** We document, with explicit grounding in Bareinboim's causal-hierarchy theorem [Bareinboim-Correa-Ibeling-Icard 2022], that an agent in the feedback loop *generates* Pearl Level-2 (interventional) data by construction: the action $a_t$ causally precedes $o_{t+1}$, so the conditional mismatch $\delta_t \mid a_t$ carries interventional information. This makes the regret bound of (i) *learnable* from on-policy interaction in regimes with sufficient identifiability, not merely provable in principle. The argument is implicit in the action-perception-loop framing of active inference [Friston et al.\ 2017; Parr-Pezzulo 2022] and the causal-RL line [Zhang-Bareinboim 2022; Lu-Meisami-Tewari 2021]; we make the *Bareinboim-hierarchy connection* explicit, partition usable strength of identification into three regimes, and flag honestly where the loop yields interventional *data* without yielding identified *do*-estimates.

**Composition.** The four together give (Section 7) a non-stationarity-aware convergence theory with three properties no existing strand exhibits jointly: the bound *handles non-stationarity* (via tempo and forgetting); has *explicit metric structure on policy space* (via the point-mass reverse-KL identity); is *learnable from on-policy data* (via closed-loop interventional access). Composition with a variation-budget block argument yields a cumulative dynamic regret bound $\mathrm{DynReg}(T) \le 2cV_{\max}\sqrt{B_T \cdot T}$ — sharper per-round coordinate than Pinsker/BH, with the cumulative rate matching the variation-budget literature [Cheung et al.\ 2020; Wei--Luo 2021] as a corollary rather than as a new rate. Each of the four components is itself a theorem (cited or proved); the paper's contribution is the recognition that the four together close a story none of the strands closes individually, plus the cumulative-regret rate that follows from composing the per-round identity with a base-learner stationary rate.

### 1.2  Scope and limitations

The paper is theory-only; we do not run experiments. The point-mass reverse-KL/TV identity is mathematically airtight (a two-line direct calculation that specializes the Bretagnolle--Huber [1978] family at its deterministic-$\pi^*$ corner; see Section 4). The empirical grounding for the strategic-tempo claim is provided indirectly through reduction to [Lee et al.\ 2023] ProST as a special case (Section 8); at the steady-state sector-parameter level the reduction is rigorous (Lemma 5.2). The composition theorem holds in the *canonical scope*: deterministic optimum policy, bounded value range, isolated optimum (so $\Delta_{\min} > 0$), and a singular causal trajectory in the sense made precise in Section 2. The deterministic-$\pi^*$ scope extends *perturbatively* to $\epsilon$-stochastic and softmax-regularized optima with quantified $O(\epsilon\log(1/\epsilon))$ and $O(\exp(-\Delta_{\min}/\tau))$ corrections respectively (§4.6); the identity is an exact equality at $\epsilon = 0$ and a perturbative identity for $\epsilon > 0$. The tied-optimum extension (Appendix A.5) remains a degraded one-sided form.

---

## 2  Setup

**Markov decision process.** A finite-horizon non-stationary MDP $(\mathcal S, \mathcal A, P_t, r_t, N_h)$ with state space $\mathcal S$, finite action space $\mathcal A$, time-indexed transition kernels $P_t(\cdot \mid s, a)$ and reward functions $r_t(s, a)$ allowed to vary across rounds $t$, and finite planning horizon $N_h$. Standard non-stationary-RL boundedness assumptions [Cheung et al.\ 2020]: per-step reward in $[0, 1]$, total cumulative variation in transition and reward bounded by a budget $B_T$.

**Policy distribution.** The agent's policy at round $t$ is a distribution $Q_t(\cdot \mid s)$ over actions; we write $Q$ when state and round are understood. Under the canonical scope, the optimum policy at $(M_t, s)$ is deterministic: $\pi^*(\cdot \mid M_t, s) = \delta_{a^*(s)}$ where $a^*(s) := \arg\max_a Q^\pi(s, a)$ is the optimal action under the agent's current model $M_t$.

**Value object.** Given an objective $O$, model $M_t$, policy $\pi$, and horizon $N_h$:
$$V_O(M_t, \pi;\, N_h) \;=\; \mathbb E\!\left[ V_{O}(\tau_{t:t+N_h}) \,\Bigm|\, M_t, \pi \right]$$
with action-value form
$$Q_O(M_t, a;\, \pi_{\mathrm{cont}}, N_h) \;=\; \mathbb E\!\left[ V_{O}(\tau) \,\Bigm|\, M_t,\, do(a_t = a),\, a_{t+1:} \sim \pi_{\mathrm{cont}} \right].$$
The $do(\cdot)$ notation [Pearl 2009] makes explicit that $Q_O$ is the value under intervention on $a_t$, not under conditioning on its observed value. The continuation policy $\pi_{\mathrm{cont}}$ is a parameter of the value object. We adopt the **one-step improvement** convention, $\pi_{\mathrm{cont}} = \pi_{\mathrm{current}}$, as default; a brief discussion of receding-horizon and Bellman alternatives is in Appendix A. The default convention is the most conservative, makes diagnostics comparable across rounds, and avoids a fixed-point dependence in the analysis.

**Bounded value range.** Write $V_{\max}(M_t) := \max_a Q_O(M_t, a; \pi_{\mathrm{cont}}, N_h) - \min_a Q_O(M_t, a; \pi_{\mathrm{cont}}, N_h)$ for the value range, finite under bounded reward and finite horizon.

**Action gap (isolated optimum).** $\Delta(a) := Q_O(M_t, a^*) - Q_O(M_t, a) \in [0, V_{\max}]$ and $\Delta_{\min} := \min_{a \neq a^*} \Delta(a) > 0$, well-defined whenever the optimum is isolated over finite $\mathcal A$.

**Strategic regret against the optimum.** For policy distribution $Q$ at $(M_t, s)$,
$$R(Q) \;:=\; Q_O(M_t, a^*) - \mathbb E_{a \sim Q}[Q_O(M_t, a)] \;=\; \sum_{a \neq a^*} Q(a) \cdot \Delta(a).$$
Three classical forms — $\mathbb E_{\pi^*}[V] - \mathbb E_Q[V]$, $V(a^*) - \mathbb E_Q[V]$, $\mathbb E_{\pi^*}[V - V_Q]$ — coincide under deterministic $\pi^*$.

**Singular trajectory.** The agent operates on a single, non-forkable causal trajectory: the action $a_t$ causally precedes $o_{t+1}$, and the model state $M_t$'s sufficiency is defined relative to *this* trajectory rather than to a model-state equivalence class. This is the operational ground for the Pearl $do$-operator on $Q_O$ above and for the closed-loop interventional access argument of Section 6.


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

### 3.2  The 2$\times$2 disambiguation

The 2$\times$2 cross of the two gaps is the load-bearing diagnostic:

| | $\delta_{\mathrm{sat}} \le 0$ (attainable) | $\delta_{\mathrm{sat}} > 0$ (unmet) |
|---|---|---|
| $\delta_{\mathrm{regret}} \approx 0$ (near-optimal policy) | **Success**: goal achievable, policy good | **Capability limit**: optimally pursuing an unmet goal |
| $\delta_{\mathrm{regret}} \gg 0$ (suboptimal policy) | **Strategy problem**: goal achievable, policy poor | **Both**: goal hard *and* policy weak |

Each cell prescribes a different corrective action:

- **Success.** No corrective action.
- **Strategy problem.** Revise the policy. The signal is informative about *which* policy revision: the strategic-calibration residual decomposes $\delta_{\mathrm{regret}}$ over the policy's structural elements (Appendix A).
- **Capability limit.** $\delta_{\mathrm{regret}} \approx 0$ rules out *the policy* as the source of failure. The remaining candidates are the model ($M_t$ may be wrong about feasibility), the policy class ($\Pi$ may be too narrow), the horizon ($N_h$ may be too short), or the objective itself (revising $O$ as last resort). The order matters: $M_t$ first, then $\Pi$ and $N_h$, then $O$.
- **Both.** Revise the policy first (cheaper, more likely to be the issue), then re-evaluate $\delta_{\mathrm{sat}}$.

### 3.3  Why two gaps, not one

A single $\delta_{\mathrm{objective}} = V_O^{\min} - V_O(M_t, \pi_{\mathrm{current}}; N_h)$ — the gap between the threshold and current performance — conflates two distinct situations. When the agent is optimally pursuing an infeasible goal, $\delta_{\mathrm{objective}}$ is large but no policy revision will help. When the agent has a weak policy on a feasible goal, $\delta_{\mathrm{objective}}$ is large *and* policy revision will help. The single-gap signal cannot distinguish these cases. The two-gap split makes the distinction structural: $\delta_{\mathrm{sat}} \approx 0$ rules out goal-side problems; $\delta_{\mathrm{regret}} \approx 0$ rules out policy-side problems.

### 3.4  Distinction from existing two-term decompositions

The closest structural neighbor is the two-term dynamic-regret decomposition of [Long-Fei Li-Zhao-Zhou 2024], which writes
$$\mathrm{DynReg}_K \;=\; \underbrace{C_{\mathrm{exp}}}_{\text{exploration / confidence-set construction}} \;+\; \underbrace{C_{\mathrm{adapt}}}_{\text{adaptation / suboptimal policy choice under non-stationarity}}.$$
[Fei-Yang-Wang-Xie 2020] provides a structurally similar decomposition for POWER and POWER++ in non-stationary policy optimization. [Stradi et al.\ 2024] applies the same shape in the constrained-MDP setting.

These decompositions decompose along an **uncertainty-source axis**: the regret decomposes into the part attributable to imperfect knowledge of $P$ vs.\ the part attributable to changing optimum. Our 2$\times$2 decomposes along a **goal-feasibility-vs-policy-quality axis**: the regret signal decomposes into the part attributable to goal infeasibility (which exploration cannot remedy) vs.\ the part attributable to policy suboptimality (which exploration can remedy). The two axes are orthogonal: a Long-Fei Li-style exploration term can be present in any of our four cells; an adaptation term can be present in any of our four cells. The diagnostic content of the two decompositions is complementary, not equivalent.

A cleaner contrast is with feasibility theory in constrained RL [Yang-Zheng-Tomizuka-Liu-Li 2024]. Their "feasibility" is *constraint-region feasibility* — whether the state can be kept inside the constraint set. Our $\delta_{\mathrm{sat}}$ is *goal feasibility* — whether the objective threshold $V_O^{\min}$ is attainable in principle from $M_t$. Both are valid notions; they answer different questions and should not be conflated.

The satisficing-bandit and satisficing-MDP literature [Hajiabolhassan-Ortner 2025; Zhang-Zhu-Xie 2026] uses "satisficing" to mean *any policy above acceptance level $\beta$ is acceptable*. Our $\delta_{\mathrm{sat}}$ is structurally distinct: $V_O^{\min}$ is a property of the objective (set by the domain), and $\delta_{\mathrm{sat}} > 0$ signals goal-relative infeasibility, not policy-relative satisficing. The vocabulary overlap is genuine and warrants careful disambiguation in any deployment of the 2$\times$2 in a satisficing context.

### 3.5  Convention dependence

The diagnostic table is convention-dependent: the value of $\delta_{\mathrm{sat}}$ and $\delta_{\mathrm{regret}}$ depends on the continuation convention $\pi_{\mathrm{cont}}$. We adopt the one-step-improvement default; the receding-horizon and Bellman conventions tighten the diagnostic at increasing computational cost. The monotonicity result $\delta_{\mathrm{sat}}^{\mathrm B} \le \delta_{\mathrm{sat}}^{\mathrm{RH}} \le \delta_{\mathrm{sat}}^{(1)}$ and the corresponding regret reversal $\delta_{\mathrm{regret}}^{(1)} \le \delta_{\mathrm{regret}}^{\mathrm{RH}} \le \delta_{\mathrm{regret}}^{\mathrm B}$ are proved in Appendix A. The 2$\times$2 structure is preserved across all three conventions; the inferential force of "goal genuinely infeasible" vs.\ "locally stuck" varies. Analyses must state the convention explicitly.

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

**Lipschitz equivalence.** Theorem 4.2 is equivalent to
$$\frac{\Delta_{\min}}{V_{\max}} \;\le\; \frac{R(Q)}{V_{\max}\bigl(1 - e^{-D_{\mathrm{KL}}}\bigr)} \;\le\; 1.$$
Regret and the identity-coordinate $\bigl(1 - e^{-D_{\mathrm{KL}}}\bigr)$ are Lipschitz-equivalent with constants $\Delta_{\min}/V_{\max}$ (below) and $1$ (above). The upper constant is tight when the value landscape is extremal; the lower constant is tight when sub-optimal actions are uniformly bad ($\Delta_{\min} = \max_{a \neq a^*} \Delta(a)$).

**Coordinate-optimality.** Both endpoints of the Lipschitz envelope are achieved on specific value landscapes: the upper bound $V_{\max}\operatorname{TV}$ is exact when $\Delta(a) = V_{\max}$ for all $a \ne a^*$ (extremal landscape); the lower bound $\Delta_{\min}\operatorname{TV}$ is exact when $\Delta(a) = \Delta_{\min}$ for all $a \ne a^*$ (uniformly-bad landscape). The identity coordinate $\operatorname{TV} = 1 - e^{-D_{\mathrm{KL}}}$ is therefore *coordinate-optimal among bounds depending only on $\operatorname{TV}(\pi^*, Q)$*: any tighter bound on $R(Q)$ requires information beyond TV (specifically, the value-landscape spread $V_{\max} - \Delta_{\min}$). The two-line identity is structurally optimal as a TV-only coordinate, not just elementary.

**Scope of "regret."** Theorem 4.2 bounds *per-state, one-step-improvement* regret under fixed model $M_t$ and the C1 continuation convention of §2 — the action-distribution regret $R(Q) = \sum_{a \neq a^*} Q(a)\Delta(a)$ at a single $(M_t, s)$. The composition theorem of §7 invokes Theorem 4.2 per-round and combines it with a variation-budget block argument to yield a cumulative dynamic regret bound (Theorem 7.1, conclusion (v)).

**Multi-step chain-rule compositionality.** For deterministic optimal trajectory $\pi^*_{1:T}$ and joint policy $Q_{1:T}$, the KL chain rule gives
$$D_{\mathrm{KL}}(\pi^*_{1:T} \,\|\, Q_{1:T}) \;=\; \sum_{t=1}^T \mathbb E_{\pi^*_{1:t-1}}\!\left[D_{\mathrm{KL}}(\pi^*_t \,\|\, Q_t)\right] \;=\; -\sum_{t=1}^T \log Q_t(a^*_t \mid h_t),$$
which is exactly the negative log-likelihood of the optimal trajectory under the agent's joint policy — equivalently, the *behavior-cloning loss against optimal-trajectory data*. The per-step identity coordinates compose additively via the chain rule, giving a multi-step quantity with independent significance in imitation-learning analysis. This is a third structural role for the identity, alongside coordinate-optimality and the cumulative-regret connection of §7.

### 4.5  Strict improvement over Pinsker

Pinsker's inequality $\operatorname{TV}(P, Q) \le \sqrt{D_{\mathrm{KL}}(P \,\|\, Q) / 2}$ [Tsybakov 2009 §2.4; Cover-Thomas 2006 §11.6] gives a regret bound that does not assume deterministic $\pi^*$:
$$R(Q) \;\le\; V_{\max} \cdot \sqrt{D_{\mathrm{KL}}(\pi^* \,\|\, Q) / 2}.$$
Under the canonical scope, this is *strictly weaker* than Theorem 4.2 in two distinct senses.

**(i) Linear vs.\ square-root in $D_{\mathrm{KL}}$ at small divergence.** For small $D_{\mathrm{KL}}$, Taylor expansion gives $1 - e^{-D_{\mathrm{KL}}} \approx D_{\mathrm{KL}} - D_{\mathrm{KL}}^2/2$, while $\sqrt{D_{\mathrm{KL}}/2}$ scales as $D_{\mathrm{KL}}^{1/2}$. The identity form is linear-in-$D_{\mathrm{KL}}$ near zero; Pinsker is square-root-in-$D_{\mathrm{KL}}$. At the level of regret, this means the identity form gives a tighter bound near optimal: $1 - e^{-D_{\mathrm{KL}}} < \sqrt{D_{\mathrm{KL}}/2}$ for all $D_{\mathrm{KL}} > 0$ in the regime where the bound is interesting.

**(ii) Pinsker becomes vacuous for $D_{\mathrm{KL}} > 2$.** The Pinsker right-hand side $V_{\max}\sqrt{D_{\mathrm{KL}}/2}$ exceeds the trivial regret envelope $V_{\max}$ once $D_{\mathrm{KL}} > 2$, giving no information beyond the trivial bound. The identity form is informative *uniformly in $D_{\mathrm{KL}}$*: $V_{\max}(1 - e^{-D_{\mathrm{KL}}})$ is monotonically increasing in $D_{\mathrm{KL}}$ but bounded by $V_{\max}$, so the bound saturates rather than blowing up. Combined with the matching lower bound, this means Theorem 4.2 has nontrivial content across the entire range of policy distributions $Q$ with $Q(a^*) > 0$, while Pinsker is silent for half of that range.

The same comparison applies against the BH inequality at this corner: BH gives $V_{\max}\sqrt{1 - e^{-D_{\mathrm{KL}}}}$, which is uniformly looser than our $V_{\max}(1 - e^{-D_{\mathrm{KL}}})$ on $D_{\mathrm{KL}} > 0$ (since $x < \sqrt{x}$ for $x \in (0, 1)$). Theorem 4.2 thus strictly improves both Pinsker and BH at the deterministic-$\pi^*$ corner.

A worked numerical comparison appears in Appendix B.

### 4.6  Perturbative extension to $\epsilon$-stochastic optima; where Pinsker / BH are the right tool

The point-mass identity holds *exactly* only under deterministic $\pi^*$. The deterministic scope is canonical for RL with discrete action spaces and isolated optima — finite-MDP RL with unique optimal action per state, the regime of [Lattimore-Szepesvári 2020] decision-theoretic analysis, most theoretical work in non-stationary MDPs. Tied-optimum extensions (Appendix A.5) handle support on a tied-optimum set $\mathcal A^* = \{a : Q_O(a) = Q_O(a^*)\}$.

**Perturbative extension to $\epsilon$-stochastic $\pi^*$.** The deterministic regime is *the unperturbed limit* of a perturbative identity, not a hard scope wall. For $\epsilon$-greedy stochastic optimum $\pi^*_\epsilon(a^*) = 1-\epsilon$, $\pi^*_\epsilon(a) = \epsilon/(|\mathcal A|-1)$ for $a \neq a^*$, the identity holds with quantified correction:

**Theorem 4.3 (Perturbative identity for $\epsilon$-stochastic optima).** *For $\epsilon$-greedy $\pi^*_\epsilon$ and any policy $Q$ with $Q(a^*) > 0$ uniformly bounded below,*
$$\boxed{\;\operatorname{TV}(\pi^*_\epsilon, Q) \;=\; 1 - e^{-D_{\mathrm{KL}}(\pi^*_\epsilon \,\|\, Q)} + O\!\left(\epsilon \log(1/\epsilon)\right).\;}$$
*The correction vanishes uniformly as $\epsilon \to 0$ and is sub-linear in $\epsilon$ (slower than $\epsilon$ but vanishing). For softmax-regularized $\pi^*_\tau \propto \exp(Q_O/\tau)$ with temperature $\tau$, the correction is $O(\exp(-\Delta_{\min}/\tau))$ — exponentially small in $1/\tau$.*

Derivation in Appendix A.6. The two-sided regret bound of Theorem 4.2 transfers with the same correction order: $\Delta_{\min}(1-e^{-D_{\mathrm{KL}}}) - O(\epsilon\log(1/\epsilon)) \le R(Q) \le V_{\max}(1-e^{-D_{\mathrm{KL}}}) + O(\epsilon\log(1/\epsilon))$. Deterministic-$\pi^*$ is therefore *not* a hard scope wall but the unperturbed corner of an extension that covers the most common stochastic-policy regularizations used in practice.

**Where Pinsker / BH remain the right tools.** Outside the perturbative regime — for stochastic $\pi^*$ that is *not* a small perturbation of a point mass (e.g., genuinely high-entropy optima in tied-optimum or hard-exploration regimes), or for general $f$-divergence inequalities on action distributions — the BH inequality $\operatorname{TV} \le \sqrt{1 - e^{-D_{\mathrm{KL}}}}$ is the relevant general bound, with Pinsker as the textbook fallback.

### 4.7  Direction of the divergence is forced

The KL direction in Theorems 4.1–4.2 is *reverse-KL* — $D_{\mathrm{KL}}(\pi^* \,\|\, Q)$, with the optimum first. This is forced by the deterministic-$\pi^*$ scope: forward-KL $D_{\mathrm{KL}}(Q \,\|\, \pi^*)$ is $+\infty$ whenever $Q$ has any mass off $a^*$, since $\pi^*(a) = 0$ for $a \neq a^*$ makes the summand $Q(a) \log(Q(a)/0) = +\infty$. A bound "$R \le +\infty$" is vacuous. The reverse direction is the only non-vacuous form under deterministic $\pi^*$.

Within the reverse direction, multiple $f$-divergences yield valid regret bounds (Appendix A surveys $\chi^2$, Rényi-$\alpha$, Hellinger). Reverse-KL is *uniquely* selected within this family by the chain-rule additivity axiom [Hobson 1969; Csiszár 1991]: it is the only smooth $f$-divergence whose value on a joint distribution decomposes additively over a factorization. The chain-rule axiom is a standard structural commitment in the decision-theoretic and information-geometric literature; we invoke it as an external axiom and refer to Appendix C for the functional-equation derivation.

### 4.8  Position within the information-theoretic RL literature

Information-theoretic regret bounds in RL form a substantial line: [Russo-Van Roy 2014a] entropy-of-optimal-action bound for Thompson sampling; [Russo-Van Roy 2014b] information-directed sampling; [Lu-Van Roy 2019] information-theoretic confidence bounds; [Min-Russo 2023] non-stationary entropy-rate bounds; [Lattimore-György 2020] mirror descent and information ratio; [Kakade-Krishnamurthy-Lowrey-Ohnishi-Sun 2020] information-theoretic regret for nonlinear control. None of these deploys the Bretagnolle--Huber inequality at all, let alone its point-mass exact specialization. They use Shannon entropy, mutual information, information ratio, Pinsker, or Hellinger as the connective tissue between the regret quantity and the divergence on policy space.

The negative signal from the Undermind retrieval (63 papers; abstract-level coverage of 75% in the relevant literature; full search documented in supplementary materials) is robust: the Bretagnolle--Huber inequality does not appear in the retrieved RL/non-stationary corpus. A fortiori, the *exact identity at its point-mass corner* — Theorem 4.1 — is also absent. The exact identity is the technical anchor of this paper, and is, to our knowledge, novel to the RL/non-stationary RL literature.


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

The persistence condition for an adaptive system is the structural inequality $\alpha > \rho / R$, where $\alpha$ is the correction rate, $\rho$ is the disturbance rate, and $R$ is the reserve [Khalil 2002 Ch.\ 4 and 9; Khasminskii 2012]. The condition is a Lyapunov-derived sufficient condition for ultimate boundedness of mismatch under sector-bounded correction and bounded disturbance, and it stands at the heart of the adaptive-control tradition: the same sector-condition template grounds Kalman-filter ultimate-error bounds [Anderson and Moore 1979] and the persistent-excitation conditions of slowly-time-varying adaptive control [Anderson 1985; Kreisselmeier 1986]. **We apply this template to the strategic substate**, treating policy-mismatch as the state variable, the policy-revision rate as the correction rate, and the rate at which the environment invalidates policies as the disturbance. The structural form is inherited; what is new is the strategic-substate instantiation and the forgetting-prerequisite specialization in §5.3.

Let $\delta_\Sigma$ denote the policy-mismatch state — the gap between the agent's current policy and the best available policy revision (operationally: the strategic-calibration residual; for analysis here, a scalar mismatch with the same Lyapunov structure as the epistemic case). Let $\rho_\Sigma$ be the disturbance rate at which the environment invalidates the agent's policy (a domain parameter, structurally analogous to environmental change rate $\rho$ in the epistemic case). Let $R_\Sigma$ be the strategic reserve — the maximum policy mismatch the correction machinery can absorb before saturation.

Under sector-bounded strategic correction with parameter $\alpha_\Sigma$, the persistence condition for $\Sigma$ is the direct instantiation of the template:
$$\alpha_\Sigma \;>\; \rho_\Sigma / R_\Sigma. \tag{P}$$

### 5.3  The forgetting prerequisite and the structural class $\mathcal A_{\mathrm{accum}}$

Condition (P) is an *instantaneous* check at the current operating point of the agent. For Bayesian-style policy updates (e.g., Beta-Bernoulli edge updates in a structured policy DAG, or any update mechanism whose effective sample size grows with experience), the per-element sector parameter has the form
$$\alpha_{ij} \;=\; \nu_{ij} \cdot \iota_{ij} \cdot \eta_{\mathrm{edge}, ij}, \qquad \eta_{\mathrm{edge}, ij} \;=\; 1 / (n_{ij} + 1),$$
with $n_{ij}$ the accumulated experience at element $(i,j)$. Define the structural class
$$\mathcal A_{\mathrm{accum}} \;:=\; \big\{\text{updates with } n_{\mathrm{eff}}(t) \to \infty \text{ as } t \to \infty\big\}$$
— count-accumulating Bayesian updates without forgetting; bounded-memory schemes with growing memory; observation-aggregating schemes without restart. *For any fixed $(\rho_\Sigma, R_\Sigma)$ with $\rho_\Sigma > 0$, every agent in $\mathcal A_{\mathrm{accum}}$ eventually violates condition (P)* — at every element, $\eta_{\mathrm{edge}, ij} \to 0$ as $n_{ij} \to \infty$, so the bottleneck $\mathcal T_\Sigma^{\mathrm{bn}}$ decays to zero with experience.

This is a *structural failure of the class $\mathcal A_{\mathrm{accum}}$*, not a tuning problem. The agent's prior calibration cannot help: at any finite calibration level, the correction rate decays below threshold once enough experience accumulates. Mechanisms outside $\mathcal A_{\mathrm{accum}}$ — constant-step-size stochastic approximation, sliding-window updates, bounded-memory learners, block-restart schemes — escape this asymptotic decay by maintaining $n_{\mathrm{eff}}$ at a finite ceiling, but then face a *bidirectional threshold* (§5.3.1).

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

The forgetting prerequisite is a *structural threshold* on the agent–environment pairing, not a tuning hyperparameter. Five structural consequences:

- **Every factor is load-bearing per element.** $\nu_{ij}, \iota_{ij}, (1-\lambda_{ij})$ enter the bottleneck multiplicatively. Setting any one to zero at any element collapses $\mathcal T_\Sigma^{\mathrm{bn,ss}}$ to zero. Observation rate, identifiability, and discount rate are not interchangeable: an agent cannot compensate for low identifiability by faster forgetting, nor for slow observation by higher gain.
- **Asymptotic failure of count-accumulating updates.** With $\lambda_{ij} = 1$ at any element (no forgetting), $\eta_{\mathrm{edge}, ij} \to 0$ as $n_{ij} \to \infty$ and the bottleneck collapses; the structural-class theorem of §5.3 covers this universally for $\mathcal A_{\mathrm{accum}}$.
- **Sharpness of the threshold within the model.** When $\mathcal T_\Sigma^{\mathrm{bn,ss}} < \rho_\Sigma / R_\Sigma$, an adversarial disturbance concentrating on the bottleneck element drives the modeled mismatch above $R_\Sigma$; when $\mathcal T_\Sigma^{\mathrm{bn,ss}} > \rho_\Sigma / R_\Sigma$, it is bounded at $R_\Sigma^* = \rho_\Sigma / \mathcal T_\Sigma^{\mathrm{bn,ss}}$. The transition is qualitative — the standard Lyapunov phase transition — though stabilization by mechanisms outside the model is not excluded.
- **Aggregate throughput as a separate floor.** The aggregate $\mathcal T_\Sigma^{\mathrm{agg,ss}}$ governs sustained-information-rate / channel-capacity considerations (the rate at which the agent can absorb information from its evidence stream as a whole). The bottleneck governs survival; the aggregate governs throughput. Both must be assessed; only the bottleneck enters the survival inequality.
- **No prior-learning subsidy.** No amount of pre-deployment calibration relaxes the prerequisite: the threshold is on the *steady-state* per-element bottleneck under continual operation, not on initial accuracy.

### 5.5  Lifting [Lee et al.\ 2023] from hyperparameter to structural threshold

[Lee et al. 2023] (the ProST framework, NeurIPS 2023) is the closest published neighbor. ProST defines an *agent tempo* — the schedule of policy update times $\{t_1, \dots, t_K\}$ — and computes the schedule that minimizes the dynamic regret upper bound under non-stationarity. The companion paper [Lee et al. 2024] (Pausing Policy Learning, ICML 2024) shows that *non-zero policy hold duration* yields sharper dynamic regret. Together, they establish tempo as a convergence-relevant variable in non-stationary RL.

Our forgetting prerequisite *lifts* the ProST move along two axes simultaneously: (a) *single-factor → multi-factor*: ProST's tempo is a single scalar (update frequency); ours is a per-element bottleneck over $\nu \cdot \iota \cdot (1-\lambda)$, with each factor independently load-bearing. ProST's schedule recovers as the special case $|E| = 1$, $\nu = \iota = 1$. (b) *Hyperparameter-optimization → structural-survival inequality*: ProST asks *given* an environment, *what tempo schedule* minimizes regret? The forgetting prerequisite asks *given* an environment with disturbance $\rho_\Sigma$ and reserve $R_\Sigma$, *what is the minimal bottleneck below which no schedule persists?* The two questions are complementary; ProST's optimal-schedule result is silent about the *threshold* below which no schedule works.

Concretely, in the Lee et al.\ ProST setup, holding policy fixed between updates is a *block-restart* mechanism. The sector-level equivalence is rigorous:

**Lemma 5.2 (ProST-forgetting equivalence at the sector parameter level).** *Under ProST's block-restart update with $K$ uniform updates over $T$ rounds (block length $\Delta = T/K$), the steady-state sector parameter satisfies $\alpha_\Sigma^{\mathrm{ss}} = K/T$. Within the sector-Lyapunov reduction, this is the same steady-state sector parameter as exponential forgetting with $1 - \lambda_{\mathrm{eff}} = K/T$. The two mechanisms are not equivalent at the cycle-by-cycle level — block-restart freezes between updates while exponential forgetting smoothly discounts — but coincide at the level of the steady-state ultimate-boundedness threshold.*

Applied to ProST: when $K/T > \rho_\Sigma / R_\Sigma$, ProST's schedule satisfies the forgetting prerequisite and the modeled mismatch dynamic remains ultimately bounded, consistent with ProST's sublinear dynamic-regret upper bound. When $K/T < \rho_\Sigma / R_\Sigma$, the schedule fails the prerequisite *within the sector-Lyapunov reduction*, and the modeled mismatch is not ultimately bounded — suggesting an analogous threshold for ProST-style dynamic regret. We do not claim that *every* RL algorithm's dynamic regret diverges in this regime; the suggestion holds for algorithms whose policy-revision dynamics fall under the sector-Lyapunov model. Section 8 develops the reduction in detail.

### 5.6  Distinction from sliding-window and weighted-LSVI forgetting

Forgetting mechanisms appear throughout the non-stationary-RL literature. [Garivier-Moulines 2008] uses sliding-window UCB in non-stationary bandits; [Touati-Vincent 2020] uses exponential-weight LSVI (OPT-WLSVI) in non-stationary linear MDPs; [Russac-Vernade-Cappé 2019] uses forgetting-factor estimators in weighted linear bandits; [Cheung-Simchi-Levi-Zhu 2020] uses sliding-window UCB with confidence widening in non-stationary MDPs.

In each of these, forgetting appears as an *algorithmic mechanism* with a tunable parameter (window size, discount factor) chosen to optimize a dynamic-regret bound. The forgetting prerequisite reframes the same machinery: it identifies the inequality $(1-\lambda) > \rho_\Sigma/R_\Sigma$ as a *structural survival condition*, with environment-side parameters $(\rho_\Sigma, R_\Sigma)$ on the right-hand side and the algorithm's discount rate on the left. This is the strategic analog of the persistence condition $\alpha > \rho / R$ from adaptive control [Khalil 2002], with $(1 - \lambda)$ playing the role of adaptive tempo.

The reframe matters: dynamic-regret-optimization analyses leave open the possibility that *some* schedule satisfies the sector prerequisite for *every* environment. Within the sector-Lyapunov reduction the prerequisite shows that this is false — there are environment regimes (any with $\rho_\Sigma / R_\Sigma \ge 1$) in which no exponential-forgetting schedule with $\lambda \in (0, 1)$ can satisfy the prerequisite, since $1 - \lambda < 1$. The threshold form makes the *existence* of such regimes explicit within the model and the *failure mode* identifiable; ultimate-boundedness via mechanisms outside the sector model is not addressed.

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

Formally: by the temporal ordering and the causal structure of the loop, $a_t$ is a cause of $o_{t+1}$ — not merely a correlate. Replaying a saved $M_t$ against a different event stream is *not* the same as intervening, because the observed consequences would be under a different causal trajectory. The loop's data generating process is therefore Level 2 in Pearl's hierarchy, by construction.

The argument is a logical consequence of the temporal ordering and the singular-trajectory commitment from Section 2. It is not a property of the agent's internal architecture: an agent with no explicit causal model — a Q-learning agent, a transformer-based agent — still operates within an action-perception loop whose data generation is Level 2 in character. The loop *provides* Level 2 data; whether the agent *exploits* this data for Level 2 reasoning depends on its update mechanism.

### 6.3  Interventional *data* is not identified *do*-estimates

A precision matters: action-generated data is Level 2 in *character*, but yielding a *clean estimate* of $P(o \mid do(a), \Omega_t)$ from such data requires more. Four typical obstacles:
1. **Coverage.** The agent must have tried diverse actions, not stayed locked on one policy.
2. **Within-step confounding.** Unobserved state variables that affect both action choice and outcome.
3. **Delay.** Consequences may appear well after $t+1$.
4. **Partial observability.** $o_{t+1}$ reveals only part of the outcome.

We honor this distinction in the regret bound: Theorem 4.2 uses $D_{\mathrm{KL}}(\pi^* \,\|\, Q)$, where $\pi^*$ is computed under the agent's current model $M_t$. The bound is *learnable from on-policy data* in the sense that the KL-coordinate quantity $-\log Q(a^*)$ is computable directly from the policy distribution — but the *meaning* of $a^*$ — that the agent's identified optimum coincides with the true optimum — depends on the strength of causal identification from the loop data.

We partition this strength into three regimes [following the regime taxonomy of Bareinboim et al.\ 2022]:

- **Regime A (intervention-rich).** Software-test domains, controlled laboratory experiments. $\iota \approx 1$. The loop data identifies $do$-effects cleanly; the regret bound is realizable on-policy.
- **Regime B (partial intervention).** Organizational decision-making with concurrent unobserved effects, mixed observation-intervention regimes. $\iota \in (0, 1)$. The loop data is partially identified; the regret bound holds *for the model the agent identifies*, with bias controlled by $1 - \iota$.
- **Regime C (observation-only).** Passive-monitoring scenarios, intelligence analysis. $\iota \approx 0$. The loop yields little usable interventional information; the regret bound is provable analytically but not realizable on-policy. Composite-extension treatment via observer-on-sub-agent interventions [as in adaptive-trial designs] can recover identifiability in some Regime-C subcases.

### 6.4  Closing the gap: the bound as learnable

The composition of Components 2 and 4 is what makes the regret bound *learnable*. The point-mass identity bound (Section 4) is a tight analytic relationship between regret and the reverse-KL coordinate on policy space. The closed-loop interventional access (this section) supplies the data substrate from which the agent's $\pi^*$ — and therefore the bound's right-hand side — can be empirically estimated under sufficient identifiability. Without component 2, the loop yields data with no metric structure on policy space. Without component 4, the metric structure is provable but not usable on-policy.

### 6.5  Distinction from active inference and causal-RL precursors

The substantive observation that the agent's actions cause its observations is implicit in any framework built around an action-perception loop, including active inference [Friston-FitzGerald-Rigoli-Schwartenbeck-Pezzulo 2017; Parr-Pezzulo 2022], control-as-inference [Levine 2018], and the broader cybernetic lineage [Wiener 1948; Conant-Ashby 1970]. Our distinctive moves are three:

(i) The *Bareinboim-hierarchy connection*. Active inference and the cybernetic lineage rest on Bayesian-network generative models (Pearl Level 1, associational). They do not invoke the causal-hierarchy theorem to argue that loop data is the substrate Level-2 queries require. We do — and the consequence is that the strategic policy structure is positioned as a *causal* DAG rather than a Bayesian-network DAG, with $do$-conditioning on $a_t$ in the value object $Q_O$ (Section 2) rather than $\mathbb E$-conditioning.

(ii) Regime-indexed strength of causal identification. The taxonomy A/B/C is explicit; the AI literature treats causal identifiability uniformly within its modeling assumptions and does not surface the regime distinction at the framework level.

(iii) Explicit scope honesty. We carefully distinguish "data generated under intervention" from "cleanly identified $do$-estimates." [Bruineberg-Dolega-Dewhurst-Baltieri 2022] document that the active-inference literature sometimes elides this distinction. The present paper is explicit about where it stops short.

The causal-RL line — [Zhang-Bareinboim 2016, 2022], [Lu-Meisami-Tewari 2021, 2022], [Wang-Yang-Wang 2020 DOVI], [Junzhe Zhang 2020 DTRs] — is the direct ancestor for regime-indexed identifiability and on-policy interventional access. None of these papers compose with non-stationarity, however: their analyses are stationary-MDP. Our composition with the dynamic-regret line and the forgetting prerequisite is, to our knowledge, novel.

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

> **(A4) Two-gap diagnostic.** *The agent applies the satisfaction-gap / control-regret 2$\times$2 to route corrective action.*

> **(A5) Identity-tight base learner (for conclusion (v)).** *In each stationary block between optimum-change events, the base learner achieves per-round regret coordinate $\mathbb E[1 - e^{-K_t}] \le c \cdot t^{-1/2}$ for a constant $c$ independent of the block (Appendix E surveys Thompson sampling and UCB as instances).*

*Then:*

*(i) Per-round regret is two-sided identity-bounded:*
$$\Delta_{\min}\bigl(1 - e^{-D_{\mathrm{KL}}(\pi^*_t \,\|\, Q_t)}\bigr) \;\le\; R(Q_t) \;\le\; V_{\max}\bigl(1 - e^{-D_{\mathrm{KL}}(\pi^*_t \,\|\, Q_t)}\bigr).$$

*(ii) Aggregate mismatch $\boldsymbol\delta_\Sigma$ is ultimately bounded under non-stationarity, with steady-state bound $R_\Sigma^* = \rho_\Sigma / \mathcal T_\Sigma^{\mathrm{bn,ss}}$.*

*(iii) The KL coordinate $D_{\mathrm{KL}}(\pi^*_t \,\|\, Q_t)$ is estimable from on-policy data via the empirical visit-frequency estimator $\hat D := -\log \hat Q_n(a^*_{\mathrm{ag}})$ satisfying, on the event $\{Q(a^*) \ge q_0\}$,*
$$\mathbb E[|\hat D - D_{\mathrm{KL,true}}|] \;\le\; (1-\iota)\log(1/q_0) \;+\; q_0^{-1}\sqrt{\log 2 / (2n)};$$
*the variance term decays at standard $1/\sqrt n$; the bias term scales linearly in $1 - \iota$ (Regime A, $\iota = 1$: zero bias; Regime B, $\iota \in (0,1)$: bias controlled by conflated-edge mass; Regime C, $\iota = 0$: bias up to $\log(1/q_0)$). Proof in Appendix F.*

*(iv) The 2$\times$2 cell containing $(\delta_{\mathrm{sat}}, \delta_{\mathrm{regret}})$ identifies the corrective action class: revise policy (regret-driven), revise model/policy-class/horizon (capability-driven), or revise objective (last resort).*

*(v) Cumulative dynamic regret obeys*
$$\mathrm{DynReg}(T) \;:=\; \sum_{t=1}^T R_t(Q_t) \;\le\; 2c\,V_{\max}\,\sqrt{B_T \cdot T},$$
*where $B_T := |\{t : a^*_t \ne a^*_{t-1}\}|$ counts optimum-change events. The per-round coordinate is sharper than Pinsker / BH; the cumulative rate matches the variation-budget literature [Cheung et al.\ 2020; Wei--Luo 2021] as a corollary, not as a new rate.*

The proof composes four component theorems with one variation-budget block argument: Theorem 4.2 gives (i); Theorem 5.1 gives (ii); the empirical-estimator analysis of Appendix F gives (iii); the 2$\times$2 disambiguation of Section 3 gives (iv); for (v), partition $[1, T]$ into $B_T + 1$ stationary blocks at the optimum-change events; in each block the per-round identity bound combines with (A5) and Cauchy–Schwarz across blocks, $\sum_i \sqrt{\tau_{i+1} - \tau_i} \le \sqrt{B_T \cdot T}$. Detailed proof of (v) in Appendix F.

**Remarks.**

- Under Thompson sampling or UCB as the base learner, (A5) holds with a stronger logarithmic rate $\mathbb E[1 - e^{-K_t}] = O(\log t / (t\Delta_{\min}))$ in each stationary block (Appendix E), giving cumulative regret $O(V_{\max} B_T \log^2(T/B_T) / \Delta_{\min})$ — sharper than $\sqrt{B_T \cdot T}$ when $\Delta_{\min}$ is bounded away from zero. The square-root rate is the worst-case Cauchy–Schwarz bound; the logarithmic rate is the typical-case bound for stochastic-bandit base learners.

- Pointwise convergence $V(\pi_t) \to V^*$ is structurally unavailable under genuine non-stationarity (the target is itself moving). The right replacement is the *Cesàro tracking statement* $\frac{1}{T}\sum_t (V^*_t - V(\pi_t)) = O(\sqrt{B_T/T}) \to 0$ when $B_T = o(T)$, which is a corollary of (v) under (A5).

### 7.2  What is new about the composition

Each individual conclusion of Theorem 7.1 has a published precedent. The novelty is in the joint statement and in the *closure*: the four components together close a story none of the strands closes individually.

- The dynamic-regret literature [Cheung-Simchi-Levi-Zhu 2020; Wei-Luo 2021] has (ii) but not (i): no metric structure on policy space, no two-sided regret bound at the level of action distributions.
- The information-theoretic regret literature [Russo-Van Roy 2014a, 2014b; Lu-Van Roy 2019; Min-Russo 2023] has (i) at the level of mutual-information bounds, but uses Pinsker or Hellinger rather than the point-mass identity (or the underlying Bretagnolle--Huber inequality), and is non-stationary only in [Min-Russo 2023] — without (ii)'s sharp threshold form.
- The causal-RL literature [Zhang-Bareinboim 2022; Wang-Yang-Wang 2020; Lu-Meisami-Tewari 2021] has (iii), but is stationary — without (ii).
- The two-term decomposition literature [Long-Fei Li-Zhao-Zhou 2024; Fei-Yang-Wang-Xie 2020] decomposes along the exploration-vs-adaptation axis but not along the goal-vs-policy axis (iv).
- The tempo literature [Lee et al.\ 2023, 2024; Touati-Vincent 2020] has (ii)'s rate-of-update component but without the threshold form: their tempo is a hyperparameter to be optimized, not a structural inequality the agent must satisfy.

### 7.3  Assembly plus a derived cumulative-regret rate

The composition theorem is positioned as *assembly + a derived rate*. Conclusions (i)–(iv) are theorems with published or directly-derived ancestors; conclusion (v) is the cumulative dynamic regret rate $\mathrm{DynReg}(T) \le 2cV_{\max}\sqrt{B_T \cdot T}$, which is *derived* by composing the per-round identity (i) with the variation-budget block argument under the base-learner stationary rate (A5). The cumulative rate is not new — it matches Cheung et al. 2020 / Wei–Luo 2021 as a corollary — but the route through the per-round identity is. The contribution is the recognition that the four components together hold simultaneously in the non-stationary regime under (A1)–(A4), supply a sharper *per-round coordinate* than Pinsker/BH, and yield the cumulative variation-budget rate by a clean compositional argument.

One way to see why this composition is non-obvious: each strand has internal reasons not to need the others. The dynamic-regret literature treats policy-space metric structure as orthogonal to its variation-budget machinery. The information-theoretic literature treats non-stationarity as orthogonal to its information-ratio machinery. The causal-RL literature treats non-stationarity as a separate layer outside its identification machinery. The composition cuts orthogonally across all three: in our derivation each strand's machinery is *load-bearing* for one of properties 1–3 and is *not, on its own, sufficient* to deliver the joint statement (we do not claim a uniqueness theorem ruling out alternative routes). The two-gap diagnostic is what makes the joint statement actionable rather than merely true.

### 7.4  Honest scope

The composition holds in the canonical scope of Section 2: deterministic $\pi^*$, bounded value, isolated optimum, singular trajectory, finite horizon. Outside this scope:

- Stochastic $\pi^*$ (softmax-smoothed or tied-optimum) breaks the point-mass identity; the Bretagnolle--Huber inequality $\operatorname{TV} \le \sqrt{1 - e^{-D_{\mathrm{KL}}}}$ becomes the relevant general bound, and (i) becomes one-sided. Pinsker is the textbook fallback. [Appendix A.5 sketches the extension.]
- Unbounded value range makes $V_{\max} = \infty$ and the upper bound trivial. The lower bound via $\Delta_{\min}$ remains informative.
- Non-isolated optima ($\Delta_{\min} = 0$) eliminate the lower bound but preserve the upper.
- Non-singular trajectories (type-like or parallel-copy agents) require additional machinery; the loop-Level-2 argument depends on singularity.

The composition is robust within its scope but degrades cleanly on each axis. We do not claim a uniqueness result — that *every* convergence theory satisfying properties 1–3 must reduce to this composition. The right uniqueness statement (if one exists) likely requires further structural axioms; we leave it to future work.


---

## 8  Worked Example: Conceptual Reduction to ProST [Lee et al.\ 2023]

The cleanest worked example is the reduction of [Lee et al. 2023]'s ProST framework to a special case of Theorem 7.1. The reduction is rigorous at the steady-state sector-parameter level (Lemma 5.2) and conceptual at the cycle-by-cycle level; ProST's published experiments provide empirical motivation rather than direct confirmation of the sector-Lyapunov ultimate-boundedness conclusion (§8.2).

### 8.1  ProST as a special case

ProST considers a non-stationary MDP with bounded variation in reward and transition, and parameterizes the agent's policy update by a *tempo schedule* $\{t_1, t_2, \dots, t_K\}$ — the times at which the policy is updated using accumulated experience. Between update times, the policy is held fixed. The dynamic regret is bounded as a function of (a) the schedule, (b) the variation budget, and (c) the stationary-MDP regret of the base learner. ProST optimizes the schedule to minimize this upper bound.

We map ProST to Theorem 7.1 as follows.

**Forgetting rate ↔ tempo schedule density.** ProST's tempo schedule corresponds to an effective forgetting rate. If the schedule has $K$ updates over $T$ rounds, each update carries weight $\sim 1/K$ relative to past evidence. The corresponding effective discount factor is $\lambda_{\mathrm{eff}} \approx 1 - K/T$, giving forgetting rate $1 - \lambda_{\mathrm{eff}} \approx K/T$.

**ProST's optimal schedule satisfies the forgetting prerequisite *when the regret bound is non-vacuous*.** ProST's dynamic-regret upper bound is sublinear in $T$ when the schedule is chosen such that the per-update bias from finite-window estimation is balanced against the per-round adaptation cost. This balance corresponds, in our framework, to $1 - \lambda_{\mathrm{eff}} > \rho_\Sigma / R_\Sigma$ — the forgetting prerequisite. When $\rho_\Sigma / R_\Sigma$ exceeds the threshold determined by the variation budget, ProST's optimization yields a schedule that satisfies (A2); below the threshold, ProST's bound becomes vacuous.

**Regime A identifiability.** ProST operates on standard MDPs with full state observability; the loop data is Regime A ($\iota \approx 1$). The point-mass identity bound holds with the identifications (A1) and (A3) of Theorem 7.1 satisfied.

**Goal-feasibility-vs-policy-quality 2$\times$2.** ProST's setup assumes the goal is feasible (the optimal policy in the stationary case achieves bounded reward); $\delta_{\mathrm{sat}} = 0$ throughout. The 2$\times$2 reduces to two cells: success and strategy problem. The strategy-problem cell is what ProST optimizes against.

### 8.2  Empirical grounding via ProST experiments

ProST is validated empirically on continuous-control tasks with non-stationary dynamics. The dynamic-regret bound's predictions are confirmed: their schedule outperforms baselines that update at fixed frequency, and the optimal schedule density tracks the rate of environmental change. Under the reduction above, ProST's empirical results *motivate* the strategic-tempo component (Component 3): their experimental finding that schedule density tracks environmental change rate is consistent with the forgetting-prerequisite threshold derived in §5.5, but does not directly test the sector-Lyapunov ultimate-boundedness conclusion. The reduction at the steady-state sector-parameter level (Lemma 5.2) is rigorous; the empirical mapping is suggestive rather than confirmatory.

The remaining components are not directly tested by ProST. Component 2 (point-mass reverse-KL/TV identity) is mathematically airtight by Theorem 4.1 — a direct two-line calculation. Component 4 (closed-loop interventional access) is implicit in ProST's setup (their training data is on-policy interventional under Regime A) but not explicitly invoked in their analysis. Component 1 (two-gap diagnostic) is trivially satisfied in their setup ($\delta_{\mathrm{sat}} = 0$) and therefore not exercised; its diagnostic content arises in cases where goal feasibility itself is in question, which ProST's setup does not exhibit.

### 8.3  Variation-budget instantiation

A second worked example is the [Cheung-Simchi-Levi-Zhu 2020] variation-budget framework. Their sliding-window UCB with confidence widening (SWUCRL2-CW) corresponds to a fixed forgetting rate $1 - \lambda = 1/W$ where $W$ is the window length. Their dynamic-regret bound depends on $W$ being chosen against the variation budget $B_T$ in a specific way; under our framework, this choice maps onto the forgetting prerequisite $(1 - \lambda) > \rho_\Sigma / R_\Sigma$ with $\rho_\Sigma$ identified with a quantity proportional to $B_T / T$ — a conceptual reduction rather than an exact algorithmic identification. The same analogy applies to [Wei-Luo 2021]'s black-box reduction (which adaptively tunes $W$) and to [Mao et al.\ 2021]'s RestartQ-UCB (which periodically restarts, similar in shape to abrupt forgetting at restart times).

### 8.4  What the worked examples don't yet cover

We have not worked through reductions to: the causal-RL line [Zhang-Bareinboim 2022; Lu-Meisami-Tewari 2021] (would primarily exercise Component 4); the satisficing line [Hajiabolhassan-Ortner 2025; Zhang-Zhu-Xie 2026] (would exercise Component 1 with $\delta_{\mathrm{sat}} > 0$); or the dynamic-regret-with-causal-knowledge composition (which our framework predicts is sharper than either lineage alone, but which we leave as future work).

---

## 9  Related Work

The four-strand structure of the prior art organizes the related-work analysis cleanly.

### 9.1  Strand 1 — Dynamic regret under drift

[Cheung-Simchi-Levi-Zhu 2020] is the canonical non-stationary RL paper, introducing sliding-window UCB with confidence widening (SWUCRL2-CW) and the variation-budget formalism. [Wei-Luo 2021] gives an optimal black-box reduction yielding $\widetilde{\mathcal O}(\min\{\sqrt{LT}, \Delta^{1/3} T^{2/3}\})$ dynamic regret without prior knowledge of the variation budget. [Mao-Zhang-Zhu-Simchi-Levi-Başar 2021] introduces RestartQ-UCB with matching upper and lower bounds in non-stationary episodic MDPs. [Cheung-Simchi-Levi-Zhu 2018, 2019] (Hedging the Drift) develops the bandit-over-bandit framework. [Gajane-Ortner-Auer 2019] gives the first variational regret bound for general RL.

Our framework recovers these as instances of the strategic-tempo + forgetting prerequisite (Component 3, Section 8.3): each defines a forgetting mechanism whose discount rate $1-\lambda$ must exceed $\rho_\Sigma / R_\Sigma$ for the sector-Lyapunov ultimate-boundedness conclusion to apply (and hence for the variation-budget-style dynamic-regret bound to be non-vacuous under the reduction). The novelty is the *threshold form* — the existence, within the sector-Lyapunov reduction, of environment regimes where the modeled mismatch dynamic does not stabilize for any schedule with $\lambda \in (0,1)$ — which dynamic-regret-optimization analyses do not surface.

### 9.2  Strand 2 — Two-term regret decompositions

[Long-Fei Li-Zhao-Zhou 2024] decompose dynamic regret of adversarial MDPs with unknown transition into two terms: one due to confidence-set construction (transition uncertainty), one due to suboptimal policy choice under non-stationarity. [Fei-Yang-Wang-Xie 2020]'s POWER and POWER++ algorithms achieve dynamic regret with explicit two-component decomposition (exploration + adaptation), the first model-free dynamic-regret analysis in non-stationary RL. [Stradi-Lunghi-Castiglioni-Marchesi-Gatti 2024] gives an analogous decomposition in non-stationary CMDPs with sublinear regret and positive constraint violation.

These are the closest structural neighbors of our two-gap diagnostic. The decomposition shape (two additive terms) is similar; the *axis* differs. Our axis is goal-feasibility vs.\ policy-quality; theirs is uncertainty-source. Both are valid; neither subsumes the other. A full analysis of the relationship is in Appendix A.

[Yang-Zheng-Tomizuka-Liu-Li 2024] presents a feasibility theory of constrained RL, distinguishing virtual-time and real-time feasibility. Their "feasibility" is constraint-region feasibility (state stays inside the constraint set); ours is goal feasibility (objective threshold is attainable). The vocabulary overlap requires careful disambiguation; the formal objects are distinct.

### 9.3  Strand 3 — Tempo and forgetting

[Lee et al. 2023] (ProST framework, NeurIPS 2023) is the closest single neighbor for Component 3. They explicitly compute a tempo schedule minimizing dynamic-regret upper bound, showing the trade-off between agent tempo and environment tempo. [Lee et al. 2024] (Pausing Policy Learning, ICML 2024) shows non-zero policy hold duration sharpens dynamic regret. Our forgetting prerequisite *lifts* their tempo result from a hyperparameter-optimization claim to a structural-threshold claim (Section 5.5).

[Touati-Vincent 2020] (OPT-WLSVI) uses exponential-weight forgetting in non-stationary linear MDPs — the closest exponential-forgetting RL ancestor. [Russac-Vernade-Cappé 2019] (weighted linear bandits) and [Garivier-Moulines 2008] (sliding-window UCB for non-stationary bandits) are the bandit ancestors.

### 9.4  Strand 4 — Causal and interventional access

[Zhang-Bareinboim 2022] (online RL for mixed policy scopes, NeurIPS 2022) is the closest causal-RL neighbor. [Zhang-Bareinboim 2016] (MDPs with unobserved confounders) formalizes the interventional-vs-observational distinction. [Lu-Meisami-Tewari 2021, 2022] (Causal MDPs / C-UCBVI) gives regret scaling on a causal-graph-dependent quantity, exponentially smaller than the action space. [Wang-Yang-Wang 2020] (DOVI) explicitly adjusts for confounding bias with strict regret improvement when observational data is informative. [Junzhe Zhang 2020] applies causal RL to dynamic treatment regimes. [Schulte-Poupart 2025] is the most recent meta-analysis of when causal structure helps RL.

The gap: none of these compose with non-stationarity. The dynamic-regret line is non-causal; the causal line is stationary. Our composition is, to our knowledge, the first to combine the two.

### 9.5  Cross-cutting — Information-theoretic regret bounds

The information-theoretic regret line [Russo-Van Roy 2014a (Thompson), 2014b (IDS); Lu-Van Roy 2019 (IT confidence bounds); Min-Russo 2023 (non-stationary entropy-rate bounds); Lattimore-György 2020 (mirror descent + information ratio)] uses Shannon entropy, mutual information, information ratio, Pinsker, or Hellinger to bound regret. [Canonne 2022] gives an inequality between KL and TV adjacent to the Bretagnolle--Huber form but as a general statistical-distance result, not RL.

The Bretagnolle--Huber inequality itself is conspicuously absent from the retrieved RL/non-stationary literature; the retrieval study (Appendix D / supplementary materials) confirms this. A fortiori, the point-mass *exact identity* of Theorem 4.1 — which sits strictly below the BH bound at the deterministic-$\pi^*$ corner and supplies the exact regret coordinate — is also absent from this corpus. The point-mass identity is, to our knowledge, novel to this paper, and it is sharper than what the underlying BH inequality would supply at this special point.

### 9.6  Adjacent — Satisficing and feasibility

[Hajiabolhassan-Ortner 2025] (online regret bounds for satisficing in MDPs, *Math.\ Operations Research*) gives constant regret with respect to a satisfaction level $\beta$. [Y. Zhang-Zhu-Xie 2026] (March 2026, contemporaneous per the NeurIPS contemporaneous-work cutoff) gives the most recent satisficing-vs-non-stationarity result. These have vocabulary overlap with our $\delta_{\mathrm{sat}}$ but a different formal axis: their satisficing is "any policy above level $\beta$ is acceptable"; our $\delta_{\mathrm{sat}}$ is "the goal is unmet from $M_t$." Careful disambiguation is in Section 3.

[Yang-Zheng-Tomizuka-Liu-Li 2024] (already noted in §9.2) gives a constraint-region-feasibility theory. Vocabulary-similar, structurally distant.

### 9.7  Contemporaneous work

Per the NeurIPS 2026 main-track cutoff, papers online after March 1, 2026 are contemporaneous. We cite without empirical comparison: [DARLING (Gerogiannis-Huang-Veeravalli 2026)] — detection-augmented RL with non-stationary guarantees; [Y. Zhang-Zhu-Xie 2026] (already discussed). Both are adjacent to our framework but neither composes the four components we identify.

---

## 10  Limitations and Conclusion

### 10.1  Limitations

**Theory-only.** The paper presents no original experiments. Mitigations: (i) the point-mass identity (Theorem 4.1) is a direct two-line calculation under deterministic $\pi^*$ — mathematically airtight. (ii) The reduction to [Lee et al.\ 2023] ProST (Section 8) provides empirical grounding for the strategic-tempo claim through ProST's published experiments. (iii) The composition theorem (Theorem 7.1) is positioned honestly as assembly + interpretation, not a derivation step requiring its own validation.

**Canonical scope.** The deterministic-$\pi^*$ assumption is essential for the point-mass *identity* — under stochastic $\pi^*$, the identity does not hold, and the Bretagnolle--Huber inequality $\operatorname{TV} \le \sqrt{1 - e^{-D_{\mathrm{KL}}}}$ becomes the relevant general bound (as do Pinsker and Hellinger). Stochastic $\pi^*$ — under softmax smoothing for differentiability, or in tied-optimum regimes — therefore replaces the identity-bound with these inequality-bounds; the regret bound becomes one-sided. Extensions are sketched in Appendix A but not developed here.

**Cumulative-regret scope.** Theorem 4.2 is a *per-state, one-step-improvement* regret bound under fixed $M_t$ and the C1 continuation convention (§2); Theorem 7.1 conclusion (v) lifts this to a cumulative dynamic regret bound $\mathrm{DynReg}(T) \le 2cV_{\max}\sqrt{B_T \cdot T}$ via composition with a variation-budget block argument and an identity-tight base learner (A5). Pointwise convergence $V(\pi_t) \to V^*$ is structurally unavailable under genuine non-stationarity (the target is itself moving); the right replacement is the Cesàro tracking statement $\frac{1}{T}\sum_t (V^*_t - V(\pi_t)) = O(\sqrt{B_T/T}) \to 0$ when $B_T = o(T)$, which is a corollary of (v). Occupancy-measure convergence requires uniform-mixing-time analysis across $\{P_t\}$ outside the present scope.

**No uniqueness result for the composition.** We claim that no existing framework composes all four components in the non-stationary regime, supported by the prior-art retrieval (Appendix D). We do *not* claim that any non-stationary convergence theory satisfying the three properties (handles non-stationarity, has policy-space metric, is on-policy learnable) must reduce to our composition. A uniqueness theorem of this shape would likely require further structural axioms (e.g., chain-rule additivity for the metric, a singular-trajectory commitment for interventional access) that we leave to future work.

**Reverse-KL chain-rule axiom.** The selection of reverse-KL as the canonical divergence in Component 2 rests on a chain-rule additivity axiom [Hobson 1969; Csiszár 1991]. The axiom is standard but is not derived from prior commitments of the framework; it is invoked as an external structural commitment.

**Strategic-disturbance parameter $\rho_\Sigma$ is a domain quantity.** The forgetting prerequisite is sharp given $\rho_\Sigma$ and $R_\Sigma$, but estimating these from data is a domain-specific problem we do not address. The structural content is the *form* of the threshold, not its numerical value in any specific deployment.

**Class 2 architectural scope.** Theorem 6.1's loop-as-Level-2-engine claim depends on a directed-separation property between the agent's model state and goal state. For agents where these are coupled (e.g., goal-conditioned LLM policies where the goal is part of the model's prompt context), the closed-loop interventional access argument requires additional machinery. We treat this as out of scope; for related discussion see [Bruineberg et al.\ 2022].

### 10.2  Future work

- **Stochastic-$\pi^*$ extension.** The perturbative identity of §4.6 (Theorem 4.3) extends Theorem 4.1 to $\epsilon$-stochastic ($\epsilon$-greedy) and softmax-regularized ($\tau$-temperature) optima with $O(\epsilon\log(1/\epsilon))$ and $O(\exp(-\Delta_{\min}/\tau))$ corrections respectively. The two-sided regret bound of Theorem 4.2 transfers with the same correction order. Genuinely high-entropy optima outside the perturbative regime are open; BH and Pinsker supply one-sided fallbacks there.
- **Tied-optimum extension.** Direct (Appendix A.5).
- **Class-2 extension.** Coupling of $M_t$ and $G_t$ degrades the loop-Level-2 argument. The right machinery is likely a Bayesian inverse-problem stability analysis [Stuart 2010; Hosseini-Hsu-Taghvaei 2025] — adjacent regularity theory we believe applies but have not developed.
- **Algorithmic instantiation.** The composition theorem is structural; a practical algorithm achieving Theorem 7.1's joint guarantees would require (a) a base learner with identity-tight regret in stationary regimes, (b) explicit exponential-forgetting schedule satisfying the prerequisite, (c) a Regime-A or Regime-B identifiability check, (d) a 2$\times$2 diagnostic readout for corrective action selection. We sketch such an algorithm in Appendix E but defer empirical evaluation to a follow-up paper.

### 10.3  Conclusion

Non-stationary RL has matured along four parallel tracks — dynamic regret under drift, two-term decompositions, tempo and forgetting analyses, and causal interventional access. None has been composed with the others. We assemble the composition with four components: a two-gap diagnostic separating goal feasibility from policy quality; an exact point-mass reverse-KL/TV regret identity under deterministic optimum, giving a tight two-sided action-distribution regret bound that extends perturbatively to $\epsilon$-stochastic and softmax-regularized optima; a multi-factor strategic-tempo forgetting prerequisite as structural survival inequality, with a structural-class theorem covering count-accumulating updates universally and bidirectional thresholds for non-accumulating mechanisms; and a closed-loop interventional access argument grounded in Pearl's causal hierarchy. Composition with a variation-budget block argument yields cumulative dynamic regret $\mathrm{DynReg}(T) \le 2cV_{\max}\sqrt{B_T T}$ — the per-round identity coordinate is sharper than Pinsker/BH, and the cumulative rate matches the variation-budget literature as a corollary. The point-mass identity, which sits at the deterministic-$\pi^*$ corner of the Bretagnolle--Huber [1978] family and strictly improves the BH inequality there, is the technical anchor — strictly improving Pinsker and absent, to our knowledge, from the prior RL/non-stationary corpus.


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

C1 is the most conservative diagnostic (most likely to diagnose "locally unattainable"); C3 is the most accurate (least false "unattainable" diagnoses). The 2$\times$2 diagnostic structure is preserved under all three; only the inferential force varies.

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

**$\epsilon$-greedy stochastic optimum.** Let $\pi^*_\epsilon(a^*) = 1-\epsilon$, $\pi^*_\epsilon(a) = \epsilon/(|\mathcal A|-1)$ for $a \neq a^*$. For any policy $Q$ with $Q(a^*) > 0$ uniformly bounded below by $q_0$, the reverse-KL admits the expansion
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

The bottleneck strategic tempo $\mathcal T_\Sigma^{\mathrm{bn,ss}}$ of Theorem 5.1 is verified — and the resulting per-topology threshold derived — across four canonical topologies under Beta-Bernoulli edge updates with per-element forgetting (full derivations in supplementary materials):

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

**Result: 63 papers retrieved, abstract-level coverage estimated 75%, no direct anticipation.** The landscape splits into four largely separate strands corresponding to our four components, with no published framework composing all four. The retrieval is documented in supplementary materials.

**Strongest negative signal: the Bretagnolle--Huber inequality is absent from the retrieved RL/non-stationary corpus.** The information-theoretic regret literature uses entropy, mutual information, information ratio, Pinsker, or Hellinger uniformly. A fortiori, the *exact identity at the BH inequality's deterministic-$\pi^*$ corner* — the point-mass reverse-KL/TV identity of Theorem 4.1, which strictly improves the BH bound at this point — is also absent and is, to our knowledge, novel.

**Closest neighbors per strand:** [Cheung-Simchi-Levi-Zhu 2020] (Strand 1); [Long-Fei Li-Zhao-Zhou 2024] (Strand 2 — closest two-term decomposition, different axis); [Lee et al.\ 2023 ProST] (Strand 3 — closest tempo result, different form); [Zhang-Bareinboim 2022] (Strand 4 — closest causal-RL, stationary only).

---

## Appendix E — Sketch of an Algorithm Achieving Theorem 7.1

A practical algorithm achieving the joint guarantees of Theorem 7.1 requires:

**(a) Base learner achieving identity-tight per-round coordinate (up to log factors).** In the stationary deterministic-$\pi^*$ regime, Thompson sampling [Russo–Van Roy 2014a] and UCB [Lattimore–Szepesvári 2020] both achieve the per-round regret coordinate
$$\mathbb E\!\left[1 - e^{-K_t}\right] \;=\; \mathbb E[1 - Q_t(a^*)] \;=\; O\!\left(\frac{\log t}{t \,\Delta_{\min}}\right),$$
matching the identity form $V_{\max}(1 - e^{-D_{\mathrm{KL}}})$ of Theorem 4.2 up to constants and a $\log t$ factor. This satisfies (A5) of Theorem 7.1 with the stronger logarithmic rate. Applied between optimum-change events in the composition of Theorem 7.1(v), this yields cumulative dynamic regret $O(V_{\max} B_T \log^2(T/B_T) / \Delta_{\min})$, sharper than the Cauchy–Schwarz $\sqrt{B_T \cdot T}$ bound when $\Delta_{\min}$ is bounded away from zero. Information-directed sampling [Russo–Van Roy 2014b] requires a different analysis here because $H(\pi^*) = 0$ collapses its information ratio; we leave the IDS analysis as future work.

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
$$\sum_{t=1}^T \mathbb E[1 - e^{-K_t}] \;\le\; 2c \sum_{i=0}^{B_T} \sqrt{\Delta_i} \;\le\; 2c\sqrt{(B_T + 1) \cdot T} \;\le\; 2c\sqrt{2 B_T \cdot T}$$
(absorbing the $+1$ into a constant; using $\sum_i \Delta_i = T$ and Cauchy–Schwarz across $B_T + 1$ blocks). Multiplying by $V_{\max}$ via Theorem 4.2 gives $\mathrm{DynReg}(T) \le 2cV_{\max}\sqrt{B_T \cdot T}$ (up to absorbed constant). The Cesàro tracking corollary $\frac{1}{T}\sum_t (V^*_t - V(\pi_t)) = O(\sqrt{B_T/T}) \to 0$ when $B_T = o(T)$ follows by dividing through by $T$. $\square$

Under Thompson sampling or UCB as the base learner (Appendix E), the per-block stationary rate sharpens to $O(\log\Delta_i / (\Delta_i \Delta_{\min}))$, summing to $O(B_T \log^2(T/B_T) / \Delta_{\min})$ across blocks — sharper than $\sqrt{B_T \cdot T}$ when $\Delta_{\min}$ is bounded away from zero.

---

## References

*Running list, alphabetical. To be cleaned and uniformized for camera-ready; flagged citations with **(verify)** are in the prior-art retrieval but warrant a final cross-check.*

- Abbasi-Yadkori, Y., György, A., Lazic, N. (2022). A new look at dynamic regret for non-stationary stochastic bandits. *J.\ Mach.\ Learn.\ Res.*
- Aczél, J., Daróczy, Z. (1975). *On Measures of Information and Their Characterizations*. Academic Press.
- Agarwal, A., Kakade, S., Lee, J., Mahajan, G. (2021). On the theory of policy gradient methods. *J.\ Mach.\ Learn.\ Res.*
- Amari, S., Nagaoka, H. (2000). *Methods of Information Geometry*. AMS.
- Bareinboim, E., Correa, J., Ibeling, D., Icard, T. (2022). On Pearl's hierarchy and the foundations of causal inference. In *Probabilistic and Causal Inference: The Works of Judea Pearl*, ACM.
- Besbes, O., Gur, Y., Zeevi, A. (2013). Non-stationary stochastic optimization. *Operations Research*.
- Bhatia, K., Sridharan, K. (2020). Online learning with dynamics: a minimax perspective. *arXiv:2012.01668*.
- Bretagnolle, J., Huber, C. (1978). Estimation des densités: risque minimax. *Séminaire de Probabilités XII*, Springer LNM 649.
- Bruineberg, J., Dolega, K., Dewhurst, J., Baltieri, M. (2022). The Emperor's new Markov blankets. *Behavioral and Brain Sciences*.
- Canonne, C. (2022). A short note on an inequality between KL and TV. *arXiv:2202.07198*.
- Cheung, W. C., Simchi-Levi, D., Zhu, R. (2018, 2019). Hedging the drift / Learning to optimize under non-stationarity. *Management Science*.
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
- Hosseini, B., Hsu, D., Taghvaei, A. (2023). Posterior stability in Bayesian inverse problems. **(verify)**
- Junzhe Zhang. (2020). Designing optimal dynamic treatment regimes: a causal RL approach. *ICML*.
- Junzhe Zhang, Bareinboim, E. (2016). MDPs with unobserved confounders: a causal approach. (Tech.\ report; Columbia.)
- Junzhe Zhang, Bareinboim, E. (2022). Online RL for mixed policy scopes. *NeurIPS*.
- Kakade, S., Krishnamurthy, A., Lowrey, K., Ohnishi, M., Sun, W. (2020). Information-theoretic regret bounds for online nonlinear control. *arXiv:2001.10001*.
- Khalil, H. (2002). *Nonlinear Systems* (3rd ed.). Prentice Hall.
- Khasminskii, R. (2012). *Stochastic Stability of Differential Equations*. Springer.
- Lattimore, T., György, A. (2020). Mirror descent and the information ratio. *COLT*.
- Lattimore, T., Szepesvári, C. (2020). *Bandit Algorithms*. Cambridge.
- Lee, H., Jin, M., Lavaei, J., Sojoudi, S. (2023). Tempo adaptation in non-stationary RL (ProST). *NeurIPS 36*.
- Lee, H., Jin, M., Lavaei, J., Sojoudi, S. (2024). Pausing policy learning in non-stationary RL. *ICML*.
- Levine, S. (2018). Reinforcement learning and control as probabilistic inference. *arXiv:1805.00909*.
- Liese, F., Vajda, I. (1987). *Convex Statistical Distances*. Teubner.
- Long-Fei Li, Zhao, P., Zhou, Z.-H. (2024). Dynamic regret of adversarial MDPs with unknown transition and linear function approximation. *AAAI*.
- Lu, X., Van Roy, B. (2019). Information-theoretic confidence bounds for reinforcement learning. *NeurIPS*.
- Lu, Y., Meisami, A., Tewari, A. (2021). Causal MDPs: learning good interventions efficiently. *arXiv:2102.07663*.
- Lu, Y., Meisami, A., Tewari, A. (2022). Efficient RL with prior causal knowledge. *CLEaR*.
- Mao, W., Zhang, K., Zhu, R., Simchi-Levi, D., Başar, T. (2021). Near-optimal model-free RL in non-stationary episodic MDPs. *ICML*.
- Min, S., Russo, D. (2023). Information-theoretic analysis of nonstationary bandit learning. *ICML*.
- Parr, T., Pezzulo, G. (2022). *Active Inference: The Free Energy Principle in Mind, Brain, and Behavior*. MIT Press.
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
- Wang, L., Yang, Z., Wang, Z. (2020). Provably efficient causal RL with confounded observational data (DOVI). *NeurIPS*.
- Wei, C.-Y., Luo, H. (2021). Non-stationary RL without prior knowledge: an optimal black-box approach. *arXiv:2102.05406*.
- Wiener, N. (1948). *Cybernetics*. MIT Press.
- Y. Zhang, Zhu, R., Xie, Q. (2026). On the peril of (even a little) non-stationarity in satisficing regret minimization. *(contemporaneous; March 2026)*.
- Yang, Y., Zheng, Z., Tomizuka, M., Liu, C., Li, S. (2024). The feasibility theory of constrained RL: a tutorial study.
- Zhao, P., Wang, Y.-X., Zhou, Z.-H. (2021). Non-stationary online learning with memory and non-stochastic control. *AISTATS*.

