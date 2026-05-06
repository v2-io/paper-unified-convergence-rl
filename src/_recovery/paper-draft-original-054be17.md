# Title placeholder

*Candidates (decide post-draft):*
- *Tight Regret Bounds and Strategic Tempo: A Unified Convergence Theory for Non-Stationary Reinforcement Learning*
- *Bretagnolle--Huber Identity for Reinforcement Learning Regret: A Unified View of Non-Stationary Convergence*
- *Composing Non-Stationary RL: Two-Gap Diagnostics, the Bretagnolle--Huber Identity, Strategic Tempo, and Closed-Loop Interventional Access*

# Abstract placeholder

*To be drafted post-body. Target ~250 words. Must convey: (i) four-strand fragmentation of non-stationary RL convergence theory; (ii) four-component composition; (iii) Bretagnolle--Huber identity for RL regret as headline technical result, exact-equality form under deterministic optimum policy; (iv) strict improvement over Pinsker; (v) reduction to Lee et al.\ ProST as worked example; (vi) honest framing as theory paper, no empirical anchor.*

---

## 1  Introduction

Non-stationary reinforcement learning has matured along four largely separate research tracks. Each has produced rigorous results; none has been composed with the others into a single convergence theory.

The four tracks are visible in the recent literature as parallel lineages. **Variation-budget dynamic regret under drift** [Cheung-Simchi-Levi-Zhu 2020; Wei-Luo 2021; Mao-Zhang-Zhu-Simchi-Levi-Başar 2021; Gajane-Ortner-Auer 2019] gives sublinear dynamic regret under bounded total variation in reward and transition dynamics. **Two-term regret decompositions** [Long-Fei Li-Zhao-Zhou 2024; Fei-Yang-Wang-Xie 2020; Stradi-Lunghi-Castiglioni-Marchesi-Gatti 2024] split the dynamic regret along an *exploration-vs-adaptation* axis, isolating the cost of confidence-set construction from the cost of tracking a moving optimum. **Tempo and forgetting analyses** [Lee-Jin-Lavaei-Sojoudi 2023 ProST; Lee-Jin-Lavaei-Sojoudi 2024; Touati-Vincent 2020; Russac-Vernade-Cappé 2019; Garivier-Moulines 2008] make policy-update timing or evidence-discount rate an explicit convergence variable. **Causal and interventional access** [Zhang-Bareinboim 2016, 2022; Lu-Meisami-Tewari 2021, 2022; Wang-Yang-Wang 2020 DOVI; Junzhe Zhang 2020] uses causal-graph structure to sharpen sample complexity, but operates in stationary settings only.

The information-theoretic regret literature [Russo-Van Roy 2014a, 2014b; Lu-Van Roy 2019; Min-Russo 2023] traverses these tracks at a different layer, using entropy, mutual information, information ratio, Pinsker, or Hellinger to bound regret. Notably absent from that literature, as we document below, is the Bretagnolle--Huber inequality [Bretagnolle-Huber 1978] — and in particular its specialization to a deterministic optimum policy, which yields an *exact identity* between reverse Kullback--Leibler divergence and total variation.

These four tracks share an evident common ancestor — the dynamic-regret analysis of online MDPs — but no published framework composes them. In particular, no framework combines (a) a regret decomposition along the *goal-feasibility-vs-policy-quality* axis (distinct from the exploration-vs-adaptation decompositions of [Long-Fei Li et al.\ 2024; Fei et al.\ 2020]); (b) the Bretagnolle--Huber identity in its exact-equality form under deterministic optimum; (c) a structural survival inequality threading rate-of-policy-revision against environment-side disturbance; and (d) a closed-loop causal-access argument that makes the regret bound *learnable* on-policy rather than merely provable analytically.

### 1.1  Contribution

This paper assembles the composition. The contribution has the shape — recognized by the NeurIPS Theory Track guidelines as a valid form of originality — of "a novel combination of existing techniques [where] the reasoning behind this combination is well-articulated." Three specific moves carry the paper:

**(i) Bretagnolle--Huber identity for RL regret (Section 4).** Under deterministic optimum policy $\pi^* = \delta_{a^*}$, the inequality $\operatorname{TV}(P, Q) \le \sqrt{1 - \exp(-D_{\mathrm{KL}}(P \,\|\, Q))}$ becomes an *exact identity*: $D_{\mathrm{KL}}(\pi^* \,\|\, Q) = -\log(1 - \operatorname{TV}(\pi^*, Q))$. Composing this with the textbook total-variation regret bound yields a tight two-sided regret characterization
$$\Delta_{\min}\bigl(1 - e^{-D_{\mathrm{KL}}(\pi^* \,\|\, Q)}\bigr) \;\le\; R(Q) \;\le\; V_{\max}\bigl(1 - e^{-D_{\mathrm{KL}}(\pi^* \,\|\, Q)}\bigr)$$
with $\Delta_{\min}$ the action-gap and $V_{\max}$ the value range. This bound *strictly improves* the Pinsker form $V_{\max}\sqrt{D_{\mathrm{KL}}/2}$: the BH form is tight on the upper side, has a matching lower bound, is informative for all $D_{\mathrm{KL}} > 0$, and remains nontrivial when $D_{\mathrm{KL}} > 2$ (where Pinsker is vacuous against the trivial $V_{\max}$ envelope). To our knowledge, the deterministic-$\pi^*$ exact-equality form has not been deployed in the RL or non-stationary-RL literature.

**(ii) Two-gap diagnostic with goal-feasibility-vs-policy-quality axis (Section 3).** We separate $\delta_{\mathrm{sat}}$ (the *satisfaction gap*: the goal is unmet under the best available one-step policy improvement given current model and horizon) from $\delta_{\mathrm{regret}}$ (the *control regret*: the gap between the best available policy improvement and the agent's current policy). The 2$\times$2 disambiguation of these two gaps routes four regimes — *success*, *strategy problem*, *capability limit*, *both* — to four distinct corrective actions. This decomposition runs along a structural axis distinct from the exploration-vs-adaptation decompositions of [Long-Fei Li et al.\ 2024] and [Fei et al.\ 2020]: their decomposition isolates uncertainty *sources* (transition uncertainty vs.\ non-stationarity); ours isolates *what is wrong* (the goal vs.\ the policy).

**(iii) Strategic tempo with forgetting prerequisite (Section 5).** We define a strategic tempo $\mathcal T_\Sigma$ — the rate at which an agent acquires useful policy revisions — and prove a structural survival inequality $(1-\lambda) > \rho_\Sigma / R_\Sigma$ that the discount factor $\lambda$ on accumulated evidence must satisfy for long-run persistence under disturbance rate $\rho_\Sigma$ and policy reserve $R_\Sigma$. This *lifts* the tempo result of [Lee et al.\ 2023] from a hyperparameter-optimization claim (find a tempo schedule that minimizes regret) to a structural threshold (without a forgetting rate exceeding the disturbance-to-reserve ratio, no schedule can persist). The forgetting prerequisite is the strategic analog of the persistence condition in adaptive control [Khalil 2002], with $(1-\lambda)$ playing the role of adaptive tempo.

**(iv) Closed-loop interventional access (Section 6).** We document, with explicit grounding in Bareinboim's causal-hierarchy theorem [Bareinboim-Correa-Ibeling-Icard 2022], that an agent in the feedback loop *generates* Pearl Level-2 (interventional) data by construction: the action $a_t$ causally precedes $o_{t+1}$, so the conditional mismatch $\delta_t \mid a_t$ carries interventional information. This makes the regret bound of (i) *learnable* from on-policy interaction in regimes with sufficient identifiability, not merely provable in principle. The argument is implicit in the action-perception-loop framing of active inference [Friston et al.\ 2017; Parr-Pezzulo 2022] and the causal-RL line [Zhang-Bareinboim 2022; Lu-Meisami-Tewari 2021]; we make the *Bareinboim-hierarchy connection* explicit, partition usable strength of identification into three regimes, and flag honestly where the loop yields interventional *data* without yielding identified *do*-estimates.

**Composition.** The four together give (Section 7) a non-stationarity-aware convergence theory with three properties no existing strand exhibits jointly: the bound *handles non-stationarity* (via tempo and forgetting); has *explicit metric structure on policy space* (via the BH identity); is *learnable from on-policy data* (via closed-loop interventional access). The composition is positioned honestly as *assembly and interpretation* — each of the four components is itself a theorem (cited or proved), and the paper's contribution is the recognition that the four together close a story that none of the strands closes individually.

### 1.2  Scope and limitations

The paper is theory-only; we do not run experiments. The BH identity is mathematically airtight (a one-line specialization of [Bretagnolle-Huber 1978] under deterministic $\pi^*$); the empirical grounding for the strategic-tempo claim is provided indirectly through reduction to Lee et al.\ 2023 ProST as a special case (Section 8). The composition theorem holds in the *canonical scope*: deterministic optimum policy, bounded value range, isolated optimum (so $\Delta_{\min} > 0$), and a singular causal trajectory in the sense made precise in Section 2. We discuss extensions (stochastic $\pi^*$ via softmax smoothing, tied-optimum sets) as future work in Section 10.

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

## 4  Component 2 — Bretagnolle--Huber Identity for RL Regret

### 4.1  Total-variation regret bound (textbook setup)

A bounded-value, deterministic-$\pi^*$ argument gives the textbook total-variation regret bound. From the regret expression $R(Q) = \sum_{a \neq a^*} Q(a) \Delta(a)$ and $\Delta(a) \le V_{\max}$,
$$R(Q) \;\le\; V_{\max} \sum_{a \neq a^*} Q(a) \;=\; V_{\max}\bigl(1 - Q(a^*)\bigr).$$
Under deterministic $\pi^* = \delta_{a^*}$, the total variation between $\pi^*$ and $Q$ is
$$\operatorname{TV}(\pi^*, Q) \;=\; \tfrac12 \sum_a |\pi^*(a) - Q(a)| \;=\; 1 - Q(a^*).$$
So
$$\boxed{\,R(Q) \;\le\; V_{\max} \cdot \operatorname{TV}(\pi^*, Q)\,} \qquad \text{(tight under deterministic } \pi^* + \text{ extremal value landscape).}$$
The bound is tight when $\Delta(a) = V_{\max}$ for all $a \neq a^*$; for typical landscapes it is loose by a factor $\mathbb E_{Q}[\Delta \mid a \neq a^*] / V_{\max} \in (0, 1]$. The matching lower bound via the action gap is:
$$R(Q) \;\ge\; \Delta_{\min} \cdot \operatorname{TV}(\pi^*, Q).$$

### 4.2  The Bretagnolle--Huber identity under deterministic $\pi^*$

The classical Bretagnolle--Huber inequality [Bretagnolle-Huber 1978; Tsybakov 2009 §2.4; Sason-Verdú 2016] is
$$\operatorname{TV}(P, Q) \;\le\; \sqrt{1 - e^{-D_{\mathrm{KL}}(P \,\|\, Q)}},$$
sharper than Pinsker for $D_{\mathrm{KL}}$ moderate-to-large. Under our canonical scope — deterministic optimum $\pi^* = \delta_{a^*}$ — this inequality specializes to an *exact identity*.

**Theorem 4.1 (BH identity for RL regret, deterministic $\pi^*$).** *For deterministic $\pi^* = \delta_{a^*}$ and any policy $Q$ with $Q(a^*) > 0$,*
$$\boxed{\;D_{\mathrm{KL}}(\pi^* \,\|\, Q) \;=\; -\log Q(a^*) \;=\; -\log\bigl(1 - \operatorname{TV}(\pi^*, Q)\bigr).\;}$$

*Proof.* The reverse-KL collapses under the point-mass $\pi^*$:
$$D_{\mathrm{KL}}(\delta_{a^*} \,\|\, Q) \;=\; \sum_a \delta_{a^*}(a) \log\frac{\delta_{a^*}(a)}{Q(a)} \;=\; \log\frac{1}{Q(a^*)} \;=\; -\log Q(a^*),$$
where the convention $0 \log 0 = 0$ disposes of the $a \neq a^*$ terms. Combined with $\operatorname{TV}(\delta_{a^*}, Q) = 1 - Q(a^*)$ from §4.1, $-\log Q(a^*) = -\log(1 - \operatorname{TV})$. $\square$

The identity is elementary — it specializes a published inequality by direct calculation. Its load-bearing consequence is that the BH bound is *exact* in the deterministic-$\pi^*$ regime, not merely tighter-than-Pinsker.

### 4.3  Two-sided regret bound and Lipschitz equivalence

Composing Theorem 4.1 with the TV-regret bound and its action-gap lower bound:

**Theorem 4.2 (Two-sided BH regret bound).** *Under bounded value range $V_{\max}$, deterministic optimum $\pi^*$, isolated optimum so $\Delta_{\min} > 0$, and any policy $Q$ with $Q(a^*) > 0$,*
$$\boxed{\;\Delta_{\min}\bigl(1 - e^{-D_{\mathrm{KL}}(\pi^* \,\|\, Q)}\bigr) \;\le\; R(Q) \;\le\; V_{\max}\bigl(1 - e^{-D_{\mathrm{KL}}(\pi^* \,\|\, Q)}\bigr).\;}$$

*Proof.* From the TV-regret bound $R(Q) \le V_{\max} \cdot \operatorname{TV}(\pi^*, Q)$ and the BH identity $\operatorname{TV}(\pi^*, Q) = 1 - e^{-D_{\mathrm{KL}}}$:
$$R(Q) \;\le\; V_{\max}\bigl(1 - e^{-D_{\mathrm{KL}}(\pi^* \,\|\, Q)}\bigr).$$
The lower bound follows analogously from $R(Q) \ge \Delta_{\min} \cdot \operatorname{TV}$. $\square$

**Lipschitz equivalence.** Theorem 4.2 is equivalent to
$$\frac{\Delta_{\min}}{V_{\max}} \;\le\; \frac{R(Q)}{V_{\max}\bigl(1 - e^{-D_{\mathrm{KL}}}\bigr)} \;\le\; 1.$$
Regret and the BH coordinate $\bigl(1 - e^{-D_{\mathrm{KL}}}\bigr)$ are Lipschitz-equivalent with constants $\Delta_{\min}/V_{\max}$ (below) and $1$ (above). The upper constant is tight when the value landscape is extremal; the lower constant is tight when sub-optimal actions are uniformly bad ($\Delta_{\min} = \max_{a \neq a^*} \Delta(a)$).

### 4.4  Strict improvement over Pinsker

Pinsker's inequality $\operatorname{TV}(P, Q) \le \sqrt{D_{\mathrm{KL}}(P \,\|\, Q) / 2}$ [Tsybakov 2009 §2.4; Cover-Thomas 2006 §11.6] gives a regret bound that does not assume deterministic $\pi^*$:
$$R(Q) \;\le\; V_{\max} \cdot \sqrt{D_{\mathrm{KL}}(\pi^* \,\|\, Q) / 2}.$$
Under the canonical scope, this is *strictly weaker* than Theorem 4.2 in two distinct senses.

**(i) Linear vs.\ square-root in $D_{\mathrm{KL}}$ at small divergence.** For small $D_{\mathrm{KL}}$, Taylor expansion gives $1 - e^{-D_{\mathrm{KL}}} \approx D_{\mathrm{KL}} - D_{\mathrm{KL}}^2/2$, while $\sqrt{D_{\mathrm{KL}}/2}$ scales as $D_{\mathrm{KL}}^{1/2}$. The BH form is linear-in-$D_{\mathrm{KL}}$ near zero; Pinsker is square-root-in-$D_{\mathrm{KL}}$. At the level of regret, this means the BH form gives a tighter bound near optimal: $1 - e^{-D_{\mathrm{KL}}} < \sqrt{D_{\mathrm{KL}}/2}$ for all $D_{\mathrm{KL}} > 0$ in the regime where the bound is interesting.

**(ii) Pinsker becomes vacuous for $D_{\mathrm{KL}} > 2$.** The Pinsker right-hand side $V_{\max}\sqrt{D_{\mathrm{KL}}/2}$ exceeds the trivial regret envelope $V_{\max}$ once $D_{\mathrm{KL}} > 2$, giving no information beyond the trivial bound. The BH form is informative *uniformly in $D_{\mathrm{KL}}$*: $V_{\max}(1 - e^{-D_{\mathrm{KL}}})$ is monotonically increasing in $D_{\mathrm{KL}}$ but bounded by $V_{\max}$, so the bound saturates rather than blowing up. Combined with the matching lower bound, this means the BH form has nontrivial content across the entire range of policy distributions $Q$ with $Q(a^*) > 0$, while Pinsker is silent for half of that range.

A worked numerical comparison appears in Appendix B.

### 4.5  Where Pinsker is the right tool

The BH identity holds *exactly* only under deterministic $\pi^*$. When $\pi^*$ is stochastic — for example, under softmax-smoothing for differentiability or in tied-optimum settings where multiple actions achieve the optimum — the identity degrades back to the BH inequality $\operatorname{TV} \le \sqrt{1 - e^{-D_{\mathrm{KL}}}}$, no longer an equality. Pinsker is the textbook fallback for general distributions, and the right tool for stochastic-$\pi^*$ extensions.

The deterministic-$\pi^*$ scope is canonical for RL with discrete action spaces and isolated optima — the regime in which the optimal policy is a point mass on $a^*$. This includes finite-MDP RL with unique optimal action per state, the regime of [Lattimore-Szepesvári 2020] decision-theoretic analysis, and most theoretical work in non-stationary MDPs. Tied-optimum extensions (Appendix A) handle the case where $\pi^*$ has support on a tied-optimum set $\mathcal A^* = \{a : Q_O(a) = Q_O(a^*)\}$.

### 4.6  Direction of the divergence is forced

The KL direction in Theorems 4.1–4.2 is *reverse-KL* — $D_{\mathrm{KL}}(\pi^* \,\|\, Q)$, with the optimum first. This is forced by the deterministic-$\pi^*$ scope: forward-KL $D_{\mathrm{KL}}(Q \,\|\, \pi^*)$ is $+\infty$ whenever $Q$ has any mass off $a^*$, since $\pi^*(a) = 0$ for $a \neq a^*$ makes the summand $Q(a) \log(Q(a)/0) = +\infty$. A bound "$R \le +\infty$" is vacuous. The reverse direction is the only non-vacuous form under deterministic $\pi^*$.

Within the reverse direction, multiple $f$-divergences yield valid regret bounds (Appendix A surveys $\chi^2$, Rényi-$\alpha$, Hellinger). Reverse-KL is *uniquely* selected within this family by the chain-rule additivity axiom [Hobson 1969; Csiszár 1991]: it is the only smooth $f$-divergence whose value on a joint distribution decomposes additively over a factorization. The chain-rule axiom is a standard structural commitment in the decision-theoretic and information-geometric literature; we invoke it as an external axiom and refer to Appendix C for the functional-equation derivation.

### 4.7  Position within the information-theoretic RL literature

Information-theoretic regret bounds in RL form a substantial line: [Russo-Van Roy 2014a] entropy-of-optimal-action bound for Thompson sampling; [Russo-Van Roy 2014b] information-directed sampling; [Lu-Van Roy 2019] information-theoretic confidence bounds; [Min-Russo 2023] non-stationary entropy-rate bounds; [Lattimore-György 2020] mirror descent and information ratio; [Kakade-Krishnamurthy-Lowrey-Ohnishi-Sun 2020] information-theoretic regret for nonlinear control. None of these deploys the Bretagnolle--Huber inequality, much less its deterministic-$\pi^*$ exact-equality form. They use Shannon entropy, mutual information, information ratio, Pinsker, or Hellinger as the connective tissue between the regret quantity and the divergence on policy space.

The negative signal from the Undermind retrieval (63 papers; abstract-level coverage of 75% in the relevant literature; full search documented in supplementary materials) is robust: BH does not appear in the retrieved RL/non-stationary corpus. The deterministic-$\pi^*$ exact-equality form is, to our knowledge, novel to this paper.


---

## 5  Component 3 — Strategic Tempo and the Forgetting Prerequisite

### 5.1  Strategic tempo

The agent's rate of *useful* policy revision is a structural quantity, not a tunable hyperparameter. In a non-stationary environment, this rate must keep pace with the rate at which the environment invalidates the agent's existing policy. We make this precise.

Consider a structured policy with $|E|$ revisable elements (in our analysis, edges of an internal causal model the agent maintains over policy components; in [Lee et al.\ 2023]'s ProST, sub-policies indexed by a tempo schedule). Index by $(i, j)$. For each element, three quantities are relevant:
- $\nu_{ij}$ — the effective observation rate at which the agent receives evidence about element $(i, j)$.
- $\eta_{\mathrm{edge}, ij}$ — the per-element update gain (how much the agent revises element $(i, j)$ per unit of evidence).
- $\iota_{ij} \in [0, 1]$ — an identifiability coefficient: the fraction of the evidence stream that genuinely identifies the element's causal effect, rather than reflecting confounded association.

**Strategic tempo.**
$$\mathcal T_\Sigma \;:=\; \sum_{(i,j) \in E} \nu_{ij} \cdot \eta_{\mathrm{edge}, ij} \cdot \iota_{ij}.$$

The product structure factors three distinct considerations: how often evidence arrives ($\nu$), how informative each piece of evidence is for revising the element ($\eta$), and how *identifiable* the element is from the evidence ($\iota$). The $\iota$ factor captures the regime distinction discussed in Section 6: in intervention-rich regimes (Regime A; software, controlled experiments) $\iota \approx 1$ and the strategic tempo runs at full rate; in observation-only regimes (Regime C) $\iota \approx 0$ and elements contribute negligibly to $\mathcal T_\Sigma$ regardless of how fast the agent acts.

The structural parallel with adaptive (epistemic) tempo $\mathcal T = \sum_k \nu^{(k)} \eta^{(k)*}$ from adaptive control [Khalil 2002] is exact at $\iota = 1$. The $\iota$ factor is what distinguishes strategic from epistemic tempo: an agent can have high observation rate and high update gain, but if the observations don't identify the policy element's causal effect, the policy element doesn't actually improve.

### 5.2  Persistence condition for strategy

The persistence condition for an adaptive system is the structural inequality $\alpha > \rho / R$, where $\alpha$ is the correction rate, $\rho$ is the disturbance rate, and $R$ is the reserve [Khalil 2002 Ch.\ 4 and 9; Khasminskii 2012]. The condition is a Lyapunov-derived sufficient condition for ultimate boundedness of mismatch under sector-bounded correction and bounded disturbance. We apply this to the strategic substate.

Let $\delta_\Sigma$ denote the policy-mismatch state — the gap between the agent's current policy and the best available policy revision (operationally: the strategic-calibration residual; for analysis here, a scalar mismatch with the same Lyapunov structure as the epistemic case). Let $\rho_\Sigma$ be the disturbance rate at which the environment invalidates the agent's policy (a domain parameter, structurally analogous to environmental change rate $\rho$ in the epistemic case). Let $R_\Sigma$ be the strategic reserve — the maximum policy mismatch the correction machinery can absorb before saturation.

Under sector-bounded strategic correction with parameter $\alpha_\Sigma$, the persistence condition for $\Sigma$ is the direct instantiation of the template:
$$\alpha_\Sigma \;>\; \rho_\Sigma / R_\Sigma. \tag{P}$$

### 5.3  The forgetting prerequisite

Condition (P) is an *instantaneous* check at the current operating point of the agent. For Bayesian-style policy updates (e.g., Beta-Bernoulli edge updates in a structured policy DAG, or any update mechanism whose effective sample size grows with experience), the sector parameter has the form
$$\alpha_\Sigma \;=\; 1 / (n + 1),$$
where $n$ is the accumulated experience (pseudo-count). Without a forgetting mechanism, $n$ grows monotonically with each observation, so $\alpha_\Sigma \to 0$ asymptotically. *For any fixed* $(\rho_\Sigma, R_\Sigma)$ *with* $\rho_\Sigma > 0$, *every agent eventually violates condition (P).*

This is structural failure, not a tuning problem. The agent's prior calibration cannot help: at any finite calibration level, the correction rate decays below threshold once enough experience accumulates.

The standard fix is exponential forgetting: at each step, shrink pseudo-counts by a factor $\lambda \in (0, 1)$ before incorporating new evidence,
$$\alpha_k \mapsto \lambda \alpha_k + y_k, \qquad \beta_k \mapsto \lambda \beta_k + (1 - y_k).$$
The effective sample size stabilizes at $n_{\mathrm{eff}} \approx 1/(1-\lambda)$, giving steady-state sector parameter
$$\alpha_\Sigma^{\mathrm{ss}} \;\approx\; 1 - \lambda.$$

**Theorem 5.1 (Forgetting prerequisite for strategic persistence).** *Under the policy-update-with-forgetting dynamics above and conditions (T1)–(T3) of [Khalil 2002 §9] applied to the strategic substate, the strategy persists in the long run if and only if*
$$\boxed{\;(1 - \lambda) \;>\; \rho_\Sigma / R_\Sigma.\;}$$

The right-hand side is environment-side (disturbance rate, reserve); the left-hand side is the agent's evidence-discount rate. The forgetting prerequisite asserts that for the agent to persist, *the rate at which it discounts old evidence must exceed the rate at which the environment invalidates its policy*.

### 5.4  Structural, not heuristic

The forgetting prerequisite is a *structural threshold* on the agent–environment pairing, not a tuning hyperparameter. Three structural consequences:

- **Asymptotic failure under fixed memory.** With $\lambda = 1$ (no forgetting; complete memory), no positive $\rho_\Sigma$ is sustainable: the agent's effective tempo decays to zero with experience.
- **Sharpness of the threshold.** When $(1-\lambda) < \rho_\Sigma / R_\Sigma$, mismatch grows unbounded; when $(1-\lambda) > \rho_\Sigma / R_\Sigma$, mismatch is bounded at $R_\Sigma^* = \rho_\Sigma / \alpha_\Sigma^{\mathrm{ss}}$. The transition is qualitative, not gradual — the standard Lyapunov phase transition.
- **No prior-learning subsidy.** No amount of pre-deployment calibration relaxes (P): the threshold is on the *steady-state* correction rate under continual operation, not on initial accuracy.

### 5.5  Lifting [Lee et al.\ 2023] from hyperparameter to structural threshold

[Lee-Jin-Lavaei-Sojoudi 2023] (the ProST framework, NeurIPS 2023) is the closest published neighbor. ProST defines an *agent tempo* — the schedule of policy update times $\{t_1, \dots, t_K\}$ — and computes the schedule that minimizes the dynamic regret upper bound under non-stationarity. The companion paper [Lee-Jin-Lavaei-Sojoudi 2024] (Pausing Policy Learning, ICML 2024) shows that *non-zero policy hold duration* yields sharper dynamic regret. Together, they establish tempo as a convergence-relevant variable in non-stationary RL.

Our forgetting prerequisite *lifts* the ProST move from a hyperparameter-optimization claim to a structural-threshold claim. ProST asks: *given* an environment, *what tempo schedule* minimizes regret? The forgetting prerequisite asks: *given* an environment with disturbance $\rho_\Sigma$ and reserve $R_\Sigma$, *what is the minimal forgetting rate* without which no schedule persists? The two questions are complementary. ProST's optimal-schedule result is silent about the *threshold* below which no schedule works; the forgetting prerequisite identifies that threshold as $(1-\lambda) = \rho_\Sigma / R_\Sigma$.

Concretely, in the Lee et al.\ ProST setup, their tempo schedule corresponds to an effective forgetting rate $1 - \lambda_{\mathrm{eff}}$ that depends on the schedule's update frequency. When $1 - \lambda_{\mathrm{eff}} > \rho_\Sigma / R_\Sigma$, ProST's schedule satisfies the forgetting prerequisite and the dynamic-regret upper bound is achieved. When $1 - \lambda_{\mathrm{eff}} < \rho_\Sigma / R_\Sigma$, the schedule fails the prerequisite and the dynamic regret grows unbounded regardless of the optimization. Section 8 develops this reduction in detail.

### 5.6  Distinction from sliding-window and weighted-LSVI forgetting

Forgetting mechanisms appear throughout the non-stationary-RL literature. [Garivier-Moulines 2008] uses sliding-window UCB in non-stationary bandits; [Touati-Vincent 2020] uses exponential-weight LSVI (OPT-WLSVI) in non-stationary linear MDPs; [Russac-Vernade-Cappé 2019] uses forgetting-factor estimators in weighted linear bandits; [Cheung-Simchi-Levi-Zhu 2020] uses sliding-window UCB with confidence widening in non-stationary MDPs.

In each of these, forgetting appears as an *algorithmic mechanism* with a tunable parameter (window size, discount factor) chosen to optimize a dynamic-regret bound. The forgetting prerequisite reframes the same machinery: it identifies the inequality $(1-\lambda) > \rho_\Sigma/R_\Sigma$ as a *structural survival condition*, with environment-side parameters $(\rho_\Sigma, R_\Sigma)$ on the right-hand side and the algorithm's discount rate on the left. This is the strategic analog of the persistence condition $\alpha > \rho / R$ from adaptive control [Khalil 2002], with $(1 - \lambda)$ playing the role of adaptive tempo.

The reframe matters: dynamic-regret-optimization analyses leave open the possibility that *some* schedule works for *every* environment. The forgetting prerequisite shows that this is false — there are environment regimes (any with $\rho_\Sigma / R_\Sigma > 1$) for which no schedule with $\lambda \in (0, 1)$ persists. The threshold form makes the *existence* of such regimes explicit and the *failure mode* identifiable.

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

- **Regime A (intervention-rich).** Software-test domains, controlled laboratory experiments. $\iota \approx 1$. The loop data identifies $do$-effects cleanly; the BH bound is realizable on-policy.
- **Regime B (partial intervention).** Organizational decision-making with concurrent unobserved effects, mixed observation-intervention regimes. $\iota \in (0, 1)$. The loop data is partially identified; the BH bound holds *for the model the agent identifies*, with bias controlled by $1 - \iota$.
- **Regime C (observation-only).** Passive-monitoring scenarios, intelligence analysis. $\iota \approx 0$. The loop yields little usable interventional information; the BH bound is provable analytically but not realizable on-policy. Composite-extension treatment via observer-on-sub-agent interventions [as in adaptive-trial designs] can recover identifiability in some Regime-C subcases.

### 6.4  Closing the gap: the bound as learnable

The composition of Components 2 and 4 is what makes the regret bound *learnable*. The BH bound (Section 4) is a tight analytic relationship between regret and the reverse-KL coordinate on policy space. The closed-loop interventional access (this section) supplies the data substrate from which the agent's $\pi^*$ — and therefore the bound's right-hand side — can be empirically estimated under sufficient identifiability. Without component 2, the loop yields data with no metric structure on policy space. Without component 4, the metric structure is provable but not usable on-policy.

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
2. **Has explicit metric structure on policy space** via the Bretagnolle--Huber identity (Component 2).
3. **Is learnable on-policy** via closed-loop interventional access (Component 4).

The two-gap diagnostic (Component 1) is the connective tissue that routes regret-signal interpretation: it tells the agent *which* corrective action the regret signal warrants, and therefore *which* of the three properties must be invoked at any given moment.

### 7.1  Composition theorem (assembly form)

**Theorem 7.1 (Unified convergence under non-stationarity).** *Let $(\mathcal S, \mathcal A, P_t, r_t, N_h)$ be a non-stationary MDP with bounded reward, finite action space, deterministic optimum policy $\pi^*_t$ at each round, and isolated optima with action gap $\Delta_{\min} > 0$. Suppose the agent operates on a singular causal trajectory in the sense of Section 2 and updates its policy with exponential forgetting at rate $1 - \lambda$. If*

> **(A1) Metric on policy space.** *The agent's policy $Q_t$ satisfies the canonical scope of Section 4: $Q_t(a^*) > 0$ at every round.*

> **(A2) Forgetting prerequisite.** *The discount rate $1 - \lambda$ exceeds the disturbance-to-reserve ratio $\rho_\Sigma / R_\Sigma$.*

> **(A3) Identifiability regime.** *The agent operates in Regime A or Regime B with identifiability coefficient $\iota$.*

> **(A4) Two-gap diagnostic.** *The agent applies the satisfaction-gap / control-regret 2$\times$2 to route corrective action.*

*Then:*

*(i) Per-round regret is two-sided BH-bounded:*
$$\Delta_{\min}\bigl(1 - e^{-D_{\mathrm{KL}}(\pi^*_t \,\|\, Q_t)}\bigr) \;\le\; R(Q_t) \;\le\; V_{\max}\bigl(1 - e^{-D_{\mathrm{KL}}(\pi^*_t \,\|\, Q_t)}\bigr).$$

*(ii) Strategic mismatch $\delta_\Sigma$ is ultimately bounded under non-stationarity, with steady-state bound $R_\Sigma^* = \rho_\Sigma / (1 - \lambda)$.*

*(iii) The KL coordinate $D_{\mathrm{KL}}(\pi^*_t \,\|\, Q_t)$ is estimable from on-policy data with error scaling as $1 - \iota$ (Regime A: zero bias; Regime B: bias proportional to confounding).*

*(iv) The 2$\times$2 cell containing $(\delta_{\mathrm{sat}}, \delta_{\mathrm{regret}})$ identifies the corrective action class: revise policy (regret-driven), revise model/policy-class/horizon (capability-driven), or revise objective (last resort).*

The proof is the composition of the four component theorems: Theorem 4.2 gives (i); Theorem 5.1 gives (ii); Theorem 6.1 plus the regime-A/B identification analysis gives (iii); the 2$\times$2 disambiguation of Section 3 gives (iv).

### 7.2  What is new about the composition

Each individual conclusion of Theorem 7.1 has a published precedent. The novelty is in the joint statement and in the *closure*: the four components together close a story none of the strands closes individually.

- The dynamic-regret literature [Cheung-Simchi-Levi-Zhu 2020; Wei-Luo 2021] has (ii) but not (i): no metric structure on policy space, no two-sided regret bound at the level of action distributions.
- The information-theoretic regret literature [Russo-Van Roy 2014a, 2014b; Lu-Van Roy 2019; Min-Russo 2023] has (i) at the level of mutual-information bounds, but uses Pinsker or Hellinger rather than the BH identity, and is non-stationary only in [Min-Russo 2023] — without (ii)'s sharp threshold form.
- The causal-RL literature [Zhang-Bareinboim 2022; Wang-Yang-Wang 2020; Lu-Meisami-Tewari 2021] has (iii), but is stationary — without (ii).
- The two-term decomposition literature [Long-Fei Li-Zhao-Zhou 2024; Fei-Yang-Wang-Xie 2020] decomposes along the exploration-vs-adaptation axis but not along the goal-vs-policy axis (iv).
- The tempo literature [Lee et al.\ 2023, 2024; Touati-Vincent 2020] has (ii)'s rate-of-update component but without the threshold form: their tempo is a hyperparameter to be optimized, not a structural inequality the agent must satisfy.

### 7.3  Assembly, not a new derivation step

The composition theorem is positioned honestly as *assembly and interpretation*, not a new derivation step. Each of (i)–(iv) is a theorem with a published or directly-derived ancestor. The contribution is the recognition that the four together hold simultaneously in the non-stationary regime under conditions (A1)–(A4), and that the resulting joint statement covers properties 1–3 of §7's opening.

One way to see why this composition is non-obvious: each strand has internal reasons not to need the others. The dynamic-regret literature treats policy-space metric structure as orthogonal to its variation-budget machinery. The information-theoretic literature treats non-stationarity as orthogonal to its information-ratio machinery. The causal-RL literature treats non-stationarity as a separate layer outside its identification machinery. The composition cuts orthogonally across all three: each strand's machinery is *necessary* for one of properties 1–3, and *insufficient on its own*. The two-gap diagnostic is what makes the joint statement actionable rather than merely true.

### 7.4  Honest scope

The composition holds in the canonical scope of Section 2: deterministic $\pi^*$, bounded value, isolated optimum, singular trajectory, finite horizon. Outside this scope:

- Stochastic $\pi^*$ (softmax-smoothed or tied-optimum) replaces the BH identity with the BH inequality; (i) becomes one-sided. Pinsker is the appropriate fallback. [Appendix A.5 sketches the extension.]
- Unbounded value range makes $V_{\max} = \infty$ and the upper bound trivial. The lower bound via $\Delta_{\min}$ remains informative.
- Non-isolated optima ($\Delta_{\min} = 0$) eliminate the lower bound but preserve the upper.
- Non-singular trajectories (type-like or parallel-copy agents) require additional machinery; the loop-Level-2 argument depends on singularity.

The composition is robust within its scope but degrades cleanly on each axis. We do not claim a uniqueness result — that *every* convergence theory satisfying properties 1–3 must reduce to this composition. The right uniqueness statement (if one exists) likely requires further structural axioms; we leave it to future work.


---

## 8  Worked Example: Reduction to ProST [Lee et al.\ 2023]

The cleanest worked example is the reduction of [Lee-Jin-Lavaei-Sojoudi 2023]'s ProST framework to a special case of Theorem 7.1, providing empirical grounding for the strategic-tempo claim through ProST's published experiments.

### 8.1  ProST as a special case

ProST considers a non-stationary MDP with bounded variation in reward and transition, and parameterizes the agent's policy update by a *tempo schedule* $\{t_1, t_2, \dots, t_K\}$ — the times at which the policy is updated using accumulated experience. Between update times, the policy is held fixed. The dynamic regret is bounded as a function of (a) the schedule, (b) the variation budget, and (c) the stationary-MDP regret of the base learner. ProST optimizes the schedule to minimize this upper bound.

We map ProST to Theorem 7.1 as follows.

**Forgetting rate ↔ tempo schedule density.** ProST's tempo schedule corresponds to an effective forgetting rate. If the schedule has $K$ updates over $T$ rounds, each update carries weight $\sim 1/K$ relative to past evidence. The corresponding effective discount factor is $\lambda_{\mathrm{eff}} \approx 1 - K/T$, giving forgetting rate $1 - \lambda_{\mathrm{eff}} \approx K/T$.

**ProST's optimal schedule satisfies the forgetting prerequisite *when the regret bound is non-vacuous*.** ProST's dynamic-regret upper bound is sublinear in $T$ when the schedule is chosen such that the per-update bias from finite-window estimation is balanced against the per-round adaptation cost. This balance corresponds, in our framework, to $1 - \lambda_{\mathrm{eff}} > \rho_\Sigma / R_\Sigma$ — the forgetting prerequisite. When $\rho_\Sigma / R_\Sigma$ exceeds the threshold determined by the variation budget, ProST's optimization yields a schedule that satisfies (A2); below the threshold, ProST's bound becomes vacuous.

**Regime A identifiability.** ProST operates on standard MDPs with full state observability; the loop data is Regime A ($\iota \approx 1$). The BH bound holds with the identifications (A1) and (A3) of Theorem 7.1 satisfied.

**Goal-feasibility-vs-policy-quality 2$\times$2.** ProST's setup assumes the goal is feasible (the optimal policy in the stationary case achieves bounded reward); $\delta_{\mathrm{sat}} = 0$ throughout. The 2$\times$2 reduces to two cells: success and strategy problem. The strategy-problem cell is what ProST optimizes against.

### 8.2  Empirical grounding via ProST experiments

ProST is validated empirically on continuous-control tasks with non-stationary dynamics. The dynamic-regret bound's predictions are confirmed: their schedule outperforms baselines that update at fixed frequency, and the optimal schedule density tracks the rate of environmental change. Under the reduction above, ProST's empirical results validate two of our four components: the strategic tempo (Component 3) and its lifting to a forgetting prerequisite (Section 5.5).

The remaining components are not directly tested by ProST. Component 2 (BH identity) is mathematically airtight by Theorem 4.1 — a one-line specialization of [Bretagnolle-Huber 1978]. Component 4 (closed-loop interventional access) is implicit in ProST's setup (their training data is on-policy interventional under Regime A) but not explicitly invoked in their analysis. Component 1 (two-gap diagnostic) is trivially satisfied in their setup ($\delta_{\mathrm{sat}} = 0$) and therefore not exercised; its diagnostic content arises in cases where goal feasibility itself is in question, which ProST's setup does not exhibit.

### 8.3  Variation-budget instantiation

A second worked example is the [Cheung-Simchi-Levi-Zhu 2020] variation-budget framework. Their sliding-window UCB with confidence widening (SWUCRL2-CW) corresponds to a fixed forgetting rate $1 - \lambda = 1/W$ where $W$ is the window length. Their dynamic-regret bound depends on $W$ being chosen against the variation budget $B_T$ in a specific way; under our framework, this choice is exactly the forgetting prerequisite $(1 - \lambda) > \rho_\Sigma / R_\Sigma$ instantiated with $\rho_\Sigma$ proportional to $B_T / T$. The same analysis applies to [Wei-Luo 2021]'s black-box reduction (which adaptively tunes $W$) and to [Mao et al.\ 2021]'s RestartQ-UCB (which periodically restarts, equivalent to abrupt forgetting at restart times).

### 8.4  What the worked examples don't yet cover

We have not worked through reductions to: the causal-RL line [Zhang-Bareinboim 2022; Lu-Meisami-Tewari 2021] (would primarily exercise Component 4); the satisficing line [Hajiabolhassan-Ortner 2025; Zhang-Zhu-Xie 2026] (would exercise Component 1 with $\delta_{\mathrm{sat}} > 0$); or the dynamic-regret-with-causal-knowledge composition (which our framework predicts is sharper than either lineage alone, but which we leave as future work).

---

## 9  Related Work

The four-strand structure of the prior art organizes the related-work analysis cleanly.

### 9.1  Strand 1 — Dynamic regret under drift

[Cheung-Simchi-Levi-Zhu 2020] is the canonical non-stationary RL paper, introducing sliding-window UCB with confidence widening (SWUCRL2-CW) and the variation-budget formalism. [Wei-Luo 2021] gives an optimal black-box reduction yielding $\widetilde{\mathcal O}(\min\{\sqrt{LT}, \Delta^{1/3} T^{2/3}\})$ dynamic regret without prior knowledge of the variation budget. [Mao-Zhang-Zhu-Simchi-Levi-Başar 2021] introduces RestartQ-UCB with matching upper and lower bounds in non-stationary episodic MDPs. [Cheung-Simchi-Levi-Zhu 2018, 2019] (Hedging the Drift) develops the bandit-over-bandit framework. [Gajane-Ortner-Auer 2019] gives the first variational regret bound for general RL.

Our framework recovers these as instances of the strategic-tempo + forgetting prerequisite (Component 3, Section 8.3): each defines a forgetting mechanism whose discount rate must exceed $\rho_\Sigma / R_\Sigma$ for the bound to be non-vacuous. The novelty is the *threshold form* — the existence of environment regimes where no schedule persists — which dynamic-regret-optimization analyses do not surface.

### 9.2  Strand 2 — Two-term regret decompositions

[Long-Fei Li-Zhao-Zhou 2024] decompose dynamic regret of adversarial MDPs with unknown transition into two terms: one due to confidence-set construction (transition uncertainty), one due to suboptimal policy choice under non-stationarity. [Fei-Yang-Wang-Xie 2020]'s POWER and POWER++ algorithms achieve dynamic regret with explicit two-component decomposition (exploration + adaptation), the first model-free dynamic-regret analysis in non-stationary RL. [Stradi-Lunghi-Castiglioni-Marchesi-Gatti 2024] gives an analogous decomposition in non-stationary CMDPs with sublinear regret and positive constraint violation.

These are the closest structural neighbors of our two-gap diagnostic. The decomposition shape (two additive terms) is similar; the *axis* differs. Our axis is goal-feasibility vs.\ policy-quality; theirs is uncertainty-source. Both are valid; neither subsumes the other. A full analysis of the relationship is in Appendix A.

[Yang-Zheng-Tomizuka-Liu-Li 2024] presents a feasibility theory of constrained RL, distinguishing virtual-time and real-time feasibility. Their "feasibility" is constraint-region feasibility (state stays inside the constraint set); ours is goal feasibility (objective threshold is attainable). The vocabulary overlap requires careful disambiguation; the formal objects are distinct.

### 9.3  Strand 3 — Tempo and forgetting

[Lee-Jin-Lavaei-Sojoudi 2023] (ProST framework, NeurIPS 2023) is the closest single neighbor for Component 3. They explicitly compute a tempo schedule minimizing dynamic-regret upper bound, showing the trade-off between agent tempo and environment tempo. [Lee-Jin-Lavaei-Sojoudi 2024] (Pausing Policy Learning, ICML 2024) shows non-zero policy hold duration sharpens dynamic regret. Our forgetting prerequisite *lifts* their tempo result from a hyperparameter-optimization claim to a structural-threshold claim (Section 5.5).

[Touati-Vincent 2020] (OPT-WLSVI) uses exponential-weight forgetting in non-stationary linear MDPs — the closest exponential-forgetting RL ancestor. [Russac-Vernade-Cappé 2019] (weighted linear bandits) and [Garivier-Moulines 2008] (sliding-window UCB for non-stationary bandits) are the bandit ancestors.

### 9.4  Strand 4 — Causal and interventional access

[Zhang-Bareinboim 2022] (online RL for mixed policy scopes, NeurIPS 2022) is the closest causal-RL neighbor. [Zhang-Bareinboim 2016] (MDPs with unobserved confounders) formalizes the interventional-vs-observational distinction. [Lu-Meisami-Tewari 2021, 2022] (Causal MDPs / C-UCBVI) gives regret scaling on a causal-graph-dependent quantity, exponentially smaller than the action space. [Wang-Yang-Wang 2020] (DOVI) explicitly adjusts for confounding bias with strict regret improvement when observational data is informative. [Junzhe Zhang 2020] applies causal RL to dynamic treatment regimes. [Schulte-Poupart 2025] is the most recent meta-analysis of when causal structure helps RL.

The gap: none of these compose with non-stationarity. The dynamic-regret line is non-causal; the causal line is stationary. Our composition is, to our knowledge, the first to combine the two.

### 9.5  Cross-cutting — Information-theoretic regret bounds

The information-theoretic regret line [Russo-Van Roy 2014a (Thompson), 2014b (IDS); Lu-Van Roy 2019 (IT confidence bounds); Min-Russo 2023 (non-stationary entropy-rate bounds); Lattimore-György 2020 (mirror descent + information ratio)] uses Shannon entropy, mutual information, information ratio, Pinsker, or Hellinger to bound regret. [Canonne 2022] gives an inequality between KL and TV adjacent to the BH form but as a general statistical-distance result, not RL.

The Bretagnolle--Huber identity is conspicuously absent from this line. The retrieval study (Appendix D / supplementary materials) confirms BH does not appear in the retrieved RL/non-stationary corpus. The deterministic-$\pi^*$ exact-equality form is, to our knowledge, novel to this paper.

### 9.6  Adjacent — Satisficing and feasibility

[Hajiabolhassan-Ortner 2025] (online regret bounds for satisficing in MDPs, *Math.\ Operations Research*) gives constant regret with respect to a satisfaction level $\beta$. [Y. Zhang-Zhu-Xie 2026] (March 2026, contemporaneous per the NeurIPS contemporaneous-work cutoff) gives the most recent satisficing-vs-non-stationarity result. These have vocabulary overlap with our $\delta_{\mathrm{sat}}$ but a different formal axis: their satisficing is "any policy above level $\beta$ is acceptable"; our $\delta_{\mathrm{sat}}$ is "the goal is unmet from $M_t$." Careful disambiguation is in Section 3.

[Yang-Zheng-Tomizuka-Liu-Li 2024] (already noted in §9.2) gives a constraint-region-feasibility theory. Vocabulary-similar, structurally distant.

### 9.7  Contemporaneous work

Per the NeurIPS 2026 main-track cutoff, papers online after March 1, 2026 are contemporaneous. We cite without empirical comparison: [DARLING (Gerogiannis-Huang-Veeravalli 2026)] — detection-augmented RL with non-stationary guarantees; [Y. Zhang-Zhu-Xie 2026] (already discussed). Both are adjacent to our framework but neither composes the four components we identify.

---

## 10  Limitations and Conclusion

### 10.1  Limitations

**Theory-only.** The paper presents no original experiments. Mitigations: (i) the BH identity (Theorem 4.1) is a one-line specialization of [Bretagnolle-Huber 1978] under deterministic $\pi^*$ — mathematically airtight. (ii) The reduction to [Lee et al.\ 2023] ProST (Section 8) provides empirical grounding for the strategic-tempo claim through ProST's published experiments. (iii) The composition theorem (Theorem 7.1) is positioned honestly as assembly + interpretation, not a derivation step requiring its own validation.

**Canonical scope.** The deterministic-$\pi^*$ assumption is essential for the BH *identity* (as opposed to inequality). Stochastic $\pi^*$ — under softmax smoothing for differentiability, or in tied-optimum regimes — degrades the identity to the textbook BH inequality. Pinsker remains the correct fallback in this regime; the regret bound becomes one-sided. Extensions are sketched in Appendix A but not developed here.

**No uniqueness result for the composition.** We claim that no existing framework composes all four components in the non-stationary regime, supported by the prior-art retrieval (Appendix D). We do *not* claim that any non-stationary convergence theory satisfying the three properties (handles non-stationarity, has policy-space metric, is on-policy learnable) must reduce to our composition. A uniqueness theorem of this shape would likely require further structural axioms (e.g., chain-rule additivity for the metric, a singular-trajectory commitment for interventional access) that we leave to future work.

**Reverse-KL chain-rule axiom.** The selection of reverse-KL as the canonical divergence in Component 2 rests on a chain-rule additivity axiom [Hobson 1969; Csiszár 1991]. The axiom is standard but is not derived from prior commitments of the framework; it is invoked as an external structural commitment.

**Strategic-disturbance parameter $\rho_\Sigma$ is a domain quantity.** The forgetting prerequisite is sharp given $\rho_\Sigma$ and $R_\Sigma$, but estimating these from data is a domain-specific problem we do not address. The structural content is the *form* of the threshold, not its numerical value in any specific deployment.

**Class 2 architectural scope.** Theorem 6.1's loop-as-Level-2-engine claim depends on a directed-separation property between the agent's model state and goal state. For agents where these are coupled (e.g., goal-conditioned LLM policies where the goal is part of the model's prompt context), the closed-loop interventional access argument requires additional machinery. We treat this as out of scope; for related discussion see [Bruineberg et al.\ 2022].

### 10.2  Future work

- **Stochastic-$\pi^*$ extension.** Soft-max smoothing of the optimum policy with temperature $\tau \to 0$ recovers the deterministic limit; the BH inequality bound is one-sided in this regime, with explicit dependence on $\tau$.
- **Tied-optimum extension.** Direct (Appendix A.5).
- **Class-2 extension.** Coupling of $M_t$ and $G_t$ degrades the loop-Level-2 argument. The right machinery is likely a Bayesian inverse-problem stability analysis [Stuart 2010; Hosseini-Hsu-Taghvaei 2023] — adjacent regularity theory we believe applies but have not developed.
- **Algorithmic instantiation.** The composition theorem is structural; a practical algorithm achieving Theorem 7.1's joint guarantees would require (a) a base learner with BH-tight regret in stationary regimes, (b) explicit exponential-forgetting schedule satisfying the prerequisite, (c) a Regime-A or Regime-B identifiability check, (d) a 2$\times$2 diagnostic readout for corrective action selection. We sketch such an algorithm in Appendix E but defer empirical evaluation to a follow-up paper.

### 10.3  Conclusion

Non-stationary RL has matured along four parallel tracks — dynamic regret under drift, two-term decompositions, tempo and forgetting analyses, and causal interventional access. None has been composed with the others. We assemble the composition with four components: a two-gap diagnostic separating goal feasibility from policy quality; a Bretagnolle--Huber identity giving a tight two-sided regret bound under deterministic optimum; a strategic-tempo forgetting prerequisite as structural survival inequality; and a closed-loop interventional access argument grounded in Pearl's causal hierarchy. The composition is positioned as assembly + interpretation: each component is a theorem (cited or proved); the contribution is the recognition that the four together close a story none closes individually. The Bretagnolle--Huber identity for RL regret, in its deterministic-$\pi^*$ exact-equality form, is the technical anchor — strictly improving Pinsker and absent, to our knowledge, from the prior RL/non-stationary corpus.


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
| $D_{\mathrm{KL}}(\pi^* \,\|\, Q)$ via BH | $V_{\max}(1 - e^{-D_{\mathrm{KL}}})$ | Tight (this paper) | Yes |
| $\chi^2(\pi^* \,\|\, Q)$ (Le Cam) | $V_{\max} \cdot \tfrac12 \sqrt{\chi^2}$ | Looser than Pinsker | $\chi^2 = 1/Q(a^*) - 1$ |
| $D_\alpha(\pi^* \,\|\, Q)$ (Rényi, $\alpha \ge 1$) | Various | Interpolates | Yes for $\alpha \ge 1$ |
| $D_{\mathrm{KL}}(Q \,\|\, \pi^*)$ (forward-KL) | $+\infty$ | Vacuous | No |

Reverse-KL is selected uniquely within the direction-forced family by the chain-rule axiom (Appendix C). The BH form is the *bound* applied to reverse-KL; the table reflects different bound shapes on the same divergence.

### A.3  Direction-forcing argument

For deterministic $\pi^* = \delta_{a^*}$ and any $Q$ with $Q(a) > 0$ for some $a \neq a^*$:
$$D_{\mathrm{KL}}(Q \,\|\, \pi^*) \;=\; \sum_a Q(a) \log\frac{Q(a)}{\pi^*(a)} \;=\; \sum_{a \neq a^*} Q(a) \log\frac{Q(a)}{0} \;=\; +\infty.$$
A bound "$R \le +\infty$" is vacuous. The reverse direction $D_{\mathrm{KL}}(\pi^* \,\|\, Q)$ is finite (and equal to $-\log Q(a^*)$) whenever $Q(a^*) > 0$. The asymmetry is forced by the regime: regret is a one-sided quantity (contributes only from $Q$'s off-optimum mass; $\pi^*$ has no support off $a^*$); divergences whose role is to bound this one-sided quantity must themselves be one-sided. Symmetric divergences (squared Hellinger, Jensen-Shannon, symmetrized KL) introduce a vacuous symmetric term.

### A.4  Action-gap matching lower bound

For any $Q$ with $\Delta_{\min} = \min_{a \neq a^*} \Delta(a) > 0$:
$$R(Q) \;=\; \sum_{a \neq a^*} Q(a) \Delta(a) \;\ge\; \Delta_{\min} \sum_{a \neq a^*} Q(a) \;=\; \Delta_{\min} \cdot (1 - Q(a^*)) \;=\; \Delta_{\min} \cdot \operatorname{TV}(\pi^*, Q).$$
By the BH identity, $\operatorname{TV}(\pi^*, Q) = 1 - e^{-D_{\mathrm{KL}}(\pi^* \,\|\, Q)}$, giving the matching lower bound of Theorem 4.2.

The lower bound is tight when sub-optimal actions are uniformly bad ($\Delta_{\min} = \max_{a \neq a^*} \Delta(a)$). For typical landscapes the gap between upper and lower bound is $V_{\max} - \Delta_{\min}$, controlled by the *spread* of action gaps.

### A.5  Tied-optimum and softmax-smoothed extensions

**Tied-optimum.** If $\pi^*$ has support on a tied-optimum set $\mathcal A^* = \{a : Q_O(a) = Q_O(a^*)\}$ with uniform mass, reverse-KL is finite whenever $Q$ covers $\mathcal A^*$. The regret bound extends:
$$R(Q) \;=\; \sum_{a \notin \mathcal A^*} Q(a) \Delta(a) \;\le\; V_{\max} \cdot \mathbb P_Q(a \notin \mathcal A^*).$$
Pinsker applies unchanged. The BH identity holds with $\pi^*(a^*) = 1/|\mathcal A^*|$ replacing the point-mass form, yielding $D_{\mathrm{KL}}(\pi^* \,\|\, Q) = -\sum_{a \in \mathcal A^*} (1/|\mathcal A^*|) \log(|\mathcal A^*| Q(a))$, a multi-action analog of $-\log Q(a^*)$. The two-sided bound becomes one-sided in general; the upper bound holds with the modified KL form.

**Softmax-smoothed $\pi^*$ with temperature $\tau \to 0$.** Replace $\pi^* = \delta_{a^*}$ with $\pi^*_\tau \propto \exp(Q_O / \tau)$. As $\tau \to 0$, $\pi^*_\tau \to \delta_{a^*}$. For finite $\tau$, the BH identity degrades to the BH inequality; Pinsker applies. The regret bound is one-sided with temperature-dependent constants; explicit form deferred to future work.

### A.6  Strategic-tempo consistency across canonical topologies

The strategic tempo $\mathcal T_\Sigma$ is verified consistent with sector-condition persistence across four canonical topologies under Beta-Bernoulli edge updates (full derivations in supplementary materials):

- **B.1 — Single edge.** $\mathcal T_\Sigma = \nu / (n+1)$. The sector parameter $\alpha_\Sigma = 1/(n+1)$ is the per-observation correction quality.
- **B.2 — Two-edge AND chain, observable intermediate.** $\mathcal T_\Sigma = \nu/(n_1+1) + \nu \theta_1/(n_2+1)$. Edge 2's rate is gated by edge 1's success $\theta_1$ — depth-gated attenuation.
- **B.3 — Two-edge AND chain, unobservable intermediate.** Per-edge tempo ill-defined; plan-level tempo $\mathcal T_{\Sigma, \mathrm{plan}} = \nu/(n_\Phi+1)$ with $\hat\Phi = p_1 p_2$ as a single tracked quantity.
- **B.4 — Two-arm OR node, $\varepsilon$-greedy.** $\mathcal T_\Sigma = \nu(1-\varepsilon)/(n_1+1) + \nu \varepsilon / (n_2+1)$. Action selection controls rate allocation — exploration-gated.

The structural decomposition: AND-chains exhibit depth-gated geometric attenuation $\nu_k = \nu \prod_{j < k} \theta_j$; OR-nodes exhibit exploration-gated allocation. Mixed AND/OR DAGs interleave both.

### A.7  Loop-as-causal-engine: three deployment modes

The Pearl-$do$ structure of closed-loop interventional access manifests at distinct layers with semantically distinct interventional mechanisms:

- **Mode 1 — agent-self-intervention.** The agent performs $do$-actions on its own action space as part of its ordinary adaptive loop. Intervention is on the agent's own action; target is the environment's response. This is Theorem 6.1's primary content.
- **Mode 2 — observer-on-sub-agent.** An observer external to a composite performs $do$-interventions on one sub-agent's action space; target is another sub-agent's response. Reveals cross-coupling structure that component-marginal observation cannot identify.
- **Mode 3 — observer-on-agent-input** (sketched, future work). An observer intervenes on the agent's observation channel; target is the agent's subsequent policy. Useful for offline architectural-class diagnosis.

Modes share the Pearl-$do$ structure but differ in who intervenes on what. The unification is at the pattern level; the mechanism is semantically distinct per layer.

---

## Appendix B — Pinsker vs.\ BH Numerical Comparison

We compare $V_{\max}(1 - e^{-D_{\mathrm{KL}}})$ (BH bound, exact under deterministic $\pi^*$) with $V_{\max}\sqrt{D_{\mathrm{KL}}/2}$ (Pinsker bound, also valid under deterministic $\pi^*$ but loose) and the trivial envelope $V_{\max}$. Set $V_{\max} = 1$ for normalization.

| $D_{\mathrm{KL}}$ | $1 - e^{-D_{\mathrm{KL}}}$ (BH) | $\sqrt{D_{\mathrm{KL}}/2}$ (Pinsker) | $\min(\sqrt{D_{\mathrm{KL}}/2}, 1)$ | Pinsker / BH ratio |
|---|---|---|---|---|
| 0.01 | 0.00995 | 0.0707 | 0.0707 | 7.10× |
| 0.1 | 0.0952 | 0.224 | 0.224 | 2.35× |
| 0.5 | 0.393 | 0.500 | 0.500 | 1.27× |
| 1.0 | 0.632 | 0.707 | 0.707 | 1.12× |
| 2.0 | 0.865 | 1.000 | 1.000 | 1.16× (Pinsker = trivial) |
| 4.0 | 0.982 | 1.414 | 1.000 | 1.02× (Pinsker vacuous) |
| 10.0 | 0.99995 | 2.236 | 1.000 | 1.00× (Pinsker fully vacuous) |

The BH bound is uniformly tighter than Pinsker, by a factor of $7\times$ at $D_{\mathrm{KL}} = 0.01$ and converging to $1$ as $D_{\mathrm{KL}}$ grows large (where both saturate at $V_{\max}$). Pinsker becomes vacuous (exceeds the trivial $V_{\max}$ envelope) for $D_{\mathrm{KL}} > 2$; the BH form remains informative up to arbitrary $D_{\mathrm{KL}}$ with smooth saturation at $V_{\max}$.

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

We conducted a structured prior-art retrieval (via Undermind) with the goal of identifying any framework composing all four target elements: (1) goal-feasibility-vs-policy-quality two-gap diagnostic; (2) Bretagnolle--Huber identity in regret analysis with deterministic-$\pi^*$ exact-equality form; (3) strategic-tempo / forgetting prerequisite tying convergence to rate of useful policy revision; (4) closed-loop interventional access making regret bounds learnable from on-policy data.

**Result: 63 papers retrieved, abstract-level coverage estimated 75%, no direct anticipation.** The landscape splits into four largely separate strands corresponding to our four components, with no published framework composing all four. The retrieval is documented in supplementary materials.

**Strongest negative signal: Bretagnolle--Huber identity is absent from the retrieved RL/non-stationary corpus.** The information-theoretic regret literature uses entropy, mutual information, information ratio, Pinsker, or Hellinger uniformly. The deterministic-$\pi^*$ exact-equality form is, to our knowledge, novel.

**Closest neighbors per strand:** [Cheung-Simchi-Levi-Zhu 2020] (Strand 1); [Long-Fei Li-Zhao-Zhou 2024] (Strand 2 — closest two-term decomposition, different axis); [Lee et al.\ 2023 ProST] (Strand 3 — closest tempo result, different form); [Zhang-Bareinboim 2022] (Strand 4 — closest causal-RL, stationary only).

---

## Appendix E — Sketch of an Algorithm Achieving Theorem 7.1

A practical algorithm achieving the joint guarantees of Theorem 7.1 requires:

**(a) Base learner with BH-tight regret in stationary regime.** A Thompson-sampling or upper-confidence-bound base learner whose regret in the stationary setting matches the BH form $V_{\max}(1 - e^{-D_{\mathrm{KL}}})$. [Russo-Van Roy 2014a]'s Thompson analysis and [Lattimore-Szepesvári 2020]'s UCB derivations provide the building blocks; specializing to deterministic $\pi^*$ gives the BH form by Theorem 4.2.

**(b) Exponential-forgetting schedule.** Discount old evidence at rate $1 - \lambda$ with $\lambda \in (0, 1)$ chosen such that $1 - \lambda > \rho_\Sigma / R_\Sigma$. When $\rho_\Sigma$ is unknown, the [Wei-Luo 2021] black-box reduction gives an adaptive choice; when $\rho_\Sigma$ is known via a variation-budget [Cheung et al.\ 2020], the choice is direct.

**(c) Identifiability check.** Test whether the loop data is in Regime A (full identifiability), Regime B (partial), or Regime C (none). In Regime A, no adjustment is needed. In Regime B, apply [Wang-Yang-Wang 2020 DOVI]-style confounding-bias adjustment. In Regime C, the bound is provable but not realizable; algorithm flags the regime as out-of-scope.

**(d) Two-gap diagnostic readout.** Compute $\delta_{\mathrm{sat}}$ (requires $A_O$, intractable in general; estimable via the policy-class supremum over recent rounds) and $\delta_{\mathrm{regret}}$ (requires $V_O$ at current policy; tractable in simulation). Route to corrective action class.

The algorithm is sketch-grade; full instantiation and empirical evaluation are deferred to a follow-up paper.

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

---

## Notes (Author working notes — to remove before submission)

**Decisions needed.**

- Title: three candidates listed at top; the third is most descriptive but long; the first balances unification framing and specificity; the second leads with the technical anchor. Joseph picks.
- Page-budget triage: the natural-length draft is ~14–16 pages of main text (excluding appendices and references). To get to 9: §3, §5, §7 are load-bearing and resist compression. Candidate cuts: (a) §6.5 distinction-from-active-inference can compress to one paragraph; (b) §9 related work can compress 30% by tightening per-strand text to 2–3 sentences each; (c) §8 worked example can compress to a half-page sketch; (d) §1 contribution preview overlaps with §7 statement and can compress; (e) move §4.4 Pinsker comparison numerical detail entirely to Appendix B (already done in part); (f) move §4.6 direction-forcing argument entirely to appendix. Total compression target 30–40% of main text.

**Citations flagged for verification before submission.**

- Hosseini-Hsu-Taghvaei 2023 — cited in §10.2 as adjacent regularity for Class-2 extension; verified in B-N8 prior-art per OUTLINE risk register. Confirm exact title and venue.
- Schulte-Poupart 2025 — cited in §9.4 as the most recent meta-analysis. Confirm published vs.\ preprint status.
- All Lee et al.\ 2023, 2024 references — confirm NeurIPS 2023 and ICML 2024 venues exactly as stated.
- Bruineberg-Dolega-Dewhurst-Baltieri 2022 — cited in §6.5 and §10.1; confirm BBS publication.
- Bareinboim-Correa-Ibeling-Icard 2022 — confirm Theorem 1 numbering matches the Probabilistic and Causal Inference volume.

**Surprises / things that emerged during drafting.**

- The composition theorem (Theorem 7.1) wants more space than the OUTLINE's 1.0-page allotment. The full statement with conditions A1–A4 and conclusions (i)–(iv) takes a half-page; the "what is new about the composition" subsection runs another half-page; the "assembly, not derivation" framing runs a third. The full §7 is closer to 1.5 pages. This is the load-bearing intellectual move and arguably *should* run long; compression candidates above target other sections first.
- The §6.5 active-inference distinction grew because the cite-and-distinguish needs three specific moves (Bareinboim-hierarchy connection, regime-indexed identifiability, scope honesty), each of which warrants a sentence. The compression target compresses these to a single bulleted list rather than prose paragraphs.
- §3.4's distinction from Long-Fei Li et al.\ 2024 wants a worked example to make the axis-distinction concrete; this is currently described in prose. A 2$\times$2 cross of "their axis × our axis" might land sharper but adds half a page.
- The forgetting-prerequisite section deliberately reframes the discount factor as a *structural* quantity rather than a *tuning* parameter; this is the move that lifts Lee et al.\ ProST. Reviewers from the dynamic-regret tradition may not initially see this as a substantive distinction — they treat $\lambda$ as an algorithm parameter. The structural-vs-hyperparameter framing needs to land cleanly; consider strengthening §5.4's "no prior-learning subsidy" point.

**Open questions.**

- Should §2 introduce the strategic-DAG vocabulary at all, or stay strictly in policy-distribution language? Current draft stays in policy-distribution language ($Q$ over actions) per the anonymization guidance, with internal-DAG content surfacing only in Component 3's $\mathcal T_\Sigma$ definition (where edges are abstract revisable elements, not specifically DAG edges). This is honest but loses some of the structural intuition. Verify the call.
- The reduction to ProST in §8 is a sketch; making it sharper would require explicit identification of ProST's tempo schedule with our $\lambda$. The two are related as $\lambda_{\mathrm{eff}} \approx 1 - K/T$ where $K$ is update count and $T$ horizon; making this exact requires more notation. Current draft sketches; consider tightening.
- Appendix E (algorithm sketch) is currently very minimal. Reviewers in the algorithmic NeurIPS tradition will want more concrete detail. The trade-off: more detail = more pages and risks scope creep into a follow-up paper's territory. Current draft errs on the side of brevity.

**Anonymization check.**

- No occurrences of: Joseph, Wecker, ASF, AAD, logogenic, ELI, PROPRIUM, ORCID, Zenodo, agentic-systems, agentic-tft, firmatum, shoshin.
- Strategic-DAG vocabulary kept abstract (revisable elements, edges as edge-tokens).
- "Loop-as-Causal-Engine" replaced with "closed-loop interventional access" throughout main text.
- $\Sigma_t$ DAG vocabulary not in main text; only "policy distribution $Q$ over actions."
- "Satisfaction gap" / "control regret" / "strategic tempo" / "forgetting prerequisite" retained — generic enough.

**Cross-paper differentiation.**

- Confirmed: 02-convergence vocabulary is BH identity / 2$\times$2 / strategic tempo / closed-loop interventional access. None of these overlap with 01-tragedy (Lyapunov-survival drive + LMI machinery) or 03-hallucinate (κ × 𝒜 + Class 1/2/3). Reviewers seeing all three should perceive distinct lanes.

