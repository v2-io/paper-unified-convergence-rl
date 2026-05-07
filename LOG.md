# LOG.md — 02-unified-convergence-rl history

*Append-only. Reverse-chronological (newest first). Never edit prior entries — LOG is the permanent record. Future agents reading this should be able to reconstruct what was tried, what worked, what failed, and why.*

For active backlog see `TODO.md`. For umbrella-level history see `~/src/neurips/LOG.md`. The source paper's full Pass-1 → Pass-5 audit cycle is the historical record at `~/src/neurips2026/02-convergence/LOG.md` and is intentionally **not** re-logged here — this LOG starts fresh from the migration milestone forward.

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
