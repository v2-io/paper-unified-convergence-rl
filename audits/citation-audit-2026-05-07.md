# Citation audit — B-CS1 (Unified RL Convergence) — 2026-05-07

## Summary

Audit covered the load-bearing claims tied to specific theorems / lemmas / rates in the unified-RL paper (B-CS1) at `02-unified-convergence-rl/src/re/`. Coverage: read all eight body / appendix segments first-hand (`01-introduction.md` through `F-prior-art.md`), enumerated the 54 distinct cite keys, verified the four PDF-confirmed primary sources at the cited-claim level (Lattimore–Szepesvári Theorem 7.1, Wei–Luo MASTER Assumption 1 + Theorem 2 at p=1/2 boundary, Mao 2021 RestartQ-UCB rate, Besbes–Gur–Zeevi 2014 lower bound), fetched + verified Hespanha–Liberzon–Teel 2008 Theorem 1 (reverse-ADT), spot-checked Pearl 2009 do-calculus Rule 2 against the cited form, and surface-checked recent / contemporaneous entries (Hajiabolhassan–Ortner 2025, Zhang 2026 Peril, Gerogiannis 2026 DARLING, Schulte–Poupart 2025).

**Top-line:** the load-bearing math citations (Lattimore Theorem 7.1, Wei–Luo Theorem 2, Besbes–Gur–Zeevi Theorem 1, Hespanha reverse-ADT) all check out at the *substantive* level — the paper's claims about what these sources prove are accurate. Two non-trivial findings, three trivial ones, one cosmetic. **Audit confidence: high** for the four PDF-anchored sources (full theorem-level verification done first-hand); **moderate** for the dozen-or-so additional cites verified via abstract / web search (claim-level plausibility checked, not theorem-text-level); **low for entries not checked** (the four "tracks" cluster cites in §1 / §2 / §F have not been theorem-checked one-by-one — see *Coverage gaps* below). No hallucinated citations found.

---

## Findings

### F1 — Broken bibkey: `auer-cesa-bianchi-fischer-2002-finitetime`  *(fixed mid-audit)*

**Where:** `src/re/A-proof-of-composition.md` line 59, in the *Bandit case sharpening* Remark.

**Symptom:** The cite key doesn't exist in `refs/entries/`; the PDF renders as `[? auer-cesa-bianchi-fischer-2002-finitetime]` (visible in the compiled PDF). The entry that *does* exist is `auer-cesa-bianchi-fischer-2002-ucb` (same paper, *Finite-time analysis of the multiarmed bandit problem*, Auer–Cesa-Bianchi–Fischer, Mach. Learn. 2002).

**Severity:** This is the only broken cite in the source — the only `[?` in the rendered PDF. Real defect for a submission.

**Action taken:** Edited `A-proof-of-composition.md` line 59 directly: `auer-cesa-bianchi-fischer-2002-finitetime` → `auer-cesa-bianchi-fischer-2002-ucb`. Will render correctly on next build.

### F2 — Mao 2021 rate misstated in Related Work

**Where:** `src/re/02-related-work.md` line 5, the *Variation-budget dynamic regret* paragraph:

> *"\cite{mao-2021-nearoptimal} achieves the near-optimal continuous-variation rate $\tilde O(SA \, V_T^{1/3} H \, T^{2/3})$ via RestartQ-UCB"*

**Source check:** The Mao et al. 2021 paper (verified PDF-first: `refs/pdfs/mao-2021-nearoptimal.pdf`) actually proves $\tilde O(S^{1/3} A^{1/3} \Delta^{1/3} H T^{2/3})$ where $\Delta$ is the variation budget. So the paper-under-audit's claim is **a coarser / incorrect S, A scaling** — it writes $SA$ when Mao proves $(SA)^{1/3}$.

**Severity:** Substantive — the cite mis-attributes a strictly weaker rate to Mao than Mao actually establishes. A reviewer who recognizes RestartQ-UCB will catch this.

**Note on the paper's other Mao references:** The body's *other* uses of `mao-2021-nearoptimal` (Section 4 *Comparator regime*, Appendix A *(A5')-BoBW Remark* line 65–67) are careful — they say "matching Mao's near-optimal $V_T$ exponent" with explicit qualifier "$S, A$-scaling differ from Mao's RestartQ-UCB," and don't repeat the incorrect $SA$ claim. So the issue is local to Related Work line 5; the rest of the paper's claims about Mao are accurate.

**Suggested fix:** Change `\tilde O(SA \, V_T^{1/3} H \, T^{2/3})` to `\tilde O((SA)^{1/3} V_T^{1/3} H \, T^{2/3})` in Related Work. Or more conservatively, drop the explicit rate and write "achieves the near-optimal continuous-variation $V_T^{1/3} T^{2/3}$ rate via RestartQ-UCB" since the precise constants aren't doing work in that sentence.

### F3 — Pearl 2009 Rule 2 cite-target imprecise (substantive identification correct)

**Where:** `src/re/B-key-lemma-proofs.md` lines 94–96, *Proof of Key Lemma 3*. The proof invokes:

> *"Pearl's do-calculus \cite{pearl-2009-causality} (Theorem 3.4.1, Rule 2 — action/observation exchange): $P(o_{t+1} \mid \mathrm{do}(a_t), H_t) = P(o_{t+1} \mid a_t, H_t)$ whenever $(o_{t+1} \perp a_t \mid H_t)_{G_{\overline{a_t}}}$"*

**Source check:** Pearl 2009 (2nd edition) Theorem 3.4.1 Rule 2 (p. 85–86) reads:

> *"P(y | x̂, ẑ, w) = P(y | x̂, z, w) if (Y ⫫ Z | X, W) in $G_{\overline{X}, \underline{Z}}$"*

— that is, Rule 2 mutilates $Z$ via *underline* (outgoing-arrows-removed), not *overline* (incoming-arrows-removed). The paper-under-audit uses `G_{\overline{a_t}}` (overline / incoming-arrows-removed of the do-variable), which is the **back-door criterion** (Definition 3.3.1, Theorem 3.3.2 in Pearl) rather than Rule 2.

**Severity:** Technical / cosmetic. The substantive identification still holds — in the singular-trajectory architecture only $H_t$ has arrows into $a_t$, and conditioning on $H_t$ blocks the only back-door path. So `G_{\overline{a_t}}` does the right d-separation here. But the cite-target ("Theorem 3.4.1, Rule 2") is the wrong theorem in Pearl; the matching theorem is **Theorem 3.3.2 (Back-door adjustment)** or alternatively the back-door form of Rule 2 (which would require renaming $a_t$ to fit Pearl's $X$ rather than $Z$ slot, plus letting $W = H_t$).

**Suggested fix (one of):**
- Cleanest: reframe as the back-door criterion. Drop "Theorem 3.4.1, Rule 2" pointer, use "Pearl's back-door criterion (Theorem 3.3.2)" instead. The notation $G_{\overline{a_t}}$ stays correct.
- Alternative: keep Rule 2 but write the d-separation in $G_{\underline{a_t}}$ (overall less natural in this setting since the singular-trajectory architecture is more obviously a back-door story).

### F4 — Mao / Cheung double-cite likely refers to the same paper twice

**Where:** `src/re/02-related-work.md` line 5, *Variation-budget dynamic regret* paragraph:

> *"\cite{cheung-2020-reinforcement, wei-luo-2021-blackbox, mao-2021-nearoptimal, gajane-2019-variational, cheung-simchi-levi-zhu-2022-blessing}"*

**Source check:** `cheung-2020-reinforcement` (entry: arXiv 2006.14389, *Reinforcement learning for non-stationary Markov decision processes: the blessing of (more) optimism*, Cheung–Simchi-Levi–Zhu) and `cheung-simchi-levi-zhu-2022-blessing` (entry: Management Science 68(3) 1697–1716, 2022, same title and authors, DOI 10.1287/mnsc.2021.4123) appear to be **the same work** — the 2020 arXiv preprint and its 2022 published-version. The MIT Open Access PDF (`cheung20a.pdf`) is the same paper; the Management Science publication supersedes the arXiv preprint.

**Severity:** Citation hygiene. Citing both as if they were distinct works is misleading — the 2022 published version is canonical and supersedes the 2020 preprint.

**Suggested fix:** In Related Work line 5, drop one of the two. Convention is to cite the published version (`cheung-simchi-levi-zhu-2022-blessing`). Since `cheung-2020-reinforcement` is also used 5 other places (`01-introduction.md`, `03-preliminaries.md`, `04-main-result.md`, `D-algorithm.md`, `F-prior-art.md`, `A-proof-of-composition.md`), a global rename to `cheung-simchi-levi-zhu-2022-blessing` everywhere would also work. Or the entries could be merged in `refs/entries/` (keeping `cheung-2020-reinforcement` and updating its YAML to point at the published version, then deleting `cheung-simchi-levi-zhu-2022-blessing`).

### F5 — Bareinboim et al. 2022 cite is foundational, not the source of A/B/C regimes  *(weak attribution, defensible)*

**Where:** `src/re/04-main-result.md` line 19, Component 4 paragraph; also `src/re/05-mechanism.md` line 34, Key Lemma 3 *Intuition*; also `src/re/01-introduction.md` line 29, Component 4 sub-bullet.

**Symptom:** The phrasing reads:

> *"Three regimes \cite{bareinboim-correa-ibeling-icard-2022-pearl-hierarchy} partition usable strength: A (intervention-rich, $\iota \approx 1$), B (partial), C (observation-only)."*

**Source check:** Bareinboim–Correa–Ibeling–Icard 2022 establishes Pearl's *Causal Hierarchy Theorem* (Layer 1 / 2 / 3 = associational / interventional / counterfactual queries are generally distinct in measure-theoretic sense). They do **not** define an A/B/C identifiability partition with the specific labels (intervention-rich, partial, observation-only) — that's the paper-under-audit's own taxonomy.

**Severity:** Weak attribution. The cite is *defensible* as a foundational anchor (the hierarchy is the structure on which the A/B/C split rests), but the phrasing reads as if the source establishes the partition.

**Suggested fix (optional, low priority):** Reword e.g. "Three regimes — A (intervention-rich, ι≈1), B (partial), C (observation-only) — partition usable strength along the identifiability axis (over the hierarchy of \cite{bareinboim-correa-ibeling-icard-2022-pearl-hierarchy})." Or simpler: keep as-is but add a comma — "Three regimes partition usable strength along the hierarchy of \cite{...}: A, B, C." The current form is in the slightly-misleading register but not factually wrong.

### F6 — `gerogiannis-2026-darling.yml` author list malformed  *(fixed mid-audit)*

**Where:** `refs/entries/gerogiannis-2026-darling.yml`, `authors:` field. The middle author "Yu-Han Huang" was YAML-split into two list entries: `Huang,` and `Y.-H,`. (Likely an earlier import bug — comma-splitting on the comma-in-name "Huang, Y.-H." that should have been kept as a single string entry.)

**Severity:** Cosmetic — would render bibliography as "Gerogiannis, A.; Huang,; Y.-H,; Veeravalli, V" with two malformed authors. arXiv 2604.16684 confirms three actual authors: Argyrios Gerogiannis, Yu-Han Huang, Venugopal V. Veeravalli.

**Action taken:** Edited `refs/entries/gerogiannis-2026-darling.yml` to combine the two split entries into `Huang, Y.-H.` (single author).

---

## Verified (no action needed)

These are the cites I confirmed at the substantive (claim-vs-source) level, with verification events appended to `refs/verifications/`:

| Bibkey | Claim verified | Verification event |
|---|---|---|
| `lattimore-2020-bandit` | Theorem 7.1: $E[T_i(n)] \le 3 + 16\log(n)/\Delta_i^2$ matches paper's $E[N_a(t)] = O(\log t/\Delta_a^2)$. | `refs/verifications/lattimore-2020-bandit/20260507T084638Z-...` |
| `wei-luo-2021-blackbox` | Assumption 1 with $C(t) = c_1 t^p + c_2$, $p \in [1/2, 1)$, plus auxiliary-quantity output condition; Theorem 2 BoBW rate at $p=1/2$ boundary. Matches `(A5')`. | `refs/verifications/wei-luo-2021-blackbox/20260507T084640Z-...` |
| `mao-2021-nearoptimal` | RestartQ-UCB rate $\tilde O(S^{1/3} A^{1/3} \Delta^{1/3} H T^{2/3})$ verified — but paper-under-audit Related Work line 5 misstates as $SA$ (see F2). | `refs/verifications/mao-2021-nearoptimal/...` (outcome: `uncertain` because of F2) |
| `besbes-gur-zeevi-2014-stochastic` | Theorem 1 lower bound $\Omega((KV_T)^{1/3} T^{2/3})$ on Bernoulli MAB with dynamic-oracle / per-round-argmax — supports paper's claim that the bound applies at the deterministic-π* corner. | `refs/verifications/besbes-gur-zeevi-2014-stochastic/...` |
| `hespanha-2008-lyapunov` | Theorem 1: ISS for impulsive systems with rate coefficients (c, d); reverse-ADT condition for d > 0, c < 0 (impulses stabilizing, continuous flows destabilizing) — matches the paper's invocation in Appendix B impulsive ProST sector reduction. PDF fetched + saved at `refs/pdfs/hespanha-2008-lyapunov.pdf`. | `refs/verifications/hespanha-2008-lyapunov/...` |
| `pearl-2009-causality` | Foundational reference exists at the right place (do-calculus, back-door); but cite-target imprecise — see F3. | `refs/verifications/pearl-2009-causality/...` (outcome: `uncertain` because of F3) |
| `bareinboim-correa-ibeling-icard-2022-pearl-hierarchy` | Causal hierarchy verified; A/B/C labels are paper's, not source's — see F5. | `refs/verifications/bareinboim-correa-ibeling-icard-2022-pearl-hierarchy/...` |

Additional surface-level confirmations (not formally appended; web-search-based):

- `lee-2023-prost-tempo` — ProST framework, schedule $\{t_1, \ldots, t_K\}$ minimizing dynamic regret upper bound. **Verified** matches paper-under-audit's characterization in §2 / §5 / Appendix B.
- `lee-2024-pausing` (Pausing Policy Learning) — companion paper to ProST. Existence verified via arXiv search; refines ProST with hold durations. Consistent with paper-under-audit's brief mention.
- `hajiabolhassan-2025-online` — *Online regret bounds for satisficing in MDPs*, Math. of OR 2025. **Verified** matches paper's "satisficing = any policy above acceptance level β" characterization.
- `zhang-2026-peril` — arXiv 2603.18514, *On the Peril of (Even a Little) Nonstationarity in Satisficing Regret Minimization*. **Verified** — submitted March 2026, correctly classified as contemporaneous.
- `gerogiannis-2026-darling` — arXiv 2604.16684, submitted April 2026, contemporaneous. **Verified.**
- `schulte-poupart-2025-causal-rl` — TMLR 2025-05-09 *When should reinforcement learning use causal reasoning?* Existence verified.
- `russo-vanroy-2014-ids` — IDS / information ratio; uses Cauchy–Schwarz + Pinsker as standard analysis. Consistent with paper's characterization.
- `bretagnolle-huber-1978-densities` — entry verified; the inequality $\operatorname{TV} \le \sqrt{1 - e^{-D_{\mathrm{KL}}}}$ is well-known and paper's claim "identity sits strictly below BH at the deterministic corner" reduces to the elementary $x < \sqrt{x}$ on (0,1) — correct.
- `aczel-1975-measures` — On Measures of Information and Their Characterizations textbook reference for the functional equation $f(rs) = f(r) + r \cdot f(s)$. Topic and provenance verified; specific §4 pointer not theorem-text-checked but consistent with the book's content.

---

## Coverage gaps

These cites I did *not* theorem-check — flagging so a follow-up audit (or Joseph) can decide whether the claims warrant deeper checking:

1. **Two-term decomposition cluster** (`li-zhao-zhou-2024-dynamic-regret`, `fei-2020-dynamic`, `stradi-2024-learning`). Cited in §1, §2 as "decompose the dynamic regret into a *confidence-set construction* term and an *adaptation-under-non-stationarity* term." If any of these papers actually decomposes along a *different* axis, the §2 paragraph's "exploration-vs-adaptation axis" attribution would be off. Surface-plausible but not source-verified.
2. **Tempo / forgetting cluster other than ProST** (`touati-2020-efficient`, `russac-2019-weighted`, `garivier-2008-upperconfidence`, `lee-2024-pausing`). Cited as establishing forgetting / sliding-window analyses. Standard line of work — not theorem-checked.
3. **Causal-RL cluster** (`zhang-2016-mdps`, `zhang-2022-online-rl`, `lu-2021-causal`, `lu-2022-efficient`, `wang-2021-provably`, `zhang-2020-designing`). The §2 / §F paragraphs claim these "use causal-graph structure to sharpen sample complexity in RL" and "operate in stationary settings only." The stationarity claim is load-bearing for the paper's compose-with-non-stationarity novelty argument; if any of these *did* address non-stationarity, the novelty framing would be partially overstated. Worth a follow-up spike.
4. **Variation-budget cluster** (`gajane-2019-variational`). Cited as establishing variation-budget dynamic regret bounds. Existence and topic plausible; not theorem-checked.
5. **Active-inference / cybernetics cluster** (`friston-2017-active-process`, `parr-2022-active`, `levine-2018-reinforcement`, `wiener-1948-cybernetics`, `conant-1970-every`, `bruineberg-dolega-dewhurst-baltieri-2022-bbs`). Cited in `C-aux-material.md` *Distinction from active inference and causal-RL precursors* with characterizations like "implicitly use the action-causes-observation observation" and (for `bruineberg-...-2022-bbs`) "documents that the active-inference literature sometimes elides this." These are framing claims; if any are misattributed it's more of an over-reading than a theorem-text mismatch.
6. **Information-theoretic regret cluster** (`russo-2014-informationtheoretic`, `lu-2019-informationtheoretic`, `min-russo-2023-nonstat-bandit`, `lattimore-2021-mirror`, `canonne-2022-short`, `kakade-2020-information-online`). Cited as the line where "BH does not appear as an upper-bound regret coordinate." This is a *negative* claim about the corpus — hard to falsify but presented as a literature-search observation against an explicitly bounded retrieval (per §F-prior-art). Acceptable framing.
7. **Simulation lemma cluster** (`kakade-2002-approximately`, `munos-2003-error`, `ross-2010-efficient`, `azar-2017-minimax`). Cited as "the simulation / performance-difference lemma" — this is a standard collective citation. Not theorem-checked but the line of work is well-known.

If a follow-up audit pass has time, the highest-value targets are **#3 (causal-RL cluster's stationarity claim)** and **#1 (two-term decomposition axis)** — both feed directly into the paper's novelty argument.

---

## Notes for the integration step

- The mid-audit fixes (F1 cite-key rename, F6 YAML repair) are already on disk; the next build will pick them up. No `bin/refs emit` or rebuild done from this audit — the integration step can decide whether to rebuild now or batch with other fixes.
- The verification events appended to `refs/verifications/<key>/*.md` are standalone artifacts; they survive the audit context and feed the `bin/refs show` overall status.
- F4 (`cheung-2020-reinforcement` vs `cheung-simchi-levi-zhu-2022-blessing` duplication) is the most consequential remaining content fix. F2 (Mao SA-vs-(SA)^{1/3}) is the most consequential math fix — a reviewer who recognizes RestartQ-UCB's rate will catch it. F3 (Pearl Rule 2 vs back-door) is a precision fix; the math itself is right.
- No hallucinated citations and no citations to nonexistent papers were found.
