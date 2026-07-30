# ISSUES — submission 33915

Ratings 3 / 3 / 2 (confidences 2 / 3 / **5**). **The AC has effectively announced a reject**: the meta-review judges the clarity and precision problems to require rewriting beyond what the response period allows. Reviewers are R-A (3, conf 2), R-B (3, conf 3), R-C (2, conf 5) — our labels, not venue identifiers.

> **Tracked in a public repo.** Paraphrase only; no reviewer pseudonyms or verbatim text. Reviews at `~/src/neurips/reviews/neurips-2026/02-unified-convergence-rl/` (gitignored).

## Read this before spending effort here

Being honest about the situation, because it determines how to allocate a scarce five days across three papers: **this paper is very unlikely to be recovered in this cycle.** The AC has pre-committed in writing, and the confidence-5 reviewer scored quality, clarity, and significance all at 1. Response effort here is not competing for an accept; it is competing for two other things that are still worth having:

1. **Correcting the factual record**, so the reject rests on true grounds. Some specific claims against the paper are wrong and checkably so.
2. **Extracting the rewrite brief.** This paper gets rewritten and resubmitted, and the reviews are the best specification we will ever get for how. That work is not deadline-bound.

Recommended allocation: a short, factual, non-defensive response here; the bulk of the five days on paper 01, which is genuinely in play.

Status codes: `CORRECT` = reviewer claim is factually wrong, correct it neutrally · `MATH` = needs checking before we can answer honestly · `CONCEDE` · `REWRITE` = for the resubmission, not this response.

| ID | Issue | Raised by | Disposition | Status |
|---|---|---|---|---|
| K-01 | "No algorithms are defined"; base learner's construction and how it follows from existing RL approaches unspecified | R-C | `CORRECT` | open |
| K-02 | Mathematical terms used without full specification (`V_T` named as an example) | R-C | `CORRECT` (verify first) | open |
| **K-03** | **Unclear how UCRL2/UCBVI satisfy (A5)'s occupancy-weighted TV condition** | R-A | **`MATH`** | **open — the load-bearing one** |
| K-04 | Theorem 4.1 holds only for the piecewise case; pushing through to `V_T` via MASTER is a significant step, not a citation | R-A | `MATH` + `CONCEDE` | open |
| K-05 | Writing reads as machine-generated: technical-sounding phrasing without precision | R-C | `CONCEDE` in substance | open |
| K-06 | Abstract introduces `T_Σ`, `ρ_Σ`, `R_Σ` without saying what they mean; abstract/intro dense with jargon and prior-method names | R-A, R-B, meta | `REWRITE` | open |
| K-07 | Main theorem is over half a page: five assumptions, four conclusions, internal forward-references to lemmas and later sections — unreadable as a main result | R-B, meta | `REWRITE` | open |
| K-08 | Novelty over the four existing directions not clear to a non-specialist | R-B, meta | `REWRITE` | open |
| K-09 | Abstract claims three joint properties, then immediately four components — reads as inconsistent | R-B | `REWRITE` (easy fix) | open |
| K-10 | Notation inconsistent with community convention | R-C | `REWRITE` | open |
| K-11 | Theory-only; practical relevance unjudgeable | R-B | `CONCEDE` | open |

---

## K-03 — does (A5) actually hold for UCRL2/UCBVI?  ← check this honestly first

**Why this one matters more than the rest.** Everything else here is presentation or record-correction. This is the only item that could mean conclusion (v) — the cumulative dynamic-regret rate, the paper's headline — has a hole.

(A5) requires a restart-on-change base learner satisfying `Σ_{t=1}^L E[TV̄_t] ≤ 2c√L` within each restarted interval, where `TV̄_t` is the **occupancy-weighted** per-round coordinate `(1/N_h) Σ_h E_{s_h ~ d_h^{Q_t}}[TV(π*_t(·|s_h), Q_t(·|s_h))]`. The paper's App D claims the standard optimistic learners lift to this. The reviewer says they don't see how.

**Do not answer this from the shape of the argument.** The honest sequence:

1. Read App D as written, first-hand, not from the LOG summary.
2. Establish whether UCRL2/UCBVI regret bounds are stated in a form that converts to a *TV-in-occupancy-measure* bound, or only to a value-gap bound. Standard analyses bound value suboptimality via optimism and confidence-set width, **not** the TV distance between the learner's policy and the optimum at visited states — those are different objects, and the conversion is not obviously free in either direction. A deterministic-argmax learner has `TV ∈ {0, 1}` per state, so `Σ E[TV̄_t]` counts *mis-ranking events weighted by occupancy*, which is closer to a `√(SAL)`-type counting bound than to a value bound. Plausible that it works, and plausible that it needs an extra step the paper skipped.
3. If a step is missing, **say so.** Per AGENTS §3.1 attempt the strengthening first; but if (A5) genuinely doesn't follow for the named learners, the correct move is to state the gap plainly in the response — a paper already being rejected loses nothing by being accurate, and the record then reflects a real limitation rather than an unexamined objection. Finding this ourselves is strictly better than having it found at the next venue.

Working notes → `math/K-03-A5-base-learner.md`.

---

## K-01 / K-02 — factually checkable claims

**K-01 ("no algorithms are defined").** App D is *Algorithm Sketch and Base-Learner Instantiation*; the base learner is named as restart-on-change UCRL2/UCBVI and a lifted rate is stated. So the claim as worded is incorrect. **But be careful how this is said** — the reviewer's underlying point may be that a *sketch* is not a specification, which is fair, and pairs with K-03. Correct the factual claim in one sentence, neutrally, then engage the substantive version rather than scoring the point. This reviewer is at confidence 5; a defensive correction reads badly and changes nothing.

**K-02 (`V_T` unspecified).** Verify before responding — grep the submitted PDF for where `V_T` is defined and confirm it is a complete definition, not a name introduced by context. If it *is* properly defined, correct it with a section pointer. If it is only defined implicitly (e.g. named in the abstract and used before §3 defines it), the reviewer is right and we concede. **Do this check before drafting; do not assume the reviewer is wrong.**

---

## K-04 — `B_T` → `V_T` via MASTER

The reviewer is right that this is a real step rather than a citation, and the paper's own history agrees: the VT-unification spike (LOG 2026-05-07) found the regimes are not structurally distinct and that the MASTER wrapping is mechanical *given* the base learner satisfies Wei–Luo's Assumption 1 at `p = 1/2` — which the paper admits explicitly. So the honest answer is: the step is real, we did the work, the wrapping requires a boundary-case admission, and the paper states it. Point at where, and concede the main text under-develops it relative to its prominence in the abstract.

Related, already known from that same spike and worth restating accurately: five angles to beat Mao's `V_T` exponent all failed at the Besbes–Gur–Zeevi lower bound, which applies at the deterministic-`π*` corner. Our contribution sharpens the per-round coordinate and constants, **not** the `V_T` exponent. If the abstract implies otherwise, that is on us.

---

## K-05 — "reads as machine-generated"

Worth sitting with rather than rebutting. Nothing in the response should address this directly — there is no reply that improves the situation, and arguing about it spends characters on the one topic where we cannot win.

What it is evidence *of*: the register is dense, heavily nominalized, and every claim carries its scope conditions inline, which reads to an outside reviewer as fluent-but-imprecise rather than careful. The paper also declares LLM assistance including theorem-proving, which is visible to reviewers and plausibly primed the reading. Two of the three papers in this batch drew a version of this. For the rewrite: shorter sentences, fewer named hypotheses in the main text, notation and definitions before use, and at least one worked concrete instance early. See K-06/K-07.

---

## K-06 – K-10 — the rewrite brief

Not response material. Capture now, work later, and treat the reviews as the specification:

- Split the main theorem. Five assumptions and four conclusions in one environment is unreadable; state the per-round identity and the cumulative rate as separate results, and move (A1)–(A5) to a standing-assumptions subsection so the theorem statement is a few lines.
- Remove all forward-references from inside theorem statements.
- Fix the three-properties/four-components collision in the abstract's first lines (K-09) — genuinely a two-minute fix that cost real credibility.
- Define `T_Σ`, `ρ_Σ`, `R_Σ` in the abstract or don't use them there.
- One paragraph, early, stating the delta over each of the four existing directions in plain language, for a reader who knows none of them.
- Align notation with the non-stationary-RL literature's conventions rather than the framework's internal ones.

Drafts → `segments/`.
