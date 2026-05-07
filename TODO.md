# TODO.md — 02-unified-convergence-rl live work

*Active items for B-CS1. Free to branch into `TODO-trim.md` / `TODO-citations.md` / etc.\ as scope grows — no fixed schema. For history see `LOG.md`. For umbrella-level migration backlog see `~/src/neurips/MIGRATE-TODO.md`.*

---

## Audit findings — 2026-05-06 (Gemini) + 2026-05-07 (Codex) — strengthening candidates

Two de-novo audits arrived. Findings below are what survived first-hand primary-source verification per AGENTS §3.5, framed as strengthening candidates per AGENTS §3.1 (try the improbable strengthening before any softening). Each is a candidate for a `spikes/<name>/` directory; the strengthening direction is sketched, not committed. Original audit reports archived to `_archive/audits/` once this integration verified.

### Verified-real, strengthening-spike candidates

- [ ] **H1 (strategic-tempo Lyapunov scaling).** The drift inequality `E[ΔV | δ_Σ] ≤ -2 𝒯_Σ V + ρ_Σ²` (`B-key-lemma-proofs.md:64-72`) honestly gives ultimate-bounded `V ≤ ρ_Σ²/(2 𝒯_Σ)`, i.e., `R* ~ ρ_Σ/√𝒯_Σ`. The text states `R* = ρ_Σ / 𝒯_Σ` (matching `04-main-result.md:42`'s conclusion (ii) and (A2)'s threshold form `𝒯_Σ > ρ_Σ/R_Σ`). The two scalings come from different disturbance models — mean-square drift vs. deterministic-adversarial cross-term cancellation. *Strengthening direction:* the model's disturbance bound `|w_ij| ≤ ρ_Σ/|E|^{1/2}` and `Σw² ≤ ρ_Σ²` (`05-mechanism.md:21`) are deterministic, so the cross-term `2δ^⊤ w` should be retained, not absorbed via `E[‖w‖²]`. With the cross-term Cauchy–Schwarz–bounded, the contraction radius depends on whether the linear (`2‖δ‖ρ`) or quadratic (`ρ²`) term dominates, giving `ρ/𝒯` in the small-𝒯 regime and `ρ/√𝒯` in the large-𝒯 regime. Spike: redo the proof preserving the cross-term, identify the regime where `ρ/𝒯` is honest, and either confirm the stated form or restate the threshold as a max-of-two-regimes condition. Source: `src/re/05-mechanism.md:20-27`, `src/re/B-key-lemma-proofs.md:64-74`, theorem use at `src/re/04-main-result.md:30,42`.

- [ ] **H2 (bias term value-scale conversion + clipping).** Theorem 4.1(v) at `src/re/04-main-result.md:48-49` adds `N_h(1-p_id) log(1/q₀) · T` directly to the dynamic-regret sum. Key Lemma 4 bounds an error in *KL coordinates* (`|D̂ − D_true| ≤ 𝟙[mismatch]·log(1/q₀)`); converting to value-scale regret needs the `1 − e^{−D}` map (1-Lipschitz) plus a `V_max` factor via the simulation lemma. *Strengthening direction:* the corrected bias term is `N_h V_max (1 − p_id) min(1, log(1/q₀)) · T` — the `min(1, log(1/q₀))` clip is a genuine *strengthen* (TV ≤ 1 always; for `q₀ < 1/e`, `log(1/q₀) > 1` and the clip kicks in, tightening the bound). Spike: redo the bias chain in §5 step 4 / Appendix A composition with proper value-scale conversion + clipping; restate the rate. Pulls Codex's softening "this could exceed the trivial value range" into a *tighter* bound. Source: theorem at `src/re/04-main-result.md:48-49`; chain at `src/re/05-mechanism.md:70-72`; Appendix A composition at `src/re/A-proof-of-composition.md:45-49`; lemma at `src/re/B-key-lemma-proofs.md:96-106`.

- [ ] **H3 (V_max convention vs. simulation-lemma usage).** `V_max(M_t)` is defined at `src/re/03-preliminaries.md:15` as `max_a Q_O(M_t, a) − min_a Q_O(M_t, a)` — the *cumulative* horizon-Q range. The simulation lemma at `src/re/05-mechanism.md:62-64` writes `V^{π*} − V^Q ≤ V_max · sum_{h=0}^{N_h−1} E[TV] = V_max · N_h · TV_bar`, multiplying the cumulative range by `N_h` again. Under the [0,1] per-step boundedness already in §3, `V_max ≤ N_h`, so `V_max · N_h ≤ N_h²` — a horizon double-count. Two equivalent fixes (no rate change): (a) redefine `V_max` as per-step reward range (≤ 1 here) and keep the `· N_h`; (b) keep cumulative `V_max` and drop the `· N_h` from the sim-lemma step. *Strengthening direction:* (a) is cleaner — `V_max` then denotes a per-step quantity throughout, the cumulative `N_h V_max` factor in conclusion (v) reads transparently, and the lifted `N_h² √(SA(B_T+1)T)` UCRL2/UCBVI rate (`04-main-result.md:61-65`, `06-conclusion.md:9`) inherits cleanly without ambiguity about which `N_h` is doing what. Spike: pick (a) or (b), cascade through Theorem 4.1(v), §5 step 1, Appendix A composition proof, the conclusion's lifted rate. Source: `src/re/03-preliminaries.md:15`, `src/re/05-mechanism.md:62-64`, `src/re/A-proof-of-composition.md:29-33`, `src/re/04-main-result.md:48-49,61-65`, `src/re/06-conclusion.md:9`.

- [ ] **H4 (B_T = optimum-change vs. stationary-segment count).** `src/re/03-preliminaries.md:7` defines `B_T := |{t : a*_t ≠ a*_{t-1}}|` (optimum-change count). Theorem 4.1(A5) at `src/re/04-main-result.md:36` and the proof's block decomposition at `src/re/05-mechanism.md:66` partition `[1,T]` at optimum-change events and assert "the MDP is stationary within each block." An MDP can change rewards/transitions while `a*` is fixed; restart-on-change at optimum-changes does not protect the base learner against non-optimum-changing kernel drift within a block. *Strengthening direction:* under (A2)'s strategic-tempo persistence, the base learner's modeled mismatch is ultimately bounded by Component 3 — *non-optimum-changing* kernel drift contributes to the disturbance budget `ρ_Σ` rather than triggering a new block. The strengthening attempt is to argue formally that this absorbs the gap, making optimum-change count the right counting measure. Fallback (only if the strengthening fails): redefine `B_T` as kernel-change count, `\sup_{t,s} \|P_t(\cdot|s,a) − P_{t-1}(\cdot|s,a)\|` thresholded, or `max(optimum-changes, kernel-changes)`. Spike: try the strengthening first; if it fails honestly, document the failure and adopt the fallback. Source: `src/re/03-preliminaries.md:7`, `src/re/04-main-result.md:36`, `src/re/05-mechanism.md:66`, `src/re/A-proof-of-composition.md:5`.

- [ ] **H5 (A1 satisfaction by deterministic base learners).** A1 at `src/re/04-main-result.md:28` requires `Q_t(a*_t | s) > 0` pointwise. Vanilla deterministic UCB / UCBVI / UCRL2 deploy a Dirac at the UCB-argmax — when the argmax ≠ a*, `Q_t(a*) = 0` and `K_t = +∞`. The bandit identity `1 − e^{−K_t} = 1 − Q_t(a*)` does extend to the limit (`e^{−∞} = 0`), so the *expectation* `E[1 − e^{−K_t}] = E[1 − Q_t(a*)] = O(log t / (t Δ_min))` is finite for UCB. But the strict pointwise reading of A1 is violated. *Strengthening direction:* (i) restrict the example list at `src/re/D-algorithm.md:5-7` to randomized base learners (Thompson Sampling — posterior is genuinely a distribution; ε-smoothed UCB — `(1-ε)·δ_{argmax} + ε·Uniform(A)`), proving each satisfies A1 pointwise with explicit `q₀ ≥ ε/|A|`; (ii) for deterministic UCB / UCRL2, define `Q_t` as the *internal sampling/planning distribution* (e.g., the soft-max-of-confidence-set proxy used by some optimism-based analyses) rather than the deployed action — this preserves A1 and the deployed regret matches the internal regret up to a controllable smoothing cost. Spike: try (ii) for UCRL2/UCBVI (the harder/more interesting case); fall back to (i) which is straightforward. Source: A1 at `src/re/04-main-result.md:28`; bandit claim at `src/re/05-mechanism.md:74`; algorithm appendix at `src/re/D-algorithm.md:5-7`.

### Verified-real, smaller fixes (no spike needed)

- [ ] **M1 (sequential-ignorability framing in headline locations).** `src/re/01-introduction.md:33` and `src/re/04-main-result.md:21` use "interventional by construction" for the closed-loop. The actual content requires (C2) sequential ignorability — temporal ordering alone (which the architecture *does* give "by construction") is necessary but not sufficient. The C2 caveat is correctly surfaced at `src/re/06-conclusion.md:15` but not at the headline. Replace headline framing with "interventional under (C1)–(C3)" or "interventional under sequential ignorability" so the assumption that does heavy work is visible at first read; preempts a likely causal-inference reviewer objection. Source: `src/re/01-introduction.md:33`, `src/re/04-main-result.md:21`.

- [ ] **M3 (perturbative `q₀` condition in §4 main-text statement).** §4 line 15 mentions the perturbative extension's correction order `O(ε log(1/ε))` without naming the full-support `Q ≥ q₀` condition under which the constants stay bounded (proof at `src/re/B-key-lemma-proofs.md:27-45`; constants depend on `q₀` and `|A|`). §5 line 16 does mention it. Add "(under full-support `Q(a) ≥ q₀ > 0`)" qualifier to the §4 statement so the constant-dependence is visible at headline location. Source: `src/re/04-main-result.md:15`.

### Strengthening direction worth flagging — optional structural elevation

- [ ] **O1 (elevate gain-decay structural-class theorem to main text).** The structural theorem on `𝒜_decay` — *every* gain-decay update class (count-accumulating Bayesian without forgetting, bounded-memory with growing memory, observation-aggregating without restart, gradient with vanishing step) eventually violates the persistence threshold for any positive disturbance — currently lives in App. C.6 and is informally referenced at `src/re/04-main-result.md:19`. This is one of the most reviewer-resonant claims in the paper: a memorable "no Bayesian-like update without forgetting can survive" structural result with concrete instances (Beta-Bernoulli, Robbins–Monro). Gemini's audit flagged the burial; paper-1 author's earlier review independently called it out as a strength. Elevation candidate: state as a formal numbered theorem in §4 alongside Component 3 (Theorem 4.2 or similar), keep the proof in App. C.6, add a one-paragraph "memorable concrete instances" callout. The bidirectional-thresholds table already in App. C.6 becomes the supporting evidence. Strengthens the abstract / §1 contribution by giving the universal-failure direction its own headline. Spike-pending.

### Held for the strategy talk (cross-referenced, not double-tracked)

- Codex's **H6** (headline rate's bias-vanishes condition) and **M2** (bundle-vs-monolithic abstract framing) overlap with already-held strategy items: identity-vs-rate-as-headline (Q2 from rc1 review) and abstract-shape (paper-1 / paper-3 / pipeline-agent reviews converged). No double-tracking; resolution at the strategy talk.

### Page-budget candidates — deferred to the compression pass (not "dismissed")

Joseph's directive on the current cycle is no compression-thinking — Strength → Wisdom → Beauty, with size emergent from organization, not enforced via mechanical trim. But a budget pass is coming. When it arrives, the items below are the first ones to try, because they're self-contained moves that don't undo any structural work or claim. Reasoning preserved per item so the budget-pass agent doesn't have to re-derive whether these were rejected on substance or just on timing.

- [ ] **Relocate §4.4 ablation ("Necessity of the four components") to an appendix.** Gemini's call. The §4.4 argument (Component-by-component: "without 1, no routing; without 2, no metric exactness; ..." `src/re/04-main-result.md:67-79`) is doing reviewer-anticipation work for the "are all four really necessary?" objection — but it's *self-contained*, not load-bearing for any theorem. Lifts cleanly to an appendix with one summarizing sentence in the main text. Real win on a page-budget pass: §4.4 currently runs ~13 lines of body prose. Note: this is genuinely good, not just a budget call — the body's narrative arc actually flows cleaner without §4.4 interrupting between Theorem 4.1 and §5's mechanism narrative; the ablation reads better as a "we anticipated this objection" appendix one comes back to. Rated by Joseph (2026-05-07) as "good thing to try when we get there."
- [ ] **Tighten BH/Pinsker repetition across abstract / §1 / §4 / §5 / §E / §6.** Codex's call. The relationship-to-BH framing is the paper's central novelty positioning, so repetition isn't gratuitous — but six surface mentions is more than "establishment + one or two callbacks." A focused one-pass review can probably collapse to two-or-three load-bearing mentions (the establishment in §4, the numerical comparison in §E, one closing reminder in §6) and prune the duplicates in abstract / §1 / §5 to single brief references. Keep the *exactness* and *strict-tighter-than* messaging at every kept location; it's only the BH/Pinsker comparison that's repeated.

### Considered and substantively dismissed (not budget-related)

- *Gemini "merge §4 and §5"* — actively bad regardless of page budget. Collapses the Jin-style restructure where §4 states the formal main result and §5 narrates the mechanism with key lemmas surfaced. The split is doing genuine narrative work (formal ↔ informal, theorem ↔ proof-story); merging undoes the restructure's whole point.
- *Codex trim "NeurIPS Theory Track guideline quote"* — the quote situates the paper in the Theory-Track tradition that values exact analytic identities over empirical fits. AUTHORING §3 calls for this kind of voice positioning; trimming would weaken the paper's self-positioning argument. Different from a page-budget call — the trim *would* lose something concrete.
- *Gemini "elevate perturbative extension to main-text Theorem"* — second-order to H1-H5 / M1 / M3. If we elevate, we'd want the scaffolding (V_max convention, bias term, support condition) cleaned up first so the elevated theorem inherits clean foundations. Revisit post-spikes — not budget-related.

---

## Migration milestones (agent #2, 2026-05-05) — landed

- [x] **Scaffolding** — dirs + `.gitignore` + `meta.md` + `LOG.md` + this file. Commit `9dd4cd7`.
- [x] **Body segments §1–§9 + references** — `src/01-introduction.md` ... `src/10-references.md`, one segment per top-level section. Inline-bold theorem pattern → Obsidian callouts. Heading-prefix numbering stripped. `\tag{P}` converted to anchored `^eq-persistence-condition`. Cross-refs via `[[#^anchor]]`. Pass-5 (a)(i) and (a)(v) coherence drifts left as `> [!todo]` callouts in §1.1 / §9.1 / §9.3 for per-paper agent. **§8 row 3 cross-reference bug** (source's "(§5.5)" → migrated as `[[#^sec-prost-lift]]` which renders "Section 5.4" correctly; bug item below resolved as a side-effect of anchor-based refs). Commit `14672ef`.
- [x] **Appendix segments A–G + manifests** — `src/A-supporting-material.md` (with A.1–A.8 sub-headings) + `src/B-pinsker-numerics.md` + `src/C-chain-rule-uniqueness.md` + `src/D-prior-art-summary.md` + `src/E-algorithm-sketch.md` + `src/F-bias-bound.md` + `src/G-proof-sketches.md`. `OUT.full-paper.md` (everything) + `OUT.neurips-2026-paper.md` (9pp draft cut — appendix B/C/D/E commented out via `<!-- ... -->` per AUTHORING §7.2; A and F kept as load-bearing; G kept for main proofs). Manifest narrative explains the cut rationale and flags within-segment compression as per-paper-agent territory. Commit `7832b79`.
- [x] **Citation migration** — `bin/migrate-cites --apply` rewrote 22 single-cite occurrences across 10 segments. Multi-cite groups `[A Year; B Year; ...]` intentionally skipped by the migrate-cites regex (per PIPELINE-TODO §C1.4); per-paper agent hand-converts those. One missing entry surfaced: `[Zhang-Bareinboim 2022]` in §D needs `bin/refs add junzhe-zhang-bareinboim-2022-online`. Commit `2f9d466`.
- [x] **`prior-art/` ported** — `query.md`, `report.md`, `positioning.md` copied verbatim from `~/src/neurips2026/02-convergence/prior-art/`.
- [x] **Build verification.** Originally blocked on three kramdown-converter rendering bugs at umbrella `PIPELINE-TODO.md ## Inbox` (commit `654da9c`): bold-prefix + `$$display$$` collision, `[[#^anchor]](text)` link-parser collision, `|…|` table-parser collision. All three resolved during the build-pipeline owner's pass; `bin/build` now runs cleanly under the post-`d24c9e8` refactored pipeline. See LOG `2026-05-06 (continued)` for the verification run.

---

## Pass-5 carry-over from source OUTLINE.md — for per-paper agent post-migration

Captured here so the per-paper agent has a clean handoff. Migration agent #2 leaves these as `> [!todo]` callouts in relevant segments where they touch concrete locations; resolution is per-paper-agent territory.

### (a) Coherence drifts (integration bugs from Pass-4 spike-N1/N2 landing)

- [ ] **$N_h$ horizon factor not propagated** — abstract, §1.1 (iv) bullet, §9.3 conclusion still carry pre-strengthen rate $O(V_{\max}\sqrt{(B_T+1)T})$; §7.1(v) and `meta.md` abstract (mirroring submission) currently lack the $N_h$ factor that landed in body Theorem 7.1(v) proof. Abstract is locked at OpenReview but body can be updated until May 6 AOE — body fix is a one-line addition; abstract+conclusion should follow in body for consistency.
- [x] **§8 row 3 cross-reference bug** — Resolved as a side-effect of migrating "(§5.5)" → `[[#^sec-prost-lift]]`, which cleveref renders to "Section 5.4" correctly. Anchor-based cross-references self-heal under renumbering.
- [ ] **$K_t$ vs $K_t(s)$ notational inconsistency** — Theorem 7.1 line 303 defines $K_t$ as per-round; conclusion (v) line 330 defines $K_t(s)$ as per-state. Both labeled $K_t$ in different places; annotate per-round form as the per-state generalization, or drop the line-303 alias.
- [ ] **$p_{\rm id}$ scope inconsistency** — body Theorem 7.1(v) treats $p_{\rm id}$ as scalar; Appendix F line 671 introduces *per-state* $p_{\rm id}(s_h)$ with min-over-states convention. Surface min-over-states scope in the body theorem rather than burying in appendix.
- [ ] **"Directed-separation" anonymization hit** — §9.1, line 371 of source paper-draft.md: `Theorem 6.1's loop-Level-2 claim depends on a directed-separation property between $M_t$ and goal state`. Same vocab-priming risk B-N8 already swept (commit `0aa533f`). Reframe as `architectural-separation property` or `conditional-independence property`. **Hard required before any submission build.**
- [ ] **(A5) base-learner regime ambiguity** — line 315 of source: "either restarted at block boundaries, or local guarantee under within-block carryover" packages two distinct regimes; proof handles cleanly only the restart case. Spike report flagged "within-block carryover interacting with $d^{Q_t}$" — make explicit in (A5) statement or proof.
- [ ] **$q_0$ scope heterogeneity unflagged** — Theorem 4.3 uses full-support lower bound; Theorem F.1 uses two-point ($\{a^*_{\rm ag,t}, \tilde a^*_t\}$). Probably fine in context; one-sentence footnote suffices.

### (b) Free / near-free trim candidates (Pass-5 audit, deferred to manifest-level)

Per AUTHORING §7.2 (reuse-over-re-edit): preserve all segments; let `OUT.neurips-2026-paper.md` omit them via row-comment. **No segment-level cuts during migration.**

- [ ] §A.4 — verbatim duplicate of §4.1's matching-lower-bound argument (~7 lines including math display). Comment out in 9pp manifest.
- [ ] §A.8 — three-deployment-modes-of-loop, self-flagged "sketched, future work." Doesn't back any main-body theorem. Comment out in 9pp manifest.
- [ ] §A.7 partial overlap with §5.3 worked-topology bullets. Per-paper agent decides whether to (a) compress §A.7 to one paragraph absorbing $\theta_1$ identity into §5.3 inline, or (b) just comment out the manifest row. Migration preserves the segment as authored.

### (c) Compression candidates (load-bearing but expository fat)

Tighten, don't cut. Per-paper-agent territory; migration preserves verbatim.

- [ ] §3.3 (Convention dependence) → could collapse to footnote on §3.1.
- [ ] §4.4 (Strict improvement over Pinsker; defers to App. B) → tighten to one sentence + forward.
- [ ] §4.6 (Direction of divergence is forced; three-sentence forward to §C) → could become footnote on Theorem 4.1.
- [ ] §6.3 (Distinction from active inference; purpose unclear from cold reading) → tighten or restructure to lead with the Bruineberg-inoculation.
- [ ] §7 lead (lines 291–299; restates §1.1 pitch) → one-line forward.
- [ ] §7.2 formal restatement compress.
- [ ] §9.2 future work, §9.3 conclusion (mostly duplicates abstract) → two sentences each.

### (d) Reviewer-objection axes the audit thinks aren't yet defended

Deadline-permitting; per-paper-agent territory.

- [ ] **"Why four components, not three or five?"** — necessity argument is split across §3–§6; no section says "if we drop component X, here's what fails." §7.1's bundle-of-guarantees framing actually *admits* not every component is necessary for the rate — undercuts the unification claim. Defensive paragraph in §7 if time permits.
- [ ] **"$\rho_\Sigma/R_\Sigma$ are unknowable in practice"** — threshold $\mathcal T_\Sigma^{\rm bn,ss} > \rho_\Sigma/R_\Sigma$ has unobservable RHS; only obliquely defended via "fail-fast pre-check." Sentence in §5.3 explicitly framing: "the structural condition; specific algorithms estimate $\rho_\Sigma$ via variation-budget analysis [Cheung et al.\ 2020]."
- [ ] **"(A5) is doing all the work"** — UCRL2/UCBVI defense is in Remark 1 not (v) body. Fold parenthetical into (v) statement: "(A5) holds for trajectory-level non-stationary RL bases — see Remark 1)."

---

## Preflight checklist (before any submission build)

- [ ] **Anonymization grep pass.** Build-side scanner runs as part of `bin/build` via `refs/deny-list.yml`. Manual scan against four AUTHORING §3.5 categories before commit. `directed-separation` hit (above) is the known one.
- [ ] **Citation verification.** `bin/refs verify` for every `\cite{key}` in segments. ~70 cites. The Hosseini-Hsu-Taghvaei 2023 hallucination caught during source's citation-verification spike was already replaced with Sprungk 2019; the import here may carry the verification status forward (PIPELINE-TODO §F5).
- [ ] **No ASF self-citation.** Zenodo DOI `10.5281/zenodo.19986312` must not appear. Spot-check during integration showed clean per source `OUTLINE.md`; `bin/refs lint` enforces.
- [ ] **Acknowledgments removed at submission.** `> [!ack]` callout (AUTHORING §1.3) auto-suppressed in anonymized builds — leave content for camera-ready.
- [ ] **AI-use disclosure.** Per source notes, no methodological-disclosure section needed (handbook §"Author Use of Agents and LLMs" exempts editing/exposition aid).
- [ ] **Contemporaneous-work cutoff (March 1, 2026).** DARLING (April 2026) and Y. Zhang-Zhu-Xie (March 2026) cited in §8 cross-cutting / contemporaneous row without empirical-comparison demand. Pre-March papers get full cite-and-distinguish.
- [ ] **Cross-paper differentiation hygiene.** B-CS1 owns identity / 2×2 / strategic tempo / closed-loop access; B-N4 owns LMI / Lyapunov-survival drive; B-N8 owns κ × 𝒜 / Class 1/2/3. (Confirmed in old workspace `common/cross-paper-differentiation.md`.)

---

## Citation migration — remaining work for per-paper agent

After `bin/migrate-cites --apply` (commit `2f9d466`):

**Multi-cite groups still in `[Author Year; Author Year]` form** — skipped by migrate-cites' regex per PIPELINE-TODO §C1.4 (intentional: the tool can't reliably auto-disambiguate the right `\cite{}` form for `[A Year; B Year]`). Hand-convert these per-paper-agent. They appear in:
- §1 — multiple multi-cite groups in opening paragraphs (~5 groups).
- §2 — one or two cross-references with multi-cite (`[Cheung-Simchi-Levi-Zhu 2020; Wei-Luo 2021]`).
- §4, §5, §6, §7, §8, §9 — scattered `[Author Year; Author Year]` patterns.
- §A — `[Hespanha–Liberzon–Teel 2008, Theorem 1, ...]`-shaped page-ref forms.
- §E — multiple multi-cites.
- §G — `[Kakade–Langford 2002; Munos 2003; Ross–Bagnell 2010; Azar–Osband–Munos 2017]`.

The bib database has all entries needed (per dry-run; only one missing — see below). Each multi-cite converts to `\cite{key1, key2, ...}` with natbib's `sort&compress` collapsing runs to ranges.

**Missing bib entry — needs `bin/refs add` before final build.**
- `[Zhang-Bareinboim 2022]` in `src/D-prior-art-summary.md` — paper-draft.md references list calls it "Junzhe Zhang, Bareinboim, E. (2022). Online RL for mixed policy scopes. *NeurIPS*." — proposed key `junzhe-zhang-bareinboim-2022-online` (matches the existing `bareinboim-correa-ibeling-icard-2022-pearl-hierarchy` naming convention).

**Citation verification (per AUTHORING §3.9 Code-of-Conduct grade).** All ~70 cites need `bin/refs verify` before any submission build. Source paper's spike-citation-verification (2026-05-05) caught the Hosseini-Hsu-Taghvaei 2023 hallucination → replaced with Sprungk 2019; that fix is preserved here. Re-verification of the 164 imported bib entries is umbrella-level work (PIPELINE-TODO §F5).

---

## Risk register (carried from source OUTLINE.md, ~13 lines)

- **Compression risk: HIGH.** 9 pages with zero margin under Option 2' framing. If §4 BH identity needs another half-page, §7 must compress, weakening the unification narrative. Watch during trim.
- **Reviewer-variance risk: HIGH.** Sympathetic reviewers see unification + BH anchor → 8-9. Skeptical reviewers see "assembled known results with cite-and-distinguish" → 4-5. Mean fine; variance high. BH anchor caps downside (a reviewer who understands information theory cannot score the BH bound below 5).
- **Reduction-not-derivation risk: medium.** Skeptical reviewer may ask "where's the new theorem?" — answer: BH-identity-in-RL is the new theorem statement; composition is assembly + interpretation. Frame honestly in abstract and §7.
- **Empirical-validation absence risk: medium.** NeurIPS reviewers expect empirical validation. Mitigations: (a) BH identity is mathematically airtight; (b) worked-example reduction to Lee et al. ProST gives empirical grounding via their experiments; (c) honest "theory paper" framing in §9 limitations.
- **Citation-hallucination risk: low-medium.** Lee et al. ProST 2023/2024, Long-Fei Li-Zhao-Zhou 2024 are recent — verify carefully; Bareinboim, Russo-Van Roy, Bretagnolle-Huber 1978 well-cited and stable.
- **Scope-creep risk: medium.** The composition theorem invites extending beyond four components to strategic-DAG details, edge-update gain derivations. Hold the line at four named components; defer everything else to appendices or follow-up.
