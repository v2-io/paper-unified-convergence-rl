## Auxiliary Mathematical Material ^sec-aux

This appendix collects supporting material used by the main results: convention hierarchy details, the admissible-divergence family, direction-forcing argument, action-gap matching lower bound, tied-optimum extensions, the structural-class theorem on gain-decay updates, strategic-tempo topologies, deployment modes, and the chain-rule uniqueness theorem.

### Convention hierarchy: C1, C2, C3 ^sec-aux-conventions

Three named continuation conventions form a hierarchy of increasing diagnostic power and computational cost.

**C1 — One-step improvement (canonical default).** $\pi_{\mathrm{cont}} = \pi_{\mathrm{current}}$. Each action is evaluated assuming current behavior continues afterward. No fixed-point computation, no global-optimality assumption. Cheapest; weakest diagnostic.

**C2 — Receding-horizon ($N_r$-step replanning).** At each future step, re-optimize over a horizon of $N_r$ steps using the model available at that step:
$$\pi_{\mathrm{RH}}(M_\tau) \;=\; \arg\max_\pi V_O(M_\tau, \pi;\, N_r)\quad \text{applied at each } \tau.$$
Captures multi-step recovery: a goal that appears unattainable under frozen continuation may be reachable with replanning. Moderate cost; moderate diagnostic.

**C3 — Bellman.** $\pi_{\mathrm{cont}} = \pi^*$ where $\pi^* = \arg\max_\pi V_O(M_t, \pi; N_h)$. The continuation is the optimal policy — a fixed-point equation. Strongest diagnostic; most expensive.

> [!proposition] Monotonicity ^prop-monotonicity
> For any model $M_t$, horizon $N_h$, and policy class $\Pi$:
> $$A_O^{(1)}(M_t;\, \Pi, N_h) \;\le\; A_O^{\mathrm{RH}}(M_t;\, \Pi, N_r, N_h) \;\le\; A_O^{\mathrm{B}}(M_t;\, \Pi, N_h).$$

> [!proof]
> C1 freezes continuation at $\pi_{\mathrm{current}}$ (possibly suboptimal); C2 re-optimizes periodically, so $\pi_{\mathrm{RH}} \succeq \pi_{\mathrm{current}}$ at each future step; C3 uses the globally optimal $\pi^*$. A weakly better continuation yields a weakly higher expected trajectory value; taking the supremum over the first action preserves the ordering.

**Corollary.** $\delta_{\mathrm{sat}}^{\mathrm B} \le \delta_{\mathrm{sat}}^{\mathrm{RH}} \le \delta_{\mathrm{sat}}^{(1)}$ (since $\delta_{\mathrm{sat}} = V_O^{\min} - A_O$, higher $A_O$ means lower $\delta_{\mathrm{sat}}$). And $\delta_{\mathrm{regret}}^{(1)} \le \delta_{\mathrm{regret}}^{\mathrm{RH}} \le \delta_{\mathrm{regret}}^{\mathrm B}$ (since $\delta_{\mathrm{regret}} = A_O - V_O(M_t, \pi_{\mathrm{current}}; N_h)$, higher $A_O$ means higher $\delta_{\mathrm{regret}}$).

C1 is the most conservative diagnostic (most likely to diagnose "locally unattainable"); C3 is the most accurate (least false "unattainable" diagnoses). The 2$\times$2 diagnostic structure is preserved under all three; only the inferential force varies.

### Admissible-divergence family for the regret bound ^sec-aux-admissible-divergence

The reverse-KL direction is forced by the deterministic-π* regime ([[#^sec-aux-direction-forcing]]). Within the direction-forced family, multiple smooth $f$-divergences yield valid regret bounds:

| Divergence | Bound on $R(Q)$ | Tightness | Finite under det.\ $\pi^*$? |
|---|---|---|---|
| $\operatorname{TV}(\pi^*, Q)$ | $V_{\max} \cdot \operatorname{TV}$ | Tight (extremal $V$) | Yes |
| $D_{\mathrm{KL}}(\pi^* \,\|\, Q)$ via Pinsker | $V_{\max} \sqrt{D_{\mathrm{KL}}/2}$ | Loose by $\sqrt{\cdot}$ | Yes |
| $D_{\mathrm{KL}}(\pi^* \,\|\, Q)$ via point-mass identity | $V_{\max}(1 - e^{-D_{\mathrm{KL}}})$ | Tight (this paper) | Yes |
| $\chi^2(\pi^* \,\|\, Q)$ (Le Cam) | $V_{\max} \cdot \tfrac12 \sqrt{\chi^2}$ | Looser than Pinsker | $\chi^2 = 1/Q(a^*) - 1$ |
| $D_\alpha(\pi^* \,\|\, Q)$ (Rényi, $\alpha \ge 1$) | Various | Interpolates | Yes for $\alpha \ge 1$ |
| $D_{\mathrm{KL}}(Q \,\|\, \pi^*)$ (forward-KL) | $+\infty$ | Vacuous | No |

Reverse-KL is selected uniquely within the direction-forced family by the chain-rule axiom ([[#^sec-aux-chain-rule]]). The point-mass identity supplies the *exact* bound on reverse-KL under deterministic $\pi^*$; the table reflects different bound shapes on the same divergence (with the BH inequality $V_{\max}\sqrt{1 - e^{-D_{\mathrm{KL}}}}$ as the looser general form to which the identity reduces outside the deterministic-$\pi^*$ corner).

### Direction-forcing argument ^sec-aux-direction-forcing

For deterministic $\pi^* = \delta_{a^*}$ and any $Q$ with $Q(a) > 0$ for some $a \neq a^*$:
$$D_{\mathrm{KL}}(Q \,\|\, \pi^*) \;=\; \sum_a Q(a) \log\frac{Q(a)}{\pi^*(a)} \;=\; \sum_{a \neq a^*} Q(a) \log\frac{Q(a)}{0} \;=\; +\infty.$$
A bound "$R \le +\infty$" is vacuous. The reverse direction $D_{\mathrm{KL}}(\pi^* \,\|\, Q)$ is finite (and equal to $-\log Q(a^*)$) whenever $Q(a^*) > 0$. The asymmetry is forced by the regime: regret is a one-sided quantity (contributes only from $Q$'s off-optimum mass; $\pi^*$ has no support off $a^*$); divergences whose role is to bound this one-sided quantity must themselves be one-sided. Symmetric divergences (squared Hellinger, Jensen-Shannon, symmetrized KL) introduce a vacuous symmetric term.

### Action-gap matching lower bound ^sec-aux-action-gap

For any $Q$ with $\Delta_{\min} = \min_{a \neq a^*} \Delta(a) > 0$:
$$R(Q) \;=\; \sum_{a \neq a^*} Q(a) \Delta(a) \;\ge\; \Delta_{\min} \sum_{a \neq a^*} Q(a) \;=\; \Delta_{\min} \cdot (1 - Q(a^*)) \;=\; \Delta_{\min} \cdot \operatorname{TV}(\pi^*, Q).$$
By the point-mass identity ([[#^lem-pointmass-identity]]), $\operatorname{TV}(\pi^*, Q) = 1 - e^{-D_{\mathrm{KL}}(\pi^* \,\|\, Q)}$, giving the matching lower bound used in the two-sided characterization.

The lower bound is tight when sub-optimal actions are uniformly bad ($\Delta_{\min} = \max_{a \neq a^*} \Delta(a)$). For typical landscapes the gap between upper and lower bound is $V_{\max} - \Delta_{\min}$, controlled by the *spread* of action gaps.

### Tied-optimum and softmax-smoothed extensions ^sec-aux-tied-softmax

**Tied-optimum.** If $\pi^*$ has support on a tied-optimum set $\mathcal A^* = \{a : Q_O(a) = Q_O(a^*)\}$ with uniform mass, reverse-KL is finite whenever $Q$ covers $\mathcal A^*$. The regret bound extends:
$$R(Q) \;=\; \sum_{a \notin \mathcal A^*} Q(a) \Delta(a) \;\le\; V_{\max} \cdot \mathbb P_Q(a \notin \mathcal A^*).$$
A multi-action analog of the point-mass identity holds with $\pi^*(a^*) = 1/|\mathcal A^*|$ replacing the point-mass form, yielding $D_{\mathrm{KL}}(\pi^* \,\|\, Q) = -\sum_{a \in \mathcal A^*} (1/|\mathcal A^*|) \log(|\mathcal A^*| Q(a))$, a multi-action analog of $-\log Q(a^*)$. The two-sided bound becomes one-sided in general; the upper bound holds with the modified KL form.

**Softmax-smoothed $\pi^*$ with temperature $\tau \to 0$.** Replace $\pi^* = \delta_{a^*}$ with $\pi^*_\tau \propto \exp(Q_O / \tau)$. As $\tau \to 0$, $\pi^*_\tau \to \delta_{a^*}$. For finite $\tau$, the *perturbative* identity ([[#^sec-perturbative]]) applies: $\operatorname{TV}(\pi^*_\tau, Q) = 1 - e^{-D_{\mathrm{KL}}(\pi^*_\tau \,\|\, Q)} + O(\tau^{-1}\exp(-\Delta_{\min}/\tau))$ — exponentially small with polynomial prefactor. The two-sided regret bound transfers with the same correction order. Outside the perturbative regime — for genuinely high-entropy optima — the BH inequality $\operatorname{TV} \le \sqrt{1 - e^{-D_{\mathrm{KL}}}}$ becomes the relevant general bound; Pinsker also applies, with a one-sided regret form.

### The gain-decay structural class $\mathcal A_{\mathrm{decay}}$ ^sec-aux-decay-class

The persistence inequality $\mathcal T_\Sigma^{\mathrm{bn,ss}} > \rho_\Sigma/R_\Sigma$ ([[#^sec-key-lemma-2]]) is an *instantaneous* check at the current operating point. Define the structural class by gain-decay rather than sample-counting:
$$\mathcal A_{\mathrm{decay}} \;:=\; \big\{\text{updates whose effective sector gain } \alpha_{ij}^{(t)} \to 0 \text{ as } t \to \infty \text{ on every revisable element}\big\}.$$

> [!theorem] Universal failure of $\mathcal A_{\mathrm{decay}}$ ^thm-decay-class
> For any fixed $(\rho_\Sigma, R_\Sigma)$ with $\rho_\Sigma > 0$, every agent in $\mathcal A_{\mathrm{decay}}$ eventually violates the persistence threshold $\mathcal T_\Sigma^{\mathrm{bn,ss}} > \rho_\Sigma/R_\Sigma$.

> [!proof]
> At every element $(i, j)$, $\alpha_{ij}^{(t)} \to 0$ by definition of $\mathcal A_{\mathrm{decay}}$, so $\min_{(i,j)} \alpha_{ij}^{(t)} \to 0$, so the bottleneck $\mathcal T_\Sigma^{\mathrm{bn}} \to 0$ with experience. Once $\mathcal T_\Sigma^{\mathrm{bn,ss}} < \rho_\Sigma/R_\Sigma$, the threshold is violated. This holds *regardless of prior calibration*: at any finite calibration level, the gain decays below threshold once enough experience accumulates.

*Scope clarification (per-element pull).* The class definition requires $\alpha_{ij}^{(t)} \to 0$ on *every* revisable element — i.e., the agent is being pulled at every element repeatedly. Beta-Bernoulli with $\eta_{\mathrm{edge}, ij} = 1/(n_{ij}+1)$ is in the class only for elements actually being updated; an agent that ignores a subset of elements is outside the class for those elements (their gain remains at initial calibration), but inside it for the elements it does touch. The theorem applies to the touched elements; persistence on the untouched ones reduces to whatever calibration was set initially.

This is a *structural* failure of the class, not a tuning problem. The class includes:
- Count-accumulating Bayesian updates without forgetting (e.g., Beta-Bernoulli with $\eta_{\mathrm{edge},ij} = 1/(n_{ij}+1)$ where $n_{ij} \to \infty$).
- Bounded-memory schemes with growing memory.
- Observation-aggregating schemes without restart.
- Gradient-based methods with vanishing step sizes ($\eta_t = c/t^\beta$ for $\beta \in (0, 1]$, the Robbins–Monro regime).

Mechanisms *outside* $\mathcal A_{\mathrm{decay}}$ — constant-step-size stochastic approximation, sliding-window updates, bounded-memory learners, block-restart schemes, Kalman filters with bounded process noise — maintain a finite gain ceiling and escape asymptotic decay, but then face a *bidirectional* threshold:

> [!table] Bidirectional thresholds for non-accumulating mechanisms outside $\mathcal A_{\mathrm{decay}}$. ^tab-bidirectional-thresholds cols="X l X"
>
> | Mechanism | $\alpha_\Sigma^{\mathrm{ss}}$ | Prerequisite holds iff |
> |---|---|---|
> | Exponential forgetting with $\lambda$ | $1 - \lambda$ | $1 - \lambda > \rho_\Sigma/R_\Sigma$ |
> | Constant-step SA with rate $\eta$ | $\eta$ | $\eta > \rho_\Sigma/R_\Sigma$ |
> | Fixed-window mechanisms (sliding-window size $W$, bounded-memory size $K$, block-restart period $T_R$) | $1/W$, $1/K$, $1/T_R$ | window/memory/period $< R_\Sigma/\rho_\Sigma$ |

For each row, the threshold is *bidirectional* and sharp within the sector-Lyapunov reduction.

### Strategic-tempo across canonical topologies ^sec-aux-tempo-topologies

The bottleneck strategic tempo $\mathcal T_\Sigma^{\mathrm{bn,ss}}$ of [[#^lem-forgetting]] is verified — and the resulting per-topology threshold derived — across four canonical topologies under Beta-Bernoulli edge updates with per-element forgetting:

- **(T1) Single edge.** $\mathcal T_\Sigma^{\mathrm{bn,ss}} = \nu \cdot \iota \cdot (1-\lambda)$. All three factors load-bearing; setting any one to zero collapses the bottleneck.
- **(T2) Two-edge AND chain, observable intermediate.** $\mathcal T_\Sigma^{\mathrm{bn,ss}} = \min\bigl(\nu_1(1-\lambda_1),\ \nu_1\theta_1(1-\lambda_2)\bigr)$. Edge 2's effective observation rate is gated by edge 1's success probability $\theta_1$ — *depth-gated attenuation*. For depth-$d$ chains, the bottleneck is $\min_k \prod_{j<k}\theta_j (1-\lambda_k)$.
- **(T3) Two-edge AND chain, unobservable intermediate.** Per-edge tempo ill-defined; plan-level bottleneck $\mathcal T_{\Sigma, \mathrm{plan}}^{\mathrm{bn,ss}} = \nu(1-\lambda_\Phi)$ over a single tracked plan-level quantity $\hat\Phi = p_1 p_2$.
- **(T4) Two-arm OR node, $\varepsilon$-greedy.** $\mathcal T_\Sigma^{\mathrm{bn,ss}} = \min\bigl((1-\varepsilon)(1-\lambda_1),\ \varepsilon(1-\lambda_2)\bigr)$. Action selection controls rate allocation; pure greedy ($\varepsilon = 0$) collapses the unexplored arm's bottleneck — *exploration-gated*.

The structural decomposition: AND-chains exhibit *depth-gated* geometric attenuation $\nu_k = \nu \prod_{j < k} \theta_j$; OR-nodes exhibit *exploration-gated* allocation. Mixed AND/OR DAGs interleave both. Each topology surfaces a different factor as the binding bottleneck.

**Regime-C contamination is structurally fatal.** $\iota_{ij} = 0$ at any single element $\Rightarrow \mathcal T_\Sigma^{\mathrm{bn,ss}} = 0$. An agent with even one fully-confounded element cannot meet the threshold by forgetting alone.

### Three deployment modes of loop-Level-2 ^sec-aux-deployment-modes

The Pearl-$\mathrm{do}$ structure of closed-loop interventional access manifests at distinct layers with semantically distinct interventional mechanisms:

- **Mode 1 — agent-self-intervention.** The agent performs $\mathrm{do}$-actions on its own action space as part of its ordinary adaptive loop. Intervention is on the agent's own action; target is the environment's response. This is [[#^lem-loop-level2]]'s primary content.
- **Mode 2 — observer-on-sub-agent.** An observer external to a composite performs $\mathrm{do}$-interventions on one sub-agent's action space; target is another sub-agent's response. Reveals cross-coupling structure that component-marginal observation cannot identify.
- **Mode 3 — observer-on-agent-input** (sketched, future work). An observer intervenes on the agent's observation channel; target is the agent's subsequent policy. Useful for offline architectural-class diagnosis.

Modes share the Pearl-$\mathrm{do}$ structure but differ in who intervenes on what. The unification is at the pattern level; the mechanism is semantically distinct per layer.

### Distinction from active inference and causal-RL precursors ^sec-aux-active-inference

Action-perception-loop frameworks — active inference \cite{friston-2017-active-process, parr-2022-active}, control-as-inference \cite{levine-2018-reinforcement}, cybernetics \cite{wiener-1948-cybernetics, conant-1970-every} — implicitly use the action-causes-observation observation. Our distinctive moves:

- *Bareinboim-hierarchy connection.* Active inference / cybernetics rest on Bayesian-network (Level 1) generative models; we invoke the causal-hierarchy theorem \cite{bareinboim-correa-ibeling-icard-2022-pearl-hierarchy} to position the policy DAG as causal with $\mathrm{do}$-conditioning in $Q_O$.
- *Regime-indexed identifiability (A/B/C).* Active inference literature treats causal identifiability uniformly within modeling assumptions; we surface the regime split at framework level.
- *Scope honesty.* We distinguish "data generated under intervention" from "cleanly identified $\mathrm{do}$-estimates"; \cite{bruineberg-dolega-dewhurst-baltieri-2022-bbs} documents that the active-inference literature sometimes elides this.

The causal-RL line \cite{zhang-2016-mdps, zhang-2022-online-rl, lu-2022-efficient, wang-2021-provably, zhang-2020-designing} is the direct ancestor for regime-indexed identifiability and on-policy interventional access; all are stationary-MDP. Composition with non-stationarity is, to our knowledge, novel.

### Chain-rule uniqueness of reverse-KL ^sec-aux-chain-rule

> [!theorem] Chain-rule uniqueness (Hobson 1969; Csiszár 1991) ^thm-chain-rule-uniqueness
> Let $D_f(P \,\|\, Q) = \sum_x Q(x) f(P(x)/Q(x))$ be a smooth $f$-divergence with $f$ convex and $f(1) = 0$. The chain rule
> $$D_f(P_{XY} \,\|\, Q_{XY}) \;=\; D_f(P_X \,\|\, Q_X) \;+\; \mathbb E_{P_X}\!\left[D_f(P_{Y|X} \,\|\, Q_{Y|X})\right]$$
> holds for all joint distributions if and only if $f(t) = c \cdot t \log t$ for some $c > 0$ — i.e., $D_f$ is reverse-KL up to positive scaling.

> [!proof]
> Take any joint $P_{XY}, Q_{XY}$ on a finite product space. Writing $r_x := P(x)/Q(x)$ and $s_{y \mid x} := P(y \mid x)/Q(y \mid x)$, the joint ratio factorizes as $P_{XY}(x,y)/Q_{XY}(x,y) = r_x \cdot s_{y \mid x}$. Expanding both sides of the chain rule:
> $$D_f(P_{XY} \,\|\, Q_{XY}) \;=\; \sum_{x,y} Q_{XY}(x,y)\, f(r_x s_{y \mid x}) \;=\; \sum_x Q(x) \sum_y Q(y \mid x)\, f(r_x s_{y \mid x}),$$
> $$D_f(P_X \,\|\, Q_X) + \mathbb E_{P_X}[D_f(P_{Y \mid X} \,\|\, Q_{Y \mid X})] \;=\; \sum_x Q(x)\, f(r_x) + \sum_x P(x) \sum_y Q(y \mid x)\, f(s_{y \mid x}),$$
> the second term using $\mathbb E_{P_X} = \sum_x P(x) = \sum_x Q(x) r_x$. Equating and collecting:
> $$\sum_x Q(x) \biggl[\sum_y Q(y \mid x)\, f(r_x s_{y \mid x}) \;-\; f(r_x) \;-\; r_x \sum_y Q(y \mid x)\, f(s_{y \mid x})\biggr] \;=\; 0.$$
> The chain rule must hold for *all* joints, including those where $r_x$ takes arbitrary positive value at one $x$ and $s_{y \mid x}$ varies independently — which forces the bracketed expression to vanish pointwise. A two-point reduction (take $Y$ binary with $s_{y \mid x}$ taking values $s$ and $1/s$ on equal-weight $Q(y \mid x)$) collapses the inner sums and yields the functional equation
> $$f(rs) \;=\; f(r) \;+\; r\, f(s) \quad \text{for all } r, s > 0.$$
> With $f$ convex and $f(1) = 0$, the unique solution is $f(t) = c \cdot t \log t$ for $c > 0$ \cite{aczel-1975-measures} (§4) — this gives $D_f(P \,\|\, Q) = c \sum_x Q(x) (P(x)/Q(x)) \log(P(x)/Q(x)) = c \sum_x P(x) \log(P(x)/Q(x)) = c \cdot D_{\mathrm{KL}}(P \,\|\, Q)$, reverse-KL up to positive scaling. $\square$

**References.** Hobson 1969 ("A new theorem of information theory," *J.\ Stat.\ Phys.*); Csiszár 1991 ("Why least squares and maximum entropy?" *Annals of Statistics*; Theorem 3 corollary, Theorem 5); Shore-Johnson 1980 ("Axiomatic derivation of the principle of maximum entropy," *IEEE Trans.\ Info.\ Theory*, system-independence axiom); Sanov 1957 (large-deviation rate function); Aczél-Daróczy 1975 (functional-equation machinery).

These references give *structurally equivalent reformulations* of the same axiom. The Cauchy functional equation each reduces to is the common content. No known uniqueness route outside the independence-on-sub-problems family exists.

**Why other family members fail the chain rule.** Concrete counterexample for $\chi^2$: take $Q_X$ uniform on $\{x_1, x_2\}$, $P_X = (3/4, 1/4)$, $Q(y|x)$ uniform, $P(y|x) = (3/4, 1/4)$. Direct calculation gives $\chi^2(P_{XY} \,\|\, Q_{XY}) = 9/16$ while $\chi^2(P_X \,\|\, Q_X) + \mathbb E_{P_X}[\chi^2(P_{Y|X} \,\|\, Q_{Y|X})] = 1/4 + 1/4 = 8/16$. Non-additive. Rényi-$\alpha$ for $\alpha \neq 1$ fails analogously; squared Hellinger likewise fails.
