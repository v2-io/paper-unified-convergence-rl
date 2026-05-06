## Preliminaries ^sec-preliminaries

We adopt strict minimalism: only the notation and assumptions used directly in the main result and the mechanism statements appear here. Convention-hierarchy details (one-step vs. receding-horizon vs. Bellman continuation), the action-gap matching argument, tied-optimum extensions, and the deterministic-trajectory deployment modes are deferred to [[#^sec-aux]].

**Episodic non-stationary MDP.** A finite-horizon non-stationary MDP $(\mathcal S, \mathcal A, P_t, r_t, N_h)$ with state space $\mathcal S$, finite action space $\mathcal A$, time-indexed transition kernels $P_t(\cdot \mid s, a)$ and reward functions $r_t(s, a)$ allowed to vary across rounds $t$, and finite planning horizon $N_h$. Standard non-stationary-RL boundedness assumptions \cite{cheung-2020-reinforcement}: per-step reward in $[0, 1]$.

**Variation regimes.** A variation budget $V_T$ measures continuous variation in transitions and rewards. We work in the *piecewise-stationary* specialization: time partitions into $B_T + 1$ stationary intervals separated by optimum-change events, with $B_T := \lvert\{t : a^*_t \ne a^*_{t-1}\}\rvert$. The two regimes are distinct in general; under abrupt-magnitude-$\delta$ piecewise-stationarity they coincide up to constants ($V_T \asymp \delta B_T$). Our results are stated for $B_T$; the continuous-$V_T$ extension is open ([[#^sec-conclusion]]).

**Policy distribution.** The agent's policy at round $t$ is a distribution $Q_t(\cdot \mid s)$ over actions; we write $Q$ when state and round are understood. We adopt the canonical scope: the optimum policy at $(M_t, s)$ is *deterministic*, $\pi^*(\cdot \mid M_t, s) = \delta_{a^*(s)}$ where $a^*(s) := \arg\max_a Q^\pi(s, a)$ is the optimal action under the agent's current model $M_t$. Deterministic $\pi^*$ is canonical for finite-MDP RL with isolated optima \cite{lattimore-2020-bandit}; the perturbative extension to $\epsilon$-stochastic and softmax-regularized optima ([[#^sec-key-lemma-1]], full derivation [[#^sec-perturbative]]) covers smooth deviations.

**Value object.** Given an objective $O$, model $M_t$, policy $\pi$, and horizon $N_h$:
$$Q_O(M_t, a;\, \pi_{\mathrm{cont}}, N_h) \;=\; \mathbb E\!\left[ V_O(\tau) \,\Bigm|\, M_t,\, \mathrm{do}(a_t = a),\, a_{t+1:} \sim \pi_{\mathrm{cont}} \right].$$
The $\mathrm{do}(\cdot)$ \cite{pearl-2009-causality} flags intervention on $a_t$, not conditioning. We adopt the **one-step improvement** convention $\pi_{\mathrm{cont}} = \pi_{\mathrm{current}}$ as default: most conservative, comparable across rounds, fixed-point-free. Receding-horizon and Bellman alternatives are in [[#^sec-aux-conventions]].

**Bounded value range.** $V_{\max}(M_t) := \max_a Q_O(M_t, a) - \min_a Q_O(M_t, a)$, finite under bounded reward and finite horizon.

**Action gap (isolated optimum).** $\Delta(a) := Q_O(a^*) - Q_O(a) \in [0, V_{\max}]$, $\Delta_{\min} := \min_{a \neq a^*} \Delta(a) > 0$ whenever the optimum is isolated.

**Strategic regret.** For policy distribution $Q$:
$$R(Q) \;:=\; Q_O(a^*) - \mathbb E_{a \sim Q}[Q_O(a)] \;=\; \sum_{a \neq a^*} Q(a) \cdot \Delta(a).$$
Three classical forms — $\mathbb E_{\pi^*}[V] - \mathbb E_Q[V]$, $V(a^*) - \mathbb E_Q[V]$, $\mathbb E_{\pi^*}[V - V_Q]$ — coincide under deterministic $\pi^*$.

**Total variation and reverse-KL.** For finite action measures $P, Q$ on $\mathcal A$,
$$\operatorname{TV}(P, Q) \;:=\; \tfrac12 \sum_a |P(a) - Q(a)|, \qquad D_{\mathrm{KL}}(P \,\|\, Q) \;:=\; \sum_a P(a) \log\tfrac{P(a)}{Q(a)}$$
with the convention $0 \log 0 = 0$. Our bounds use the *reverse* KL direction (with $P = \pi^*$); the forward direction $D_{\mathrm{KL}}(Q \,\|\, \pi^*)$ is $+\infty$ whenever $Q$ has off-optimum mass and is therefore vacuous as a regret coordinate (full direction-forcing argument in [[#^sec-aux-direction-forcing]]).

**Singular trajectory.** The agent operates on a single, non-forkable causal trajectory: $a_t$ causally precedes $o_{t+1}$, and the model state $M_t$'s sufficiency is defined relative to *this* trajectory rather than to a model-state equivalence class. This is the operational ground for the $\mathrm{do}$-operator above and for the closed-loop interventional-access argument ([[#^sec-key-lemma-3]]).
