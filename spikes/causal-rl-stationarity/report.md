# Causal-RL stationarity spike — report

## Summary

**Outcome: CONFIRM.** All six papers in the §1 line 5 causal-RL cluster — `zhang-2016-mdps`, `zhang-2022-online-rl`, `lu-2021-causal`, `lu-2022-efficient`, `wang-2021-provably`, `zhang-2020-designing` — are unambiguously stationary at the substantive level. The §1 / §2 / §C / §F characterizations hold. **No counterexample. No softening recommended.**

**One incidental finding worth a small edit (sharpens rather than weakens):** §F line 9 names `zhang-2022-online-rl` as the Strand-4 closest neighbor; on first-source read, `zhang-2016-mdps` is the more natural foundational ancestor for Component 4 since it introduced the MDPUC formalism and the closed-loop interventional-vs-observational structural argument. Optional swap, integration-step call.

**Strengthening pass per AGENTS §3.1.** I attempted to find a counterexample or boundary case across all six. Closest candidates — Wang DOVI's H-step time-indexed kernels, Zhang 2020 DTR's "time-varying state" phrasing — turn out on careful read to be the standard within-episode finite-horizon convention, fixed across episodes; not the non-stationarity B-CS1 composes with. The §1 unhedged "stationary settings only" formulation is correct as stated. **Strengthening = ratify the unhedged claim + sharpen §F closest-neighbor designation; not soften §1.**

---

## Per-paper findings

### `zhang-2016-mdps` — Zhang & Bareinboim 2016, *MDPs with Unobserved Confounders: A Causal Approach* (Columbia tech report; PDF: causalai.net/mdp-causal.pdf)

Preliminaries Definition 1 (p. 3): MDP tuple ⟨S, X, T, R⟩ with fixed transition T : S × X × S → [0,1] and fixed reward R : S × X → ℝ. Infinite-horizon discounted with fixed γ ∈ [0,1). MDPUC adds an SCM with unobserved confounders affecting both action / outcome and action / next-state; structural contribution is the interventional-vs-observational distinction (do-randomization washes out useful information about UCs that the agent's natural decision contains). The MDPUC at every t is the same fixed SCM — only the data-collection mode (passive vs do-intervention) varies. Grep for non-stationarity markers: zero hits. **Unambiguously stationary. §1 claim holds.** This is the foundational MDPUC paper, the most direct ancestor for B-CS1 Component 4.

### `zhang-2022-online-rl` — Zhang & Bareinboim 2022, *Online Reinforcement Learning for Mixed Policy Scopes* (NeurIPS 2022; tech report R-84; PDF: causalai.net/r84.pdf)

Section 2 (p. 3): Agent interacts with a fixed SCM M to optimize reward Y over rounds t = 1,...,T. Cumulative regret R(T,M) = Σ_t (E_M[Y|do(π*)] − Y_t) measured against the *fixed* optimum π* ∈ Π_S in the *fixed* SCM M. Sublinear regret target lim_{T→∞} R(T,M)/T = 0 — *static-regret* notion. Mixed policy scopes = combinatorial sets of state-action subspaces over the same fixed SCM. Grep returns one "adversarial" hit, in a citation title; not relevant. **Unambiguously stationary. §1 claim holds.**

### `lu-2021-causal` — Lu, Meisami & Tewari 2021, *Causal Markov Decision Processes: Learning Good Interventions Efficiently* (arXiv:2102.07663)

Section 3 Preliminaries (p. 3): tabular episodic MDP tuple (S, A, P, R, H) with fixed P(·|s,a) and fixed deterministic R : S × A → [0,1]. Total time T = KH across K episodes; regret Õ(HS√(ZT)) against the *fixed* π*. C-MDP adds two stationary causal graphs G^R(s), G^S(s) per state. The graphs' identity does not vary by state ("the underlying distributions can change" by state, but not over time). Quantity Z is a causal-graph-dependent quantity that can be exponentially smaller than A. Only "changing" reference is "changing the domain range of m" in an experimental ablation. **Unambiguously stationary. §1 claim holds.**

### `lu-2022-efficient` — Lu, Meisami & Tewari 2022, *Efficient Reinforcement Learning with Prior Causal Knowledge* (CLeaR 2022, PMLR v177)

Conference (CLeaR) version of the same C-MDP formalism in `lu-2021-causal`. Identical Preliminaries, identical regret, identical algorithms (C-UCBVI / CF-UCBVI). **Unambiguously stationary. §1 claim holds.**

*Citation-hygiene side note (parallels audit F4):* `lu-2021-causal` and `lu-2022-efficient` are the same paper at arXiv preprint (Feb 2021) and CLeaR conference (April 2022) stages. Citing both as if distinct is the same situation audit F4 flagged for the cheung pair. **Not blocking; flagging for integration step.** Convention is to cite the published version (`lu-2022-efficient`).

### `wang-2021-provably` — Wang, Yang & Wang 2021, *Provably Efficient Causal Reinforcement Learning with Confounded Observational Data (DOVI)* (NeurIPS 2021; arXiv:2006.12311)

Section 2 (p. 5): confounded MDP tuple (S, A, W, H, P, r) where W is the confounder space, P = {P_h, P̃_h}_{h∈[H]} are kernels *time-indexed within an episode*, r = {r_h}_{h∈[H]} time-indexed reward. Cumulative regret over K episodes against globally optimal π* in hindsight, T = HK. DOVI uses backdoor adjustment when confounders are partially observed; DOVI⁺ uses frontdoor (composition of two backdoor adjustments) when confounders are unobserved. Regret Δ_H · √(d³H³T) where Δ_H ≤ 1 is information-gain factor.

**Stationarity caveat (closest the cluster gets to a "boundary case" — but it is not).** The kernels P_h are time-indexed within an episode but *fixed across all K episodes*. Standard finite-horizon nonstationary-MDP convention (cf. Jin 2018/2019, Azar 2017): "non-stationary across stages within an episode, stationary across episodes." NOT the cross-episode drift / variation-budget setting B-CS1 targets. The "online vs offline" split is interventional vs observational data on the *same* SCM, not drift between two stages of the world. Grep for non-stationarity markers: zero hits.

**Stationary in the sense the §1 / §2 claims rely on. §1 claim holds.** A pedantic reviewer might raise an eyebrow at "stationary settings only" against P_h-time-indexed notation; if so a small parenthetical clarifying "no across-round drift" closes the gap, but I judge this unnecessary — context disambiguates.

### `zhang-2020-designing` — Zhang & Bareinboim 2020, *Designing Optimal Dynamic Treatment Regimes: A Causal Reinforcement Learning Approach* (ICML 2020, PMLR v119)

Section 2 (p. 3): online learning over a fixed SCM M* = ⟨U, V, F, P(u)⟩ with unknown parameters; T iid experiments. At each round t the agent picks a policy σ_X^(t) in Π and observes Y^(t). Regret Õ(√(|D_{X∪S}| T)) against the fixed optimal DTR σ_X* in M*. DTR machinery: solubility / irrelevant-treatment / irrelevant-evidence reductions of policy space.

**Stationarity caveat.** Abstract phrases "treatment, repeatedly tailored to the patient's *time-varying, dynamic state*" — but reading on, "time-varying" means within an episode (across DTR decision stages of one patient), not across episodes. Underlying SCM M* fixed across all T trials; only the agent's policy evolves. Grep for non-stationarity markers: zero hits. **Stationary in the sense the §1 claims rely on. §1 claim holds.** Same disambiguation comment as Wang.

---

## Synthesis

All six papers fix the underlying environment (MDP / SCM / confounded-MDP) across rounds and define regret against a fixed optimal policy or DTR in that fixed environment. None invoke variation budget V_T, piecewise-stationary segment count B_T, drift, abrupt change, or dynamic-regret machinery. Closed-loop interventional-vs-observational structural arguments operate within the stationary regime.

The dimension along which all six share the stationarity assumption, more precisely:

> **The underlying SCM / MDP parameters (P, R, F, P(U)) are fixed across the T rounds of online interaction.** Within a single episode (where applicable), kernel and reward indexing by step h ∈ [H] is permitted (Wang DOVI, Lu C-MDP) or treatment-stage indexing is permitted (Zhang DTR), but these within-episode time-indexings are themselves fixed across episodes.

This is the precise stationarity assumption B-CS1's compose-with-non-stationarity claim relaxes — the unified-RL paper allows the underlying (P_t, R_t) to drift across rounds, with V_T or B_T controlling the drift.

**The strengthening (per AGENTS §3.1) is at §F closest-neighbor, not §1 softening.** Three sharpening moves I considered:

1. *Tighten framing language at §1 line 5.* Replace "operates in stationary settings only" with "operates under fixed-SCM stationarity (no across-round drift in P, R)." More precise but heavy in a §1 paragraph already carrying four track descriptions. Probably not worth the prose cost.

2. *Sharpen §F line 9 closest-neighbor designation.* Change `zhang-2022-online-rl` → `zhang-2016-mdps` (or both). MDPUC is the foundational paper that introduced the closed-loop interventional-vs-observational structural argument Component 4 generalizes; Zhang 2022 mixed-policy-scopes is downstream. **Highest-leverage edit if one is to be made.**

3. *Note Wang DOVI's within-episode time-indexed kernels as a distinct slicing.* Belt-and-suspenders precision; optional.

I do **not** recommend softening "operates in stationary settings only" — that phrasing is correct as stated and the soften-form would weaken a true claim for no substantive reason.

---

## Recommendations

### Keep (no edit needed)
- **§1 line 5** "operates in stationary settings only" — accurate as stated, all six cites verified stationary.
- **§2 line 11** "Their setting is *stationary*" — accurate.
- **§C line 127** "all are stationary-MDP. Composition with non-stationarity is, to our knowledge, novel" — accurate.

### Optional sharpening (low priority, integration-pass call)

- **§F line 9 closest-neighbor swap (preferred edit if making one).** Change `zhang-2022-online-rl` → `zhang-2016-mdps`, or to `zhang-2016-mdps, zhang-2022-online-rl` if naming both ends of the lineage is preferred. Higher-fidelity ancestry attribution for Component 4.

- **§C line 127 precision (if integration step wants it).** Suggested verbatim: "all are stationary-MDP (in the across-round sense: the underlying SCM / kernel / reward parameters are fixed across the online-interaction rounds; some use within-episode time-indexing of P_h, r_h in the standard finite-horizon convention)."

### Citation-hygiene side note (parallel to audit F4)

- **`lu-2021-causal` and `lu-2022-efficient` are the same paper.** arXiv preprint (Feb 2021) + CLeaR conference (April 2022). Currently both appear in the §1 line 5 cluster, §2 line 11, §C line 127. Convention: cite the published version (`lu-2022-efficient`); preprint can be dropped. Same hygiene issue as audit F4's cheung pair. Not blocking.

---

## Artifacts produced

**Six new PDFs registered** at `refs/pdfs/`:

- `zhang-2016-mdps.pdf` (causalai.net/mdp-causal.pdf)
- `zhang-2022-online-rl.pdf` (causalai.net/r84.pdf)
- `lu-2021-causal.pdf` (arxiv.org/pdf/2102.07663.pdf)
- `lu-2022-efficient.pdf` (ambujtewari.com/research/lu22causal.pdf)
- `wang-2021-provably.pdf` (arxiv.org/pdf/2006.12311.pdf)
- `zhang-2020-designing.pdf` (proceedings.mlr.press/v119/zhang20a/zhang20a.pdf)

**Six new verification events recorded** under `claim-supported` at `refs/verifications/<key>/`. These close the prior auditor's coverage gap #3 (causal-RL cluster's stationarity claim) at PDF-anchored, first-source-verified level.
