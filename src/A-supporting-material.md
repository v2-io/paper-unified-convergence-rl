## Supporting Material for the Main Components ^sec-supporting-material

### Convention hierarchy: C1, C2, C3 ^sec-convention-hierarchy

Three named continuation conventions form a hierarchy of increasing diagnostic power and computational cost.

**C1 — One-step improvement (canonical default).** $\pi_{\mathrm{cont}} = \pi_{\mathrm{current}}$. Each action is evaluated assuming current behavior continues afterward. No fixed-point computation, no global-optimality assumption. Cheapest; weakest diagnostic.

**C2 — Receding-horizon ($N_r$-step replanning).** At each future step, re-optimize over a horizon of $N_r$ steps using the model available at that step:
$$\pi_{\mathrm{RH}}(M_\tau) \;=\; \arg\max_\pi V_O(M_\tau, \pi;\, N_r)\quad \text{applied at each } \tau.$$
Captures multi-step recovery: a goal that appears unattainable under frozen continuation may be reachable with replanning. Moderate cost; moderate diagnostic.

**C3 — Bellman.** $\pi_{\mathrm{cont}} = \pi^*$ where $\pi^* = \arg\max_\pi V_O(M_t, \pi; N_h)$. The continuation is the optimal policy — a fixed-point equation. Strongest diagnostic; most expensive.

**Monotonicity.** For any model $M_t$, horizon $N_h$, and policy class $\Pi$:
$$A_O^{(1)}(M_t;\, \Pi, N_h) \;\le\; A_O^{\mathrm{RH}}(M_t;\, \Pi, N_r, N_h) \;\le\; A_O^{\mathrm{B}}(M_t;\, \Pi, N_h).$$

> [!proof]
> C1 freezes continuation at $\pi_{\mathrm{current}}$ (possibly suboptimal); C2 re-optimizes periodically, so $\pi_{\mathrm{RH}} \succeq \pi_{\mathrm{current}}$ at each future step; C3 uses the globally optimal $\pi^*$. A weakly better continuation yields a weakly higher expected trajectory value; taking the supremum over the first action preserves the ordering.

**Corollary.** $\delta_{\mathrm{sat}}^{\mathrm B} \le \delta_{\mathrm{sat}}^{\mathrm{RH}} \le \delta_{\mathrm{sat}}^{(1)}$ (since $\delta_{\mathrm{sat}} = V_O^{\min} - A_O$, higher $A_O$ means lower $\delta_{\mathrm{sat}}$). And $\delta_{\mathrm{regret}}^{(1)} \le \delta_{\mathrm{regret}}^{\mathrm{RH}} \le \delta_{\mathrm{regret}}^{\mathrm B}$ (since $\delta_{\mathrm{regret}} = A_O - V_O(M_t, \pi_{\mathrm{current}}; N_h)$, higher $A_O$ means higher $\delta_{\mathrm{regret}}$).

C1 is the most conservative diagnostic (most likely to diagnose "locally unattainable"); C3 is the most accurate (least false "unattainable" diagnoses). The $2{\times}2$ diagnostic structure is preserved under all three; only the inferential force varies.

### Admissible-divergence family for the regret bound ^sec-admissible-divergence

The reverse-KL direction is forced by the deterministic-$\pi^*$ regime ([[#^sec-direction-forced]]). Within the direction-forced family, multiple smooth $f$-divergences yield valid regret bounds:

| Divergence | Bound on $R(Q)$ | Tightness | Finite under det.\ $\pi^*$? |
|---|---|---|---|
| $\operatorname{TV}(\pi^*, Q)$ | $V_{\max} \cdot \operatorname{TV}$ | Tight (extremal $V$) | Yes |
| $D_{\mathrm{KL}}(\pi^* \,\|\, Q)$ via Pinsker | $V_{\max} \sqrt{D_{\mathrm{KL}}/2}$ | Loose by $\sqrt{\cdot}$ | Yes |
| $D_{\mathrm{KL}}(\pi^* \,\|\, Q)$ via point-mass identity | $V_{\max}(1 - e^{-D_{\mathrm{KL}}})$ | Tight (this paper) | Yes |
| $\chi^2(\pi^* \,\|\, Q)$ (Le Cam) | $V_{\max} \cdot \tfrac12 \sqrt{\chi^2}$ | Looser than Pinsker | $\chi^2 = 1/Q(a^*) - 1$ |
| $D_\alpha(\pi^* \,\|\, Q)$ (Rényi, $\alpha \ge 1$) | Various | Interpolates | Yes for $\alpha \ge 1$ |
| $D_{\mathrm{KL}}(Q \,\|\, \pi^*)$ (forward-KL) | $+\infty$ | Vacuous | No |

Reverse-KL is selected uniquely within the direction-forced family by the chain-rule axiom ([[#^sec-chain-rule-uniqueness]]). The point-mass identity supplies the *exact* bound on reverse-KL under deterministic $\pi^*$; the table reflects different bound shapes on the same divergence (with the BH inequality $V_{\max}\sqrt{1 - e^{-D_{\mathrm{KL}}}}$ as the looser general form to which the identity reduces outside the deterministic-$\pi^*$ corner).

### Direction-forcing argument ^sec-direction-forcing

For deterministic $\pi^* = \delta_{a^*}$ and any $Q$ with $Q(a) > 0$ for some $a \neq a^*$:
$$D_{\mathrm{KL}}(Q \,\|\, \pi^*) \;=\; \sum_a Q(a) \log\frac{Q(a)}{\pi^*(a)} \;=\; \sum_{a \neq a^*} Q(a) \log\frac{Q(a)}{0} \;=\; +\infty.$$
A bound "$R \le +\infty$" is vacuous. The reverse direction $D_{\mathrm{KL}}(\pi^* \,\|\, Q)$ is finite (and equal to $-\log Q(a^*)$) whenever $Q(a^*) > 0$. The asymmetry is forced by the regime: regret is a one-sided quantity (contributes only from $Q$'s off-optimum mass; $\pi^*$ has no support off $a^*$); divergences whose role is to bound this one-sided quantity must themselves be one-sided. Symmetric divergences (squared Hellinger, Jensen-Shannon, symmetrized KL) introduce a vacuous symmetric term.

### Action-gap matching lower bound ^sec-action-gap-lower-bound

For any $Q$ with $\Delta_{\min} = \min_{a \neq a^*} \Delta(a) > 0$:
$$R(Q) \;=\; \sum_{a \neq a^*} Q(a) \Delta(a) \;\ge\; \Delta_{\min} \sum_{a \neq a^*} Q(a) \;=\; \Delta_{\min} \cdot (1 - Q(a^*)) \;=\; \Delta_{\min} \cdot \operatorname{TV}(\pi^*, Q).$$
By the point-mass identity ([[#^thm-pointmass-identity]]), $\operatorname{TV}(\pi^*, Q) = 1 - e^{-D_{\mathrm{KL}}(\pi^* \,\|\, Q)}$, giving the matching lower bound of [[#^thm-twosided-regret]].

The lower bound is tight when sub-optimal actions are uniformly bad ($\Delta_{\min} = \max_{a \neq a^*} \Delta(a)$). For typical landscapes the gap between upper and lower bound is $V_{\max} - \Delta_{\min}$, controlled by the *spread* of action gaps.

### Tied-optimum and softmax-smoothed extensions ^sec-tied-softmax-extensions

**Tied-optimum.** If $\pi^*$ has support on a tied-optimum set $\mathcal A^* = \{a : Q_O(a) = Q_O(a^*)\}$ with uniform mass, reverse-KL is finite whenever $Q$ covers $\mathcal A^*$. The regret bound extends:
$$R(Q) \;=\; \sum_{a \notin \mathcal A^*} Q(a) \Delta(a) \;\le\; V_{\max} \cdot \mathbb P_Q(a \notin \mathcal A^*).$$
Pinsker applies unchanged. A multi-action analog of the point-mass identity holds with $\pi^*(a^*) = 1/|\mathcal A^*|$ replacing the point-mass form, yielding $D_{\mathrm{KL}}(\pi^* \,\|\, Q) = -\sum_{a \in \mathcal A^*} (1/|\mathcal A^*|) \log(|\mathcal A^*| Q(a))$, a multi-action analog of $-\log Q(a^*)$. The two-sided bound becomes one-sided in general; the upper bound holds with the modified KL form.

**Softmax-smoothed $\pi^*$ with temperature $\tau \to 0$.** Replace $\pi^* = \delta_{a^*}$ with $\pi^*_\tau \propto \exp(Q_O / \tau)$. As $\tau \to 0$, $\pi^*_\tau \to \delta_{a^*}$. For finite $\tau$, the *perturbative* identity of [[#^thm-perturbative-eps]] applies: $\operatorname{TV}(\pi^*_\tau, Q) = 1 - e^{-D_{\mathrm{KL}}(\pi^*_\tau \,\|\, Q)} + O(\tau^{-1}\exp(-\Delta_{\min}/\tau))$ — exponentially small with polynomial prefactor. The two-sided regret bound transfers with the same correction order. (Derivation in [[#^sec-perturbative-derivation]].) Outside the perturbative regime — for genuinely high-entropy optima — the BH inequality $\operatorname{TV} \le \sqrt{1 - e^{-D_{\mathrm{KL}}}}$ becomes the relevant general bound; Pinsker also applies, with a one-sided regret form.

### Perturbative identity for $\epsilon$-stochastic and softmax-regularized optima ^sec-perturbative-derivation

We establish [[#^thm-perturbative-eps]] via a uniform perturbation lemma rather than an alignment-specific calculation.

**$\epsilon$-greedy stochastic optimum.** Let $\pi^*_\epsilon(a^*) = 1-\epsilon$, $\pi^*_\epsilon(a) = \epsilon/(|\mathcal A|-1)$ for $a \neq a^*$, and assume the full-support lower bound $Q(a) \ge q_0 > 0$ for all $a \in \mathcal A$. (This is required since $\pi^*_\epsilon$ has positive mass on every action — without $Q(a) > 0$ on the full support, $D_{\mathrm{KL}}(\pi^*_\epsilon \,\|\, Q) = +\infty$.)

**Step 1 — TV is Lipschitz under the point-mass perturbation.** $\operatorname{TV}(\pi^*_\epsilon, \delta_{a^*}) = \epsilon$, so by the triangle inequality
$$\bigl|\operatorname{TV}(\pi^*_\epsilon, Q) - \operatorname{TV}(\delta_{a^*}, Q)\bigr| \le \operatorname{TV}(\pi^*_\epsilon, \delta_{a^*}) = \epsilon.$$
This holds *uniformly in $Q$*, with no alignment hypothesis.

**Step 2 — Reverse-KL admits a uniform expansion.** Direct computation:
$$
\begin{aligned}
D_{\mathrm{KL}}(\pi^*_\epsilon \,\|\, Q) - D_{\mathrm{KL}}(\delta_{a^*} \,\|\, Q)
&= (1-\epsilon)\log(1-\epsilon) - \log\!\frac{Q(a^*)}{Q(a^*)} \\
&\quad + \tfrac{\epsilon}{|\mathcal A|-1}\sum_{a \neq a^*}\!\Bigl(\log\!\tfrac{\epsilon}{|\mathcal A|-1} - \log Q(a)\Bigr) - 0 \\
&\quad + \epsilon\log Q(a^*) - \log Q(a^*)\bigl((1-\epsilon)-1\bigr).
\end{aligned}
$$
After cancellations, the leading-order behavior is
$$D_{\mathrm{KL}}(\pi^*_\epsilon \,\|\, Q) = D_{\mathrm{KL}}(\delta_{a^*} \,\|\, Q) + \epsilon\log\epsilon - \epsilon\bigl(\log(|\mathcal A|-1) + 1\bigr) + \tfrac{\epsilon}{|\mathcal A|-1}\sum_{a \neq a^*}(-\log Q(a)) + O(\epsilon^2).$$
Under $Q(a) \ge q_0$, the term $-\log Q(a) \le \log(1/q_0)$ is uniformly bounded; the dominant correction is $\epsilon\log\epsilon$, giving
$$\bigl|D_{\mathrm{KL}}(\pi^*_\epsilon \,\|\, Q) - D_{\mathrm{KL}}(\delta_{a^*} \,\|\, Q)\bigr| = O\bigl(\epsilon \log(1/\epsilon) + \epsilon \log(1/q_0)\bigr) = O(\epsilon\log(1/\epsilon))$$
absorbing $\epsilon \log(1/q_0)$ into the leading $\epsilon\log(1/\epsilon)$ term as $\epsilon \to 0$ (constants depend on $q_0$ and $|\mathcal A|$).

**Step 3 — Map through $1 - e^{-x}$ via Lipschitzness.** The map $x \mapsto 1 - e^{-x}$ is 1-Lipschitz on $[0, \infty)$, so
$$\bigl|(1 - e^{-D_{\mathrm{KL}}(\pi^*_\epsilon \,\|\, Q)}) - (1 - e^{-D_{\mathrm{KL}}(\delta_{a^*} \,\|\, Q)})\bigr| = O(\epsilon\log(1/\epsilon)).$$
Combining with Step 1 and the unperturbed identity $\operatorname{TV}(\delta_{a^*}, Q) = 1 - e^{-D_{\mathrm{KL}}(\delta_{a^*} \,\|\, Q)}$:
$$\operatorname{TV}(\pi^*_\epsilon, Q) = 1 - e^{-D_{\mathrm{KL}}(\pi^*_\epsilon \,\|\, Q)} + O(\epsilon\log(1/\epsilon))$$
uniformly in $Q$ over the class $\{Q : Q(a) \ge q_0\}$, with constants depending on $q_0$ and $|\mathcal A|$. This establishes the $\epsilon$-greedy form of [[#^thm-perturbative-eps]]. $\square$

**Softmax-regularized optimum.** Let $\pi^*_\tau(a) \propto \exp(Q_O(a)/\tau)$. Under isolated optimum with action gap $\Delta_{\min}$, the off-optimum mass concentrates exponentially in $1/\tau$:
$$1 - \pi^*_\tau(a^*) = \frac{\sum_{a \neq a^*} \exp(-(Q_O(a^*) - Q_O(a))/\tau)}{1 + \sum_{a \neq a^*} \exp(-(Q_O(a^*) - Q_O(a))/\tau)} \le (|\mathcal A|-1)\exp(-\Delta_{\min}/\tau).$$
Treating $\pi^*_\tau$ as $\pi^*_\epsilon$ with effective $\epsilon = O(\exp(-\Delta_{\min}/\tau))$, the $\epsilon$-greedy expansion above applies with $\epsilon\log(1/\epsilon) = O((\Delta_{\min}/\tau)\exp(-\Delta_{\min}/\tau)) = O(\tau^{-1}\exp(-\Delta_{\min}/\tau))$, exponentially small with a polynomial prefactor. (Equivalently, $O(\exp(-c\Delta_{\min}/\tau))$ for any fixed $c \in (0, 1)$ since the exponential dominates the polynomial.) $\square$

**Two-sided regret bound under perturbation.** Composing [[#^thm-perturbative-eps]] with the TV-regret bounds of [[#^sec-tv-regret-bound]]:
$$\Delta_{\min}\bigl(1 - e^{-D_{\mathrm{KL}}(\pi^*_\epsilon \,\|\, Q)}\bigr) - O(\epsilon\log(1/\epsilon)) \;\le\; R(Q) \;\le\; V_{\max}\bigl(1 - e^{-D_{\mathrm{KL}}(\pi^*_\epsilon \,\|\, Q)}\bigr) + O(\epsilon\log(1/\epsilon)).$$
The two-sided bound transfers from [[#^thm-twosided-regret]] with the same correction order; the regret-vs-identity-coordinate Lipschitz equivalence holds up to perturbative correction.

### Strategic-tempo consistency across canonical topologies ^sec-tempo-topologies

The bottleneck strategic tempo $\mathcal T_\Sigma^{\mathrm{bn,ss}}$ of [[#^thm-forgetting-prereq]] is verified — and the resulting per-topology threshold derived — across four canonical topologies under Beta-Bernoulli edge updates with per-element forgetting:

- **B.1 — Single edge.** $\mathcal T_\Sigma^{\mathrm{bn,ss}} = \nu \cdot \iota \cdot (1-\lambda)$. All three factors load-bearing; setting any one to zero collapses the bottleneck.
- **B.2 — Two-edge AND chain, observable intermediate.** $\mathcal T_\Sigma^{\mathrm{bn,ss}} = \min\bigl(\nu_1(1-\lambda_1),\ \nu_1\theta_1(1-\lambda_2)\bigr)$. Edge 2's effective observation rate is gated by edge 1's success probability $\theta_1$ — *depth-gated attenuation*. For depth-$d$ chains, the bottleneck is $\min_k \prod_{j<k}\theta_j (1-\lambda_k)$.
- **B.3 — Two-edge AND chain, unobservable intermediate.** Per-edge tempo ill-defined; plan-level bottleneck $\mathcal T_{\Sigma, \mathrm{plan}}^{\mathrm{bn,ss}} = \nu(1-\lambda_\Phi)$ over a single tracked plan-level quantity $\hat\Phi = p_1 p_2$.
- **B.4 — Two-arm OR node, $\varepsilon$-greedy.** $\mathcal T_\Sigma^{\mathrm{bn,ss}} = \min\bigl((1-\varepsilon)(1-\lambda_1),\ \varepsilon(1-\lambda_2)\bigr)$. Action selection controls rate allocation; pure greedy ($\varepsilon = 0$) collapses the unexplored arm's bottleneck — *exploration-gated*.

> [!todo] Sub-label inconsistency
> The "B.1"–"B.4" labels above are sub-labels within §A.7, not within Appendix B. Source carries "B." rather than "A.7." prefix — looks like a labeling drift. Per-paper agent may want to renumber as "A.7.1"–"A.7.4" for clarity.

The structural decomposition: AND-chains exhibit *depth-gated* geometric attenuation $\nu_k = \nu \prod_{j < k} \theta_j$; OR-nodes exhibit *exploration-gated* allocation. Mixed AND/OR DAGs interleave both. Each topology surfaces a different factor as the binding bottleneck.

### Loop-as-causal-engine: three deployment modes ^sec-loop-deployment-modes

The Pearl-$do$ structure of closed-loop interventional access manifests at distinct layers with semantically distinct interventional mechanisms:

- **Mode 1 — agent-self-intervention.** The agent performs $do$-actions on its own action space as part of its ordinary adaptive loop. Intervention is on the agent's own action; target is the environment's response. This is [[#^thm-loop-level2]]'s primary content.
- **Mode 2 — observer-on-sub-agent.** An observer external to a composite performs $do$-interventions on one sub-agent's action space; target is another sub-agent's response. Reveals cross-coupling structure that component-marginal observation cannot identify.
- **Mode 3 — observer-on-agent-input** (sketched, future work). An observer intervenes on the agent's observation channel; target is the agent's subsequent policy. Useful for offline architectural-class diagnosis.

Modes share the Pearl-$do$ structure but differ in who intervenes on what. The unification is at the pattern level; the mechanism is semantically distinct per layer.
