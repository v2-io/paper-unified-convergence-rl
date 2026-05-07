## Proofs of the Key Lemmas ^sec-key-lemma-proofs

This appendix gives full proofs of the four key lemmas surfaced in [[#^sec-mechanism]], plus the perturbative extension and the ProST sector-level reduction.

### Proof of Key Lemma 1 (Point-mass reverse-KL/TV identity) ^sec-proof-key-lemma-1

For deterministic $\pi^* = \delta_{a^*}$ and any policy $Q$ (extending $D_{\mathrm{KL}} = +\infty$ when $Q(a^*) = 0$, which gives $1 - e^{-D_{\mathrm{KL}}} = 1 = \operatorname{TV}$ trivially):

The reverse-KL collapses under the point-mass:
$$D_{\mathrm{KL}}(\delta_{a^*} \,\|\, Q) \;=\; \sum_a \delta_{a^*}(a) \log\frac{\delta_{a^*}(a)}{Q(a)} \;=\; \log\frac{1}{Q(a^*)} \;=\; -\log Q(a^*),$$
where the convention $0 \log 0 = 0$ disposes of the $a \neq a^*$ terms. Combined with the textbook total-variation calculation
$$\operatorname{TV}(\delta_{a^*}, Q) \;=\; \tfrac12 \sum_a |\delta_{a^*}(a) - Q(a)| \;=\; \tfrac12 \bigl(|1 - Q(a^*)| + \sum_{a \neq a^*} Q(a)\bigr) \;=\; \tfrac12 \bigl((1 - Q(a^*)) + (1 - Q(a^*))\bigr) \;=\; 1 - Q(a^*),$$
we have $-\log Q(a^*) = -\log(1 - \operatorname{TV})$. Solving for TV gives $\operatorname{TV} = 1 - e^{-D_{\mathrm{KL}}}$. $\square$

**Strict-improvement substitution.** Substituting the identity into the Bretagnolle–Huber inequality $\operatorname{TV}(P, Q) \le \sqrt{1 - e^{-D_{\mathrm{KL}}(P \,\|\, Q)}}$ at $P = \delta_{a^*}$ yields $1 - e^{-D_{\mathrm{KL}}} \le \sqrt{1 - e^{-D_{\mathrm{KL}}}}$, which holds *strictly* on $D_{\mathrm{KL}} > 0$ since $x < \sqrt{x}$ on $(0, 1)$. The identity therefore supplies the *exact* TV value at the deterministic-π* corner and lies strictly below the BH envelope there.

**Two-sided regret bound** (stated as Component 2's two-sided characterization in [[#^sec-four-components]] and as Key Lemma 1's *Per-round regret bound* paragraph in [[#^sec-key-lemma-1]]). From the regret expression $R(Q) = \sum_{a \neq a^*} Q(a) \Delta(a)$ and $\Delta(a) \le V_{\max}$:
$$R(Q) \;\le\; V_{\max} \sum_{a \neq a^*} Q(a) \;=\; V_{\max} \bigl(1 - Q(a^*)\bigr) \;=\; V_{\max} \operatorname{TV}(\delta_{a^*}, Q) \;=\; V_{\max} (1 - e^{-D_{\mathrm{KL}}}).$$
The matching action-gap lower bound (full statement in [[#^sec-aux-action-gap]]) gives
$$R(Q) \;=\; \sum_{a \neq a^*} Q(a) \Delta(a) \;\ge\; \Delta_{\min} \sum_{a \neq a^*} Q(a) \;=\; \Delta_{\min} \operatorname{TV}(\delta_{a^*}, Q) \;=\; \Delta_{\min} (1 - e^{-D_{\mathrm{KL}}}).$$
Combining gives the two-sided characterization. The bound is *Lipschitz-equivalent* with constants $\Delta_{\min}/V_{\max}$ and $1$, both achieved on extremal value landscapes — therefore *coordinate-optimal among bounds depending only on TV*. $\square$

### Proof of the perturbative extension — ε-stochastic and softmax-regularized ^sec-perturbative

We establish the perturbative identity via a uniform perturbation argument rather than an alignment-specific calculation.

**ε-greedy stochastic optimum.** Let $\pi^*_\epsilon(a^*) = 1 - \epsilon$ and $\pi^*_\epsilon(a) = \epsilon/(|\mathcal A| - 1)$ for $a \neq a^*$, and assume the full-support lower bound $Q(a) \ge q_0 > 0$ for all $a \in \mathcal A$. (Full support is required since $\pi^*_\epsilon$ has positive mass on every action; without $Q(a) > 0$ on the full support, $D_{\mathrm{KL}}(\pi^*_\epsilon \,\|\, Q) = +\infty$.)

*Step 1 — TV is Lipschitz under the point-mass perturbation.* $\operatorname{TV}(\pi^*_\epsilon, \delta_{a^*}) = \epsilon$. By the triangle inequality for total variation,
$$\bigl|\operatorname{TV}(\pi^*_\epsilon, Q) - \operatorname{TV}(\delta_{a^*}, Q)\bigr| \;\le\; \operatorname{TV}(\pi^*_\epsilon, \delta_{a^*}) \;=\; \epsilon,$$
uniformly in $Q$ — no alignment hypothesis needed.

*Step 2 — Reverse-KL admits a uniform expansion.* Direct computation:
$$
\begin{aligned}
D_{\mathrm{KL}}(\pi^*_\epsilon \,\|\, Q) - D_{\mathrm{KL}}(\delta_{a^*} \,\|\, Q)
&= (1 - \epsilon) \log\frac{1 - \epsilon}{Q(a^*)} + \tfrac{\epsilon}{|\mathcal A| - 1} \sum_{a \neq a^*} \log\frac{\epsilon/(|\mathcal A| - 1)}{Q(a)} - \bigl(-\log Q(a^*)\bigr) \\
&= (1 - \epsilon) \log(1 - \epsilon) + \epsilon \log Q(a^*) + \epsilon \log\tfrac{\epsilon}{|\mathcal A| - 1} - \tfrac{\epsilon}{|\mathcal A| - 1} \sum_{a \neq a^*} \log Q(a),
\end{aligned}
$$
where the second equality collects the $\log Q(a^*)$ contributions via $-(1 - \epsilon) \log Q(a^*) + \log Q(a^*) = \epsilon \log Q(a^*)$ and splits the constant log term off the conditional sum. Expanding $(1 - \epsilon) \log(1 - \epsilon) = -\epsilon + O(\epsilon^2)$ and $\epsilon \log(\epsilon/(|\mathcal A| - 1)) = \epsilon \log \epsilon - \epsilon \log(|\mathcal A| - 1)$, the leading-order behavior is
$$D_{\mathrm{KL}}(\pi^*_\epsilon \,\|\, Q) = D_{\mathrm{KL}}(\delta_{a^*} \,\|\, Q) + \epsilon \log \epsilon - \epsilon \bigl(\log(|\mathcal A| - 1) + 1\bigr) + \epsilon \log Q(a^*) - \tfrac{\epsilon}{|\mathcal A| - 1} \sum_{a \neq a^*} \log Q(a) + O(\epsilon^2).$$
Under $Q(a) \ge q_0$, each $|\log Q(a)| \le \log(1/q_0)$ is uniformly bounded; the dominant correction is the $\epsilon \log \epsilon$ leading term, giving
$$\bigl|D_{\mathrm{KL}}(\pi^*_\epsilon \,\|\, Q) - D_{\mathrm{KL}}(\delta_{a^*} \,\|\, Q)\bigr| = O\bigl(\epsilon \log(1/\epsilon) + \epsilon \log(1/q_0)\bigr) = O(\epsilon \log(1/\epsilon))$$
absorbing $\epsilon \log(1/q_0)$ into the leading $\epsilon \log(1/\epsilon)$ term as $\epsilon \to 0$ (constants depend on $q_0$ and $|\mathcal A|$).

*Step 3 — Map through $1 - e^{-x}$ via Lipschitzness.* The map $x \mapsto 1 - e^{-x}$ is $1$-Lipschitz on $[0, \infty)$, so
$$\bigl|(1 - e^{-D_{\mathrm{KL}}(\pi^*_\epsilon \,\|\, Q)}) - (1 - e^{-D_{\mathrm{KL}}(\delta_{a^*} \,\|\, Q)})\bigr| = O(\epsilon \log(1/\epsilon)).$$
Combining Steps 1 and 3 with the unperturbed identity $\operatorname{TV}(\delta_{a^*}, Q) = 1 - e^{-D_{\mathrm{KL}}(\delta_{a^*} \,\|\, Q)}$:
$$\operatorname{TV}(\pi^*_\epsilon, Q) \;=\; 1 - e^{-D_{\mathrm{KL}}(\pi^*_\epsilon \,\|\, Q)} + O(\epsilon \log(1/\epsilon))$$
uniformly in $Q$ over the class $\{Q : Q(a) \ge q_0\}$, with constants depending on $q_0$ and $|\mathcal A|$. This establishes the ε-greedy form of the perturbative identity. $\square$

**Softmax-regularized optimum.** Let $\pi^*_\tau(a) \propto \exp(Q_O(a)/\tau)$. Under isolated optimum with action gap $\Delta_{\min}$, the off-optimum mass concentrates exponentially in $1/\tau$:
$$1 - \pi^*_\tau(a^*) \;=\; \frac{\sum_{a \neq a^*} \exp(-(Q_O(a^*) - Q_O(a))/\tau)}{1 + \sum_{a \neq a^*} \exp(-(Q_O(a^*) - Q_O(a))/\tau)} \;\le\; (|\mathcal A| - 1) \exp(-\Delta_{\min}/\tau).$$
Treating $\pi^*_\tau$ as $\pi^*_\epsilon$ with effective $\epsilon = O(\exp(-\Delta_{\min}/\tau))$, the ε-greedy expansion above applies with
$$\epsilon \log(1/\epsilon) \;=\; O\bigl((\Delta_{\min}/\tau) \exp(-\Delta_{\min}/\tau)\bigr) \;=\; O\bigl(\tau^{-1} \exp(-\Delta_{\min}/\tau)\bigr),$$
exponentially small with a polynomial prefactor (equivalently, $O(\exp(-c \Delta_{\min}/\tau))$ for any fixed $c \in (0, 1)$ since the exponential dominates the polynomial). $\square$

**Two-sided regret bound under perturbation.** Composing with the TV-regret bounds:
$$\Delta_{\min}\bigl(1 - e^{-D_{\mathrm{KL}}(\pi^*_\epsilon \,\|\, Q)}\bigr) - O(\epsilon \log(1/\epsilon)) \;\le\; R(Q) \;\le\; V_{\max}\bigl(1 - e^{-D_{\mathrm{KL}}(\pi^*_\epsilon \,\|\, Q)}\bigr) + O(\epsilon \log(1/\epsilon)).$$

### Proof of Key Lemma 2 (Multi-factor forgetting prerequisite) ^sec-proof-key-lemma-2

We work in the continuous-time idealization of Model (Σ): $\dot{\boldsymbol\delta}_\Sigma = -\mathbf F(\boldsymbol\delta_\Sigma) + \mathbf w$ with $\mathbf F(\boldsymbol\delta_\Sigma) := (\alpha_{ij}^{\mathrm{ss}} \delta_{ij})_{(i,j) \in E}$ the sector correction and $\mathbf w$ the deterministic disturbance with $\|\mathbf w\| \le \rho_\Sigma$. The discrete-time form recovers the same scaling up to absorbable constants via an AM–GM expansion of $\|\boldsymbol\delta_\Sigma - \mathbf F + \mathbf w\|^2$ (sketched below).

For the Lyapunov function $V(\boldsymbol\delta_\Sigma) := \|\boldsymbol\delta_\Sigma\|^2$,
$$\dot V \;=\; 2\,\boldsymbol\delta_\Sigma^\top \dot{\boldsymbol\delta}_\Sigma \;=\; -2\,\boldsymbol\delta_\Sigma^\top \mathbf F(\boldsymbol\delta_\Sigma) \;+\; 2\,\boldsymbol\delta_\Sigma^\top \mathbf w.$$
The first term is the *sector product*, bounded below by the bottleneck:
$$\boldsymbol\delta_\Sigma^\top \mathbf F(\boldsymbol\delta_\Sigma) \;=\; \sum_{(i,j)} \alpha_{ij}^{\mathrm{ss}} \delta_{ij}^2 \;\ge\; \min_{(i,j)} \alpha_{ij}^{\mathrm{ss}} \cdot \|\boldsymbol\delta_\Sigma\|^2 \;=\; \mathcal T_\Sigma^{\mathrm{bn,ss}} \cdot \|\boldsymbol\delta_\Sigma\|^2,$$
with the bound tight: setting $\boldsymbol\delta_\Sigma$ proportional to $\mathbf e_{(i^*, j^*)}$ where $(i^*, j^*) \in \arg\min_{(i,j)} \alpha_{ij}^{\mathrm{ss}}$ saturates it. The cross-term is bounded by Cauchy–Schwarz:
$$|2\,\boldsymbol\delta_\Sigma^\top \mathbf w| \;\le\; 2\,\|\boldsymbol\delta_\Sigma\| \cdot \|\mathbf w\| \;\le\; 2\,\rho_\Sigma\, \|\boldsymbol\delta_\Sigma\|.$$
Combining:
$$\dot V \;\le\; -2\,\mathcal T_\Sigma^{\mathrm{bn,ss}}\,\|\boldsymbol\delta_\Sigma\|^2 \;+\; 2\,\rho_\Sigma\,\|\boldsymbol\delta_\Sigma\| \;=\; -2\,\|\boldsymbol\delta_\Sigma\| \cdot \bigl(\mathcal T_\Sigma^{\mathrm{bn,ss}}\,\|\boldsymbol\delta_\Sigma\| - \rho_\Sigma\bigr).$$
The drift is strictly negative whenever $\|\boldsymbol\delta_\Sigma\| > \rho_\Sigma / \mathcal T_\Sigma^{\mathrm{bn,ss}}$; the trajectory is ultimately bounded by $R_\Sigma^* = \rho_\Sigma / \mathcal T_\Sigma^{\mathrm{bn,ss}}$. So whenever $\mathcal T_\Sigma^{\mathrm{bn,ss}} > \rho_\Sigma / R_\Sigma$, we have $R_\Sigma^* \le R_\Sigma$ and the modeled mismatch is ultimately bounded within the strategic reserve. This is the standard Khalil sector-Lyapunov ultimate-boundedness setup with the cross-term retained \cite{khalil-2002-nonlinear} (Section 9.2; comparison-principle form). $\square$

*Discrete-time form.* For the discrete update $\boldsymbol\delta_\Sigma^+ = \boldsymbol\delta_\Sigma - \mathbf F(\boldsymbol\delta_\Sigma) + \mathbf w$ with $\alpha_{ij}^{\mathrm{ss}} \in [0,1]$ and AM–GM with parameter $\eta = \mathcal T_\Sigma^{\mathrm{bn,ss}}/(2(1 - \mathcal T_\Sigma^{\mathrm{bn,ss}}))$, the steady-state bound is $\|\boldsymbol\delta_\Sigma^*\| \le 2 \rho_\Sigma / \mathcal T_\Sigma^{\mathrm{bn,ss}}$ — same scaling, factor-of-2 absorbable into the threshold constant.

*Mean-square corollary.* If instead $\mathbf w$ is interpreted as zero-mean stochastic disturbance with $\mathbb E[\|\mathbf w\|^2] \le \rho_\Sigma^2$, the cross-term $\mathbb E[2\,\boldsymbol\delta_\Sigma^\top \mathbf w \mid \boldsymbol\delta_\Sigma] = 0$ vanishes in expectation, and the drift inequality gives mean-square ultimate boundedness $\sqrt{\mathbb E[V^*]} \le \rho_\Sigma / \sqrt{2\,\mathcal T_\Sigma^{\mathrm{bn,ss}}}$ under threshold $\mathcal T_\Sigma^{\mathrm{bn,ss}} > \rho_\Sigma^2 / (2 R_\Sigma^2)$. The deterministic-disturbance form above is the operative one for [[#^thm-composition]](A2) since non-stationarity-induced drift is structurally adversarial-aligned (consistently moving mismatch in one direction) rather than zero-mean.

**Sharpness.** When the inequality reverses ($\mathcal T_\Sigma^{\mathrm{bn,ss}} \le \rho_\Sigma / R_\Sigma$), the adversarial disturbance $\mathbf w^*(t) = \rho_\Sigma\, \boldsymbol\delta_\Sigma(t) / \|\boldsymbol\delta_\Sigma(t)\|$ saturates Cauchy–Schwarz and gives $d\|\boldsymbol\delta_\Sigma\|/dt \ge -\mathcal T_\Sigma^{\mathrm{bn,ss}}\,\|\boldsymbol\delta_\Sigma\| + \rho_\Sigma$. By the comparison principle, $\|\boldsymbol\delta_\Sigma(t)\|$ exceeds any $R < \rho_\Sigma / \mathcal T_\Sigma^{\mathrm{bn,ss}}$ in finite time. The bottleneck-element refinement: pick $\mathbf w$ and $\boldsymbol\delta_\Sigma$ both supported on $(i^*, j^*) \in \arg\min_{(i,j)} \alpha_{ij}^{\mathrm{ss}}$ with $|w_{i^* j^*}| = \rho_\Sigma$ — saturates the sector inequality on the contracting side and aligns with $\mathbf w$ on the disturbance side simultaneously. The threshold is *sharp inside the diagonal sector model*. The theorem is silent about non-diagonal correction architectures or stabilization mechanisms outside Model (Σ). $\square$

### Proof of the impulsive ProST sector-level reduction ^sec-proof-prost-impulsive

Within Model (Σ), idealize ProST \cite{lee-2023-prost-tempo} as an impulsive system: between scheduled update times $\{t_1, \ldots, t_K\} \subset [0, T]$ the agent's policy is held fixed and the strategic mismatch evolves under disturbance budget $\rho_\Sigma$ alone (continuous-destabilizing in the Lyapunov $V = \|\boldsymbol\delta_\Sigma\|^2$); at each update time $t_i$ the policy is revised, contracting the modeled mismatch by per-update impulse gain $\gamma \in (0, 1]$ so $\|\boldsymbol\delta_\Sigma(t_i^+)\| \le (1 - \gamma) \|\boldsymbol\delta_\Sigma(t_i^-)\|$. Then $V(t_i^+) \le (1 - \gamma)^2 V(t_i^-)$.

This fits the Hespanha–Liberzon–Teel \cite{hespanha-2008-lyapunov} (Theorem 1) framework for impulsive systems with destabilizing continuous evolution and stabilizing impulses (the *reverse-ADT* branch). The continuous-evolution rate is $\lambda_c = \rho_\Sigma / R_\Sigma$ (from linearizing the disturbance contribution to $\dot V \approx +\lambda_c V + \gamma_c(\rho_\Sigma)$). The impulse contraction in $V$ is $\mu = (1 - \gamma)^2$. The reverse-ADT condition for ultimate boundedness — impulses cannot be too sparse — gives
$$\Delta_{\max} \cdot \lambda_c \;<\; -\ln \mu \quad\Longleftrightarrow\quad \Delta_{\max} \cdot \rho_\Sigma / R_\Sigma \;<\; -\ln(1 - \gamma)^2,$$
where $\Delta_{\max} := \sup_i (t_{i+1} - t_i)$ is the longest inter-update gap. Under the condition the modeled mismatch is ultimately bounded within $R_\Sigma^* \approx \rho_\Sigma \Delta_{\max} / \gamma$ (leading order in $\gamma$).

Under uniform blocks $\Delta_i = T/K$ the condition reduces to $K/T > \rho_\Sigma / (2 \gamma R_\Sigma)$ to leading order in $\gamma$ — recovering ProST's $K/T$ form with the impulse gain made explicit. Under nonuniform schedules the impulsive form is *strictly stronger*: the longest block sets the threshold, not the average. The lemma is rigorous for the modeled impulsive sector dynamics; the per-update impulse gain $\gamma$ is a domain quantity (depends on within-block sample size and update-time discount) and is left abstract. $\square$

### Proof of Key Lemma 3 (Loop generates interventional samples) ^sec-proof-key-lemma-3

By temporal ordering and the singular-trajectory commitment of [[#^sec-preliminaries]], $a_t$ causally precedes $o_{t+1}$. We invoke Pearl's back-door criterion \cite{pearl-2009-causality} (Theorem 3.3.2):
$$P(o_{t+1} \mid \mathrm{do}(a_t), H_t) \;=\; P(o_{t+1} \mid a_t, H_t) \quad \text{whenever}\quad (o_{t+1} \perp\!\!\!\perp a_t \mid H_t)_{G_{\overline{a_t}}},$$
where $G_{\overline{a_t}}$ denotes the graph $G$ with all incoming arrows to $a_t$ removed. In the singular-trajectory + agent-as-policy graph of [[#^sec-preliminaries]], the only arrows into $a_t$ come from $H_t$ (the agent's policy is $\pi_t(a_t \mid H_t)$); deleting them severs the only confounding path from $a_t$ to $o_{t+1}$. Whether $H_t$ d-separates $a_t$ from $o_{t+1}$ in $G_{\overline{a_t}}$ is precisely the content of (C2). The identity $P(o_{t+1} \mid \mathrm{do}(a_t), H_t) = P(o_{t+1} \mid a_t, H_t)$ holds on the support where $\pi_t(a_t \mid H_t) > 0$, which is automatic since $a_t$ was drawn from $\pi_t$. $\square$

*Remark on equivalent forms of (C2).* The d-separation condition $(o_{t+1} \perp a_t \mid H_t)_{G_{\overline{a_t}}}$ is equivalent to the potential-outcome form $a_t \perp Y^{(a_t)} \mid H_t$ (sequential ignorability in the Robins/Murphy sense), and to the truncated-factorization condition $P(\mathbf U \mid a_t, H_t) = P(\mathbf U \mid H_t)$ where $\mathbf U$ are the latent variables jointly affecting $a_t$ and $o_{t+1}$ — applied to the *prior* on $\mathbf U$ in the do-marginalization, not to the conditional on $o_{t+1}$. The three forms are interchangeable; the d-separation form is the most direct handle for the proof above.

*On the role of (C1) and (C3).* The identification proof above is load-bearing on (C2) alone: (C1)'s full-support positivity and (C3)'s known-action-mechanism are *automatic* under the paper's architecture — (C1) reduces to realized-action positivity ($\pi_t(a_t \mid H_t) > 0$ tautologically since $a_t$ was drawn from $\pi_t$); (C3) reduces to agent-policy-queryability ($Q_t$ is the agent's own policy, internally accessible). Both are stated as named conditions for forward-compatibility with off-policy / counterfactual extensions where they would do real work; for the on-policy identification claim of this lemma, (C2) is the substantive condition.

### Proof of Key Lemma 4 (Bias bound) ^sec-proof-key-lemma-4

On the matching event $\{a^*_{\mathrm{ag},t}(s) = \tilde a^*_t(s)\}$, the difference $\hat D_t(s) - D_t^{\mathrm{true}}(s)$ vanishes identically (both quantities equal $-\log Q_t$ at the same action). On the complementary misidentification event, the support condition $Q_t(\cdot \mid s) \ge q_0$ at *both* $a^*_{\mathrm{ag},t}(s)$ and $\tilde a^*_t(s)$ implies $\log Q_t(a^*_{\mathrm{ag},t}(s) \mid s) \in [\log q_0, 0]$ and $\log Q_t(\tilde a^*_t(s) \mid s) \in [\log q_0, 0]$, so their absolute difference is at most $\log(1/q_0)$. Therefore
$$\bigl|\hat D_t(s) - D_t^{\mathrm{true}}(s)\bigr| \;\le\; \mathbf 1[a^*_{\mathrm{ag},t}(s) \ne \tilde a^*_t(s)] \cdot \log(1/q_0).$$
Taking expectations: $\mathbb E[\lvert \hat D_t(s) - D_t^{\mathrm{true}}(s) \rvert] \le (1 - p_{\mathrm{id}}(s)) \log(1/q_0)$ by definition of $p_{\mathrm{id}}(s)$. $\square$

**High-probability form.** With probability $1 - \delta$ over a single round's identification event, $\bigl|\hat D_t(s) - D_t^{\mathrm{true}}(s)\bigr| \le \log(1/q_0) \cdot \mathbf 1[\mathrm{misid event}]$ where $\Pr[\mathrm{misid event}] = 1 - p_{\mathrm{id}}(s)$. The bias decomposes cleanly: in Regime A ($p_{\mathrm{id}}(s) \to 1$) it vanishes; in Regime B ($p_{\mathrm{id}}(s) \in (0,1)$) it is controlled by misidentification mass; in Regime C ($p_{\mathrm{id}}(s) \to 0$) it saturates at $\log(1/q_0)$. This proves Key Lemma 4. $\square$

**Per-state, per-horizon-step lift (used in [[#^thm-composition]](v)).** The lemma applies pointwise at any visited state $s_h$ along a horizon-$N_h$ trajectory: $\mathbb E[\lvert \hat D_t(s_h) - D_t^{\mathrm{true}}(s_h) \rvert] \le (1 - p_{\mathrm{id}}(s_h)) \log(1/q_0)$, with $p_{\mathrm{id}}(s_h)$ the per-state argmax-correctness probability. Aggregating linearly over the horizon and assuming the per-state floor $p_{\mathrm{id}} := \min_s p_{\mathrm{id}}(s)$, the per-round bias contribution to the trajectory-level value gap is at most $N_h (1 - p_{\mathrm{id}}) \log(1/q_0)$, contributing the second term of [[#^thm-composition]](v).

**Remarks.**
- Both $a^*_{\mathrm{ag},t}(s)$ and $\tilde a^*_t(s)$ must satisfy $Q_t \ge q_0$ for the $\log(1/q_0)$ ceiling to apply; the support condition is on *both* points, not just the agent's estimate. This is *strictly weaker* than the perturbative identity's full-support condition $Q(a) \ge q_0$ for *all* $a$ ([[#^sec-perturbative]]) — the perturbative identity needs full support because $\pi^*_\epsilon$ has positive mass on every action; the bias bound here needs only two-point support.
- This is *not* an estimation-from-samples lemma. If $Q_t$ is unavailable internally and must be estimated from on-policy rollouts, an additional concentration argument (Hoeffding under iid fixed-policy samples, or martingale concentration for adaptive policies) is needed; in the architectures this paper considers, $Q_t$ is the agent's policy and is internally available.
