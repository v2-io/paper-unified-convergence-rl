# TODO.md — 02-unified-convergence-rl live work

*Active items for B-CS1. Free to branch into `TODO-trim.md` / `TODO-citations.md` / etc.\ as scope grows — no fixed schema. For history see `LOG.md`. For umbrella-level migration backlog see `~/src/neurips/MIGRATE-TODO.md`.*

---

## Read-through notes — 2026-05-06 (rc1 = `OUT.full-paper-re.md`)

Read through `out/full-paper-re.pdf` carefully. Genuinely strong — the four-component composition reads as one argument now, not four parallel pieces. A few reactions and a few suggestions:

**What's working well:**

1. *Related-work organization.* Six named lineages (variation-budget dynamic regret, two-term decompositions, tempo and forgetting, causal/interventional access, info-theoretic regret cross-cutting, satisficing/feasibility adjacent, contemporaneous post-March-2026), each with explicit "Same shape, different axis" or "We compose with non-stationarity" discrimination. This reads as a contribution-distinguishing prior-art map rather than a literature dump. Reviewers love this shape.
2. *Honest scope statements.* "Theory only / No experiments. The point-mass identity is mathematically airtight..." and "Canonical scope" + "Comparator regime" make the paper's limits visible immediately rather than requiring §6 archaeology to find. AUTHORING §3.3 voice discipline at work.
3. *Title.* Colon-free, descriptive, fits §6.1 alternative form: "A Unified Convergence Theory for Non-Stationary Reinforcement Learning." Easy to remember; the four-component composition is the hook in §1, not the title.
4. *Point-mass identity placement.* The exact algebraic form `TV(δ_{a^*}, Q) = 1 - e^{-D_KL}` lands as a closed-form identity rather than an inequality, and the strict-tighter-than-Pinsker-and-BH framing in §1.2 is doing genuine novelty-positioning work. Worth keeping that as a load-bearing thread through §4 and §C.

**Suggestions:**

1. *Abstract shape — the (i)-(iv) enumeration reads as a TOC, not a result-arc.* The four-component bullet-prose listing through the middle of the abstract gives a reviewer who skims a punch list of structural ingredients rather than an arc that lands a result. AUTHORING §6.3 frames the abstract as "what we do → how we do it → what we find" — so the gap-and-compose framing → rate → identity-vs-Pinsker tightness → orthogonal-axis distinction from the frequency lineage would read as a single arc, with the (i)-(iv) breakdown landing in the §1.2 contributions list where it has room. The "structural failure class" / "perturbative extension" / "ProST reduction" closing details are also each their own §1.2 paragraph; the abstract closing tightens if it ends on the orthogonal-axis-to-frequency framing.
2. *Wide-table overflow at §F (extended related work).* The 7-row strand-comparison table at `src/08-related-work.md:5` is currently a bare markdown table emitting `\begin{tabular}{lll}` with natural-width columns — and the middle column has multi-cite bibkey lists that don't break across lines. lualatex's compile log shows a 1319.59 pt overfull-hbox warning on this table, meaning a column runs ~18 inches past `\textwidth` (content silently clips off the right page edge in the rendered PDF). AUTHORING §1.4 covers wide tables: wrap in a `[!table] cols="l X X" ^tab-related-work-strands` callout and the `X` columns text-wrap within `\textwidth`. The same treatment helps the §3 two-gap diagnostic (167pt over) and §5 strategic-tempo mechanism table (267pt over). All three are flagged with concrete recipes earlier in this Inbox.
3. *Closing-sentence tone in abstract.* "Theory-only; Lemma 5.2 makes the ProST reduction rigorous at the sector-parameter level, while their published experiments motivate the strategic-tempo component" reads slightly defensive — "theory-only" + "motivate via others' experiments" is two scope-acknowledgments in one sentence. Consider closing on the result rather than the limit; the scope already lives in §1's "Theory only" callout, no need to double it in abstract.
4. *Forward `(2)` ref in abstract or main text.* Numbered eqref forms render fine via `[[#^eq-...]]` (AUTHORING §1.7 / §2.2). Spot-check that any "as in (2)" prose in §3+ uses anchored cross-refs not literal numbers — the anchored form survives reorganization, the literal form drifts silently.

---

## Inbox — flagged 2026-05-06 by build-pipeline agent

**Page-budget reality check.** `bin/build 02-unified-convergence-rl neurips-2026-paper` currently produces a 22-page PDF; main text (§1–§9) ends at page 12, References starts at page 13. That's **3 pages over** the 9-page main-text limit — the existing manifest already comments out appendices B/C/D/E but the body is still long. The migration agent's earlier note ("9pp draft cut … A and F kept as load-bearing; G kept for main proofs") was about appendix selection; main-text trim hasn't happened yet. Suggested next: either (i) deeper segment-level compression (the OUTLINE risk register identified candidates: §6.1 fold to footnote, §6.5 collapse to paragraph, §5.7/§5.8 unify, §5.6 Theorem 5.5 to Appendix C), or (ii) commit to a heavier appendix shift. Please verify the actual numbers via `pdfinfo out/neurips-2026-paper.pdf` and update the OUTLINE before deciding.

**Bold-prefix vs callout form — verify intentional.** `src/A-supporting-material.md:21` uses `**Corollary.** $\delta_{\mathrm{sat}}^{\mathrm B} \le \dots$` as a paragraph-prefix. AUTHORING §1.1 prefers Obsidian `> [!corollary] ^anchor` for theorem-shaped semantic blocks; AUTHORING §1.9 allows bold-prefix for plain paragraph headings. The corollary at line 21 has no number and no cross-ref, so bold-prefix may be intentional — but if the claim is referenced anywhere or warrants a numbered counter, it should move to a callout. Quick read: if `\Cref{cor-…}` is wanted, convert; otherwise leave.

**Three wide-table overflows — `[!table] cols="..."` refactor candidate.** lualatex compile log reports three overfull-hbox warnings on bare markdown tables, with the worst at **1319 pt too wide** (≈18 inches past page edge — content silently runs off the page in the rendered PDF):

- `src/08-related-work.md:5` — *Strand × Closest neighbors × Our distinction* table. The middle column has multi-cite bibkey lists that don't break across lines. **1319.59pt overfull** — the worst of the three; clipping is severe.
- `src/05-strategic-tempo.md:74` — *Mechanism × α_Σ^ss × Prerequisite-holds-iff* table. **267.20pt overfull**.
- `src/03-two-gap-diagnostic.md:25` — Two-Gap 2×2 diagnostic table with prose-shaped cells. **167.13pt overfull**.

These are bare markdown tables (no `[!table]` callout wrapper), so the pipeline emits them as `\begin{tabular}{lll}` with natural-width columns — fine for narrow tables, broken for prose-cell tables. Wrap each in a captioned + anchored `[!table]` callout with `cols="l X X"` (or as appropriate) per AUTHORING §1.4. The `X` columns wrap text within `\textwidth`. Concrete example for the related-work strand table:

```
> [!table] Closest-neighbor strands and their distinctions. ^tab-related-work-strands
> 
> | Strand | Closest neighbors | Our distinction |
> |:-------|:------------------|:----------------|
> | 1. Dynamic regret under drift | \cite{cheung-2020-reinforcement, ...} | Recovered as instances ... |
```

…with `cols="l X X"` on the marker. That'll re-flow the long-prose cells within textwidth and eliminate the 1319pt clipping. The two narrower overflows (267pt / 167pt) are also good candidates for the same treatment — the prose-shaped cells in the diagnostic and mechanism tables benefit from `X`-column text-wrapping. Wrapping them in `[!table]` callouts also gives them captions + cross-ref anchors, which the bare markdown tables currently lack.

---

## Migration milestones (agent #2, 2026-05-05) — landed

- [x] **Scaffolding** — dirs + `.gitignore` + `meta.md` + `LOG.md` + this file. Commit `9dd4cd7`.
- [x] **Body segments §1–§9 + references** — `src/01-introduction.md` ... `src/10-references.md`, one segment per top-level section. Inline-bold theorem pattern → Obsidian callouts. Heading-prefix numbering stripped. `\tag{P}` converted to anchored `^eq-persistence-condition`. Cross-refs via `[[#^anchor]]`. Pass-5 (a)(i) and (a)(v) coherence drifts left as `> [!todo]` callouts in §1.1 / §9.1 / §9.3 for per-paper agent. **§8 row 3 cross-reference bug** (source's "(§5.5)" → migrated as `[[#^sec-prost-lift]]` which renders "Section 5.4" correctly; bug item below resolved as a side-effect of anchor-based refs). Commit `14672ef`.
- [x] **Appendix segments A–G + manifests** — `src/A-supporting-material.md` (with A.1–A.8 sub-headings) + `src/B-pinsker-numerics.md` + `src/C-chain-rule-uniqueness.md` + `src/D-prior-art-summary.md` + `src/E-algorithm-sketch.md` + `src/F-bias-bound.md` + `src/G-proof-sketches.md`. `OUT.full-paper.md` (everything) + `OUT.neurips-2026-paper.md` (9pp draft cut — appendix B/C/D/E commented out via `<!-- ... -->` per AUTHORING §7.2; A and F kept as load-bearing; G kept for main proofs). Manifest narrative explains the cut rationale and flags within-segment compression as per-paper-agent territory. Commit `7832b79`.
- [x] **Citation migration** — `bin/migrate-cites --apply` rewrote 22 single-cite occurrences across 10 segments. Multi-cite groups `[A Year; B Year; ...]` intentionally skipped by the migrate-cites regex (per PIPELINE-TODO §C1.4); per-paper agent hand-converts those. One missing entry surfaced: `[Zhang-Bareinboim 2022]` in §D needs `bin/refs add junzhe-zhang-bareinboim-2022-online`. Commit `2f9d466`.
- [x] **`prior-art/` ported** — `query.md`, `report.md`, `positioning.md` copied verbatim from `~/src/neurips2026/02-convergence/prior-art/`.
- [ ] **Build verification — blocked.** `bin/build 02-unified-convergence-rl OUT.full-paper.md` failed at lualatex pass 1 with three kramdown-converter rendering bugs filed at umbrella `PIPELINE-TODO.md ## Inbox` (commit `654da9c`): (1) bold-prefix paragraph + immediately-following `$$display$$` math emits unbalanced `\begin{equation}…$$`; (2) `[[#^anchor]](text)` parses as markdown link `[label](url)`; (3) unescaped `|…|` in inline math triggers kramdown table parser. Build verification waits on pipeline-owner fixes. Per-paper agent should re-run `bin/build` once the inbox bugs land as `RESOLVED-IN-<commit>`.

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
