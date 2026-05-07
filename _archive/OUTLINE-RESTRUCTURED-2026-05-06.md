# OUTLINE-RESTRUCTURED.md — Body + appendix shape for `src/re/`

*Drafted 2026-05-06 after first-hand reading of Jin 2020 (LSVI-UCB) and OUTLINE-STRATEGY.md's 24 examples drawn from both Jin 2020 and Jin 2018 (Q-learning UCB). The structure imitates the Jin pattern: narrative-gap intro → minimal preliminaries → main result with intuition-before-theorem and narrative-unpacking → mechanism as proof-story → conclusion. The four-component framing lives **inside the body's main-result and mechanism sections** as ingredients of the composition, not as one-section-per-component as in the current `src/`.*

*Purpose: surface this for Joseph to review and revise before authoring `src/re/` segments against it. Strengthens-cheatsheet (`src/_recovery/STRENGTHENS-CHEATSHEET.md`) governs what claim-language survives recovery prose. Truth above all.*

---

## Body shape (~8 pages target before page-budget enforcement)

### §1 Introduction (~1.5 pp)

**Narrative gap setup.** Open with the four-strand maturity of non-stationary RL theory: variation-budget dynamic regret; two-term decompositions along exploration-vs-adaptation axis; tempo-and-forgetting analyses; causal/interventional access (stationary throughout). Information-theoretic regret cuts across these tracks using entropy / mutual information / Pinsker / Hellinger; an exact reverse-KL/TV identity at the deterministic-π* corner — strictly tighter than both Pinsker and BH at this corner — has not been deployed as an upper-bound coordinate in the retrieved corpus.

**The gap.** No published framework composes the four tracks into a non-stationarity convergence theory that *also* has explicit metric structure on policy space *and* is learnable from on-policy data. Boxed/centered: *"Can we design a unified convergence theory for non-stationary RL that handles non-stationarity, has explicit metric structure on policy space, and is learnable from on-policy data?"*

**Informal main result.** "We present a composition that delivers all three properties jointly. Composing four structural elements — two-gap diagnostic, point-mass reverse-KL/TV identity, multi-factor strategic tempo with forgetting prerequisite, closed-loop interventional access — yields cumulative dynamic regret `Õ(N_h √((B_T+1) T))` in the piecewise-stationary regime, with per-round coordinate sharper than Pinsker and Bretagnolle–Huber. The technical anchor is a *point-mass reverse-KL/TV identity* at the deterministic-π* corner: an exact algebraic identity, `TV(δ_{a*}, Q) = 1 - exp(-D_KL(δ_{a*} ‖ Q))`, that lies strictly below both the Pinsker and BH envelopes there."

End-of-intro paragraph: brief preview of paper structure.

### §2 Related Work (~0.75 pp)

Conceptually grouped paragraphs with paragraph-headings (Jin 2020's `\paragraph{}` style):

- **Variation-budget dynamic regret.** Cheung-Simchi-Levi-Zhu 2020; Wei-Luo 2021; Mao et al.\ 2021; Gajane-Ortner-Auer 2019. Continuous-V_T regime; `Õ(T^{2/3})` rates. We recover the piecewise-stationary `√(B_T T)` corollary; the route through the per-round identity is new.
- **Two-term regret decompositions.** Long-Fei Li-Zhao-Zhou 2024; Fei-Yang-Wang-Xie 2020; Stradi et al.\ 2024. Decompose along exploration-vs-adaptation axis; ours decomposes along goal-feasibility-vs-policy-quality, a structurally distinct axis.
- **Tempo and forgetting.** Lee et al.\ 2023 (ProST); Lee et al.\ 2024 (Pausing); Touati-Vincent 2020; Russac et al.\ 2019; Garivier-Moulines 2008. Tempo as tunable hyperparameter; we lift to a structural survival inequality.
- **Causal / interventional access.** Junzhe Zhang-Bareinboim 2016, 2022; Lu-Meisami-Tewari 2021, 2022; Wang-Yang-Wang 2021 (DOVI); Junzhe Zhang 2020. Stationary throughout; we compose with non-stationarity.
- **Information-theoretic regret (cross-cutting).** Russo-Van Roy 2014a/b; Lu-Van Roy 2019; Min-Russo 2023; Lattimore-György 2021; Canonne 2022. Uses Pinsker / mutual information / Hellinger; the BH-form coordinate as upper-bound regret deployment is absent (App. F).

### §3 Preliminaries (~1.5 pp)

Strict minimalism. Only define what's needed for the main theorem:

- Episodic non-stationary MDP `(S, A, P_t, r_t, N_h)`, time-indexed transitions/rewards.
- Variation-budget `V_T` definition + piecewise-stationary specialization with `B_T` count of optimum-change events. Note the two regimes are distinct in general; this paper's results are for piecewise-stationary `B_T`.
- Round-`t` policy `Q_t(· | s)`; deterministic optimum `π*_t(· | s) = δ_{a*_t(s)}` (canonical scope).
- Value object, bounded value range `V_max`, action gap `Δ(a) := Q_O(a*) - Q_O(a)`, `Δ_min > 0` for isolated optimum.
- Strategic regret `R(Q) := Q_O(a*) - E_{a∼Q}[Q_O(a)]`.
- Singular causal trajectory commitment (`a_t` causally precedes `o_{t+1}`; one sentence, sets up loop-Level-2 in §5).
- `do(·)` operator from Pearl 2009 (one-line note).

Move to appendix: convention-hierarchy details (C1/C2/C3 monotonicity), receding-horizon and Bellman alternatives, full action-gap matching argument, tied-optimum and softmax extensions.

### §4 Main Result (~2.25 pp)

**Component intuition before the theorem (Jin's "algorithm + intuition" slot, here filled by motivation for the four ingredients):**

Four paragraphs, one per component, framing each as the *unique* source of one epistemic property:
- **Two-gap diagnostic** routes corrective-action interpretation (Component 1).
- **Point-mass reverse-KL/TV identity** supplies the metric layer (Component 2). Inline informal statement: under deterministic π*, `KL = -log Q(a*) = -log(1 - TV)`. Exact, two-line calculation; strictly tighter than Pinsker and BH at this corner.
- **Multi-factor strategic tempo with forgetting prerequisite** supplies non-stationarity persistence (Component 3). Inline informal statement: bottleneck `T_Σ^bn,ss := min_{(i,j)} ν_ij ι_ij (1-λ_ij) > ρ_Σ/R_Σ` is a structural survival inequality.
- **Closed-loop interventional access** supplies on-policy learnability (Component 4). Inline informal statement: under (C1)/(C2)/(C3), the loop generates Pearl Level-2 samples by construction.

**Composition Theorem (formal, in main).** Statement as a single labeled theorem with five conclusions (i)–(v) under five assumptions (A1)–(A5). The cumulative-rate conclusion (v) is the headline.

**Narrative unpacking** (Jin's Examples 12/13 pattern — full paragraphs, not bulleted Remarks):

- Theorem ... asserts that the per-round regret is bounded by the identity coordinate `V_max(1 - e^{-K_t(s)})`, sharper than Pinsker and BH at this corner. We emphasize that the per-round coordinate is *exact* under deterministic π*, not just an inequality.
- The strategic-tempo persistence (A2) gates non-stationarity tracking: without it, the modeled mismatch grows unboundedly under any positive disturbance — the structural-class theorem on `A_decay` (Theorem in App.) rules out every gain-decay update without forgetting.
- The loop-Level-2 access (A3) gates learnability: under (C1)/(C2)/(C3), the bound's RHS is computable from `Q_t` and the bias from misidentified argmax is controlled by `(1 - p_id) log(1/q_0)` per state per step.
- The two-gap diagnostic (A4) gates corrective-action interpretation: the `δ_sat / δ_regret` 2×2 routes "revise policy" vs. "revise model / policy class / horizon" vs. "revise objective."
- The cumulative rate (v) is in the *piecewise-stationary B_T family*: same `√((B_T+1) T)` block-shape as the restart-on-change variants of Cheung et al. 2020. *Not directly comparable* to the continuous-variation rate `Õ(SA V_T^{1/3} H T^{2/3})` of Mao et al.\ 2021 — different parameter, T-scaling, S/A-scaling, horizon-scaling. The two regimes coincide up to constants under abrupt-magnitude piecewise-stationarity.

**Necessity of the four components** (defensive paragraph, ~0.25 pp). Brief: each component supplies a distinct property. Drop Component 2 → Pinsker/BH coarsening. Drop Component 3 → unbounded mismatch. Drop Component 4 → linear-in-T bias, vacuous rate. Drop Component 1 → corrective-action routing fails. The unification is about the joint properties, not the rate alone (which goes through (A5)+(i)).

### §5 Mechanism (~2.25 pp)

**Proof strategy overview** (Jin's mechanism opening pattern). The cumulative rate (v) follows from four key lemmas chained together: per-round identity (Component 2) gives the per-round coordinate; persistence (Component 3) bounds the per-round mismatch under non-stationarity; loop-Level-2 (Component 4) controls the bias from misidentification; the two-gap diagnostic (Component 1) is structural setup. Block decomposition at optimum-change events plus Cauchy-Schwarz lifts the per-round coordinate to the cumulative rate.

**Key Lemma 1 (Point-Mass Identity).** Formal statement: `D_KL(δ_{a*} ‖ Q) = -log Q(a*) = -log(1 - TV(δ_{a*}, Q))`. Brief intuition (~1 paragraph): the reverse-KL collapses under the point-mass; the TV is `1 - Q(a*)` directly; combining gives the identity. Substituting into BH gives the strict-improvement claim. Full proof: App. B.

**Key Lemma 2 (Multi-Factor Forgetting Prerequisite).** Formal statement: within the diagonal sector model, ultimate boundedness of `‖δ_Σ‖` is sufficient under `T_Σ^bn,ss > ρ_Σ / R_Σ`. Sharpness inside the model. Brief intuition (~1 paragraph in narrative form, Jin's Example 15 style — no algebra): "The per-element correction rate is the *product* `ν · ι · (1-λ)` because each factor is an *independent gate* — observation-rate, identifiability, discount-rate. The bottleneck `min` (rather than the sum) enters the threshold because adversarial concentration of `δ_Σ` on the weakest element saturates the Lyapunov inequality at `min`, not `sum`." Full proof: App. B.

**Key Lemma 3 (Loop-Level-2 Access).** Formal statement: under (C1) positivity, (C2) sequential ignorability, (C3) known action mechanism, the loop generates samples from `P(o_{t+1} | do(a_t), H_t)`. Brief intuition: temporal ordering (`a_t` precedes `o_{t+1}`) plus information-state conditioning blocks the action-selection confounding pathway, so observational and interventional kernels coincide on `H_t`. Full proof: App. B.

**Key Lemma 4 (Bias Bound).** Formal statement: under two-point support `Q_t ≥ q_0` at agent's identified optimum and true optimum, `E[|hat D_t - D_t^true|] ≤ (1 - p_id) log(1/q_0)`. Brief intuition: indicator-decomposition over the misidentification event; on the matching event the difference vanishes, on the misidentification event both `-log Q_t` values lie in `[log q_0, 0]`. Full proof: App. B.

**Step-by-step combination** (Jin's Examples 16/17 pattern — narrative, no algebra in main):
- Step 1 — *Per-round bound via the identity.* Under deterministic π* (canonical scope), Key Lemma 1 + the textbook TV-regret bound give `R(Q_t) ≤ V_max(1 - e^{-K_t(s)})` per state. The per-state form, lifted via the simulation lemma to occupancy-weighted TV, gives `V^{π*_t} - V^{Q_t} ≤ V_max N_h \overline{TV}_t` for any starting state.
- Step 2 — *Block decomposition at optimum-change events.* Partition `[1, T]` into `B_T + 1` piecewise-stationary intervals at switch events. Within each interval, (A5) — restart-on-change base-learner — gives within-block cumulative `≤ 2c√L`.
- Step 3 — *Cauchy-Schwarz across blocks.* `Σ_i √Δ_i ≤ √((B_T+1) T)` from Cauchy-Schwarz. Multiplication by `V_max N_h` yields the trajectory-level rate term.
- Step 4 — *Bias term.* Key Lemma 4 aggregated over the `N_h` horizon steps and the `T` rounds contributes `N_h(1 - p_id) log(1/q_0) · T`. Vanishes in Regime A (`p_id → 1`).

Combining Steps 1–4 yields the cumulative rate `Õ(N_h √((B_T+1) T))` (modulo Regime-B/C bias). The full proof of (v) is in App. A.

**Failure modes referenced.** A deeper sketch of the persistence argument (Theorem in App. for `A_decay` structural-class) and the impulsive-system reduction for ProST (Lemma 5.2-equivalent in App.) is in App. C.

### §6 Conclusion (~0.5 pp)

**Wrap-up.** Connect back to §1's gap. We present a unified non-stationary RL convergence theory composing four structural elements; the joint guarantees deliver three properties (non-stationarity tracking + metric structure + on-policy learnability) that no published strand has individually. The point-mass identity at the deterministic-π* corner of the BH family is the technical anchor — absent, to our knowledge, from the prior RL/non-stationary corpus as an upper-bound coordinate.

**Concluding observations** (Jin's Example 19 pattern):
- *On the optimal dependencies on `N_h`, `S`, `A`.* The lifted rate `Õ(N_h^2 √(SA(B_T+1) T))` has linear-in-`N_h` per-round penalty (simulation-lemma) plus an additional `N_h` from the `c` constant in (A5). Bernstein-type concentration in the base learner could potentially shave one `√N_h` factor (analogous to `Azar-Osband-Munos 2017` for tabular).
- *On the continuous-variation extension.* The current rate is in the piecewise-stationary `B_T` family; lifting to continuous variation `V_T` would require a different block structure (variation-aware windows) and is open.
- *On joint uniqueness.* §C establishes the metric layer is uniquely reverse-KL under chain-rule additivity. Joint uniqueness across all three properties (non-stationarity / metric / learnable) would require further axioms (singular-trajectory + sector-boundedness + identifiability conditions) — open.
- *On coupled-goal architectures.* (C2)'s sequential ignorability assumes architectural separation between `M_t` and goal state; goal-conditioned LLM policies and similar coupled-goal architectures need additional machinery (Bruineberg et al.\ 2022).

**Practitioner takeaways** (Jin's Example 18 pattern):
1. The *point-mass identity at the deterministic-π* corner* is a corner-case sharpening of BH that hasn't been deployed in RL regret upper bounds. Where deterministic π* applies (most tabular finite-MDP regret analyses), the identity yields exact regret-vs-KL Lipschitz equivalence rather than the loose Pinsker / BH inequalities.
2. The *multi-factor strategic tempo* lifts ProST's tempo-as-hyperparameter framing into a structural survival inequality with three independent factors (observation rate, identifiability, discount rate). Operationalizable via variation-budget pre-checks + adaptive base-learner choice.
3. The *closed-loop interventional access* makes regret bounds learnable on-policy under sufficient identifiability — Regime A vanishes the bias; Regimes B/C carry quantified corrections.

### References (~1–2 pp)

Auto-generated via `bin/refs emit`; bracketed superscript via natbib `super,sort&compress`.

---

## Appendix shape

Reorganized from current A–G to a Jin-style by-proof-purpose layout:

### App. A. Proof of the Composition Theorem 7.1 (~3–5 pp)

Notation paragraph. Formal restatement of Theorem 7.1 (with full (A1)–(A5) and (i)–(v)). Step-by-step proof for each conclusion:
- (i) per-round bound — via Key Lemma 1 + TV-regret bound (App. B)
- (ii) ultimate boundedness — via Key Lemma 2's persistence (App. B)
- (iii) bias bound — via Key Lemma 4 (App. B)
- (iv) 2×2 routing — via the diagnostic structure (definitional)
- (v) cumulative rate — via simulation-lemma + Cauchy-Schwarz block decomposition

### App. B. Proofs of the Key Lemmas (~3–5 pp)

- B.1 Proof of Key Lemma 1 (Point-Mass Identity): the two-line calculation, the strict-improvement-over-BH substitution, the Lipschitz-equivalence two-sided regret bound (current Theorem 4.2).
- B.2 Proof of Theorem 4.3 perturbative ε-stochastic identity: Steps 1–3 (Lipschitz TV under perturbation, reverse-KL uniform expansion, mapping through `1 - e^{-x}`). The corrected algebra from `8d82b13`.
- B.3 Proof of Key Lemma 2 (Forgetting Prerequisite): bottleneck-vs-sum Lyapunov derivation; sharpness inside the model. Hespanha-Liberzon-Teel 2008 reverse-ADT for the impulsive-system Lemma 5.2 form (ProST sector reduction).
- B.4 Proof of Key Lemma 3 (Loop-Level-2): conditioning-on-`H_t`-blocks-confounding argument; coincidence of conditional and interventional kernels.
- B.5 Proof of Key Lemma 4 (Bias Bound): indicator-decomposition + concentration.

### App. C. Auxiliary mathematical material (~2–4 pp)

- C.1 Convention hierarchy (C1/C2/C3 with monotonicity).
- C.2 Admissible-divergence family + chain-rule uniqueness (Theorem C.1, Hobson 1969 / Csiszár 1991 / Aczél-Daróczy 1975).
- C.3 Direction-forcing argument.
- C.4 Action-gap matching lower bound.
- C.5 Tied-optimum and softmax-smoothed extensions.
- C.6 `A_decay` structural-class theorem (universal failure of gain-decay updates).
- C.7 Strategic-tempo across canonical topologies (T1)–(T4): single edge, AND-chain, OR-node, Regime-C contamination.
- C.8 Three deployment modes of loop-Level-2 (agent-self / observer-on-sub-agent / observer-on-input).

### App. D. Algorithm sketches and base-learner instantiation (~1–2 pp)

- D.1 Base-learner achieving (A5) — Thompson sampling (bandit), UCB, UCRL2/UCBVI (MDP). Identity-tight up to log factors.
- D.2 Multi-factor forgetting schedule choice; variation-budget adaptive choice [Wei-Luo 2021].
- D.3 Identifiability check (Regime A/B/C).
- D.4 Two-gap diagnostic readout.

### App. E. Pinsker numerical comparison (~1 pp)

Current §B numerical table.

### App. F. Prior-art retrieval summary (~1 pp)

Current §D Undermind-retrieval positioning.

---

## Mapping current `src/` segments → `src/re/` segments

For reference; not authoritative until I'm authoring against this outline.

| Current `src/` | Maps to `src/re/` |
|---|---|
| `01-introduction.md` | `01-introduction.md` (rewritten in Jin-narrative form with informal main theorem) |
| `02-setup.md` | `02-related-work.md` (related work) + `03-preliminaries.md` (minimal MDP setup; rest moves to App. C.1) |
| `03-two-gap-diagnostic.md` | `04-main-result.md` (Component 1 motivation paragraph) + `05-mechanism.md` (in narrative) |
| `04-pointmass-identity.md` | `04-main-result.md` (Component 2 motivation + informal identity) + `05-mechanism.md` (Key Lemma 1) + `B-proofs.md` (full proofs of Theorems 4.1, 4.2, 4.3) |
| `05-strategic-tempo.md` | `04-main-result.md` (Component 3 motivation) + `05-mechanism.md` (Key Lemma 2) + `B-proofs.md` (full proofs of Theorem 5.1, Lemma 5.2; bidirectional table) + `C-aux.md` (topology corollaries, A_decay class) |
| `06-loop-level2.md` | `04-main-result.md` (Component 4 motivation) + `05-mechanism.md` (Key Lemma 3) + `B-proofs.md` (full proof of Theorem 6.1) + `C-aux.md` (regimes A/B/C, deployment modes, distinction-from-active-inference) |
| `07-composition.md` | `04-main-result.md` (formal composition theorem statement + unpacking + necessity paragraph) + `05-mechanism.md` (Step 1–4 narrative) + `A-proof-of-composition.md` (full proof) |
| `08-related-work.md` | `02-related-work.md` |
| `09-limitations-conclusion.md` | `06-conclusion.md` (concluding observations + practitioner takeaways) |
| `10-references.md` | (keep as-is, rendered via natbib) |
| `A-supporting-material.md` | Splits across `B-proofs.md` (proofs) + `C-aux.md` (auxiliary) |
| `B-pinsker-numerics.md` | `E-pinsker-numerics.md` |
| `C-chain-rule-uniqueness.md` | `C-aux.md` §C.2 |
| `D-prior-art-summary.md` | `F-prior-art-summary.md` |
| `E-algorithm-sketch.md` | `D-algorithm.md` |
| `F-bias-bound.md` | `B-proofs.md` §B.5 + `04-main-result.md` (informal) + `05-mechanism.md` (Key Lemma 4) |
| `G-proof-sketches.md` | `A-proof-of-composition.md` (subsumed by formal proofs) |

---

## What I want Joseph to weigh in on before I author against this

1. **Body shape OK?** — The Jin-style mechanism-with-key-lemmas vs. current one-section-per-component. I'm leaning Jin-style; flag if you want the four-component-as-body-organization preserved.

2. **The headline informal theorem framing.** I propose the cumulative-rate corollary `Õ(N_h √((B_T+1) T))` *with* the technical anchor (point-mass identity at deterministic-π* corner) flagged in §1's informal main. Two anchors in the headline. Alternative: lead with the identity itself as the headline, and the rate as a corollary in §4. Both are defensible; the Jin-pattern fits the rate-as-headline form.

3. **§3 minimalism — what to push to App. C.1.** Currently `02-setup.md` includes value-object definition with `do(·)`, the singular trajectory commitment, the action-gap definition, the strategic-regret definition. All are arguably needed for the main theorem. The convention hierarchy (C1/C2/C3) is appendix material. Two-variation-regimes paragraph could go either way.

4. **Appendix granularity** — App. A (composition proof) + App. B (component lemma proofs) + App. C (auxiliary) is the core organization. Within those, do I want sub-sections (B.1, B.2, ...) as I've sketched, or one segment per sub-section in `src/re/`? My instinct: keep sub-sections inside one segment per top-level appendix, like Jin 2020 does (`proofs.tex` is a single file with multiple lemmas inside).

5. **Mechanism-section length** — I have 4 Key Lemmas surfaced. Jin 2020 has 2 main "Steps" with one or two surfaced lemmas. Maybe I'm surfacing too many in §5? Consolidating to 2–3 would be tighter.

6. **Concluding-observations vs. practitioner-takeaways** — I have both in §6. Jin 2020's conclu.tex has just observations; Jin 2018 (per Example 18) has takeaways. Either alone is enough; both might be redundant. Prefer one or the other?

Once you weigh in, I'll author `src/re/` segments against the agreed outline.
