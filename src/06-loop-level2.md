## Component 4 — Closed-Loop Interventional Access ^sec-loop-level2

We invoke Pearl's causal hierarchy [Pearl 2009] and the causal-hierarchy theorem [Bareinboim-Correa-Ibeling-Icard 2022, Theorem 1]: Level 2 (interventional) queries — $P(Y \mid do(X))$ — are not in general identifiable from Level 1 (associational) data $P(Y \mid X)$, so causal claims at Level 2 require interventional data or structural identifying assumptions (e.g., back-door admissibility [Pearl 2009]).

### The loop generates interventional data ^sec-loop-generates

> [!theorem] Loop generates interventional samples — conditional on sufficient history ^thm-loop-level2
> Let $H_t$ denote the agent's information state at round $t$ (model state $M_t$, history of observations and actions). Suppose: (C1) **Positivity** — the action mechanism $\pi_t(a \mid H_t) \ge p_{\min} > 0$ on the support of $H_t$ for the actions of interest; (C2) **Sequential ignorability / sufficient state** — $H_t$ blocks unobserved confounding of the action mechanism, so $a_t \perp\!\!\!\perp (\text{environment latents}) \mid H_t$; (C3) **Known action mechanism** — the behavior policy $\pi_t(a \mid H_t)$ is known. Then executed actions are interventions relative to the environment's transition kernel, and the loop generates samples from $P(o_{t+1} \mid \mathrm{do}(a_t = a), H_t)$ — i.e., Pearl Level-2 interventional kernels conditional on the decision context.

By temporal ordering and the singular-trajectory commitment of [[#^sec-setup]], $a_t$ is a cause of $o_{t+1}$. Under (C1)–(C3), conditioning on the information state $H_t$ blocks the action-selection confounding pathway, so the conditional distribution $P(o_{t+1} \mid a_t, H_t)$ coincides with the interventional distribution $P(o_{t+1} \mid \mathrm{do}(a_t), H_t)$ on the support where $\pi_t(a_t \mid H_t) > 0$. The architecture of the agent (Q-learning, transformer, etc.) does not enter the theorem: the loop generates conditional-Level-2 samples whenever (C1)–(C3) hold, irrespective of how the agent updates internally. Whether the agent *exploits* this data for Level-2 reasoning is a separate (algorithmic) question.

### Interventional *data* is not identified *do*-estimates ^sec-regimes-abc

Action-generated data is Level 2 in *character*, but a clean estimate of $P(o \mid do(a), \Omega_t)$ requires overcoming four typical obstacles: coverage (diverse actions tried), within-step confounding, delay (consequences past $t+1$), and partial observability. We honor this distinction: [[#^thm-twosided-regret]] uses $D_{\mathrm{KL}}(\pi^* \,\|\, Q)$ with $\pi^*$ computed under $M_t$; the KL coordinate $-\log Q(a^*)$ is computable directly from the policy, but the meaning of $a^*$ matching the true optimum depends on causal identification strength from loop data. Three regimes [Bareinboim et al.\ 2022 taxonomy]:

- **Regime A (intervention-rich, $\iota \approx 1$).** Software tests, controlled labs. $do$-effects identified cleanly; bound realizable on-policy.
- **Regime B (partial, $\iota \in (0, 1)$).** Mixed observation-intervention. Bound holds for the model the agent identifies; bias $\propto 1 - \iota$.
- **Regime C (observation-only, $\iota \approx 0$).** Passive monitoring. Bound provable analytically but not realizable on-policy; observer-on-sub-agent extensions can recover identifiability in subcases.

Components 2 and 4 are jointly load-bearing: [[#^sec-pointmass-identity]]'s identity gives the analytic regret-KL relationship; [[#^sec-loop-level2]]'s loop-Level-2 access gives the data substrate from which $\pi^*$ — and the bound's RHS — can be empirically estimated under sufficient identifiability. Neither alone makes the bound learnable.

### Distinction from active inference and causal-RL precursors ^sec-active-inference-distinction

Action-perception-loop frameworks — active inference [Friston-FitzGerald-Rigoli-Schwartenbeck-Pezzulo 2017; Parr-Pezzulo 2022], control-as-inference [Levine 2018], cybernetics [Wiener 1948; Conant-Ashby 1970] — implicitly use the action-causes-observation observation. Our distinctive moves:

- *Bareinboim-hierarchy connection.* Active inference / cybernetics rest on Bayesian-network (Level 1) generative models; we invoke the causal-hierarchy theorem to position the policy DAG as causal with $do$-conditioning in $Q_O$ ([[#^sec-setup]]).
- *Regime-indexed identifiability (A/B/C).* AI literature treats causal identifiability uniformly within modeling assumptions; we surface the regime split at framework level.
- *Scope honesty.* We distinguish "data generated under intervention" from "cleanly identified $do$-estimates"; [Bruineberg-Dolega-Dewhurst-Baltieri 2022] documents that the active-inference literature sometimes elides this.

The causal-RL line [Junzhe Zhang-Bareinboim 2016, 2022; Lu-Meisami-Tewari 2021, 2022; Wang-Yang-Wang 2021 DOVI; Junzhe Zhang 2020] is the direct ancestor for regime-indexed identifiability and on-policy interventional access; all are stationary-MDP. Composition with non-stationarity is, to our knowledge, novel.
