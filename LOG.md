# LOG.md — 02-unified-convergence-rl history

*Append-only. Reverse-chronological (newest first). Never edit prior entries — LOG is the permanent record. Future agents reading this should be able to reconstruct what was tried, what worked, what failed, and why.*

For active backlog see `TODO.md`. For umbrella-level history see `~/src/neurips/LOG.md`. The source paper's full Pass-1 → Pass-5 audit cycle is the historical record at `~/src/neurips2026/02-convergence/LOG.md` and is intentionally **not** re-logged here — this LOG starts fresh from the migration milestone forward.

---

## 2026-07-29 — Post-review sync: dead reference list archived, as-submitted state pinned

Reviews returned 2026-07-23 (submission 33915: ratings 3 / 3 / 2, the last at confidence 5). The AC's meta-review states the clarity and precision problems require "significant rewriting outside the scope of the author response period" — i.e. a reject announced in advance. No paper content touched here; revisions are forbidden during the response period anyway.

**A 76-line manual reference list had been dead since before submission.** `src/re/10-references.md` carried a hand-maintained alphabetical bibliography whose own header said *"Phase B will switch to natbib + `refs.bib` per AUTHORING §1.11; until then, manual list."* That switch happened — the paper renders natbib numbered references (`[1] Cheung, Simchi-Levi, Zhu...`), verified against the built PDF — but neither the file nor its note was removed, so the source tree read as though a manual list were live. `bin/build`'s bibliography branch short-circuits before `render_segment`, so the file was never rendered and this cost nothing in the submitted paper; the risk was a future agent maintaining or trusting it.

Archived to `_archive/references-manual-list-2026-05-07.md` rather than deleted, and replaced with the stub convention paper 01 already uses. Kept because several entries carry journal / publisher / edition detail that looks *better* than the corresponding `refs/entries/` YAML — the same field-hygiene direction as the stray relata emission found today (umbrella `PIPELINE-TODO.md` §F7). Anyone landing §F7 should read that archived list as a third data point.

**README advertised manifests that do not exist** — `OUT.full-paper.md` and `OUT.neurips-2026-paper.md`, both migration-era and archived, plus "currently bootstrapping the segmented layout." There is one live manifest. Fixed.

**As-submitted state pinned.** Tag `submitted/neurips-2026` at `38463e2` (last content commit before submission), plus `submitted-neurips-2026.pdf` — a frozen blind build under a stem no manifest owns, so `bin/build` cannot clobber it. The tracked `unified-rl-neurips-2026.pdf` is regenerated on every run (overwritten twice on 2026-07-29 alone) and is therefore not a record of anything.

*Basis for calling the current source as-submitted:* no source commits between 2026-05-07 and 2026-07-29 except the 2026-05-22 `meta.md` author-info edit, which only affects `--preprint` / `--final` renders. Cross-checked by word-frequency comparison against the stamped non-anon copy in `~/Documents/submitted-papers/`: all content phrases match; differences confined to author block, stamp banner, preprint footer. One token count (`Vmax`, 80 vs 78 raw occurrences) resisted full accounting and traced to math-glyph extraction differences in the flattened stamped PDF rather than content — every surrounding phrase and every other body token matches exactly. Residual caveat: OpenReview records the submission as *modified 27 May 2026*; no source commit exists in that window, so a re-upload would have come from this same source, but the PDF downloadable from OpenReview remains the authoritative artifact.

---

## 2026-05-07 — VT-unification spike + integration: BoBW closes §6's "open question"

Joseph requested launching a serious Opus spike on the §6 "open question whether the per-round-identity route can match the T^{2/3} continuous-variation rate, or whether the regimes are structurally distinct." Spike launched with the AGENTS §5.4 trichotomy framing pointing at the H4 archived report as direct prior context. Agent did substantial mathematical work (110 lines of substantive reasoning visible in transcript reconstruction), got stuck on the report-write step due to harness Write restrictions, returned the full report content as text via task notification once unstuck-and-killed. Report saved to `spikes/VT-unification/report.md` and archived to `_archive/spikes/VT-unification/` per AGENTS §4.2 step 7 once integration verified.

**Spike outcome — STRENGTHENED at Completion State 2 (match existing claim) + Completion State 3 (map structural boundary).**

The §6 "open question" framing was too pessimistic — the regimes are *not* structurally distinct. Both B_T and V_T regimes are reachable via the same per-round identity coordinate; what differs is the *aggregation strategy* (block-Cauchy–Schwarz across B_T+1 stationary intervals for B_T; adaptive-window MASTER over log T parallel base-learner instances for V_T). Wei-Luo 2021's MASTER black-box reduction wraps any base learner with Õ(√L) stationary regret into a non-stationary algorithm achieving Best-of-Both-Worlds dynamic regret `Õ(min{√((B_T+1)T), V_T^{1/3} T^{2/3}})` automatically, without prior knowledge of either variation budget. Our framework's identity-routed (A5)-compatible learner satisfies Wei-Luo's Assumption 1 at the boundary `p = 1/2` (explicitly admitted). The wrapping is mechanical: Steps 1-2-4 of the proof of (v) are stationarity-agnostic; only Step 3's aggregator changes.

Five distinct angles tried for Completion State 1 (beat Mao's exponent) — all failed at the Besbes-Gur-Zeevi 2014 lower bound, which holds at the deterministic-π* corner (their construction is a Bernoulli MAB with dynamic oracle playing per-round argmax, identical to B-CS1's canonical scope). The per-round identity sharpens *constants and per-round form*, not the V_T exponent. Two genuinely-open future-work directions surfaced: V_T^{eff,Σ} (continuous-variation analog of H4's B_T^{eff,Σ}) and V_T^{(K)} (variation on the identity coordinate). Both agent-coupled, both bounded above by environment-side V_T but typically much smaller in absorbing regimes.

**Primary-source verification done first-hand (2026-05-07).** All three load-bearing claims confirmed against registered PDFs in `refs/pdfs/`:
- Wei-Luo 2021 Theorem 2 + Assumption 1 (44pp, v3 Sept 2021) — black-box reduction + base-learner conditions confirmed; Table 1 lists `MASTER + Q-UCB` for episodic tabular MDPs achieving min-of-both BoBW form, improving over Mao 2021 by parameter-free property.
- Mao 2021 (50pp, v4 Aug 2022) — RestartQ-UCB rate `Õ((SA)^{1/3} ∆^{1/3} H T^{2/3})`; matching lower bound `Ω((SA)^{1/3} ∆^{1/3} H^{2/3} T^{2/3})`. **Correction surfaced:** the spike's earlier draft and the existing §4.3 *Comparator regime* paragraph both stated Mao's prefactor as `SA · V_T^{1/3} · H` — actual is `(SA)^{1/3} · H` (cube-root SA, not linear). Fixed in the §4.3 update.
- Besbes-Gur-Zeevi 2014 Theorem 1 (30pp, OR journal version 2019; bib registered as `besbes-gur-zeevi-2014-stochastic`) — lower bound `Ω(K^{1/3} V_T^{1/3} T^{2/3})` at deterministic-per-round optimum (Bernoulli MAB with dynamic oracle). The lower bound applies at the deterministic-π* corner exactly as the spike claimed.

**Integration landed in source (this commit):**

- **Tier 1: §6 conclusion paragraph replacement** at `src/re/06-conclusion.md`. Replaced "open question whether regimes are structurally distinct" with the BoBW commitment paragraph: Wei-Luo MASTER wrapping + Mao rate match + BGZ lower bound at deterministic-π* + parameter-free property + V_T^{eff,Σ} / V_T^{(K)} future work. ~6-line paragraph; net ~+3 lines vs. previous "open" paragraph.
- **Tier 2a: §4.3 *Comparator regime* sentence update** at `src/re/04-main-result.md:61`. Replaced "Generalizing the per-round-identity route to the continuous-variation regime is open" with pointer to §6 commitment + §A (A5')-BoBW Remark. Also corrected the displayed Mao rate's SA exponent (`Õ(SA · V_T^{1/3} · H · T^{2/3})` → `Õ((SA)^{1/3} · V_T^{1/3} · H · T^{2/3})`), self-consistent with the SA-scaling comparison list a few lines later.
- **Tier 2b: §A (A5')-BoBW Remark** at end of `src/re/A-proof-of-composition.md`. New formal Remark establishing theorem-grade availability of the BoBW form via (A5'). Includes explicit citation chain (Wei-Luo Theorem 2 + Assumption 1; Mao 2021 rate match; BGZ 2014 lower bound at deterministic-π*) and full rate statement with both rate-term and bias-term carrying through.

**Bib housekeeping:** `besbes-gur-zeevi-2014-stochastic` registered via `bin/refs add` from BibTeX (NeurIPS 2014 / Stochastic Systems 2019 dual attribution). Wei-Luo and Mao bib entries already existed.

**Build verification:** clean. Main text remains 13pp (Tier 1 §6 commitment fits in existing main-text page); appendices grew by ~1 page from the §A Remark (§B p20, §C p23, §D p27, §E p28, §F p29). Total ~29pp. No new overfulls.

**Pattern observation (per AGENTS §3.1).** This is the third spike-validation of the strengthen-before-soften principle in this sprint: H4 was negative-with-payoff (failure mapped structural boundary + surfaced B_T^{eff,Σ} future work); H2 went sharper than the parent-anticipated clip (structural-fix gave V_max-only bound with no log factor); now VT-unification went from "open question" to "BoBW theorem-grade-available" by recognizing the aggregation/identity separability. Each spike that came in *as a softening recommendation* or *open question* ended up as a strengthening. The §3.1 escalation-path framing — depth of attention separable from raw capability; routing to spike mode when triage cadence wants to soften — has been empirically validated three times running on this paper.

**Pattern observation (per AGENTS §3.5).** The spike's draft included one prefactor overstatement (Mao's `SA · V^{1/3} · H` instead of `(SA)^{1/3} · H`) that didn't catch on its own re-read. The primary-source verification step caught it before it landed in §6. Worth surfacing as a memory-worthy data point: even a 9-minute focused-attention spike can carry citation-level overstatements, because the spike's core math doesn't depend on the exact prefactor. Always primary-source-verify rate constants and SA-exponents before committing them to source.

---

## 2026-05-07 — Integration pass: H1 + M2 + H2 + H3 + H5 + H4 + Tier-2 quick wins landed in source

Coordinated integration pass applying the five-spike sweep outcomes plus Tier-2 quick wins to `src/re/`. Five batches, one commit each, build clean after each. Spike directories archived to `_archive/spikes/` per AGENTS §4.2 step 7.

**Decisions locked in (Joseph 2026-05-07):**
- H1 cross-paper Model (S) naming: rename B-CS1's "Model (S)" → "Model (Σ)" (paper-01 stays untouched).
- M2 (C1)–(C3) restructuring: conservative — keep three named conditions with parenthetical clarifier "(C1) and (C3) are automatic; substantive content is (C2)".
- H2 strict assumption-set weakening: surface in §4.3 unpacking (rate (v) no longer requires `Q_t ≥ q_0`).
- H4 redefinition + B_T^{eff,Σ} future-work: confirmed.
- H5 side finding: Lattimore-Szepesvári Theorem 7.1 verified first-hand against `refs/pdfs/lattimore-2020-bandit.pdf` — `R_n ≤ 3 Σ Δ_i + Σ (16 log n)/Δ_i` confirms `E[N_a(n)] = O(log n / Δ_a²)`. Δ_min² rate-constant correction applied with citation.

**Batch 1 — H2 + H3 + H5 side finding (commit `17639d5`).** Bias term `N_h(1-p_id) log(1/q_0) T` → `V_max N_h(1-p_id) T` in Theorem 4.1(v), §A Step 4, §5 Step 4 narrative, §4.3 unpacking. Step 4 derivation switched from Lemma-4-routed (KL coordinates) to direct value-side simulation-lemma argument on the per-step Q-difference between true-optimum and agent-identified-optimum policies. Lemma 4 retained for conclusion (iii)'s diagnostic computability — its native role. Strict assumption-set weakening surfaced: rate (v) does *not* require `Q_t ≥ q_0`, only conclusion (iii) does. Bandit case constant corrected to `O(V_max (B_T+1) log(T/(B_T+1)) / Δ_min²)` with Lattimore-Szepesvári Theorem 7.1 citation; honest framing that the V_max·TV chain is structurally one Δ_min looser than direct gap-aware UCB.

**Batch 2 — H1 + Model (Σ) rename (commit `448f267`).** Lemma 2 proof in §B rewritten using deterministic-cross-term Cauchy-Schwarz on `2 δ^⊤ w` (continuous-time idealization V̇ = -2 δ^⊤ F + 2 δ^⊤ w gives `R_Σ* = ρ_Σ / 𝒯_Σ^{bn,ss}` honestly). Discrete-time form added as a one-line corollary via AM-GM. Mean-square corollary documented for reference. Sharpness paragraph upgraded to formal comparison-principle witness `w*(t) = ρ_Σ δ(t)/‖δ(t)‖`. Khalil 2002 §9.2 / comparison-principle form cited (more precise than the prior "Lemma 9.2 / Theorem 9.1" reference). Algebra error from Codex H1 / my N4 / Opus M1 fixed: no more silent model-swap from deterministic to mean-square mid-proof. All "Model (S)" → "Model (Σ)" in src/re/.

**Batch 3 — M2 (commit `56f9816`).** §B Lemma 3 proof rewritten using Pearl Rule 2 form of do-calculus directly. The misleading intermediate step `(C2) gives a_t ⊥ U | H_t, so P(o_{t+1} | a_t, H_t, U) = P(o_{t+1} | a_t, H_t)` retired; the d-separation form `(o_{t+1} ⊥ a_t | H_t)_{G_overline{a_t}}` is exactly the content of (C2) and gives the identification claim immediately. Equivalent forms (potential-outcome, truncated-factorization) documented as a remark. (C1)/(C3) automatic-vs-substantive clarification added to both §B proof and §5 lemma statement. §6 conclusion's "On coupled-goal architectures" remark made concrete: goal-conditioned LLM policies have the goal influencing both action selection and environment response via shared latent context (user intent / dialogue history / task framing), so H_t doesn't block the goal-mediated path.

**Batch 4 — H5 + Δ_min² correction + M3 (commit `19f6921`).** (A1) reformulated as "Metric extension" — extended-real reading of the identity at K_t = ∞ (e^{-∞} = 0); pointwise positivity not required; vanilla deterministic UCB / UCBVI / UCRL2 first-class without modification. §5 Lemma 1 hypothesis relaxed; §A Step 1 simplified; §B proof of Lemma 1 carries the convention. M3 fix folded in: Lemma 1 now reads with explicit per-state qualifier "(per visited state, with a* = a*(s) and Q = Q(·|s))" so the lemma reads correctly out of context. §D algorithm appendix's bandit-case rate constant corrected to Δ_min² with explicit Lattimore-Szepesvári Theorem 7.1 citation; vanilla UCB joins Thompson sampling on the example list with the (A1) extended-real annotation.

**Batch 5 — H4 (commit `379cfae`).** B_T redefined in §3 from optimum-change count to kernel-stationary-segment count (`|{t : (P_t, r_t) ≠ (P_{t-1}, r_{t-1})}|`). Cascade through §1 headline, §5 Step 2, §A notation paragraph, §6 conclusion continuous-variation paragraph. Three concrete examples of when optimum-change diverges from segment-change added in §3 (uniform shifts, argmax-preserving kernel shifts, sub-arm swaps). Bandit-case + isolated-optimum + reward-shifts-cross-argmax corollary ("regime ARG") preserved as a sub-case where the two counters coincide. §6 added "On agent-experienced block count" paragraph introducing `B_T^{eff,Σ}` as future work — the count of times `‖δ_Σ‖` exits its ultimate-bounded region, an agent-experienced block count strictly less than `B_T^{kernel}` under (A2). Explains why agents in high-V_T slow-drift environments often outperform B_T-pessimistic predictions.

**Tier-2 quick wins (this commit).** M5 — gain-decay class scope clarification: per-element pull is required for the universal-failure theorem to apply; agents that ignore a subset of elements are outside the class for those elements (parenthetical added in `C-aux-material.md` §`thm-decay-class` proof). M6 — chain-rule uniqueness proof expanded from a single comma-reduction sentence to the full reduction: factorize joint ratio, expand both sides, collapse to functional equation `f(rs) = f(r) + r f(s)` via two-point reduction, cite Aczél-Daróczy 1975 §4 for the unique solution `f(t) = c · t log t`. M3 already folded in during Batch 4.

**Deferred:** C3 (Vieillard-Pietquin "Leverage the Average" connection in §2) — needs `bin/refs add` for the citation and first-hand verification of the connection's form per the Opus audit's spike-pending note. Filed in TODO as a hold-for-citation-add item.

**Spike directories archived** to `_archive/spikes/` per AGENTS §4.2 step 7. Empty `spikes/.gitkeep` retained for future use. The five spike reports remain accessible at their archived paths for any future agent who wants to re-derive the integration's substantive moves.

**Final state.** PDF `unified-rl-neurips-2026.pdf` builds clean. Page progression: §6 starts p13 (was p12 — main text grew by 1 page from the proof rewrites in §B + §5); appendices p17+; total ~28pp. Two cosmetic <30pt overfulls remain (kerning slop in tabularx columns, unchanged from before the integration). All proof-correctness items from the four-read audit (Codex H1-H5 / M1-M3, Opus M1-M8, my self-read N1-N5) either landed in source or are explicitly held for the strategy talk (H6 / N1 / A7 headline-rate form, N2 numbering inconsistency, M2-bundle abstract framing). Tier-4 budget-pass writing tightening (Opus A2-A6, S1-S7, N1-N10, P1-P6) preserved as the next-pass project once the strategy-talk decisions land.

The §3.1 strengthen-before-soften principle was empirically validated again across the integration: every spike outcome that came in *as a softening recommendation* (Codex H2, H4, H5) ended up as a *strengthening* in source (H2's structural-fix gives a tighter bound than the parent-anticipated clip; H4's negative-with-payoff produced a cleaner B_T definition + a new future-work direction; H5 made vanilla UCB unconditionally compatible). The Codex "theorem-to-rhetoric tension" framing was the right diagnostic for what each finding was pointing at; the 10+ minute focused-attention spike model was the right escalation path.

---

## 2026-05-07 — Five-spike Opus 4.7 sweep on Tier 1 audit findings; substantive results

Per Joseph's invitation and AGENTS §5.4 spike-briefing guidance (trichotomy framing: succeed beyond / succeed at strengthening / map structural boundary), launched five Opus 4.7 spike agents in parallel on the Tier 1 proof-correctness items: H1, M2, H2, H4, H5. All five returned substantive reports; all are saved at `spikes/<name>/report.md`. Three "succeeded beyond what we currently claim" findings, one "succeeded at strengthening to match the existing claim", one "mapped a structural boundary" — every spike yielded value, none defaulted to softening.

**H1 (forgetting-proof scaling) — CRACKED.** Strengthening recovers `R_Σ* = ρ_Σ/𝒯_Σ` honestly via deterministic-disturbance Cauchy–Schwarz on the cross-term `2δ^⊤w` that the current proof implicitly drops. Paper-01's `lem-persistence-d`(i) is the canonical template (continuous-time form); discrete-time analog goes through via AM–GM with absorbable constants. Local fix; no downstream propagation. *Plus strategic call:* B-CS1's "Model (S)" naming collides with paper-01's "Model S" — they name *opposite* things (B-CS1's deterministic vs paper-01's stochastic). Cross-paper coordination decision for Joseph.

**M2 (loop-Level-2 (C2) proof) — CRACKED with structural payoff.** Beyond ratifying Opus's sketch (Pearl Rule 2 form, ~6 lines), the spike found the (C1)–(C3) decomposition is *over-decomposed* for the identification claim. Only (C2) is load-bearing — (C1) reduces to realized-action positivity (automatic), (C3) "known action mechanism" isn't used in the identification step at all (loads only for off-policy estimation). And (C2) itself is stated in a misleading form (`a_t ⊥ U | H_t` is the wrong handle for the proof; d-separation form `(o_{t+1} ⊥ a_t | H_t)_{G_overline{a_t}}` or potential-outcome `a_t ⊥ Y^{(a_t)} | H_t` are correct). The Codex M1 / Opus A8 §1/§4 framing concern lands cleaner under this restructure: instead of "interventional under three conditions," becomes "interventional under sequential ignorability; positivity and known-policy are automatic in our architecture." Strategic call: aggressive collapse to single H_t-sufficiency vs conservative keep-three-with-clarifier. Lean conservative for this cycle.

**H2 (bias term) — CRACKED beyond expectation.** The strengthening went sharper than the parent-anticipated `min(1, log(1/q_0))` clip. The corrected bias term is `V_max · N_h · (1-p_id) · T` — *no log factor, no clip needed*. Lemma 4 was the wrong vehicle (it bounds a KL-readout-bias, not a value-coordinate misidentification penalty); the right tool is a direct value-side simulation-lemma argument on the per-step Q-difference. Strictly tighter than both the current form (units error + can exceed trivial envelope) and the parent's anticipated clipped form. *Plus structural finding:* the support condition `Q_t ≥ q_0` at the two argmax candidates is no longer needed for the rate term — only conclusion (iii)'s KL-readout requires it. Rate (v) provable under a strictly *weaker* assumption set. Three reads of Codex's H2: (1) softening, (2) clipped strengthening, (3) structural-fix strengthening — successive reads found more value, exactly the AGENTS §3.1 pattern. **H3 fix (a) (per-step V_max convention) should land in the same integration pass** — the bias term's `V_max · N_h` prefactor mirrors the rate term's transparently under (a).

**H4 (B_T definitional alignment) — NEGATIVE-WITH-PAYOFF.** Five distinct strengthening attempts all failed for *substantive* reasons (not effort-based) — all documented in the spike report so future agents don't re-attempt. The (A2)-absorbs-kernel-drift argument doesn't compose: Model (S)'s `δ_Σ` substate is decoupled from the base learner's confidence sets in (P, r) space (this is a *strength* of the four-component design but means (A2) can't rescue (A5)). Three concrete counterexamples (uniform reward shift, kernel-only shift with N_h=2, sub-arm reward swap) demonstrate the optimum-change-count framing is genuinely wrong as the block-counter. Recommendation: redefine `B_T` as kernel-stationary-segment count, keeping optimum-change-count as a corollary in regime ARG (where the bandit-case sharpening lives). ~15 lines diff across 8 segments. Headline rate `√((B_T+1) T)` unchanged; only the *meaning* of B_T shifts. Related-work positioning vs. piecewise-stationary literature becomes more consistent. *Plus bonus structural payoff:* the failed strengthening surfaced a *third* counter `B_T^{eff,Σ}` — the count of times `‖δ_Σ‖` exits its ultimate-bounded region, an *agent-experienced* block count strictly less than `B_T^kernel` under (A2). Worth a §6 future-work paragraph; explains why agents in high-V_T slow-drift environments often outperform B_T-pessimistic predictions.

**H5 (A1 / deterministic base learners) — CRACKED.** Strengthening landed at completion state 1: A1 is too strong. The identity `TV(δ_{a*}, Q_t) = 1 - e^{-K_t}` extends *unconditionally* through the K_t = ∞ limit (`e^{-∞} = 0`), and the TV-regret bounds don't depend on `Q_t(a*) > 0` at all. Vanilla deterministic UCB / UCBVI / UCRL2 are first-class without modification. ~5 lines of edits. Framing payoff: paper currently reads as if (A1) restricts scope to randomized-Q learners; after the strengthening, vanilla UCB is unconditionally compatible — addresses the implicit reviewer concern "why use this framework when I'm running vanilla UCB?". *Plus side finding (separate from H5):* the bandit-case rate constant `O(log t / (t Δ_min))` is off by one factor of Δ_min. Standard UCB analysis (Lattimore-Szepesvári Theorem 7.1) gives `E[N_a(T)] = O(log T / Δ_a²)` per arm; the framework's V_max·TV chain therefore yields `O(V_max · log² T / Δ_min²)` cumulative regret. Structurally looser than direct gap-aware UCB by one Δ_min — feature, not bug, but the rate constant in `D-algorithm.md` should be corrected. ~90% confident.

**Pattern observations across the sweep.**
- *Three of five spikes returned strictly stronger results than the parent-anticipated strengthening direction* (H1 cracks via cross-term Cauchy–Schwarz exactly as anticipated; M2 + H2 + H5 all went *beyond* the parent's framing — found that the load-bearing claim was sharper than the parent had imagined). The trichotomy framing's permission to find a stronger claim than the parent had in mind paid off.
- *Failure-mode H4 was the most informative spike* — it documented five concrete counterexamples and five distinct strengthening attempts that all fail. Codified the "optimum-change ≠ stationary-segment" boundary so future agents don't re-attempt. Plus surfaced the `B_T^{eff,Σ}` future-work direction as a side benefit.
- *Strategic-call items surfaced for owner attention:* (H1) cross-paper Model (S) naming collision; (M2) aggressive vs. conservative (C1)-(C3) restructuring; (H2) support-condition `q_0` no longer needed for rate term — strict assumption-set weakening worth surfacing in §3 / §4; (H4) `B_T^{eff,Σ}` future-work direction for §6; (H5) bandit rate constant Δ_min² correction in `D-algorithm.md` (textbook-verify before applying).

**Integration territory next.** The five spike outcomes need to land in the source via a coordinated integration pass. Order: H3 (V_max convention) + H2 (bias term) together → H1 (forgetting proof rewrite) → M2 (loop-Level-2 proof rewrite) → H5 (A1 reformulation) → H4 (B_T redefinition with cascade through 8 segments). Strategic-call items get owner discussion before the integration pass commits. M3 (per-state qualifier in Lemma 1), M5 (gain-decay subtlety), M6 (chain-rule proof expansion), C3 (Vieillard-Pietquin connection) ride along as Tier-2 quick wins. Smaller refinements (Opus A2-A6, S1-S7, N1-N10, P1-P6) cluster into the budget-pass writing tightening as before.

The §3.1 strengthen-before-soften principle was empirically validated again. Codex's audit findings, when first attempted in-context for triage, would have soft-resolved several of these (the parent context had H2 framed as a clip, which is itself a softening; H4 framed as an attempt-then-fallback, which would have likely soft-resolved to "redefine B_T"; H5 framed as an attempt-then-restrict, which would have likely soft-resolved to "restrict to randomized base learners"). The 10+ minute focused-attention spike model produced strictly stronger outcomes than triage cadence would have. The pattern Codex named ("theorem-to-rhetoric tension") and the §3.1 escalation-path paragraph predict this exactly: depth of attention is a separable resource from raw capability; routing to spike mode is the trigger.

---

## 2026-05-07 — Opus auditor's findings landed; four-read triangulation complete

The Opus 4.7 auditor's de-novo audit dropped at `audits/audit-2026-05-06-claude-opus47.md`. Substantially more thorough than Codex / Gemini / my own self-read combined: M1–M8 math correctness findings, A1–A9 argument-strength findings, plus prose / structure / citation / nit / page-pressure suggestions. Joseph's directive ("do not just take their findings at face value; verify; with Codex specifically, attempt strengthening before any softening") applies equally to the Opus audit.

**Triangulation across four reads.** Two strong-convergence signals:

1. **Forgetting-proof algebra error.** Codex H1 / my N4 / Opus M1 — three independent reads catch the same algebra hop at `B-key-lemma-proofs.md:72`. The proof correctly displays `V > ρ²/(2𝒯) gives negative drift`, then jumps to `R*² = ρ²/𝒯²` without justification. The honest steady state of the displayed inequality is `V ≤ ρ²/(2𝒯)`, giving `R* ≤ ρ/√(2𝒯)` — not `ρ/𝒯`. Three reads agreeing makes this the highest-confidence technical finding.

2. **Headline-rate bias-term omission.** Codex H6 / my N1 / Opus A7 — three reads catch the same elision: theorem (v) has both `√((B_T+1) T)` rate term AND linear-in-T bias term `N_h(1-p_id)log(1/q_0)·T`; abstract / §1 intro / §6 conclusion all give just the rate without the bias. Cesàro tracking → 0 only in Regime A (where `p_id → 1`). Held for the strategy talk under "identity-vs-rate-as-headline" Q2.

**Opus's most important new finding (M2): loop-Level-2 proof misuses (C2).** Verified against primary source — Opus is right. The proof at `B-key-lemma-proofs.md:88-89` writes "(C2) gives `a_t ⊥ U | H_t`, *so* `P(o_{t+1} | a_t, H_t, U) = P(o_{t+1} | a_t, H_t)`." The implication is wrong: `a_t ⊥ U | H_t` is equivalent to `P(U | a_t, H_t) = P(U | H_t)` (definition); the proof's intermediate step would require `o_{t+1} ⊥ U | (a_t, H_t)`, a different conditional independence. The right argument applies (C2) to the *prior on U* in the truncated factorization, not to the conditional on `o_{t+1}`. Lemma conclusion stands; ~4–6 lines of central-calculation surgery in §B. Codex *and* I both missed this — a substantive proof bug visible only on careful walk-through. Strong argument for the four-independent-read approach.

**Other Opus-only new findings landed in TODO:** M3 (per-state qualifier in lemma statement, trivial), M4 (A5 base-learner conversion needs explicit citation or derivation), M5 (`𝒜_decay` class subtlety re Beta-Bernoulli on un-touched elements, parenthetical), M6 (chain-rule uniqueness proof is gestural, needs 5–8 lines of reduction), C3 (Vieillard-Pietquin "Leverage the Average" / KL-regularized RL connection — useful prior-art neighbor).

**~20 smaller refinements** (A2-A6, S1-S7, N1-N10, P1-P6, C1-C2-C5) clustered into a single "budget-pass writing tightening" project in TODO rather than tracked individually. Examples: "technical anchor" framing self-discounts (A2); "coordinate-optimal" terminology non-standard (A4); ProST lift direction can be misread (A5); regime A/B/C taxonomy overstated as contribution (A6); repeated "no published framework" phrasing across intro/main/conclusion (S2/P3); §5 Mechanism could fold remarks into forward-pointers (S5/P2); Q_O / Q^π / Q_t notation triple-shadowing (N3); Pinsker / sequential-ignorability missing citations (C1/C2). Budget-pass agent should read the archived Opus audit as a checklist.

**Updated triage and priorities (revised from yesterday).**

*Tier 1 — proof-correctness fixes (theorem-repair pass per Codex's ordering advice):*
- H1 / M1 / N4 (forgetting-proof scaling) — three-read convergence; spike priority.
- M2 (loop-Level-2 (C2) misuse) — Opus-only but high-confidence on verification; ~5 line surgery.
- H2 (bias-term V_max + clipping) — Codex-flagged; clean strengthening.
- H3 (V_max double-count) — Codex-flagged; notational hygiene.
- H4 (B_T optimum-change vs stationary-segment) — Codex-flagged; strengthening attempt then fallback.
- H5 (A1 with deterministic UCB) — Codex-flagged; restriction or smoothing.

*Tier 2 — small targeted fixes (quick wins between spikes):*
- M3 (per-state qualifier) — 1 line.
- M5 (gain-decay class clarification) — 1 line.
- M6 (chain-rule proof expansion) — 5–8 lines.
- C3 (Vieillard-Pietquin connection) — 1–2 lines in §2.

*Tier 3 — owner-level decisions held for strategy talk:*
- H6 / N1 / A7 (headline-rate bias-term and form-instability across abstract/§1/theorem/§6) — three-read convergence.
- M2-Codex (bundle-vs-monolithic abstract framing) / N2 (numbering inconsistency abstract vs §1.1).
- §5 mechanism narrative shape (paper-1 author flag).

*Tier 4 — budget-pass writing tightening:*
- §4.4 relocation to appendix (Gemini-flagged; case strengthened on self-read; Opus didn't object).
- ~20 Opus-flagged refinements clustered (above).
- BH/Pinsker repetition across abstract/§1/§4/§5/§E/§6 (Codex-flagged; Opus A3 confirmed wording inconsistency).

The two Opus closing recommendations match my Tier 1 ordering: M2 first (highest-leverage technical fix, gives credibility hit to closed-loop access argument), M1 second.

**Audit reports archived.** Both Opus audit (`audit-2026-05-06-claude-opus47.md`) and my self-read notes (`de-novo-self-read-2026-05-07.md`) move to `_archive/audits/` per AGENTS §3.4 — this LOG entry is the integration confirmation that prerequisites the move.

---

## 2026-05-07 — Self-read of the rendered tex; hardcoded-ref bug fixed; N1–N5 findings landed

While the Opus auditor was working an independent de-novo audit, I did my own first-hand cold read of `unified-rl-neurips-2026.tex` (the kramdown-emitted intermediate, since `pdftotext` flattens math). Per AGENTS §3.5: audit-grade work stays first-hand; the two independent reads will be more useful than one once Opus's findings land — convergence builds confidence, divergence is a learning signal.

**Notes file at `audits/de-novo-self-read-2026-05-07.md`** (kept active while Opus is in flight; archive once Opus integrated). Five new findings beyond what Codex / Gemini surfaced:

- **N1 — headline rate stated four different ways.** Abstract: `O(V_max √((B_T+1) T))` (no N_h, no bias). §1 / §6: `Õ(N_h √((B_T+1) T))` (no V_max, no bias). Theorem 4.1(v): full form including N_h, V_max, *and* the bias term. Form-instability across locations is a separate finding from Codex H6's bias-term-omission — readers comparing four statements will reasonably ask which is the actual headline. Resolution depends on the held strategy decision (identity-vs-rate-as-headline); once committed, propagate consistently.

- **N2 — numbering inconsistency abstract vs. §1.1.** Abstract has four equal components (i)–(iv); §1.1 Contribution has three (i)–(iii) plus "Connective tissue: the two-gap diagnostic"; §4 returns to four-equal; §4.4 says "bundle of compatible guarantees, not a single integrated theorem." Concrete surface of Codex M2.

- **N3 — stale `Theorem 4.2` / `Theorem 4.3` / `Lemma 5.2` cross-refs in `src/re/B-key-lemma-proofs.md`.** Migration-era leftovers in proof-section labels referencing theorems that don't exist as numbered in §4 / §5. **Fixed in this commit:** parenthetical numbers removed; replaced with anchored `[[#^sec-...]]` references where useful. The structural question of whether these results *should* be named numbered theorems is filed as O2 in TODO (pairs with O1 / Gemini's universal-failure-class elevation).

- **N4 — algebra error in §B Lemma 2 proof.** This is Codex H1 made on-page visible. The proof writes "V > ρ²/(2𝒯) gives negative drift" (correct), then "iterating gives R*² = ρ²/𝒯² to leading order, i.e., R* = ρ/𝒯" — which doesn't follow. The correct conclusion from `V ≤ ρ²/(2𝒯)` is `R* ≤ ρ/√(2𝒯)` (mean-square form). On-page algebra hop makes the H1 spike higher priority than just "Codex flagged this" — the displayed step has a real error.

- **N5 — direction-forcing rhetoric undersold.** §3 line 25 says "we use reverse KL because forward KL is +∞ off-optimum" as a one-line convention remark. This is *the* structural reason the framework works (forward KL is *forced* into vacuity at the deterministic-π* corner; reverse-KL is structural, not stylistic). Could be promoted to a paragraph-prefix declaration. Minor, budget-pass tightening item.

**Joseph's directive ("there definitely shouldn't be any hardcoded references in the source")** prompted the N3 fix and a broader sweep against AUTHORING §1.7 / §2.2's anti-hardcoded-ref convention. Sweep clean post-fix: no remaining numbered self-references; all surviving "Theorem N" / "Lemma N" mentions are to cited papers (Hespanha-Liberzon-Teel Theorem 1, Khalil Lemma 9.2, Csiszár Theorems 3 and 5, Aczél-Daróczy §4) or to internal anchored cross-refs.

**Confirmation of H1–H5 / M1 / M3 against primary source.** Holistic read confirms each finding I integrated yesterday holds up — most visibly in §B Lemma 2's algebra (H1 / N4) and §A simulation-lemma step (H3). M1's C2 caveat *is* properly surfaced in §6 conclusion line 510 ("On coupled-goal architectures") — it just doesn't appear in headline locations. M3's `q_0` condition is in §5 line 16 but missing from §4 line 15.

**§4.4 page-budget candidate confirmed and sharpened.** On re-read, the case for relocating §4.4 ("Necessity of the four components") to an appendix is stronger than I framed yesterday: not just "self-contained ablation" but the body's narrative arc actually flows better without §4.4 interrupting between Theorem 4.1's unpacking (§4.3) and §5's mechanism narrative. Small refinement: when relocating, port the §1.1 line-254 framing of "without Component 2" (which says "loses *exactness* and the behavior-cloning interpretation vanishes") into §4.4's currently-thinner "we'd use Pinsker or BH instead" framing.

**Standing by for the Opus auditor's findings.** When they land, compare against my N1–N5; convergence is high-confidence, divergence is interesting.

---

## 2026-05-07 — De-novo audits (Gemini + Codex) integrated into TODO

Two de-novo audit reports landed: Gemini at `audits/de_novo_audit_2026_05_06.md` (terse, page-budget-leaning) and Codex at `audits/de-novo-audit-2026-05-07.md` (detailed, technical, six H-findings + three M-findings + trim recommendations + suggested triage). Joseph's instruction: do not take findings at face value; verify; with Codex specifically, attempt strengthening before any softening; add to TODO what *I* judge most valuable. Following AGENTS §3.5 (audit-grade tasks stay first-hand, no sub-agent farming) and §3.1 (strengthen-before-soften).

**Primary-source verification pass.** Read all cited segments first-hand: `src/re/01-introduction.md`, `03-preliminaries.md`, `04-main-result.md`, `05-mechanism.md`, `06-conclusion.md`, `B-key-lemma-proofs.md`, `D-algorithm.md`, `A-proof-of-composition.md`. Confirmed Codex's source extracts are accurate at the line numbers cited; the structural claims in each finding hold against the actual prose.

**Five real technical findings (H1–H5) — strengthening-spike candidates landed in TODO.**

- *H1 (Lyapunov scaling):* the proof's drift inequality `E[ΔV] ≤ -2𝒯_Σ V + ρ_Σ²` honestly gives `R* ~ ρ/√𝒯` (mean-square ultimate-bounded radius), not the stated `R* = ρ/𝒯`. The disturbance bound in `Model (S)` is *deterministic* (`|w_ij| ≤ ρ_Σ/|E|^{1/2}`), so the cross-term `2δ^⊤ w` was dropped (presumably treating `w` like zero-mean noise) when retaining it would give an honest analysis. The strengthening direction is to keep the cross-term with Cauchy–Schwarz; the resulting radius depends on regime — `ρ/𝒯` for slow correction, `ρ/√𝒯` for fast — and the threshold form may need a max-of-two-regimes restatement. Real, real spike.
- *H2 (bias term value-scale conversion):* the `N_h(1-p_id)log(1/q₀)·T` term added to dynamic regret is in *KL coordinates* added to a *value-scale* sum. The conversion needs the `1-e^{-D}` map (1-Lipschitz) plus `V_max`. The strengthening: the corrected term `N_h V_max(1-p_id) min(1, log(1/q₀)) T` *clips* to `V_max` (TV ≤ 1 always), which is a genuine *tighter* bound for `q₀ < 1/e`. So Codex's "this could exceed the trivial value range" softening recommendation, properly handled, becomes a strengthening — exactly the §3.1 pattern Joseph repeatedly observes.
- *H3 (V_max double-count):* `V_max(M_t)` defined in §3 as cumulative horizon-Q range; sim-lemma uses `V_max·N_h·TV`, multiplying by `N_h` again. Two equivalent fixes; pick (a) per-step convention so `V_max` denotes one thing throughout. Clean, no rate change, just notational hygiene.
- *H4 (B_T = optimum-change vs. stationary-segment):* §3 defines `B_T` as optimum-change count; (A5) and the proof's block decomposition assume *kernel*-stationarity within blocks. Real concern. Strengthening: argue under (A2) that non-optimum-changing kernel drift is absorbed by the strategic mismatch dynamics, making optimum-change count the right counting measure. Failing that, fall back to redefining `B_T` as kernel-change count (cleaner-but-different headline).
- *H5 (A1 satisfaction by deterministic UCB):* A1 requires `Q_t(a*)>0` pointwise; deterministic UCB sometimes deploys at `a' ≠ a*` with `Q_t(a*) = 0`. The expectation identity `E[1-e^{-K_t}] = E[1-Q_t(a*)]` extends through the K_t = ∞ limit and is finite, so the rate goes through in expectation, but the strict pointwise reading of A1 fails. Strengthening: define `Q_t` for deterministic UCRL2/UCBVI as the internal sampling/planning distribution rather than the deployed action; show smoothed deployment cost is controllable.

**Two smaller real fixes (M1, M3) — landed in TODO without spike.** "Interventional by construction" → "interventional under (C1)–(C3)" in headline locations (M1); add `(Q ≥ q₀)` qualifier to §4 perturbative-extension statement (M3).

**Two findings overlap with already-held strategy items.** Codex H6 (headline rate's bias-vanishes condition) is the same content as the held identity-vs-rate-as-headline question. Codex M2 (bundle-vs-monolithic abstract framing) is the same content as paper-1 / paper-3 / pipeline-agent's converging abstract-shape suggestion. Cross-referenced in TODO, not double-tracked.

**Strengthening direction worth flagging (O1).** Gemini's "elevate the universal-failure-class theorem to main text" is genuinely interesting — the gain-decay `𝒜_decay` structural theorem currently lives in App. C.6 and is one of the most reviewer-resonant claims in the paper. Paper-1 author's earlier review independently flagged it as a strength. Elevation to a numbered §4 theorem alongside Component 3, with proof staying in C.6, is a real strengthening of the §1 contribution headline. Filed as O1 in TODO.

**Dismissals — both Gemini and Codex on page-budget, plus two structural collapse recommendations.** Captured in TODO with reasoning; key calls:
- Gemini "merge §4–§5" — collapses the Jin-style restructure where §4 is formal main result and §5 is mechanism narrative with key lemmas surfaced. The split is doing genuine narrative work; merging undoes the restructure's whole point.
- Gemini "relocate §4.4 ablation to appendix" — page-budget thinking; §4.4 necessity argument is doing reviewer-anticipation work.
- Codex "trim NeurIPS Theory Track quote" / "trim BH/Pinsker repetition" — page-budget thinking. The repetition is doing emphasis-of-novelty work for a result whose entire technical contribution is its BH-corner relationship; a one-shot tightening pass might be worth it post-strategy-talk, but not via the kind of mechanical compression cycle Joseph has redirected away from.

**Audit reports archived** to `_archive/audits/` once integration into TODO verified (this entry is the verification — AGENTS §3.4 dictates LOG-confirms-integration before any move to `_archive/`).

---

## 2026-05-06 (rename + supersede) — Manifest stem renamed; rc1 archived

`OUT.full-paper-re.md` → `OUT.unified-rl-neurips-2026.md`. The `-re` suffix had been doing distinguishing work against the migration trim-survivor's `OUT.full-paper.md`, but `3e1a111` archived that manifest to `_archive/OUT.full-paper-migration.md`, retiring the disambiguation work. The new stem ties to title + venue, lowercase-with-hyphens to match the kebab-case slug pattern of the paper-dirs (`02-unified-convergence-rl`) and to play nicely on case-insensitive filesystems and as a directory name (`.build/<stem>/`).

Cascade: `git mv` for the manifest preserved history; rebuild emitted the new `.build/unified-rl-neurips-2026/` ephemeral dir, the canonical `unified-rl-neurips-2026.pdf` at paper root, and the `unified-rl-neurips-2026.extracted.bib` repo-visibility snapshot. Old-stem artifacts (`full-paper-re.pdf`, `full-paper-re.extracted.bib`) `git rm`'d; `full-paper-re.prior.pdf` was gitignored so just removed; `.build/full-paper-re/` (ephemeral, gitignored) `rm -rf`'d.

`paper-rc1.pdf` archived to `_archive/paper-rc1.pdf` as superseded. The only substantive difference between rc1 and `unified-rl-neurips-2026.pdf` is the §E table redesign (no content / theorem / proof change), so any "page X of rc1" notes still resolve to the same content. The single living PDF is now `unified-rl-neurips-2026.pdf`.

VISION.md updated in two places to reflect the rename: the line-39 "as of 2026-05-06" snapshot and the line-53 "manifest swap" step (now marked done with the new stem). The commit-`1d201f6` and "as of 2026-05-06" anchors stay as time-stamped statements; only the path/stem references updated.

Renamed-manifest header replaced its old "Coexists with `OUT.full-paper.md`" framing with the new context: rename rationale + pointer to the archived outline. Footer of the renamed manifest still mentions the stable umbrella infrastructure (`meta.md`, the umbrella-shared bib database via `bin/refs emit`).

---

## 2026-05-06 (cleanup) — TODO.md stale sections folded into LOG history

Three sections of `TODO.md` had outlived their use and got pulled out so the live tracker reflects only live work:

- **Read-through notes (build-pipeline agent's review of `paper-rc1.pdf`).** Strengths already itemized in the 2026-05-06 archival-pass entry above; suggestions either held for the strategy talk (abstract shape, closing-sentence tone) or rendered moot by `3e1a111`'s archival of the migration trim-survivor (the §F wide-table-overflow flag pointed at `src/08-related-work.md` which is now `_archive/migration-src/08-related-work.md`, no longer in any active manifest). The forward-`(2)`-ref spot-check is a one-time discipline note on cross-ref hygiene that doesn't need a TODO slot to live in — AUTHORING §1.7 / §2.2 cover it as standing convention.
- **Inbox flagged 2026-05-06 by build-pipeline agent.** Most concrete asks pointed at paths under `src/*.md` (the migration trim-survivor) which `3e1a111` archived to `_archive/migration-src/`, making the wide-table-overflow recommendations (1319pt at `src/08-related-work.md:5`, 267pt at `src/05-strategic-tempo.md:74`, 167pt at `src/03-two-gap-diagnostic.md:25`) all moot — no active manifest builds those paths. The page-budget reality check on `OUT.neurips-2026-paper` is moot for the same reason (manifest archived as `_archive/OUT.neurips-2026-paper-migration.md`). The bold-prefix-vs-callout question on `src/A-supporting-material.md:21` is moot (file archived). The §E Pinsker-numerics fix landed in `d35e6fd` (this session). All paths now resolved or archived; nothing carried forward.
- **Build-pipeline notice (BUILD-CHANGE.md announcement).** Adopted in `cb5e6fb` (this session): `out/` removed, `refs.bib` removed, `.gitignore` updated to ignore `.build/` and `*.prior.pdf`, `full-paper-re.pdf` and `full-paper-re.extracted.bib` tracked. The 2026-05-06 (continued) entry below covers the substance; the duplicated TODO section was a heads-up for a per-paper agent who hadn't yet adopted, which I have.

Also marked the migration-milestones list's `[ ] Build verification — blocked` line as resolved — the three kramdown-converter rendering bugs that originally blocked it (bold-prefix + `$$display$$` collision, `[[#^anchor]](text)` link-parser collision, `|…|` table-parser collision) were fixed during the build-pipeline owner's pass, and `bin/build` now runs cleanly under the new pipeline. The migration-milestones list itself stays in TODO as quick-orientation context for any agent picking up B-CS1 work.

What stays in TODO: Pass-5 carry-over (the per-paper-agent items — coherence drifts, free-trim candidates, compression candidates, reviewer-objection axes), preflight checklist (anonymization grep, citation verification, no-self-citation, etc.), citation-migration remaining work (multi-cite group hand-conversion, missing `junzhe-zhang-bareinboim-2022-online` bib entry), risk register. All real future-work items.

---

## 2026-05-06 (continued) — Build-pipeline refactor adopted; §E table redesigned

Resumed after the umbrella's `bin/build` interface refactor landed (umbrella `d24c9e8`; `BUILD-CHANGE.md` is the orientation doc). First action was confirming the new cwd-aware default works from inside the paper-dir: `bin/build` (no args) found `OUT.full-paper-re.md`, ran `bin/refs emit` to generate `.build/full-paper-re/full-paper-re.references.bib` from the umbrella YAML database, compiled cleanly, and printed the integrated page-budget report inline (13pp main-text, +4 over 9pp; appendices start p16, §E p25, §F p26). New tree artifacts after build: `.build/full-paper-re/` (ephemeral), `full-paper-re.pdf` (canonical), `full-paper-re.prior.pdf` (last-known-good snapshot), `full-paper-re.extracted.bib` (repo-visibility snapshot of the bib bibtex actually used). The pre-refactor `out/` and `refs.bib` are now orphans, both staged for a separate cleanup pass (not yet done).

**Read-through of `TODO.md` for current actionable items.** Most of the build-pipeline agent's earlier inbox flags pointed at paths under `src/*.md` (the migration trim-survivor) which I archived to `_archive/migration-src/` in `3e1a111` — those flags are now moot. The page-budget reality check on `OUT.neurips-2026-paper.md` is moot for the same reason (that manifest archived as `_archive/OUT.neurips-2026-paper-migration.md`). The build-pipeline agent's read-through-of-rc1 suggestions (abstract shape; closing-sentence tone; forward-(2)-ref check) overlap with the items already held for the strategy talk and are not for me to action solo.

**The single current actionable item: §E Pinsker-numerics table rendering.** The build-pipeline agent walked back their earlier framing and explained the actual issue: I had wrapped the table in `[!table] ... cols="r r r r r X"` correctly per AUTHORING §1.4, so it didn't show up in `Overfull \hbox` log warnings (tabularx-X never overflows horizontally — it just wraps too narrowly). Visible at `paper-rc1.pdf` p25: the 5 r-aligned numeric headers (especially the wide `$\min(\sqrt{D_{\mathrm{KL}}/2}, 1)$` column-5 header) claimed natural width, leaving the X column squeezed to maybe one third of its share, with the "Pinsker / iden- tity ra- tio" header breaking vertically and "(Pinsker fully vac- u- ous)" cell content stacking. Three layered fixes recommended; all three apply, all three converge to the same redesign:

1. *Drop column 5* (`$\min(\sqrt{D_{\mathrm{KL}}/2}, 1)$` — mathematically equivalent to col 4 below $D_{\mathrm{KL}} = 2$ and equal to $V_{\max} = 1$ above; the saturation gradient lives more cleanly in prose).
2. *Move the `(Pinsker = trivial)` / `(Pinsker vacuous)` / `(Pinsker fully vacuous)` annotations* out of the data column into the explanatory sentence below the table.
3. *Rename the rightmost header* "Pinsker / identity ratio" → "ratio" — the prior columns make the rationing object obvious.

Strengthen-before-soften check: this isn't softening. Col 5 is a derived view of col 4 (the cap operation), so removing it is information-equivalent. The interpretive prose in data cells is layout-clutter, not load-bearing claim. Information content of the table + surrounding prose is unchanged after the fix; the prose now carries the saturation gradient cleanly: *"Pinsker, by contrast, hits the trivial envelope $V_{\max} = 1$ at $D_{\mathrm{KL}} = 2$ and exceeds it beyond — vacuous from $D_{\mathrm{KL}} = 4$ onward (where the unclipped form returns 1.414), fully vacuous by $D_{\mathrm{KL}} = 10$."*

Post-fix verification: rebuilt clean, two cosmetic <30pt overfulls remain (the same residual already noted in the prior LOG entry — kerning slop in tabularx columns, not the §E case). `pdftotext -layout` extraction of pages 25–26 confirms the table renders as a tight 5-column grid with all rows on one line and the "ratio" header fitting flush. §F shifts from p25 to p26 as a side-effect — §E is now compact enough that §F's prior-art retrieval summary doesn't collide on the same page.

**Honesty note on durability slip from prior session.** The previous session's commit `3e1a111` ("Archival pass: feedback captured in LOG, migration trim-survivor + completed outline → _archive") claimed in its message that the 2026-05-06 feedback was captured in LOG.md, but the LOG.md edit was authored in working-tree only and never staged for that commit — the archival moves landed; the LOG record did not. This is exactly the AGENTS §3.3 failure mode (durability-claims-must-be-verified): the commit message asserted a durable artifact existed when only the working-tree edit existed. The 29-line LOG.md hunk was carried forward uncommitted into this session and is being committed now alongside the §E fix and this continuation entry. Flagging here so the chronology is honest: future agents reading the LOG will find the 2026-05-06 archival-pass entry, and from `git log` will see it actually landed two commits later (this one) rather than at `3e1a111`.

---

## 2026-05-06 — Peer + pipeline review captured; concrete bugs fixed; archival pass

Three independent reviews of `paper-rc1.pdf` (the rc1 snapshot of `OUT.full-paper-re.md`) landed on 2026-05-06: the paper-1 author left peer notes at `audits/peer-review-from-01-tragedy-2026-05-06.md`, the paper-3 author at `REVIEW-FROM-paper-3.md`, and the build-pipeline agent appended read-through notes to the top of `TODO.md`'s inbox. Capturing what they collectively said and what was acted on before any archival cleanup.

**What they collectively flagged.**

*Strengths landed across reviewers (no action — mentioned to log what's working):*
- The "compose four existing pieces, no new bounding technique" epistemic register (paper-3): the careful self-positioning that the identity is "BH's envelope collapsed at the deterministic corner" rather than a new inequality.
- Related-work organization by named lineages with explicit "same shape, different axis" / "we compose with non-stationarity" discrimination (pipeline-agent, paper-1 explicitly will borrow this pattern back).
- Honest scope statements — "Theory only / no experiments. The point-mass identity is mathematically airtight..." plus "Canonical scope" + "Comparator regime" make the paper's limits visible immediately (pipeline-agent).
- The `\mathcal A_{\mathrm{decay}}` "universal failure" theorem with concrete instances (Bayesian count-accumulation, Robbins–Monro) — paper-1 noted this makes the claim verifiable in a reader's hands rather than abstract.
- Quantitative comparisons at concrete operating points ("$D_{\mathrm{KL}} = 0.01$ → 7× tighter; $D_{\mathrm{KL}} > 2$ → Pinsker vacuous") — paper-1 noted the reader felt the tightening rather than being told about it.
- ProST recovery as Beta-Bernoulli special case `|E|=1, ν=ι=1` preempts the "is this just composition?" reviewer concern (paper-1).
- §1.2 Scope-and-Limitations placed before §2 Setup forecloses misreading early — paper-3 will copy this pattern back.

*Concrete bugs flagged, both fixed in commit `bb2bc5b`:*
- **Five unresolved cross-refs in `src/checklist.tex`** (`sec-pointmass-identity`, `sec-strategic-tempo`, `sec-loop-level2`, `sec-composition`, `sec-limitations`) — old `src/` anchor names not present in `src/re/`'s structure. Paper-3 noted the draft-mode fallback also disables clickable hyperlinks PDF-wide whenever any cross-ref is unresolved. Fixed by rewriting the two affected justifications descriptively without specific section refs (robust to either manifest, future-restructure-safe). 0 undefined-reference warnings after.
- **Two visible-in-PDF wide-table overflows** — bidirectional thresholds (App. C.6, 267pt over) and Pinsker numerics (App. E, 114pt over). Both wrapped in `[!table] cols=...` callouts per AUTHORING §1.4 (`cols="X l X"` for the bidirectional table; `cols="r r r r r X"` for Pinsker). All large overfulls (>50pt) gone after; remaining <30pt overfulls are cosmetic kerning slop within tabularx columns — verified by `pdftotext -layout` extraction that the rendered output wraps cleanly.

*Held for the strategy talk (not actioned):*
- Abstract length — three independent reviewers flagged this as the longest of the three papers (~30 lines / ~400 words, vs. NeurIPS norm ~150-250 words). Held because the OpenReview-locked vs. body-update window question is owner-level.
- "Identity buried as Component 2" framing (paper-3) — alternative answer to my Q2 (cumulative-rate-as-headline + identity-as-technical-anchor vs. identity-as-headline). Held for the strategy talk.
- §5 mechanism narrative chain (paper-1) — Jin 2020 mechanism.tex pattern is *naive-approach → obstacle → resolution-via-lemma*. My current §5 states the lemma first, then gives intuition. Re-read confirmed paper-1's reading; rewrite is real but prose-style work that benefits from owner review before committing.
- Pipeline agent's page-count / abstract-trim feedback — explicitly out of scope per Joseph's directive (no compression-thinking).

**Archival action.** With concrete bugs fixed and held items captured here, the source feedback files move to `_archive/`. The migration trim-survivor (`src/*.md`, old `OUT.full-paper.md`, old `OUT.neurips-2026-paper.md`) also moves to `_archive/migration-src/` since `src/re/` + `OUT.full-paper-re.md` are now the canonical working set. `OUTLINE-RESTRUCTURED.md` moves to `_archive/` since its purpose (guiding the `src/re/` authoring pass) is complete and the result is committed. `src/checklist.tex`, `src/_recovery/`, and the active `OUT.full-paper-re.md` + `src/re/*` stay at the working tree's top level.

---

## 2026-05-05 — Migration: body + appendices + citations + prior-art ported

Following the scaffolding milestone (below), the substantive content migration landed across four further commits:

**Body segments §1–§9 + references** (`14672ef`). Segmented `paper-draft.md` into `src/01-introduction.md` ... `src/10-references.md` — one segment per top-level section. Inline-bold theorem pattern (`**Theorem 4.1 (...)**`) converted to Obsidian callouts (`> [!theorem] ... ^thm-pointmass-identity`) so the build can route to AmsThm `\begin{theorem}` envs with shared counters per AUTHORING §1.1. Heading-prefix manual numbering (`## 1  Introduction`) stripped per §1.8. Cross-references via `[[#^anchor]]` (cleveref auto-types). Source's lone `\tag{P}` (the persistence condition equation in §5.2) converted to anchored `^eq-persistence-condition` per §1.7; in-text "(P)" references rewritten as `[[#^eq-persistence-condition]]` rendering "(N)". `[Author Year]` citations preserved verbatim for the migrate-cites pass. Pass-5 (a)(i) ($N_h$ horizon factor not propagated to abstract / §1.1 / §9.3) and (a)(v) (`directed-separation` anonymization hit at §9.1) left as inline `> [!todo]` callouts for per-paper agent — parity-first reframe per Joseph's direction.

**Appendix segments A–G + manifests** (`7832b79`). Appendix A as one segment with internal A.1–A.8 sub-headings (per orientation plan; finer split deferred to per-paper agent if trim needs it). Appendices B, C, D, E, F, G each as separate segments. `OUT.full-paper.md` lists everything; `OUT.neurips-2026-paper.md` is the 9pp draft cut omitting B/C/D/E via `<!-- ... -->` row-comments per AUTHORING §7.2 — A and F kept (load-bearing for body theorems), G kept (main proofs). Manifest narrative explains the cut rationale and flags within-segment compression (§3.3, §4.4, §4.6, §6.3, §7 lead, §7.2, §9.2, §9.3) as per-paper-agent territory post-migration. Source's "(§5.5)" reference bug in §8 row 3 self-resolves under anchor-based references — `[[#^sec-prost-lift]]` renders to "Section 5.4" automatically.

**Citation migration via `bin/migrate-cites`** (`2f9d466`). Tool dry-ran first on §1 as pilot — clean. `--apply` rewrote 22 single-cite occurrences across 10 segments. Multi-cite groups `[Author Year; Author Year; ...]` intentionally skipped by the migrate-cites regex per PIPELINE-TODO §C1.4 (the tool can't auto-disambiguate the right `\cite{}` form for groups); per-paper agent will hand-convert. One missing bib entry surfaced: `[Zhang-Bareinboim 2022]` in `src/D-prior-art-summary.md` — paper-draft.md references "Junzhe Zhang, Bareinboim 2022 — Online RL for mixed policy scopes, NeurIPS"; needs `bin/refs add junzhe-zhang-bareinboim-2022-online` before any final build.

**`prior-art/` ported.** Three files copied verbatim from `~/src/neurips2026/02-convergence/prior-art/` — `query.md`, `report.md`, `positioning.md`. Old workspace stays as historical record; no history surgery.

**Build verification — blocked by pipeline bugs.** First `bin/build 02-unified-convergence-rl OUT.full-paper.md` failed at lualatex pass 1. Three distinct kramdown-converter rendering bugs filed at umbrella `PIPELINE-TODO.md ## Inbox` (commit `654da9c`):

1. **Bold-prefix paragraph + immediately-following `$$display math$$` emits unbalanced `\begin{equation}…$$`.** Pattern `**Term.**\n$$math$$\nmore prose` (no blank line between bold-prefix-with-continuation and display math) — converter opens `\begin{equation}` but leaves closing `$$` unconverted; from that point on, the rest of the segment leaks through as raw markdown. Source paper-draft.md uses this definition-style pattern ~15+ times.
2. **`[[#^anchor]](text)` parses as markdown link `[label](url)`.** Source `[[#^thm-composition]](v)` (intended: cleveref + literal "(v)") renders as `\href{v}{[#^thm-composition]}` because kramdown's link parser merges `]](` into `[label](url)` matching.
3. **Unescaped `|…|` in inline math triggers kramdown table parser.** Source `$B_T := |\{t : ...\}|$` (set cardinality / absolute value) gets paragraphed as a `\begin{tabular}` block. Bug 3 is independently reinforced by a finding paper #1's migration agent already filed at the same inbox.

Per Joseph's direction, no source-side workarounds applied — the build sits broken until the pipeline owner addresses the inbox bugs. Source files are durable as parity ports regardless of build state. Per-paper agent should re-run `bin/build` once each inbox bug lands `RESOLVED-IN-<commit>`.

**Pattern observation for future migration agents.** The build-as-you-go discipline (per AUTHORING §8.10 / paper #1 orientation §1.10) was not applied early enough — I authored §1–§9 + Appendix A before running the first build. A build at, say, the §1+§2 mark would have surfaced bugs 2 and 3 before they propagated through 10 segments. Recommendation for paper #3 migration: build after the first 1–2 segments, before scaling.

---

## 2026-05-05 — Migration scaffolding

Migration agent #2 began per `MIGRATE-TODO.md` §A2. Pre-work orientation snapshot landed at `_archive/orientation-2026-05-05.md` (Joseph's request — pre-work record, not an integrated artifact, the conventional `_archive/` use per AGENTS §3.4).

**Scaffolding milestone.** Created submodule layout per AUTHORING §7.1: `audits/`, `out/` (build artifacts, gitignored), `spikes/`, `src/`. Skipped `simulations/` and `results/` — B-CS1 is theory-only, no empirical anchor (worked-reduction to Lee et al.\ ProST via Lemma 5.2 composes their published experiments rather than running new sims). `.gitignore` covers `out/` + `.DS_Store`. `meta.md` carries title (locked 2026-05-05 evening per source `OUTLINE.md`) + author block (suppressed in default anonymized builds by the neurips_2026 sty) + 250-word abstract verbatim from `paper-draft.md` line 5. `TODO.md` distills the residual content from source `OUTLINE.md` so the per-paper agent has a clean handoff.

**Migration framing — parity-first.** Per Joseph's reframe: the migration agent's job is parity, not iterative improvement. Pass-5 (a) coherence drifts, (b) free-trim candidates, (c) compression candidates, and (d) reviewer-objection axes are *not* my territory — they go into `TODO.md` as `> [!todo]` callouts in the relevant segments and as per-paper-agent items in `TODO.md`. The 14pp→9pp gap is enabled (not closed) by manifest-level subsetting via `<!-- ... -->` row commenting per AUTHORING §7.2; cuts wait for the per-paper agent post-migration. `long-form.md` augmentation likewise deferred — author segments from `paper-draft.md` only.

**What's next.** Body segments §1–§9 from `paper-draft.md` (Task #2); appendix segments A–G (Task #3); manifests (Task #4); citation migration via `bin/migrate-cites` (Task #5; signed off per `PIPELINE-TODO.md` §C1.4); build verification (Task #6); `prior-art/` port (Task #7); final tracker polish + push (Task #8).
