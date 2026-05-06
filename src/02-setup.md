## Setup ^sec-setup

**Markov decision process.** A finite-horizon non-stationary MDP $(\mathcal S, \mathcal A, P_t, r_t, N_h)$ with state space $\mathcal S$, finite action space $\mathcal A$, time-indexed transition kernels $P_t(\cdot \mid s, a)$ and reward functions $r_t(s, a)$ allowed to vary across rounds $t$, and finite planning horizon $N_h$. Standard non-stationary-RL boundedness assumptions \cite{cheung-2020-reinforcement}: per-step reward in $[0, 1]$, total cumulative variation in transition and reward bounded by a *variation budget*
$$V_T \;:=\; \sum_{t=1}^{T-1} \max\!\Bigl\{ \sup_{s,a} \|P_{t+1}(\cdot \mid s, a) - P_t(\cdot \mid s, a)\|_{\mathrm{TV}},\ \ \sup_{s,a} |r_{t+1}(s, a) - r_t(s, a)| \Bigr\}.$$

**Two variation regimes.** [[#^thm-composition]](v) is stated for the *piecewise-stationary* specialization: $B_T + 1$ stationary blocks separated by optimum-change events, with $B_T := |\{t : a^*_t \ne a^*_{t-1}\}|$. $B_T$ and $V_T$ are distinct in general (piecewise-stationary abrupt magnitude-$\delta$: $V_T \asymp \delta B_T$; continuous optimum-preserving: $V_T > 0$, $B_T = 0$; tiny optimum-flipping: $B_T \gg V_T / \delta_{\min}$). The rate $O(V_{\max}\sqrt{(B_T+1)T})$ is not directly comparable to the variation-budget rate $\widetilde O(V_T^{1/3} T^{2/3})$ \cite{cheung-2020-reinforcement, wei-luo-2021-blackbox} in continuous regimes; they coincide up to constants under abrupt-magnitude-$\delta$. Generalization to continuous variation is future work.

**Policy distribution.** Round-$t$ policy $Q_t(\cdot \mid s)$; we write $Q$ when state and round are understood. Under canonical scope, $\pi^*(\cdot \mid M_t, s) = \delta_{a^*(s)}$ where $a^*(s) := \arg\max_a Q^\pi(s, a)$.

**Value object.**
$$V_O(M_t, \pi;\, N_h) \;=\; \mathbb E[V_O(\tau_{t:t+N_h}) \mid M_t, \pi], \qquad Q_O(M_t, a;\, \pi_{\mathrm{cont}}, N_h) \;=\; \mathbb E[V_O(\tau) \mid M_t,\, do(a_t = a),\, a_{t+1:} \sim \pi_{\mathrm{cont}}].$$
The $do(\cdot)$ \cite{pearl-2009-causality} flags intervention on $a_t$ rather than conditioning. Default continuation: **one-step improvement** $\pi_{\mathrm{cont}} = \pi_{\mathrm{current}}$ — most conservative, comparable across rounds, fixed-point-free; receding-horizon and Bellman alternatives in [[#^sec-convention-hierarchy]].

**Bounded value range.** $V_{\max}(M_t) := \max_a Q_O(M_t, a) - \min_a Q_O(M_t, a)$, finite under bounded reward, finite horizon.

**Action gap (isolated optimum).** $\Delta(a) := Q_O(a^*) - Q_O(a) \in [0, V_{\max}]$, $\Delta_{\min} := \min_{a \neq a^*} \Delta(a) > 0$ whenever the optimum is isolated.

**Strategic regret.** $R(Q) := Q_O(a^*) - \mathbb E_{a \sim Q}[Q_O(a)] = \sum_{a \neq a^*} Q(a) \Delta(a)$; three classical forms ($\mathbb E_{\pi^*}[V] - \mathbb E_Q[V]$, $V(a^*) - \mathbb E_Q[V]$, $\mathbb E_{\pi^*}[V - V_Q]$) coincide under deterministic $\pi^*$.

**Singular trajectory.** Single, non-forkable causal trajectory: $a_t$ causally precedes $o_{t+1}$, $M_t$'s sufficiency defined relative to *this* trajectory. This grounds the Pearl $do$-operator above and the closed-loop interventional-access argument of [[#^sec-loop-level2]].
