## Component 2 — A Point-Mass Reverse-KL/TV Identity for Action-Distribution Regret ^sec-pointmass-identity

### Total-variation regret bound (textbook setup) ^sec-tv-regret-bound

A bounded-value, deterministic-$\pi^*$ argument gives the textbook total-variation regret bound. From the regret expression $R(Q) = \sum_{a \neq a^*} Q(a) \Delta(a)$ and $\Delta(a) \le V_{\max}$,
$$R(Q) \;\le\; V_{\max} \sum_{a \neq a^*} Q(a) \;=\; V_{\max}\bigl(1 - Q(a^*)\bigr).$$
Under deterministic $\pi^* = \delta_{a^*}$, the total variation between $\pi^*$ and $Q$ is
$$\operatorname{TV}(\pi^*, Q) \;=\; \tfrac12 \sum_a |\pi^*(a) - Q(a)| \;=\; 1 - Q(a^*).$$
So
$$\boxed{\,R(Q) \;\le\; V_{\max} \cdot \operatorname{TV}(\pi^*, Q)\,} \qquad \text{(tight under deterministic } \pi^* + \text{ extremal value landscape).}$$
The bound is tight when $\Delta(a) = V_{\max}$ for all $a \neq a^*$; for typical landscapes it is loose by a factor $\mathbb E_{Q}[\Delta \mid a \neq a^*] / V_{\max} \in (0, 1]$. The matching lower bound via the action gap is:
$$R(Q) \;\ge\; \Delta_{\min} \cdot \operatorname{TV}(\pi^*, Q).$$

### Point-mass reverse-KL/TV identity (deterministic $\pi^*$) ^sec-identity-statement

Under our canonical scope — deterministic optimum $\pi^* = \delta_{a^*}$ — reverse Kullback--Leibler divergence and total variation between $\pi^*$ and any policy $Q$ are related by an *exact identity*, not by an inequality.

> [!theorem] Point-mass reverse-KL/TV identity ^thm-pointmass-identity
> For deterministic $\pi^* = \delta_{a^*}$ and any policy $Q$ with $Q(a^*) > 0$,
> $$\boxed{\;D_{\mathrm{KL}}(\pi^* \,\|\, Q) \;=\; -\log Q(a^*) \;=\; -\log\bigl(1 - \operatorname{TV}(\pi^*, Q)\bigr),\;}$$
> equivalently $\operatorname{TV}(\pi^*, Q) = 1 - e^{-D_{\mathrm{KL}}(\pi^* \,\|\, Q)}$.

> [!proof]
> The reverse-KL collapses under the point-mass $\pi^*$:
> $$D_{\mathrm{KL}}(\delta_{a^*} \,\|\, Q) \;=\; \sum_a \delta_{a^*}(a) \log\frac{\delta_{a^*}(a)}{Q(a)} \;=\; \log\frac{1}{Q(a^*)} \;=\; -\log Q(a^*),$$
> where the convention $0 \log 0 = 0$ disposes of the $a \neq a^*$ terms. Combined with $\operatorname{TV}(\delta_{a^*}, Q) = 1 - Q(a^*)$ from [[#^sec-tv-regret-bound]], $-\log Q(a^*) = -\log(1 - \operatorname{TV})$. Solving for TV gives the equivalent form.

The identity is elementary — a direct two-line calculation under the point-mass assumption — and is *not* the Bretagnolle--Huber inequality at equality. BH \cite{bretagnolle-huber-1978-densities, tsybakov-2009-nonparametric, sason-2016-divergence} gives the general bound $\operatorname{TV}(P, Q) \le \sqrt{1 - e^{-D_{\mathrm{KL}}(P \,\|\, Q)}}$; substituting [[#^thm-pointmass-identity]]'s identity into the BH RHS yields $1 - e^{-D_{\mathrm{KL}}} \le \sqrt{1 - e^{-D_{\mathrm{KL}}}}$, which is strict on $D_{\mathrm{KL}} > 0$ since $x < \sqrt{x}$ on $(0, 1)$. At the deterministic-$\pi^*$ corner the identity supplies the *exact* TV value strictly below the BH envelope, positioning BH as the lineage the identity sits within rather than as the source it instantiates.

### Two-sided regret bound and Lipschitz equivalence ^sec-twosided-regret

Composing [[#^thm-pointmass-identity]] with the TV-regret bound of [[#^sec-tv-regret-bound]] and its action-gap lower bound:

> [!theorem] Two-sided point-mass regret bound ^thm-twosided-regret
> Under bounded value range $V_{\max}$, deterministic optimum $\pi^*$, isolated optimum so $\Delta_{\min} > 0$, and any policy $Q$ with $Q(a^*) > 0$,
> $$\boxed{\;\Delta_{\min}\bigl(1 - e^{-D_{\mathrm{KL}}(\pi^* \,\|\, Q)}\bigr) \;\le\; R(Q) \;\le\; V_{\max}\bigl(1 - e^{-D_{\mathrm{KL}}(\pi^* \,\|\, Q)}\bigr).\;}$$

> [!proof]
> From the TV-regret bound $R(Q) \le V_{\max} \cdot \operatorname{TV}(\pi^*, Q)$ and the point-mass identity $\operatorname{TV}(\pi^*, Q) = 1 - e^{-D_{\mathrm{KL}}}$:
> $$R(Q) \;\le\; V_{\max}\bigl(1 - e^{-D_{\mathrm{KL}}(\pi^* \,\|\, Q)}\bigr).$$
> The lower bound follows analogously from $R(Q) \ge \Delta_{\min} \cdot \operatorname{TV}$.

**Lipschitz equivalence and coordinate-optimality.** [[#^thm-twosided-regret]] gives $\Delta_{\min}/V_{\max} \le R(Q) / [V_{\max}(1 - e^{-D_{\mathrm{KL}}})] \le 1$ — Lipschitz equivalence with constants tight on extremal ($\Delta(a) = V_{\max}$) and uniformly-bad ($\Delta(a) = \Delta_{\min}$) landscapes respectively. The identity coordinate is therefore *coordinate-optimal among TV-only bounds*: any tighter bound on $R(Q)$ requires information beyond TV (the value-landscape spread).

**Scope.** [[#^thm-twosided-regret]] is per-state, one-step-improvement regret under fixed $M_t$ and C1 ([[#^sec-setup]]); [[#^sec-composition]] invokes it per round and combines with a variation-budget block argument for cumulative dynamic regret.

**Multi-step chain-rule compositionality.** $D_{\mathrm{KL}}(\pi^*_{1:T} \,\|\, Q_{1:T}) = -\sum_t \log Q_t(a^*_t \mid h_t)$ — the negative log-likelihood of the optimal trajectory, equivalently the *behavior-cloning loss against optimal-trajectory data*. Per-step identity coordinates compose additively via the chain rule.

### Strict improvement over Pinsker ^sec-pinsker-comparison

Pinsker \cite{tsybakov-2009-nonparametric, cover-thomas-2006-info-theory} (§§2.4, 11.6) gives $R(Q) \le V_{\max}\sqrt{D_{\mathrm{KL}}/2}$ without assuming deterministic $\pi^*$. Under the canonical scope, [[#^thm-twosided-regret]] lies strictly below it: (i) *linear vs.\ square-root* — $1 - e^{-D_{\mathrm{KL}}} < \sqrt{D_{\mathrm{KL}}/2}$ for $D_{\mathrm{KL}} > 0$; (ii) *Pinsker becomes vacuous for $D_{\mathrm{KL}} > 2$* (exceeds the trivial $V_{\max}$ envelope), while $V_{\max}(1 - e^{-D_{\mathrm{KL}}})$ saturates at $V_{\max}$. The same comparison against BH gives $V_{\max}(1 - e^{-D_{\mathrm{KL}}}) < V_{\max}\sqrt{1 - e^{-D_{\mathrm{KL}}}}$ on $(0, \infty)$. Worked numerical comparison: [[#^sec-pinsker-numerics]].

### Perturbative extension to $\epsilon$-stochastic optima; where Pinsker / BH are the right tool ^sec-perturbative-extension

Deterministic $\pi^*$ is canonical for finite-MDP RL with isolated optima \cite{lattimore-2020-bandit}; tied-optimum extensions ([[#^sec-tied-softmax-extensions]]). The deterministic regime is the unperturbed limit, not a hard wall:

> [!theorem] Perturbative identity for $\epsilon$-stochastic optima ^thm-perturbative-eps
> For $\epsilon$-greedy $\pi^*_\epsilon$ and any policy $Q$ with full-support lower bound $Q(a) \ge q_0 > 0$ for all $a \in \mathcal A$,
> $$\boxed{\;\operatorname{TV}(\pi^*_\epsilon, Q) \;=\; 1 - e^{-D_{\mathrm{KL}}(\pi^*_\epsilon \,\|\, Q)} + O\!\left(\epsilon \log(1/\epsilon)\right).\;}$$
> The correction vanishes uniformly (over $Q$ with full-support lower bound $q_0 > 0$) as $\epsilon \to 0$ and is sub-linear in $\epsilon$. For softmax-regularized $\pi^*_\tau \propto \exp(Q_O/\tau)$ with temperature $\tau$, the correction is $O(\tau^{-1}\exp(-\Delta_{\min}/\tau))$ — exponentially small with a polynomial prefactor (equivalently, $O(\exp(-c\Delta_{\min}/\tau))$ for any $c < 1$).

Derivation in [[#^sec-perturbative-derivation]]. The two-sided regret bound of [[#^thm-twosided-regret]] transfers with the same correction order. **Outside the perturbative regime** — genuinely high-entropy optima, tied-optimum, hard-exploration — the BH inequality $\operatorname{TV} \le \sqrt{1 - e^{-D_{\mathrm{KL}}}}$ is the relevant general bound; Pinsker is the textbook fallback.

### Direction of the divergence is forced ^sec-direction-forced

Reverse-KL is forced: forward-KL $D_{\mathrm{KL}}(Q \,\|\, \pi^*) = +\infty$ whenever $Q$ has off-optimum mass (since $\pi^*(a) = 0$ for $a \neq a^*$). Within the reverse direction, reverse-KL is *uniquely* selected by the chain-rule additivity axiom \cite{hobson-1969-new-theorem, csiszar-1991-why-ls-maxent} — the only smooth $f$-divergence decomposing additively over factorizations ([[#^sec-chain-rule-uniqueness]]). Detailed direction-forcing argument and admissible-divergence comparison: [[#^sec-admissible-divergence]] and [[#^sec-direction-forcing]].
