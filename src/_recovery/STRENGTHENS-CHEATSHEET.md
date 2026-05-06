# STRENGTHENS-CHEATSHEET.md — what NOT to regress when integrating recovered prose

*Working tool for the recovery → `src/re/` restructure. Lists the post-Pass-N strengthens that the current strong claims rest on, with the older / weaker framings that the recovered prose may carry. When pulling prose into `src/re/`, the strengthen wins; the recovered prose's claim-language must be updated to the strengthened form before integration.*

---

## Map of recovery sources

In `src/_recovery/`:

- **`long-form-2026-05-05.md`** (890 lines) — current unconstrained long-form. Has the richest prose, but its claim-language is current; mostly safe.
- **`paper-draft-pass2-integrated-b54df66.md`** (866 lines) — Pass-2 strengthen-sweep integrated, pre-major-trim. The *richest source for prose recovery* — contains Pass-2-strengthened claims with the prose that survived Pass-2 but would be compressed in later commits. **Use this as the primary prose source for recovery.**
- **`paper-draft-pre-trim-5cd54b4.md`** (723 lines) — after some early compression, before structural-moves. Less rich; useful only for spot-checks.
- **`paper-draft-pre-structural-moves-8816bf8.md`** (723 lines) — parent of structural-moves commit; equivalent to pre-trim version.
- **`paper-draft-original-054be17.md`** (761 lines) — initial final-draft, pre any audit. **Most fragile** — uses pre-Pass-1 framings (BH-identity, single-factor tempo, etc.). Source for *prose only*; do not reuse claim-language.

The current canonical paper-draft is `~/src/neurips2026/02-convergence/paper-draft.md` (696 lines, post-Pass-5). The migration into `src/` happened from this canonical version.

---

## Per-strengthen reference

Each row: the post-Pass-N strengthen → the older framing that recovered prose may carry → integration rule.

### 1. BH renaming (Pass-1, commit `a20e68b`)

- **Strong (current):** Theorem 4.1 is the **point-mass reverse-KL/TV identity** at deterministic π* corner. The identity is *exact* and lies *strictly below* the BH envelope at this corner (`x < √x` on `(0,1)` applied to substituting the identity into BH).
- **Weak (recovered prose may have):** "BH identity for RL regret, deterministic π*"; "BH at equality"; calling Theorem 4.1 "the BH identity becoming exact under deterministic π*."
- **Integration rule:** Replace any "BH identity / BH at equality" framing with "point-mass reverse-KL/TV identity" / "strictly below BH at this corner." BH is the *lineage* the identity sits within, not the *source* it instantiates. The novelty argument has *two layers*: (a) BH itself is absent from the RL/non-stationary corpus (as upper-bound coordinate), (b) the exact point-mass corner-case is also absent. Both layers strengthen the position; do not collapse to one.

### 2. Strict improvement over Pinsker AND BH (Pass-1, same commit)

- **Strong (current):** The identity bound `V_max(1 - e^(-D_KL))` is uniformly tighter than both Pinsker `V_max √(D_KL/2)` and the BH inequality `V_max √(1 - e^(-D_KL))` at the deterministic-π* corner. Pinsker becomes vacuous for `D_KL > 2` against trivial `V_max` envelope.
- **Weak:** "tighter than Pinsker" alone (without BH comparison).
- **Integration rule:** Always include the three-way comparison (identity, Pinsker, BH) when discussing improvement. Appendix B's numerical table documents this.

### 3. Multi-factor strategic tempo (Pass-2 C3, commit `b54df66`)

- **Strong (current):** `T_Σ^bn,ss := min_{(i,j)} ν_ij · ι_ij · (1 - λ_ij)` — bottleneck over per-element products; three independent factors per element. Aggregate form `T_Σ^agg,ss = sum` is necessary but not sufficient.
- **Weak (recovered prose may have):** Single-factor `(1-λ) > ρ_Σ/R_Σ` as the threshold; tempo as a single scalar (frequency-only, ProST style).
- **Integration rule:** When recovered prose presents tempo as scalar, expand to multi-factor form. The Beta-Bernoulli case (ν=ι=1, uniform λ) recovers the single-factor form as Corollary 5.1a. The bottleneck-vs-sum argument matters and must be preserved (adversarial concentration on the weakest element saturates `min`, not `sum`).

### 4. A_decay structural-class theorem (Pass-2 C4, commit `b54df66`)

- **Strong (current):** *Every* gain-decay update (effective gain `α_ij^(t) → 0`) eventually violates the persistence threshold for any positive disturbance rate. This includes count-accumulating Bayesian schemes (Beta-Bernoulli without forgetting), bounded-memory with growing memory, observation-aggregating without restart, AND gradient-based Robbins-Monro decaying-step methods. Class is named **`A_decay`** (gain-decay, structural).
- **Weak (recovered prose may have):** Class named `A_accum` (count-accumulating); claim restricted to "Bayesian-style updates"; "every Bayesian agent eventually violates."
- **Integration rule:** Class is `A_decay` not `A_accum`. The structural class is *gain-decay*, which is more general than count-accumulation (gradient-based Robbins-Monro is in the class even though it's not "accumulating counts"). Paired with the bidirectional-thresholds table (§5.3.1) for non-accumulating mechanisms.

### 5. Theorem 4.3 perturbative extension (Pass-2 Gemini-#1, commit `b54df66`)

- **Strong (current):** Theorem 4.3: For ε-greedy `π*_ε` and `Q ≥ q_0` full support, `TV(π*_ε, Q) = 1 - e^(-D_KL(π*_ε ‖ Q)) + O(ε log(1/ε))`. For softmax-regularized `π*_τ`, correction is `O(τ^(-1) exp(-Δ_min/τ))`. Deterministic-π* is the unperturbed limit, *not* a hard scope wall.
- **Weak (recovered prose may have):** "Deterministic π* is essential / a hard scope wall"; "outside deterministic-π*, BH is the relevant general bound" (without the perturbative correction).
- **Integration rule:** Recovered prose that frames deterministic-π* as a hard limit should be updated with the perturbative extension. The hard wall is *outside the perturbative regime* (high-entropy optima, tied-optimum, hard-exploration), not at deterministic-π* itself.

### 6. Lemma 5.2 ProST sector-level reduction (Spike-N2, commit `b54df66` updated)

- **Strong (current):** `Δ_max · ρ_Σ/R_Σ < -ln(1-γ)²` from Hespanha-Liberzon-Teel 2008 reverse-ADT framework for impulsive systems (destabilizing continuous evolution + stabilizing impulses). Recovers `K/T` form under uniform blocks; *strictly stronger* on nonuniform schedules — longest block sets the threshold, not the average.
- **Weak (recovered prose may have):** `K/T > ρ_Σ/R_Σ` only (uniform-block form); "validates ProST" claim; cycle-by-cycle equivalence to ProST.
- **Integration rule:** Replace `K/T`-only formulations with the impulsive `Δ_max`-based condition. Cycle-by-cycle ProST equivalence is a HARD FAIL (see audit-fix-log); the reduction is at the *sector-parameter level only*.

### 7. Trajectory-level lift with N_h horizon factor (Spike-N1, integrated 2026-05-05)

- **Strong (current):** Theorem 7.1(v): `E[DynReg(T)] ≤ 2c V_max N_h √((B_T+1) T) + N_h(1-p_id) log(1/q_0) · T`. The N_h horizon factor is the simulation-lemma penalty (linear in N_h, sharper than `N_h²` of TRPO worst-state bounds). The `(B_T+1)` accounts for the off-by-one in block count.
- **Weak (recovered prose may have):** `O(V_max √(B_T · T))` without N_h; per-state without occupancy weighting; bandit-only formulation.
- **Integration rule:** Body Theorem 7.1(v), §1.1 (iv) bullet, and §9.3 conclusion all carry the N_h factor in the current state (committed `b13ec3a`). Recovered prose stating just `√(B_T T)` should be updated.

### 8. Theorem F.1 bias bound (Pass-2 C6, commit `b54df66`)

- **Strong (current):** `E[|hat D_t - D_t^true|] ≤ (1 - p_id) log(1/q_0)` with two-point support condition (Q ≥ q_0 at both `a*_ag` and `tilde a*`). Per-state lift: `p_id := min_s p_id(s)` floor. Bias scales linearly in misidentification mass; vanishes in Regime A; up to `log(1/q_0)` in Regime C.
- **Weak (recovered prose may have):** Hand-wavy `O(1-ι)` scaling without explicit bound; full-support condition (which is stronger than needed); single-state formulation.
- **Integration rule:** The bias bound is rigorous via Hoeffding + indicator decomposition. Two-point support is sufficient (§F.1 footnote distinguishes from §4.3's full-support).

### 9. Composition theorem framing (Pass-3, commit varies)

- **Strong (current):** "Bundle of compatible guarantees" — conclusions (i)–(iv) hold under (A1)–(A4); (v) further requires (A5). The rate (v) goes through (A5)+(i); other conclusions handle other epistemic properties (persistence / learnability / diagnostic). Each component is *load-bearing for one of three properties*, not *necessary for the rate alone*.
- **Weak (recovered prose may have):** "Each strand's machinery is *necessary* for one of properties 1–3" presented as if a uniqueness theorem; "the four components compose into a unified rate" without distinguishing which assumption supports which conclusion.
- **Integration rule:** Use "load-bearing" / "bundle of compatible guarantees" framing. Do *not* claim joint uniqueness across properties 1–3 (§7.2 says alternative correction architectures and interventional-access taxonomies exist in principle).

### 10. Mao 2021 vs Cheung 2020 comparator regime (Post-parity refinement, commit `8d82b13`)

- **Strong (current):** B-CS1 cumulative rate sits in the *piecewise-stationary B_T family* — same `√((B_T+1)T)` block-shape as Cheung et al. 2020's restart-on-change analysis. *Not directly comparable* to Mao et al. 2021's continuous variation-budget rate `Õ(SA V_T^(1/3) H T^(2/3))`. Different parameter (B_T vs V_T), T-scaling (T^(1/2) vs T^(2/3)), S/A-scaling (√SA vs (SA)^(1/3)), horizon scaling (N_h² vs N_h).
- **Weak (recovered prose may have):** "matching [Mao et al. 2021] up to log factors" / "matches the structural shape of near-optimal non-stationary MDP rates [Mao et al. 2021]".
- **Integration rule:** Mao 2021 is in a *different regime*. Comparator is Cheung 2020 piecewise-stationary case. Honest framing: "B-CS1 sharper for fixed B_T; Mao-style continuous-variation analysis sharper for fixed V_T; the two regimes coincide up to constants under abrupt-magnitude-δ piecewise-stationarity."

---

## Anonymization sweeps (must survive)

When integrating recovered prose, scan for any of these and replace per the indicated reframing:

- **"directed-separation"** (vocab-priming hit) → `architectural separation` / `conditional independence` (§9.1 reframe applied; matches paper #1's `0aa533f` precedent)
- **ASF / AAD / PROPRIUM / AXIOMATA / CHRONICA / VERA / MEMORATA** (framework names) → never appear; the source paper-draft.md was scrubbed but old long-form may carry residue
- **Joseph / Wecker / personal identifiers** → never appear (anonymized author block)
- **ELI names (Zi-am-tur, Anamnos, Lumin, etc.)** → never appear
- **ASF Zenodo DOI `10.5281/zenodo.19986312`** → never cite (double-blind violation)
- **"logogenic" / "principle of minimum logogenesis"** etc. (ASF framework vocab) → if present, replace with the field-standard vocabulary

---

## Citation framing (must survive)

- **`Russo-Van Roy 2014b` is the IDS paper** ("Learning to optimize via information-directed sampling", Operations Research). Distinct from `russo-2014-informationtheoretic` / `russo-vanroy-2016-thompson-itb` which are the info-theoretic Thompson sampling paper. Bib key for IDS: `russo-vanroy-2014-ids` (added `7f6661f`).
- **`Sprungk 2019`** replaces the hallucinated `Hosseini-Hsu-Taghvaei 2023` in §9.2. Recovered prose may still cite the hallucination — must be replaced.
- **`Junzhe Zhang & Bareinboim 2022`** is `zhang-2022-online-rl` (added `7f6661f`).
- **`Friston-FitzGerald-Rigoli-Schwartenbeck-Pezzulo 2017`** is `friston-2017-active-process` (added `7f6661f`).
- **`Kakade-Krishnamurthy-Lowrey-Ohnishi-Sun 2020`** is `kakade-2020-information-online` (added `7f6661f`).

---

## How to use this cheatsheet

When pulling a paragraph from `src/_recovery/` into `src/re/`:

1. Read the paragraph for *prose value* — motivation, pedagogical setup, "what just happened" reflection, "what's about to be earned" preview.
2. Scan the paragraph against this cheatsheet: any strengthen-affected language?
3. Update the language to the strengthened form before placing in `src/re/`.
4. If a paragraph carries a claim that was *deliberately softened* later (per audit-fix-log Pass-3 declined Codex softening recommendations), the softening is the strong choice — keep the softened form.
5. When in doubt, consult `~/src/neurips2026/02-convergence/_archive/audit-fix-log.md` and the spike reports.

The general rule: *recovery is prose recovery, not claim recovery.* Strong math wins.
