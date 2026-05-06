# LOG.md — 02-unified-convergence-rl history

*Append-only. Reverse-chronological (newest first). Never edit prior entries — LOG is the permanent record. Future agents reading this should be able to reconstruct what was tried, what worked, what failed, and why.*

For active backlog see `TODO.md`. For umbrella-level history see `~/src/neurips/LOG.md`. The source paper's full Pass-1 → Pass-5 audit cycle is the historical record at `~/src/neurips2026/02-convergence/LOG.md` and is intentionally **not** re-logged here — this LOG starts fresh from the migration milestone forward.

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
