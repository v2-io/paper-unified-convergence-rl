# Migration agent orientation — 02-unified-convergence-rl (B-CS1)

*Written by the second migration agent, 2026-05-05, before any migration work has begun. Frozen pre-work snapshot — what I think the job is, where I'd flag uncertainty, what I'm committing to attend to.*

*Lives in `_archive/` per Joseph's ask, parallel to paper #1's orientation. This isn't an integrated audit (the convention `_archive/` is normally for, AGENTS §3.4); it's a "what I thought before I started" record. Once migration is meaningfully under way, the live state moves to this submodule's `TODO.md` / `LOG.md` and this file is no longer load-bearing.*

---

## What this work is

Second migration agent — **paper #2**: *A Unified Convergence Theory for Non-Stationary Reinforcement Learning* (inventory ID **B-CS1**). Single-author theory paper. Per `MIGRATE-TODO §A2`, paper #2 is "mechanical" once paper #1 establishes the pattern — but reading the umbrella state, paper #1's first migration agent has only laid down its orientation file (no segments, no manifests yet). I'm effectively starting in parallel from the same point, holding to whatever pattern emerges from paper #1's actual execution as it lands.

**Source** at `~/src/neurips2026/02-convergence/`:

- `paper-draft.md` (696 lines) — most-correct form, post Pass-1 → Pass-5 audit cycle + spike-N1/N2 strengthen integration. Compiles clean to `paper.pdf` (388 KB, 14pp main + appendices). Per AUTHORING §8.0 this is the authoritative substrate.
- `long-form.md` (890 lines) — content superset, unconstrained version. Likely substrate for `OUT.full-paper.md` augmentation; but late-cycle Pass-3/4/5 fixes only landed in paper-draft, so paper-draft is the baseline.
- `OUTLINE.md` (175 lines) — paper plan + Pass-5 live audit findings + page-budget tracker + risk register. Phased out as a doc form (replaced by `OUT.*.md` + per-paper `TODO.md`/`LOG.md`); content I need migrates into the new trackers.
- `LOG.md` (~35 KB, 318 lines) — full Pass-1 → Pass-5 audit history + spike integration. Stays in old workspace as historical record; new `LOG.md` here starts fresh from migration milestone forward.
- `prior-art/` — `query.md`, `report.md`, `positioning.md` — Undermind retrieval + four-strand positioning synthesis (63 papers, ~75% abstract-level coverage).
- `_archive/` — Pass-3/4 audit relics (CODEX, GEMINI, CODEX-detailed) + three spike directories (`spike-citation-verification`, `spike-N1-N2-strengthen`, `spike-page-budget-compression`, `strategic-tempo-derivation/`, `strengthen-sweep/`) + `audit-fix-log.md`. Per AUTHORING §8.0 + `MIGRATE-TODO §C3`: **do not port** — fresh `_archive/` here; old workspace stays as historical record at `~/src/neurips2026/02-convergence/_archive/`.

**Target submodule** at `~/src/neurips/02-unified-convergence-rl/` — currently empty except `README.md`, this file, and a `.git` pointer to `v2-io/paper-unified-convergence-rl`.

**Key paper-#2-specific facts** (worth knowing on cold start):

- **Theory-only.** No `simulations/` or `results/` scaffolding (those are B-N4 territory). Empirical anchor is the worked reduction to Lee et al.\ 2023 ProST via Lemma 5.2 — composed from their published experiments, no original sim code.
- **14pp main + heavy appendices A–G.** Currently 5pp over the 9pp Main Track limit. Trim is the most pressing in-flight challenge for the per-paper agent, but per AUTHORING §8.0 trim is *not a migration concern* — the right move is segmentation that *enables* downstream trim via manifest-level subsetting (§7.2). The 9pp `OUT.neurips-2026-paper.md` manifest will reference a subset / reordering of the same `src/` segments the unconstrained `OUT.full-paper.md` references.
- **Heaviest appendix structure of the three papers.** A.1–A.8 (sub-appendices under A — Convention hierarchy, Admissible-divergence family, Direction-forcing, Action-gap matching lower bound, Tied-optimum + softmax, Theorem 4.3 derivation, Strategic-tempo across topologies, Loop-as-causal-engine deployment modes) plus standalone B (Pinsker numerics) / C (Chain-rule uniqueness Theorem C.1) / D (Prior-art retrieval summary) / E (Algorithm sketch) / F (Theorem F.1 bias bound) / G (Proof sketches for Theorem 5.1 + 7.1(v)). Segmentation choice will affect manifest-trim flexibility a lot here.
- **Inline-bold theorem pattern** (per `MIGRATE-TODO §A2`): `**Theorem 4.1 (Point-mass reverse-KL/TV identity).**` → Obsidian callout `> [!theorem] Point-mass reverse-KL/TV identity ^thm-pointmass-identity`. Counted: 7 named theorems (4.1, 4.2, 4.3, 5.1, 6.1, 7.1, F.1, C.1) plus Lemma 5.2, Corollaries 5.1a / 5.1b. Counterexamples / proof sketches use plain prose.
- **~70 citations**, bracketed-author-year inline form `[Cheung-Simchi-Levi-Zhu 2020]` / `[Lee et al.\ 2023]`. Hyphenated multi-author keys (Long-Fei Li-Zhao-Zhou; Cheung-Simchi-Levi-Zhu) are the trickiest cases for `bin/migrate-cites`.
- **Equation anchors needed.** Source uses display math liberally but I need to count `\tag` usage; cross-reference vocabulary in prose ("Theorem 4.1", "(P)", "(C1)–(C3)", "(A1)–(A5)", "§4.6", "Lemma 5.2") needs the `[[#^anchor]]` treatment.
- **Pass-5 in-flight findings carry forward.** OUTLINE.md §"Pass 5" enumerates seven (a)-coherence drifts and three (c)-compression candidates that landed in OUTLINE notes but not the source. Not my primary scope — but if I'm touching the relevant segment for migration, drive-by fixes are welcome per AUTHORING §8.0 (with a `LOG.md` note for traceability). Strengthening the (d)-reviewer-objection items is per-paper-agent territory after migration lands.

## What I read to orient

In load order:

1. `~/src/neurips/AGENTS.md` (= `CLAUDE.md`) — process / language / multi-agent coordination.
2. `~/src/neurips/AUTHORING.md` — segment authoring rules, NeurIPS-relevant slice, per-paper layout (§7), migration recipe (§8 + §8.0 answers from paper #1 framing).
3. `~/src/neurips/MIGRATE-TODO.md` — §A2 is this work; §C3 already-resolved (no `_archive/` port).
4. `~/src/neurips/PIPELINE-TODO.md` — `## Inbox` is the channel back to the build-pipeline owner; signed-off `bin/migrate-cites` per §C1.4; preamble cleveref additions per §C5.
5. `~/src/neurips/LOG.md` — migration-agent prep landed (cite-migration tool + bold-paragraph + heading-numbering + `\tag{N}` lint + anchored equations + segment anonymization scanner + `[!figure]` images). Convention-enforcement infrastructure is in place.
6. `~/src/neurips/01-tragedy-confident-agent/_archive/orientation-2026-05-05.md` — paper #1's first-agent orientation. I'm following its shape; calling out paper-#2-specific deltas where they matter.
7. `~/src/neurips/00-test-paper/` — pipeline regression harness; the segment exemplars are the canonical reference for theorem callouts + cross-references + equation anchors + figure callouts.
8. Source paper directory: `OUTLINE.md` (Pass-5 findings + page-budget + risk register), `paper-draft.md` (full body + appendices A–G + references), spot-checks in `LOG.md` for audit history, `prior-art/positioning.md` for the four-strand cite-and-distinguish.

## What I plan to do (rough order)

1. **Per-paper directory scaffolding** (MIGRATE-TODO §B1–B3). Create `audits/`, `out/`, `spikes/`, `src/`. **Skip** `simulations/` and `results/` (theory-only paper). `.gitignore` for `out/` (build artifacts). `meta.md` with title / anonymized author block / abstract (the body's §"Abstract" carries a 250-word version; OpenReview submission per OUTLINE notes is locked, but body abstract can be updated until May 6 AOE — Pass-5 (a) flags abstract still missing the $N_h$ horizon factor that landed in body §7.1(v); my pass should propagate that consistency). `TODO.md` initialized (live work; OUTLINE.md content distilled in). `LOG.md` initialized (migration milestone + per-segment authoring choices forward). Commit + push as scaffolding milestone.

2. **Source-substrate decision.** Following AUTHORING §8.0: start from `paper-draft.md` (most-correct, post-Pass-5) as the baseline; consult `long-form.md` only where it carries content not present in paper-draft and where that content is currently load-bearing (much of long-form is supplementary that was already trimmed *intentionally* during Pass-2/3/4/5 — restoring it would re-introduce the cuts). I'll surface specific cases to Joseph if I'm unsure. Default reading: paper-draft is the substrate; long-form serves as a content reservoir for `OUT.full-paper.md` augmentation if specific segments could naturally hold more material than the 9pp body permits.

3. **Segmentation.** Default boundary per AUTHORING §8 — one segment per top-level section: `## 1 Introduction` → `src/01-introduction.md`; `## 4 Component 2 — A Point-Mass Reverse-KL/TV Identity for Action-Distribution Regret` → `src/04-pointmass-identity.md`; etc. Sub-section breaks where natural: §4 has six sub-sections (4.1–4.6) — the largest carry their own segment if trim flexibility benefits (§4.5 perturbative extension is the most likely candidate). Appendix segmentation matters more than body segmentation here because of the 5pp gap: each of A.1–A.8 plus B–G is a candidate for its own segment, so the 9pp manifest can drop them individually. **Slugs are stable once chosen** — I'll think about naming before locking.

4. **Authoring-pattern conversion** as I write each segment (AUTHORING §1, §2, §8.3):
   - **Inline-bold theorems** `**Theorem 4.1 (Point-mass reverse-KL/TV identity).**` → `> [!theorem] Point-mass reverse-KL/TV identity ^thm-pointmass-identity`. Anchor names: short, kebab-case, content-derived (`thm-pointmass-identity`, `thm-twosided-regret`, `thm-perturbative-eps`, `thm-forgetting-prereq`, `thm-loop-level2`, `thm-composition`, `thm-bias-bound`, `thm-chain-rule-uniqueness`, `lem-prost-impulsive`, `cor-beta-bernoulli`, `cor-aggregate-necessary`).
   - **Manual heading numbering** `## 4  Component 2 — A Point-Mass Reverse-KL/TV Identity ...` → strip the `4  ` prefix; LaTeX numbers automatically. The lint hook now warns; I'll fix-as-I-go.
   - **Manual `\tag{N}` in display math** → `^eq-name` anchor; `[[#^eq-name]]` in prose. I haven't counted yet (paper #1's count was 21 in paper-draft / 29 in long-form; B-CS1 likely similar magnitude). The `(P)` tag at §5.2 line 197 is one explicit case; equation displays in §4 / §6 / §7 / §A.6 are the bulk.
   - **`[Author Year]` citations** → `\cite{key}` / `\citet{key}` per AUTHORING §2.3. `bin/migrate-cites` is signed off (PIPELINE-TODO §C1.4). I'll dry-run it on a pilot segment first to verify behavior on B-CS1's hyphenated-multi-author patterns (`[Cheung-Simchi-Levi-Zhu 2020]`, `[Long-Fei Li-Zhao-Zhou 2024]`, `[Bareinboim-Correa-Ibeling-Icard 2022]`, `[Anderson-Moore 1979]`, etc.) before bulk apply. Ambiguous matches surface to Joseph; missing entries get added via `bin/refs add`.
   - **Cross-references** `Theorem 4.1`, `Lemma 5.2`, `Corollary 5.1a`, `(P)`, `§4.6`, `Appendix A.6`, `(C1)–(C3)`, `(A1)–(A5)`, `(i)–(v)` → `[[#^thm-pointmass-identity]]`, etc. The (i)–(v) numbered conclusions in Theorem 7.1 will need careful anchor naming — they're sub-anchors within one theorem, e.g., `^thm-composition-i` ... `^thm-composition-v` referenced separately in body and in §F.1 lift discussion.
   - **`$..$` inline math** — paper-draft already uses single-`$` form; keep as-is. No conversion needed.

5. **Manifests.** `OUT.full-paper.md` (everything; full appendix complement; long-form augmentation if surfaced). `OUT.neurips-2026-paper.md` (9pp budget — drops A.4 verbatim duplicate per Pass-5 (b); drops A.8 self-flagged future-work per Pass-5 (b); compresses A.7 absorbing $\theta_1$ identity into §5.3 inline per Pass-5 (b); applies expository compression per Pass-5 (c) where natural). Manifest tables follow §-Type-Slug-Title-Stage column shape; Bibliography row, then Appendix rows, then Checklist last (AUTHORING §5.2). Manifest narrative blocks between tables capture "this segment bridges to §X via Y" commentary.

6. **Anonymization sweep** (AUTHORING §3.5, §5.3). Build-side scanner via `refs/deny-list.yml` runs as part of `bin/build`; I'll also do a manual scan against the four categories before commit. **Known hit to fix:** `paper-draft.md` line 371 (§9.1 limitations) — `directed-separation property between $M_t$ and goal state` → `architectural-separation property` or `conditional-independence property`. OUTLINE.md flags this as the same risk B-N8 already swept (`0aa533f`). I'll pick `architectural-separation` (matches B-N8's resolution; preserves Pearl-blanket framing without priming) — surfacing to Joseph if he wants `conditional-independence` instead. **ASF self-citation prohibition** — Zenodo DOI `10.5281/zenodo.19986312` must not appear; spot-check during integration showed clean per OUTLINE.md, formal verification at `bin/refs lint` time.

7. **Auxiliary-content port** (MIGRATE-TODO §C1–C3). `prior-art/` (`query.md`, `report.md`, `positioning.md`) → fresh `prior-art/` subdir at submodule root. The four-strand positioning synthesis is content I'll lean on for §8 Related Work segment authoring. `_archive/` audit relics: **do not port** (MIGRATE-TODO §C3 — fresh `_archive/`; old workspace stays at `~/src/neurips2026/02-convergence/_archive/` as historical record). My migration `LOG.md` carries SHA pointers if any audit-finding context is needed downstream.

8. **Build as I go.** `bin/build 02-unified-convergence-rl <manifest-stem>` after every meaningful chunk — not just at the end. Open `out/<manifest>.pdf` visually; confirm rendering, citation form, anonymization. Boundary between "I fix" and "flag pipeline-owner inbox" per AUTHORING §8.10:
   - **Content-side (fix yourself):** missing bib key → `bin/refs add`; broken `[[#^anchor]]` → add anchor or fix link; wrong slug path in `OUT.*.md`; `[Author Year]` left in a sentence → run migrate-cites or hand-convert; rubocop offense in any Ruby I touch.
   - **Pipeline-side (atomic-append to PIPELINE-TODO ## Inbox):** kramdown breaks on AUTHORING-conformant syntax; missing preamble package; rendering wrong despite source being conformant; build pipeline crashes on input AUTHORING says should work. Likely candidates B-CS1 may surface: handling of complex multi-line `\begin{align*}` blocks in §A.6 (the perturbative-identity derivation has a 6-line aligned proof step); handling of the boxed-equation pattern in §4.1 / §4.2 / §4.3 / §4.5 (paper-draft uses `\boxed{...}` liberally); the multi-conclusion theorem statement in Theorem 7.1 with five sub-conclusions (i)–(v) — the cleanest authoring shape may need experimentation.

9. **Per-paper trackers.** Initialize `TODO.md` at submodule root capturing remaining work distilled from OUTLINE.md (Pass-5 (a)/(b)/(c)/(d) live findings; preflight checklist; cross-paper hygiene; trim path). Initialize `LOG.md` capturing the migration milestone forward; old workspace's full Pass-1 → Pass-5 audit history stays at `~/src/neurips2026/02-convergence/LOG.md` as the historical record.

10. **Commit per milestone, push to submodule remote.** Don't lump scaffolding + segmentation + manifests + cite migration + heading sweep into one commit; bounded `git commit -- <pathspec>` for reviewable diffs. Push to `origin/main` (`v2-io/paper-unified-convergence-rl`) regularly. Umbrella owner advances the submodule pointer when ready.

## Constraints I'll hold to

- **Strengthen before softening** (AGENTS §3.1). If I encounter audit-derived softens in the source — particularly the Pass-3 declined-Codex-softening list — I won't re-soften. I'll preserve the strengthened claims as the source has them.
- **Truth above all else** (AGENTS §6, §8). No "100% complete" / "comprehensive" claims in my LOG / TODO / commits. Mark uncertainty explicitly. "I hadn't thought of that — let me check" beats false confidence.
- **Durability claims must be actions** (AGENTS §3.3). When I say "logged" / "committed" / "pushed", the corresponding tool action must have happened in this session.
- **Verify integration before archiving** (AGENTS §3.4). Before any `git mv` to `_archive/`, the LOG entry must confirm findings have been integrated.
- **Primary-source verification** (AGENTS §3.5). I won't synthesize claims from `OUTLINE.md` summaries or audit reports without spot-checking against the actual `paper-draft.md` / `long-form.md` text.
- **No chronicle voice in formal text** (AUTHORING §3.2). Theorem and proof bodies don't reference change history. The Pass-history nuances ("renamed from Bretagnolle-Huber identity per Pass-1 audit fix") I see in OUTLINE.md belong in *my* `LOG.md` or `> [!todo]` working callouts, never in formal expression. Formal text speaks as the current theory.
- **Anonymization is non-negotiable** (AUTHORING §3.5, §5.3). Every segment scanned against the four categories before commit; the `directed-separation` hit at §9.1 fixed in segmentation.
- **Peer-to-peer voice** (AGENTS §5.3) when instructing any sub-agent I spawn. Share context, not prescriptions; trust their deliberation.
- **No drift onto trim or strengthening** (AUTHORING §8.0 — *"Trim is not a migration concern"*). Migration is parity with the paper outline. The 14→9pp gap is real but it's the per-paper agent's territory after migration lands; I'll preserve segmentation flexibility for downstream trim and stop.

## Things I'd surface to Joseph

In rough priority order:

1. **Substrate-strategy confirmation.** Per AUTHORING §8.0 the answer is settled (paper-draft baseline; long-form as content reservoir). For B-CS1 specifically: long-form's content beyond paper-draft is mostly material that was *intentionally* trimmed during Pass-2/3/4/5. I'm reading "long-form augmentation" narrowly — only restore content where it serves a clear segment-level purpose unattainable from paper-draft alone. Check before locking in.

2. **Segmentation granularity in the appendix** — A.1–A.8 plus B–G means up to 14 appendix segments if I split aggressively. Trim flexibility argues for the splits; Obsidian-readability and slug-stability argue for moderation. My instinct: split at the standalone-appendix level (A, B, C, D, E, F, G as separate segments) and keep A.1–A.8 as a single `src/A-supporting-material.md` segment with internal sub-headings. If trim needs finer granularity later, A can be split then. Want a quick check before locking slugs.

3. **`bin/migrate-cites` dry-run pilot.** PIPELINE-TODO §C1.4 says signed off. I'd like to dry-run on one pilot segment (probably §1 Introduction — has a high density of bracketed-author-year citations including the trickiest `[Cheung-Simchi-Levi-Zhu 2020]` and `[Long-Fei Li-Zhao-Zhou 2024]` shapes) before bulk apply, and surface any tool gaps to PIPELINE-TODO ## Inbox. If the dry-run is clean, bulk-apply per segment as I author it.

4. **Pass-5 (a) coherence drifts as drive-by during migration.** OUTLINE.md flags seven coherence bugs the per-paper agent hasn't yet integrated:
   - (i) abstract / §1.1 (iv) bullet / §9.3 conclusion still carrying pre-strengthen rate $O(V_{\max}\sqrt{(B_T+1)T})$ without the $N_h$ horizon factor that landed in §7.1(v);
   - (ii) §8 row 3 cross-reference bug to "§5.5" (now §5.4 after structural-move-B renumbering);
   - (iii) `$K_t$` vs `$K_t(s)$` notational inconsistency between Theorem 7.1 line 303 and (v) line 330;
   - (iv) `$p_{\rm id}$` scalar-vs-per-state scope inconsistency between body and Appendix F line 671;
   - (v) the `directed-separation` anonymization hit at §9.1 line 371 (already covered above);
   - (vi) (A5) base-learner regime ambiguity ("either restarted at block boundaries, or local guarantee under within-block carryover");
   - (vii) `$q_0$` scope heterogeneity between Theorem 4.3 (full-support) and Theorem F.1 (two-point).
   These are small clean diffs I *could* fix as drive-by during segment authoring (with a LOG entry per AUTHORING §8.0). Fixes (i), (ii), (v) feel safe (mechanical propagations / one-character / known reframe). Fixes (iii), (iv), (vi), (vii) are mathematical-disambiguation choices that the per-paper agent should make. **My default**: fix (i), (ii), (v) as drive-by; defer (iii), (iv), (vi), (vii) to per-paper agent with `> [!todo]` callouts in the relevant segments. Confirm before I commit.

5. **Pass-5 (b) free-trim candidates as drive-by — or hold for trim agent?** OUTLINE.md flags three near-free trims (cut §A.4 verbatim duplicate; cut §A.8 sketched-future-work; compress §A.7 absorbing $\theta_1$ identity into §5.3). My default: don't apply these in segment authoring; instead, *preserve* §A.4 / §A.7 / §A.8 as separate segments and *omit* them from `OUT.neurips-2026-paper.md` while keeping them in `OUT.full-paper.md`. That's the manifest-trim path AUTHORING §7.2 prefers (reuse-over-re-edit; less drift between versions). Confirm.

6. **Per-paper `refs.bib` resolution.** PIPELINE-TODO §F1 confirms: `bin/refs emit <paper-dir>` writes the per-paper `refs.bib` as a generated artifact. Today the build reads a hand-maintained `<paper-dir>/refs.bib`; switching to emit-on-build is a config one-liner. Once C1.4 (cite migration) lands per paper, the switch is natural. I'll proceed with hand-maintained `refs.bib` until then. Flag if I'm misreading.

7. **Pipeline-side things I'm pre-committed to flagging** (per AUTHORING §8.10). Likely candidates B-CS1 may surface: complex multi-line `\begin{align*}` blocks in §A.6 perturbative-identity derivation; `\boxed{...}` rendering in §4.1–§4.5 main-result equations; multi-conclusion theorem statement in Theorem 7.1 (cleanest authoring of (i)–(v) sub-conclusions); the `(P)` tag in §5.2 line 197 (manual `\tag{P}`-in-equation pattern — anchored-equation conversion needs to handle named tags, not just numeric); table rendering in §A.2 admissible-divergence family / §B Pinsker-numerics / §8 Related Work. Each surfaces on first encounter; flag-as-encountered.

## What I'll log in this submodule's LOG.md

The migration milestone forward. Per-segment authoring choices that future agents would benefit from knowing — e.g., why `§4` got split at §4.5 rather than kept whole; what `[Hintikka 1991]`-shaped disambiguations resolved to; which Pass-5 (a)/(b) drive-bys I applied vs deferred. Source paper's Pass-1 → Pass-5 audit cycle is **not** re-logged here — it's the historical record at `~/src/neurips2026/02-convergence/LOG.md`. This LOG starts fresh from the migration milestone forward. Any spike directories that surface during migration (e.g., from the per-paper agent attempting a Pass-5 (a) strengthening, or from the trim path) get archived under `_archive/` here once integrated.

---

*Snapshot only. The live plan is in `TODO.md` once initialized.*
