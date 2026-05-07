# De Novo Audit - 2026-05-07

Paper: `A Unified Convergence Theory for Non-Stationary Reinforcement Learning`

Scope: first-hand read of the built PDF plus source markdown in `src/re/`. I did not do external bibliographic verification. I focused on theorem correctness, assumption visibility, and reviewer-facing overclaim risk.

## Overall Read

The point-mass reverse-KL/TV identity is correct and useful as a clean coordinate at the deterministic-optimum corner. The higher-risk parts are the pieces composed around it: the strategic-tempo Lyapunov proof, the conversion of identifiability bias into value regret, and the base-learner instantiation of the TV/KL coordinate. The theorem is also honest in one place that the four-component composition is a bundle rather than one integrated necessity theorem; the abstract and introduction should carry that same honesty.

## Findings

### H1 - The strategic-tempo Lyapunov proof mixes deterministic and mean-square disturbance scalings

Source: model statement at `src/re/05-mechanism.md:20-27`; proof at `src/re/B-key-lemma-proofs.md:64-74`; theorem use at `src/re/04-main-result.md:17-19`, `src/re/04-main-result.md:30`, `src/re/04-main-result.md:42`.

The model states a discrete-time expected update with bounded disturbance:

`E[Delta delta_ij | delta_ij] = -alpha_ij delta_ij + w_ij`

and `sum w_ij^2 <= rho_Sigma^2`. The proof then writes

`E[Delta V | delta] = -2 delta^T F(delta) + E[||w||^2]`

and obtains negative drift for `V > rho_Sigma^2 / (2 T_Sigma)`. That threshold corresponds to a radius scaling like `rho_Sigma / sqrt(T_Sigma)`, not `rho_Sigma / T_Sigma`. The text then jumps to `R*_Sigma = rho_Sigma / T_Sigma`, which is the deterministic bounded-disturbance scaling from a cross-term argument.

There are two plausible ways to strengthen this, but the current proof is between them:

- Deterministic/adversarial disturbance: write the dynamics as `Delta delta = -F(delta) + w`, keep the cross term `2 delta^T w`, and recover a radius of order `rho/T`.
- Mean-square martingale disturbance: assume zero-mean noise with second moment `rho^2`; keep the displayed drift proof and state the radius/threshold with the `sqrt` scaling it actually gives.

Until this is fixed, the strategic-tempo threshold `T_Sigma^{bn,ss} > rho_Sigma/R_Sigma` is not proved from the displayed Model (S).

### H2 - The identifiability bias term has units/scale mismatch in the regret theorem

Source: theorem at `src/re/04-main-result.md:44-49`; proof sketch at `src/re/05-mechanism.md:70-72`; appendix proof at `src/re/A-proof-of-composition.md:45-49`; bias lemma at `src/re/B-key-lemma-proofs.md:96-106`.

Key Lemma 4 bounds an error in KL coordinates:

`|Dhat - Dtrue| <= log(1/q0)`.

The theorem adds `N_h (1-p_id) log(1/q0) T` directly to dynamic regret, which is a value-scale quantity. To convert KL-coordinate error into regret, the proof needs at least the map `D -> 1-exp(-D)` plus the TV-to-value factor. A natural bound would look like a clipped value-scale term, for example with a `V_max` factor or a `min{1, log(1/q0)}` after the `1-exp(-D)` Lipschitz/clipping step.

As written, the bias term can have the wrong units and can exceed the trivial per-state value range without the theorem saying it is clipped.

### H3 - `V_max` and the extra `N_h` factor are ambiguous and may double-count horizon

Source: `V_max` definition at `src/re/03-preliminaries.md:15`; simulation-lemma step at `src/re/05-mechanism.md:62-64`; full proof at `src/re/A-proof-of-composition.md:29-33`; theorem at `src/re/04-main-result.md:48-49`.

`V_max(M_t)` is defined as the range of the horizon-level `Q_O` object. The simulation-lemma proof then bounds every per-step bracket by `V_max` and sums over `N_h`, producing `V_max * N_h`. If `V_max` is already a horizon-value range, this can double-count the horizon. If `V_max` is intended to be a per-step range, the definition should say so.

Suggested fix: choose one convention and make the theorem follow it:

- Per-step reward range: use `V_step <= 1` inside the simulation lemma and get the standard `N_h` factor.
- Horizon `Q` range: use it once, without multiplying it by another `N_h`, or define a step-indexed `V_{max,h}` and sum those.

This also affects the lifted `N_h^2 sqrt(SA...)` claim in `src/re/04-main-result.md:61-65` and `src/re/06-conclusion.md:9`.

### H4 - `B_T` as optimum-change count is not enough to define stationary blocks

Source: variation definition at `src/re/03-preliminaries.md:7`; theorem A5 at `src/re/04-main-result.md:36`; proof block decomposition at `src/re/05-mechanism.md:66`; appendix notation at `src/re/A-proof-of-composition.md:5`.

The paper defines `B_T` as the number of optimum-action changes. The proof partitions into blocks and then says the MDP is stationary within each block. Those are different notions. An MDP can change rewards/transitions while the optimal action remains the same; a restart-on-change learner may still need to relearn values, confidence sets, or occupancy distributions even though `a*_t` did not change.

Suggested fix: define the block count as environmental stationary-segment count, then optionally relate it to optimum-change count under an added assumption. If the intended result only pays for optimum changes, the proof needs an argument that non-optimum-changing environmental drift does not affect the base learner's TV coordinate or regret.

### H5 - The base-learner instantiation does not yet satisfy the theorem's policy-coordinate assumptions

Source: A1 at `src/re/04-main-result.md:28`; A5 at `src/re/04-main-result.md:36`; bandit claim at `src/re/05-mechanism.md:74`; algorithm appendix at `src/re/D-algorithm.md:5-7`.

The theorem's coordinate requires a policy distribution `Q_t` with `Q_t(a*_t | s) > 0`. The algorithm appendix says Thompson sampling and UCB satisfy

`E[1 - Q_t(a*)] = O(log t/(t Delta_min))`.

For Thompson sampling, `Q_t` can plausibly be the posterior action-sampling distribution. For vanilla UCB, the deployed policy is typically deterministic; if it chooses a non-optimal action, then `Q_t(a*) = 0`, the reverse KL is infinite, and A1 fails. UCRL2/UCBVI have the same issue unless the policy is smoothed or `Q_t` is redefined as an internal randomized/planning distribution.

Suggested fix: either restrict the base-learner example to randomized policies with explicit full-support floors, or add a smoothing wrapper and show its regret cost. If using UCB anyway, define exactly what `Q_t` is and prove it satisfies A1/A5.

### H6 - The theorem's headline rate omits the condition that the linear bias term vanishes

Source: intro rate at `src/re/01-introduction.md:13-15`; theorem at `src/re/04-main-result.md:48-49`; unpacking at `src/re/04-main-result.md:63`; scope at `src/re/01-introduction.md:41`.

The abstract/intro headline gives the sublinear square-root rate. The theorem itself has a second term linear in `T` unless `p_id -> 1` fast enough. Section 4 does admit that the bias term is vacuous as a rate outside Regime A or very high-identifiability Regime B. This condition should be attached to the headline rate everywhere it appears.

Suggested fix: state the rate as "in Regime A, or when the identifiability bias term is negligible." Otherwise the theorem is a bound but not a convergence rate.

### M1 - "Interventional by construction" depends on a strong ignorability assumption

Source: intro/contribution at `src/re/01-introduction.md:33`; component statement at `src/re/04-main-result.md:21`; lemma at `src/re/05-mechanism.md:37-47`; proof at `src/re/B-key-lemma-proofs.md:86-92`; conclusion caveat at `src/re/06-conclusion.md:15`.

The loop-Level-2 lemma is standard and useful under C1-C3, but C2 is doing heavy work: the information state must block unobserved confounding of action selection. The phrase "the loop generates interventional data by construction" can make it sound as if temporal ordering alone is sufficient. The later conclusion correctly notes that coupled-goal architectures need additional machinery for C2.

Suggested fix: use "interventional under sequential ignorability" in the abstract/introduction, and move the C2 caveat forward. This avoids a likely causal-inference reviewer objection.

### M2 - The "four-component unified theory" claim should mirror the paper's own bundle caveat

Source: intro at `src/re/01-introduction.md:13-17`, composition at `src/re/01-introduction.md:37`, theorem discussion at `src/re/04-main-result.md:67-79`.

Section 4.4 is admirably clear that the rate goes through A5 plus the point-mass identity alone, while the other components add persistence, learnability, and diagnostic routing. The abstract and introduction present the composition more monolithically.

Suggested fix: describe the contribution as a "compatible bundle of guarantees" rather than a single theorem in which all four components are necessary for the rate. This is not a downgrade; it makes the actual theorem easier to defend.

### M3 - The perturbative extension needs its support constants visible

Source: perturbative statement at `src/re/05-mechanism.md:16`; proof at `src/re/B-key-lemma-proofs.md:50-60`.

The `epsilon`-greedy extension is uniform only over `Q(a) >= q0`, with constants depending on `q0` and `|A|`. If `q0` decays with time, the perturbative correction can be much less benign than the main text suggests. Add "for fixed full-support floor `q0`" to the main-text statement.

## Trim-Adjacent Opportunities

- The NeurIPS Theory Track guideline quote in `src/re/01-introduction.md:23` is not doing theorem work and may invite reviewer meta-discussion. It can be cut.
- The BH/Pinsker positioning is repeated in abstract, intro, Section 4, Section 5, Appendix E, and conclusion. One or two of those can be shortened after the identity is established.
- Section 4.4's "why exactly four?" paragraph is useful, but it can be made shorter once the abstract is adjusted to "bundle of guarantees."

## Suggested Triage

1. Repair the strategic-tempo Lyapunov proof and choose deterministic-vs-mean-square scaling.
2. Fix the bias term's value-scale conversion.
3. Clarify `V_max` convention and horizon factors.
4. Redefine `B_T` as stationary-segment changes, or prove optimum-change blocks suffice.
5. Add explicit full-support/randomized-policy assumptions to the base-learner instantiation.
