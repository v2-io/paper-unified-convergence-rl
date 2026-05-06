# VISION.md — what this paper is and how we author it

*Written 2026-05-06 by paper #2 agent. Lives at the submodule root because it's the orientation doc most worth re-reading before any substantive work session — both as self-check during a long task and as handoff when context fills up.*

---

## What this paper is

**B-CS1: *A Unified Convergence Theory for Non-Stationary Reinforcement Learning.*** Single-author theory paper, NeurIPS 2026 Main Track (track decision pending; Position-Papers Track is a live alternative). The contribution is "a novel combination of existing techniques" composing four structural strands that no published RL framework has unified: (1) two-gap diagnostic separating goal-feasibility from policy-quality; (2) point-mass reverse-KL/TV identity at deterministic-π* corner — *exact* algebraic identity strictly improving Pinsker and BH at this corner; (3) multi-factor strategic tempo with structural forgetting prerequisite as a survival inequality; (4) closed-loop interventional access making regret bounds learnable on-policy. Composing the four yields cumulative dynamic regret `Õ(N_h √((B_T+1)T))` in piecewise-stationary regime; the rate is the "narrative anchor" but the *technical anchor* (the part a reviewer can independently verify) is the point-mass identity and its two-sided regret bound under deterministic π*.

Origin: the substance was developed by Joseph as a single ~14pp manuscript at `~/src/neurips2026/02-convergence/paper-draft.md` through Pass-1 → Pass-5 audit cycles, Codex/Gemini reviews, and several spike-style strengthen-sweeps. That manuscript was then migrated faithfully into segments here (parity port). The math is solid: Pass-2 cracked C2/C4/C5/C6/C7/C8 + Gemini #1; spike-N1/N2 cracked the trajectory-level lift and the impulsive-system Lemma 5.2; the §A.6 perturbative algebra has been verified by hand (one small intermediate-line slip noticed and fixed); §B numerics and §C χ² counterexample verified numerically.

## The three pillars and their order

The frame Joseph put in place, which this paper now subordinates itself to:

1. **Truth / Strength** — strong math, defensible claims, complete proofs, verified citations, scope honesty. This is non-negotiable. The audit cycle does this well; keep doing it. Strengthen-before-soften (`AGENTS.md` §3.1) governs both formal claims and prose; when an audit recommends softening, attempt the strengthen first.

2. **Wisdom** — coherence, flow, pedagogy. The reader builds a mental model in stages (setup → motivation → formal statement → unpacking → forward-reference); the prose has to support that arc. Wisdom comes from giving the mathematics its proper home in the appendix and letting the body breathe and tell the story. *The body breathes when each thing lives where it belongs* — not when expository tissue gets cut to fit a budget.

3. **Beauty** — organization. Size is *emergent* from where things naturally belong, not enforced by compression. Jin 2020 (arXiv:1907.05388) has 33 pages of math in the appendix and 8 pages of body that *feel complete* because the body is written for narrative and the math has its own place. That's the calibration. The body is a paper; the appendix is the math; both are first-class.

The order matters: Strength must be in place before Wisdom can rest on it; Wisdom must be in place before Beauty can emerge. We can review by walking the ladder back the other way (Wisdom · Strength · Beauty per AGENTS.md), but the *writing* needs all three from the start, with Strength as the foundation.

## The failure mode we're correcting for

The old workspace's audit cycle was excellent at *Strength* — the Pass-1 → Pass-5 rounds plus Codex/Gemini verifications produced a manuscript with very few defensibility holes. But the audits were **always framed in compression terms**: "Pass-5 (b) free-trim candidates," "Pass-5 (c) compression candidates," "13 mechanical edits saving ~9 pages." Each individual edit was reasonable; the cumulative effect was a body that the audit *itself* characterized as "dense to the point of being borderline-unfriendly." The wisdom-layer prose — motivation paragraphs, "what just happened" reflections, "what's about to be earned" previews — got classified as expository fat and cut. What survived was the formal scaffolding without the connective tissue that makes the scaffolding legible.

The strengthen-before-soften principle was applied beautifully to formal claims; it was never applied to prose-level wisdom. When Pass-5 said "compress §3.3 to a footnote on §3.1," the dual question was never asked: *could this be expanded to give the reader the motivation that's currently compressed to a single dense paragraph?* That's the move that recovers wisdom.

The trap is to keep thinking *compression*. The reframe: the body needs *organization*, not *subtraction*. The math needs *its own home*, not less of itself. Size is a function of organization, not the constraint that organization serves.

If a future agent (or future-me) finds themselves reaching for "compression" / "trim" / "cut to fit" language: stop. That's the failure-mode signal. The right question is "where does this *want* to live?" not "what can be cut?"

## How we author this paper

The work is organized as a multi-step refactor. We're not editing the current `src/` in place — we're authoring a parallel restructured version in `src/re/`, swapping manifests when ready.

1. **Recover prose.** `src/_recovery/` (orphan segments, not in any manifest). Wholesale port of `~/src/neurips2026/02-convergence/long-form.md` (890 lines vs paper-draft's 696 — the delta is mostly living prose that didn't survive compression) plus surgical pulls from earlier `paper-draft.md` git revisions (pre Pass-2, pre Pass-3, etc.) where motivation paragraphs got compressed away. Filenames preserve provenance.

2. **Strengthens cheatsheet.** A small markdown doc listing the post-Pass-N strengthens that must be preserved when integrating recovered prose: BH-renaming → point-mass-identity, single-factor → multi-factor strategic tempo, K/T → impulsive Δ_max-condition (Lemma 5.2), A_accum → A_decay class theorem, perturbative Theorem 4.3, the spike-N1 N_h horizon factor, the spike-N2 reverse-ADT framing. When recovered prose carries an older / weaker framing, the strengthen wins; recovery is *prose recovery*, not claim recovery.

3. **Read Jin 2020 first-hand.** `spikes/paper_structure/1907.05388/mainQlin.tex` and the supporting files. Internalize the *feel* — what the body does, what Remarks-after-theorems actually look like, where proof sketches sit, what gets formal-restated in appendix vs. main. The strategy doc summarizes this; the actual paper is the calibration tool.

4. **New outline.** `OUTLINE-RESTRUCTURED.md` (or similar). Drafted from steeping in the theory + the Jin-2020 calibration + the recovered material. Body shape, appendix shape, key-lemma list, where Remarks land, what the headline informal theorem is. Surface to Joseph for review before authoring against it.

5. **`src/re/` authoring.** New segments against the outline. The body tells the four-component story narratively; mathematics lives in appendices where it belongs; mechanism / proof-sketch section ties the key lemmas. No page budget. Truth + Wisdom in dialogue from the start.

6. **Manifest swap.** New `OUT.full-paper.md` pointing at `src/re/` segments. The current `src/` stays as historical reference for the migration milestone (and as the structurally-correct trim-survivor).

7. **NeurIPS curation.** *After* the full version exists naturally. Different question entirely: which segments, which subsections, which ordering, projected onto 9pp from the natural-length theory.

## What's load-bearing — never regress

These are the spike / audit results that the current strong claims rest on. Pulling in older prose that uses earlier weaker framings would silently regress the paper. When integrating recovered material:

- **Theorem 4.1 is the *point-mass reverse-KL/TV identity*** at deterministic-π* corner. Not "BH-identity at equality." (Pass-1 audit-fix; the strengthen is that the identity sits *strictly below* BH at this corner.)
- **The two-sided regret bound (Theorem 4.2)** has $\Delta_{\min}(1-e^{-D_\mathrm{KL}}) \le R \le V_{\max}(1-e^{-D_\mathrm{KL}})$ — Lipschitz-equivalent and coordinate-optimal among TV-only bounds.
- **Theorem 4.3** is the perturbative extension to ε-stochastic and softmax-regularized optima with $O(\epsilon\log(1/\epsilon))$ and $O(\tau^{-1}\exp(-\Delta_\mathrm{min}/\tau))$ corrections (Pass-2 Gemini-#1 strengthen; deterministic-π* is no longer a hard scope wall).
- **Strategic tempo is multi-factor**: $\mathcal T_\Sigma^\mathrm{bn,ss} = \min_{(i,j)} \nu_{ij}\iota_{ij}(1-\lambda_{ij})$. Not single-factor $(1-\lambda)$ alone. (Pass-2 C3 strengthen; the bottleneck-vs-sum argument matters.)
- **A_decay structural-class theorem**: every gain-decay update eventually violates the persistence threshold (Pass-2 C4 strengthen). The class is named A_decay, not A_accum.
- **Lemma 5.2 ProST reduction is *impulsive-system reverse-ADT*** (HLT 2008 framework): $\Delta_\mathrm{max} \cdot \rho_\Sigma/R_\Sigma < -\ln(1-\gamma)^2$. Recovers $K/T$ form under uniform blocks; strictly stronger on nonuniform schedules. (Spike-N2 strengthen; never regress to the K/T-only form.)
- **Theorem 7.1(v) cumulative regret has the $N_h$ horizon factor**: $2cV_\mathrm{max} N_h \sqrt{(B_T+1)T} + N_h(1-p_\mathrm{id})\log(1/q_0)\cdot T$. (Spike-N1 strengthen; N_h is the simulation-lemma penalty, linear not quadratic.)
- **Theorem 7.1 is conditional on (A5) restart-on-change**; carryover variant noted but deferred (per the post-parity refinement).
- **The composition is "bundle of guarantees + each component load-bearing for a distinct epistemic property"** — not "every component necessary for the rate." The §7 Necessity subsection (added in commit `8d82b13`) is the defense against "(A5) is doing all the work" and "why four, not three?" objections.

## What to keep in mind for future-me / future-agent

- `AGENTS.md` and `AUTHORING.md` at umbrella root are the meta-orientation. `AGENTS.md` §3.1 (strengthen-before-soften), §3.5 (primary-source verification), §5.3 (peer voice for sub-agents), §6 (truth above all) are the load-bearing principles.
- `~/src/neurips2026/02-convergence/_archive/audit-fix-log.md` and `spike-N1-N2-strengthen-2026-05-05.md` and `spike-page-budget-compression-2026-05-05.md` are the substantive history of how the current strong claims came to be. Read first if you're integrating older prose or making a structural call.
- `_archive/orientation-2026-05-05.md` is the migration agent's pre-work snapshot. Useful for context but somewhat dated; this VISION.md supersedes it as the live orientation.
- `LOG.md` carries chronological history; `TODO.md` carries live work + Pass-5 carry-over for per-paper agent.
- The current `src/` segments are the trim-survivor (commit `8d82b13`). Don't edit in place during the refactor; author `src/re/` parallel.
- The build pipeline (`bin/build`) is being actively improved at the umbrella; pipeline-side issues go to `PIPELINE-TODO.md ## Inbox` via atomic append (`AGENTS.md` §5.1).
- **The temptation when stuck or under perceived time pressure is to fall back into compression-thinking.** That's the failure-mode signal. When you notice it, return to the reframe: *where does this want to live?*

---

*This document is meant to evolve. Update it when the work's frame shifts, when major decisions land, or when something here turns out to be wrong. It's a vision, not a contract — the project's relationship to truth supersedes any specific framing here.*
