# TODO.md — 02-unified-convergence-rl live work

*Active items for B-CS1. Free to branch into `TODO-trim.md` / `TODO-citations.md` / etc.\ as scope grows — no fixed schema. For history see `LOG.md`. For umbrella-level migration backlog see `~/src/neurips/MIGRATE-TODO.md`.*

---

## Migration in-flight (agent #2, 2026-05-05)

Working tasks tracked in the agent's TaskList. Milestones surface here once landed and tracked durably:

- [x] Scaffolding milestone (dirs + .gitignore + meta.md + LOG.md + this file).
- [ ] Body segments §1–§9 from `paper-draft.md` → `src/01-introduction.md` ... `src/09-limitations-conclusion.md`. Default boundary: one segment per top-level section.
- [ ] Appendix segments A–G → `src/A-supporting-material.md` (with A.1–A.8 sub-headings) + `src/B-pinsker-numerics.md` + `src/C-chain-rule-uniqueness.md` + `src/D-prior-art-summary.md` + `src/E-algorithm-sketch.md` + `src/F-bias-bound.md` + `src/G-proof-sketches.md`.
- [ ] Manifests: `OUT.full-paper.md` (everything) + `OUT.neurips-2026-paper.md` (9pp budget — same segments, with §A.4 / §A.7 / §A.8 commented out via `<!-- | ... | -->` per AUTHORING §7.2).
- [ ] Citation migration via `bin/migrate-cites` (signed off per `PIPELINE-TODO.md §C1.4`). Pilot on §1 first; bulk-apply per segment; ambiguous matches and missing keys flagged below.
- [ ] Build verification: `bin/build 02-unified-convergence-rl OUT.full-paper.md` + `OUT.neurips-2026-paper.md`. Visual confirm.
- [ ] `prior-art/` port from old workspace (`query.md`, `report.md`, `positioning.md`).
- [ ] Final commit + push to `v2-io/paper-unified-convergence-rl`.

---

## Pass-5 carry-over from source OUTLINE.md — for per-paper agent post-migration

Captured here so the per-paper agent has a clean handoff. Migration agent #2 leaves these as `> [!todo]` callouts in relevant segments where they touch concrete locations; resolution is per-paper-agent territory.

### (a) Coherence drifts (integration bugs from Pass-4 spike-N1/N2 landing)

- [ ] **$N_h$ horizon factor not propagated** — abstract, §1.1 (iv) bullet, §9.3 conclusion still carry pre-strengthen rate $O(V_{\max}\sqrt{(B_T+1)T})$; §7.1(v) and `meta.md` abstract (mirroring submission) currently lack the $N_h$ factor that landed in body Theorem 7.1(v) proof. Abstract is locked at OpenReview but body can be updated until May 6 AOE — body fix is a one-line addition; abstract+conclusion should follow in body for consistency.
- [ ] **§8 row 3 cross-reference bug** — Related Work table row 3 references "(§5.5)"; the §5.5 lift content now lives in §5.4 after structural-move B renumbering. One-character fix.
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

## Citation migration — pending and ambiguous matches

*Populated as `bin/migrate-cites` runs surface them. Hyphenated multi-author keys (Cheung-Simchi-Levi-Zhu, Long-Fei Li-Zhao-Zhou, Bareinboim-Correa-Ibeling-Icard, Anderson-Moore) are the trickiest pattern; same-year ambiguities likely on Lee 2023/2024.*

(Empty until migration runs.)

---

## Risk register (carried from source OUTLINE.md, ~13 lines)

- **Compression risk: HIGH.** 9 pages with zero margin under Option 2' framing. If §4 BH identity needs another half-page, §7 must compress, weakening the unification narrative. Watch during trim.
- **Reviewer-variance risk: HIGH.** Sympathetic reviewers see unification + BH anchor → 8-9. Skeptical reviewers see "assembled known results with cite-and-distinguish" → 4-5. Mean fine; variance high. BH anchor caps downside (a reviewer who understands information theory cannot score the BH bound below 5).
- **Reduction-not-derivation risk: medium.** Skeptical reviewer may ask "where's the new theorem?" — answer: BH-identity-in-RL is the new theorem statement; composition is assembly + interpretation. Frame honestly in abstract and §7.
- **Empirical-validation absence risk: medium.** NeurIPS reviewers expect empirical validation. Mitigations: (a) BH identity is mathematically airtight; (b) worked-example reduction to Lee et al. ProST gives empirical grounding via their experiments; (c) honest "theory paper" framing in §9 limitations.
- **Citation-hallucination risk: low-medium.** Lee et al. ProST 2023/2024, Long-Fei Li-Zhao-Zhou 2024 are recent — verify carefully; Bareinboim, Russo-Van Roy, Bretagnolle-Huber 1978 well-cited and stable.
- **Scope-creep risk: medium.** The composition theorem invites extending beyond four components to strategic-DAG details, edge-update gain derivations. Hold the line at four named components; defer everything else to appendices or follow-up.
