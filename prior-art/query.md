# Undermind Query — B-CS1 Unified RL Convergence Theory Under Non-Stationarity

**Research question:** Has any prior RL or online-learning framework unified non-stationary convergence theory by composing four specific structural elements: (1) a two-gap diagnostic separating environmental constraint from agent suboptimality, (2) the Bretagnolle-Huber identity giving exact (not inequality) regret bounds under deterministic optimum, (3) strategic tempo / cycle-rate analysis tying convergence to per-update information acquisition, (4) closed-loop interventional access making regret bounds learnable from on-policy data?

This is a focused prior-art investigation seeking a SPECIFIC COMPOSITION — most of the constituents are themselves studied (regret bounds; non-stationary RL; information-theoretic analysis of bandits/RL), but it is the COMPOSITION giving a unified non-stationarity-aware theory, with all four properties simultaneously, that is sought.

== The claim ==

RL has many regret bounds (UCB-V variants, Thompson sampling under drift, RLSVI, posterior sampling, online RL) but typically: (a) assumes stationarity, (b) lacks explicit metric structure on policy space, (c) lacks connection to satisfaction-gap diagnostic separating "environment doesn't permit it" from "agent suboptimality," (d) treats regret bounds as proven-but-not-learnable.

In Adaptation and Actuation Dynamics (AAD), four independently-derived results compose into a unified RL convergence story under non-stationarity:

1. **Two-Gap Diagnostic Separation** (`#satisfaction-gap`, `#control-regret`): rigorously separates evaluation of the goal (Satisfaction Gap = ideal minus best-achievable, "the world doesn't permit it") from evaluation of the current plan (Control Regret = best-achievable minus current, "you're not doing it well enough"). The 2×2 disambiguation table routes four regimes to four distinct corrective actions. Active inference's preferences-as-priors form *collapses* this distinction; AAD preserves it. Convention hierarchy monotonicity (C1 one-step / C2 receding-horizon / C3 Bellman) gives strict inferential-force ordering.

2. **Bretagnolle-Huber Identity for Strategic Regret Under Deterministic Optimum**: under deterministic $\pi^*$, the BH identity $D_{\mathrm{KL}}(\pi^* \| Q) = -\log(1 - \mathrm{TV}(\pi^*, Q))$ holds *exactly* (not as inequality), yielding the tight two-sided regret bound $\Delta_{\min}(1 - e^{-D_{\mathrm{KL}}}) \leq R \leq V_{\max}(1 - e^{-D_{\mathrm{KL}}})$. **Strictly sharper than Pinsker**, factor-of-2 improvement. The reverse-KL direction in AAD's strategy-cost objective ($\pi^*$-first) is forced by the regret-bound derivation: forward-KL is vacuous under deterministic $\pi^*$. Rubin 2012 Theorem 3 PAC-Bayes generalization gives finite-sample guarantees.

3. **Strategic Tempo $\mathcal{T}_\Sigma$** (`#schema-strategy-persistence`, `#strategic-tempo`): the rate of useful $\Sigma_t$ revision (rate of edge-credence updates with non-trivial information content). Plays the role of $\mathcal{T}$ in the strategic analog of the persistence condition $(1-\lambda) > \rho_\Sigma/R_\Sigma$. Verified across four canonical topologies (linear chain, balanced tree, unbalanced tree, full DAG with feedback). The forgetting prerequisite forces $1-\lambda > \rho_\Sigma/R_\Sigma$ as structural requirement.

4. **Loop-as-Causal-Engine** (`#scope-agent-identity`): closed feedback loops automatically generate Pearl Level-2 (interventional) data by structural construction. The agent's action causally precedes the next observation, so $o_{t+1}$ is a sample from $P(o \mid \text{do}(a_t))$, not $P(o)$. This makes the regret bound *learnable*: the agent generates the interventional data needed to estimate it, without external experimentation infrastructure.

The composition gives a **non-stationarity-aware convergence theory** with three properties no existing RL framework has all of: handles non-stationarity (via tempo / forgetting prerequisite); has explicit metric structure on policy space (via BH identity / Pinsker / Wasserstein); makes regret bound learnable (via Loop-Level-2 access).

== Distinctive features compared to existing RL convergence literature ==

- **Non-stationarity is structural, not bolt-on**: the persistence condition $(1-\lambda) > \rho_\Sigma/R_\Sigma$ is a structural requirement, not a slowly-changing-environment assumption.
- **Two-gap separation is built into the diagnostic**: most RL regret bounds collapse satisfaction-gap and control-regret into a single regret quantity.
- **Exact (not inequality) regret under deterministic $\pi^*$**: BH identity gives equality, not just an upper bound. This matters when assumptions on policy support are violated.
- **Causal access is automatic**: regret bounds usually assume access to "true" rewards or transitions; closed-loop derivation gives this as a structural property.

== What I'm looking for ==

(a) Has any prior framework UNIFIED RL convergence under non-stationarity using a composition with all four properties above (or three out of four with the fourth as adjacent literature)?

(b) Has the BH identity been used in RL regret bounds (vs. just used in non-RL statistical estimation)? Specifically, has the deterministic-$\pi^*$ exact-equality version been deployed for RL?

(c) Has the satisfaction-gap vs control-regret distinction been rigorously formalized as a 2×2 diagnostic in any non-AAD framework?

(d) Has strategic tempo (rate of useful policy/strategy revision) been studied as a quantity tying convergence rate to information acquisition rate, rather than as a side observation?

(e) Has the closed-loop-as-Pearl-Level-2 connection been used to give *learnable* regret bounds, rather than just analytical regret bounds?

== Adjacent formal frameworks worth checking ==

1. **NON-STATIONARY MAB / RL** (Cheung-Simchi-Levi-Zhu 2019 "Non-Stationary Stochastic Optimization"; Russac-Vernade-Cappé 2019 weighted linear bandits; Auer-Gajane-Ortner 2019 sliding-window UCB; Garivier-Moulines 2008 piecewise-stationary MAB). The most directly relevant non-stationary regret-bound literature.

2. **THOMPSON SAMPLING / POSTERIOR SAMPLING UNDER DRIFT** (Russo-Van Roy 2014, 2016; Osband-Russo-Van Roy 2013 randomized least-squares value iteration). Posterior sampling has been adapted for non-stationary settings; has the metric structure been made explicit?

3. **ONLINE RL REGRET BOUNDS** (Auer-Cesa-Bianchi-Fischer 2002 UCB; Jaksch-Ortner-Auer 2010 UCRL; Bartlett-Tewari 2009 REGAL). Standard regret-bound RL.

4. **INFORMATION-THEORETIC REGRET BOUNDS** (Russo-Van Roy 2016 "An Information-Theoretic Analysis of Thompson Sampling"; Kveton et al. on entropy regularization). Closest existing work using information-theoretic tools for RL regret.

5. **CAUSAL RL** (Bareinboim-Pearl line; Lu-Schölkopf-Hardt 2018; Zhang-Bareinboim 2018; Lattimore-Hutter 2014 multi-armed bandits with structural causal models). Causal RL exists; has it been combined with non-stationarity convergence?

6. **PAC-BAYES IN RL** (Rubin-Shamir-Tishby 2012; Maurer-Pontil 2009; Tolstikhin-Seldin 2013). PAC-Bayes for finite-sample regret bounds.

7. **POLICY-SPACE METRICS** (Schulman et al. TRPO 2015 KL-divergence trust regions; Pajarinen-Kyrki 2014 natural-gradient; Wasserstein-RL line). Metric structure on policies.

8. **REINFORCEMENT LEARNING AS PROBABILISTIC INFERENCE** (Levine 2018 tutorial and review; Toussaint-Storkey 2006; Kappen 2005; Levine 2018 explicitly contrasts the "control as inference" lineage with classical RL).

9. **PARTIAL OBSERVABILITY / POMDP CONVERGENCE** (Kaelbling-Littman-Cassandra 1998). Non-Markov settings adjacent to non-stationary settings.

10. **STREAMING / ONLINE LEARNING WITH DRIFT** (Hoeffding race; Tieleman-Hinton; Fishburn-Davis on streaming convex optimization). Statistical-learning treatments of drift adjacent to RL.

11. **BRETAGNOLLE-HUBER LITERATURE** (Bretagnolle-Huber 1979; subsequent uses). The identity is classical; specific RL applications are the question.

12. **TWO-ENVELOPE / SATISFACTION-VS-REGRET DECOMPOSITION** (Loomes-Sugden 1982 regret theory; Bell 1982; subsequent decision-theoretic literature). Has the satisfaction-vs-regret distinction been formalized in any decision-theoretic framework?

== What's already known at targeted-search depth ==

- Non-stationary RL regret bounds exist for specific models (sliding-window UCB, weighted linear bandits, etc.) but typically without the two-gap structure or strategic-tempo quantification.
- BH identity is classical (1979); its specific use as a regret-bound derivation in non-stationary RL has not been surfaced at targeted depth.
- Causal RL (Bareinboim-Pearl line) gives interventional structure but typically does not address non-stationarity.
- Information-theoretic regret bounds exist (Russo-Van Roy 2016) but use Shannon entropy regularization, not the satisfaction-gap separation.
- The "decades-open question" framing (a unified non-stationarity-aware regret-bound theory with all four properties) is informal AAD-internal characterization; the question is whether the field has formally posed and answered it.

== What I want from Undermind ==

A structured prior-art map covering:
1. **Direct anticipation**: any framework with all four properties simultaneously
2. **Closest 3-out-of-4 candidates**: which RL frameworks have three of the four properties with the fourth as adjacent literature
3. **BH-identity in RL**: specific applications of the deterministic-$\pi^*$ exact-equality form (vs. the inequality form)
4. **Satisfaction-vs-regret decomposition**: any prior decision-theoretic or RL formalization of the same 2×2 disambiguation
5. **Strategic-tempo analogs**: RL or adaptive-control quantities playing the same role
6. **Closed-loop Level-2 access in regret-bound derivations**: whether anyone has used it to make regret bounds *learnable*

Output format: same as the separability-ladder report — direct answer, closest prior-art parallels (ranked), domain-specific findings, naming candidates, evidence register, search scope statement, bottom line.
