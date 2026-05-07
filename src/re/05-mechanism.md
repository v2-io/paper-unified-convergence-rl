## Mechanism ^sec-mechanism

We surface four key lemmas and chain them through a block decomposition. Algebra is deferred to the appendices.

### Key Lemma 1: Point-mass reverse-KL/TV identity ^sec-key-lemma-1

> [!lemma] Point-mass reverse-KL/TV identity ^lem-pointmass-identity
> For deterministic $\pi^* = \delta_{a^*}$ (per visited state, with $a^* = a^*(s)$ and $Q = Q(\cdot \mid s)$) and any policy $Q$,
> $$D_{\mathrm{KL}}(\pi^* \,\|\, Q) \;=\; -\log Q(a^*) \;=\; -\log\bigl(1 - \operatorname{TV}(\pi^*, Q)\bigr),$$
> equivalently $\operatorname{TV}(\pi^*, Q) = 1 - e^{-D_{\mathrm{KL}}(\pi^* \,\|\, Q)}$. The identity is read in the extended real sense: when $Q(a^*) = 0$ both sides equal $1$, with the natural convention $D_{\mathrm{KL}} = +\infty$ and $e^{-\infty} = 0$ — the identity holds for *all* policies $Q$, including Diracs at suboptimal actions.

*Intuition.* Both $D_{\mathrm{KL}}$ and TV collapse cleanly under the point-mass — every $a \neq a^*$ term vanishes by $0\log 0 = 0$. Substituting into BH gives $1 - e^{-D_{\mathrm{KL}}} \le \sqrt{1 - e^{-D_{\mathrm{KL}}}}$, strict on $D_{\mathrm{KL}} > 0$: the identity is the *exact value* at this corner rather than BH-at-equality. Extends perturbatively to $\epsilon$-greedy and softmax-regularized optima with $O(\epsilon\log(1/\epsilon))$ and $O(\tau^{-1}\exp(-\Delta_{\min}/\tau))$ corrections under full-support $Q \ge q_0$ ([[#^sec-perturbative]]). Full proof in [[#^sec-key-lemma-proofs]].

### Key Lemma 2: Forgetting prerequisite ^sec-key-lemma-2

The strategic mismatch state $\boldsymbol\delta_\Sigma = (\delta_{ij})_{(i,j) \in E}$ in the Euclidean norm $\|\boldsymbol\delta_\Sigma\|^2 = \sum_{(i,j)} \delta_{ij}^2$ evolves in discrete time as
$$\mathbb E[\Delta \delta_{ij} \mid \delta_{ij}] = -\alpha_{ij} \delta_{ij} + w_{ij}, \qquad |w_{ij}| \le \rho_\Sigma / |E|^{1/2},$$
where $\alpha_{ij} \ge 0$ is a per-element correction rate, $w_{ij}$ a bounded disturbance with budget $\sum w_{ij}^2 \le \rho_\Sigma^2$, and $R_\Sigma$ the operative norm-radius beyond which tracking is lost. Beta-Bernoulli edge updates and Robbins–Monro gradient updates both instantiate this *Model (Σ)*.

> [!lemma] Multi-factor forgetting prerequisite ^lem-forgetting
> Within Model (Σ) with diagonal per-element correction rates $\alpha_{ij} = \nu_{ij} \cdot \iota_{ij} \cdot \eta_{\mathrm{edge},ij}$ (per-element observation rate $\nu_{ij} \in [0,1]$, regime-adjusted identifiability $\iota_{ij} \in [0,1]$, edge-update gain $\eta_{\mathrm{edge},ij} > 0$), under per-element exponential forgetting at rate $\lambda_{ij} \in (0,1)$ giving steady-state $\eta_{\mathrm{edge},ij}^{\mathrm{ss}} \approx 1 - \lambda_{ij}$, ultimate boundedness of $\|\boldsymbol\delta_\Sigma\|$ within $R_\Sigma^* = \rho_\Sigma / \mathcal T_\Sigma^{\mathrm{bn,ss}}$ is *sufficient* under
> $$\mathcal T_\Sigma^{\mathrm{bn,ss}} \;:=\; \min_{(i,j) \in E} \bigl(\nu_{ij} \cdot \iota_{ij} \cdot (1 - \lambda_{ij})\bigr) \;>\; \rho_\Sigma / R_\Sigma.$$
> The threshold is *sharp inside the diagonal sector model*: when the inequality reverses, an adversarial disturbance concentrating on the bottleneck element drives $\|\boldsymbol\delta_\Sigma\|$ beyond $R_\Sigma$ within the modeled dynamics.

*Intuition (why bottleneck, not sum).* Bounding $\sum_{(i,j)} \alpha_{ij}\delta_{ij}^2 \ge c\|\boldsymbol\delta_\Sigma\|^2$ uniformly forces $c = \min \alpha_{ij}$ — adversarial disturbance concentrating on the weakest element saturates at the bottleneck (the same effect makes per-dimension persistence conditions sharper than scalar conditions in adaptive control \cite{anderson-1985-bursting}). Ultimate boundedness then follows from Khalil's sector-Lyapunov template; full proof in [[#^sec-key-lemma-proofs]], structural-class theorem on $\mathcal A_{\mathrm{decay}}$ in [[#^sec-aux-decay-class]].

*ProST as a sector-level reduction.* The block-restart structure of ProST \cite{lee-2023-prost-tempo} recovers as the *impulsive limit* of Model (Σ): between updates the mismatch evolves under disturbance alone, and each update contracts the mismatch by per-update gain $\gamma$. The Hespanha–Liberzon–Teel reverse-ADT framework \cite{hespanha-2008-lyapunov} gives the threshold (full development in [[#^sec-proof-prost-impulsive]]); under uniform schedules this recovers ProST's $K/T$ form with the impulse gain $\gamma$ made explicit. The reframe lifts ProST's *tempo-as-hyperparameter* framing into *tempo-as-stability-margin* — the longest inter-update gap, not the average, sets the threshold, exposing the tradeoff between update frequency and per-update impulse strength at the sector-Lyapunov level.

### Key Lemma 3: Closed-loop interventional access ^sec-key-lemma-3

> [!lemma] Loop generates interventional samples ^lem-loop-level2
> Let $H_t$ denote the agent's information state at round $t$ (model state $M_t$, history of observations and actions). Under (C1) *positivity* — $\pi_t(a \mid H_t) \ge p_{\min} > 0$ on the support of $H_t$ for the actions of interest; (C2) *sequential ignorability* — $H_t$ d-separates $a_t$ from $o_{t+1}$ in the mutilated graph $G_{\overline{a_t}}$ (equivalently, $a_t \perp\!\!\!\perp Y^{(a_t)} \mid H_t$ in potential-outcome notation); (C3) *known action mechanism* — the behavior policy $\pi_t(a \mid H_t)$ is known: executed actions are interventions relative to the environment's transition kernel, and the loop generates samples from $P(o_{t+1} \mid \mathrm{do}(a_t = a), H_t)$ — i.e., Pearl Level-2 interventional kernels conditional on the decision context. The substantive content for the identification claim is (C2); under the singular-trajectory + agent-as-policy architecture of [[#^sec-preliminaries]], (C1) reduces to realized-action positivity (automatic) and (C3) to agent-policy-queryability (architectural). (C1) and (C3) are stated explicitly for forward-compatibility with off-policy / counterfactual extensions.

*Intuition.* By temporal ordering, $a_t$ causally precedes $o_{t+1}$. Under (C1)–(C3) the conditional $P(o_{t+1} \mid a_t, H_t)$ coincides with the interventional $P(o_{t+1} \mid \mathrm{do}(a_t), H_t)$ on the support where $\pi_t(a_t \mid H_t) > 0$ — the loop generates conditional-Level-2 samples regardless of how the agent updates internally. Three regimes \cite{bareinboim-correa-ibeling-icard-2022-pearl-hierarchy} partition usable strength: A ($\iota \approx 1$, $\mathrm{do}$-effects identified cleanly), B ($\iota \in (0,1)$, bias $\propto 1 - \iota$), C (observation-only, not on-policy realizable; observer-on-sub-agent extensions in [[#^sec-aux-deployment-modes]]). Components 2 and 4 are jointly load-bearing — neither alone makes the bound learnable.

### Key Lemma 4: Bias bound under partial identifiability ^sec-key-lemma-4

> [!lemma] Bias bound ^lem-bias-bound
> Assume $Q_t(a \mid s) \ge q_0 > 0$ for both $a = a^*_{\mathrm{ag},t}(s)$ (the agent's identified optimum) and $a = \tilde a^*_t(s)$ (the true optimum). Let $\hat D_t(s) := -\log Q_t(a^*_{\mathrm{ag},t}(s) \mid s)$ and $D_t^{\mathrm{true}}(s) := -\log Q_t(\tilde a^*_t(s) \mid s)$. Then
> $$\bigl|\hat D_t(s) - D_t^{\mathrm{true}}(s)\bigr| \le \mathbf 1[a^*_{\mathrm{ag},t}(s) \ne \tilde a^*_t(s)] \cdot \log(1/q_0),$$
> and taking expectations: $\mathbb E[\lvert \hat D_t(s) - D_t^{\mathrm{true}}(s) \rvert] \le (1 - p_{\mathrm{id}}(s)) \log(1/q_0)$, where $p_{\mathrm{id}}(s) := \Pr[a^*_{\mathrm{ag},t}(s) = \tilde a^*_t(s)]$.

*Intuition.* Indicator decomposition: zero on the matching event; on the misidentification event both $\log Q_t$ values lie in $[\log q_0, 0]$ so their absolute difference is at most $\log(1/q_0)$. Bias scales linearly in misidentification mass — vanishes in Regime A, controlled in Regime B, saturates at $\log(1/q_0)$ in Regime C. The two-point support condition (at the two argmax candidates) is strictly weaker than the perturbative identity's full-support condition ([[#^sec-key-lemma-1]]). Full proof in [[#^sec-key-lemma-proofs]].

### Combining the lemmas: cumulative regret in four steps ^sec-rate-combination

*Step 1 — Per-round bound via the simulation lemma.* The performance-difference lemma \cite{kakade-2002-approximately, munos-2003-error, ross-2010-efficient, azar-2017-minimax} gives, for any policies $\pi^*_t$ and $Q_t$,
$$V^{\pi^*_t}_t(s_0) - V^{Q_t}_t(s_0) \;\le\; V_{\max} \cdot N_h \cdot \overline{\mathrm{TV}}_t,$$
where $\overline{\mathrm{TV}}_t$ is the occupancy-weighted TV of (A5). By Key Lemma 1 applied per state, $\operatorname{TV}(\pi^*_t, Q_t) = 1 - e^{-K_t(s)}$ — sharper than Pinsker, preserved under occupancy aggregation. The $N_h$ is the linear-in-$N_h$ simulation-lemma penalty, sharper than the $N_h^2$ of TRPO-style worst-state bounds \cite{schulman-2015-trust}.

*Step 2 — Block decomposition.* Partition $[1, T]$ at $(P, r)$-change events into blocks of lengths $\Delta_i$ (the kernel-stationary-segment boundaries of [[#^sec-preliminaries]]). (A5) — restart-on-change base learner — gives within-block $\sum_{t \in \mathrm{block}_i} \mathbb E[\overline{\mathrm{TV}}_t] \le 2c \sqrt{\Delta_i}$.

*Step 3 — Cauchy–Schwarz across $B_T + 1$ blocks.* $\sum_i \sqrt{\Delta_i} \le \sqrt{(B_T+1) T}$, giving $\sum_t \mathbb E[V_{\max} N_h \overline{\mathrm{TV}}_t] \le 2c V_{\max} N_h \sqrt{(B_T+1) T}$.

*Step 4 — Misidentification penalty.* Steps 1–3 give the rate term against the agent's *identified* optimum $\pi^*_{\mathrm{ag},t}$ (what (A5) delivers); (v) compares to the *true* optimum $\tilde\pi^*_t$. Applying the simulation lemma to the two point-mass policies, each per-step bracket reduces to $Q^{\tilde\pi^*}_h(s_h, \tilde a^*) - Q^{\tilde\pi^*}_h(s_h, a^*_{\mathrm{ag}}) \in [0, V_{\max}]$ on the misidentification event and zero on the matching event. Aggregated over $N_h$ horizon steps with floor $p_{\mathrm{id}} := \min_s p_{\mathrm{id}}(s)$, the per-round penalty is at most $V_{\max} N_h (1 - p_{\mathrm{id}})$ — value-coordinate throughout, vanishing in Regime A. The support condition $Q_t \ge q_0$ is needed only for conclusion (iii)'s diagnostic readout (Lemma 4), not for the rate term.

Combining: $\mathbb E[\mathrm{DynReg}(T)] \le 2c V_{\max} N_h \sqrt{(B_T+1) T} + V_{\max} N_h (1 - p_{\mathrm{id}}) \cdot T$, with Cesàro tracking $\tfrac{1}{T} \sum_t (V^{\tilde\pi^*_t}_t - V^{Q_t}_t) \to 0$ when $B_T = o(T)$ and $p_{\mathrm{id}} \to 1$.

In the bandit special case the rate sharpens logarithmically; the non-restarting carryover variant of (A5) is deferred. Full proof of (v) in [[#^sec-proof-composition]]; bandit-case sharpening + algorithmic instantiation in [[#^sec-algorithm]].
