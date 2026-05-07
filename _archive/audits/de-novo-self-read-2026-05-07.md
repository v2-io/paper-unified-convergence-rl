# De-novo self-read — 2026-05-07

*First-hand cold read of `unified-rl-neurips-2026.tex` (the kramdown-emitted intermediate, since the .pdf flattens math). Goal: independent observations to compare against the Opus auditor's findings when those land. Note as I go; don't get sucked into verifying every detail; flag what catches.*

## Headline-level observations (new findings beyond Codex/Gemini)

### N1 — The headline rate is stated four different ways in four locations

| Location | Form | Has N_h? | Has V_max? | Has bias term? |
|----------|------|----------|------------|----------------|
| Abstract (line 211) | `O(V_max √((B_T+1) T))` | ✗ | ✓ | ✗ |
| §1 intro (line 230) | `Õ(N_h √((B_T+1) T))` | ✓ | ✗ | ✗ |
| Theorem 4.1(v) (line 367) | `2c V_max N_h √((B_T+1) T) + N_h(1-p_id)log(1/q_0)·T` | ✓ | ✓ | ✓ |
| §6 conclusion (line 498) | `Õ(N_h √((B_T+1) T))` | ✓ | ✗ | ✗ |

Three of four headline locations omit the bias term entirely; two of four omit one of {`V_max`, `N_h`}. Only the theorem statement carries the full honest form. This is partly Codex's H6 (bias-term-omitted-from-headline), but the *form-instability across locations* is a separate finding — readers comparing the abstract to the conclusion to the theorem will see three different "headline rates" and reasonably wonder which one is the theorem actually proving.

The strengthening direction is to commit to a single headline form and use it consistently. Candidates:
- *Per-step convention* with `V_max` redefined per-step ≤ 1 under §3's [0,1] reward bound. Headline becomes `Õ(N_h √((B_T+1) T))` everywhere; the cumulative bound in (v) reads `2c · N_h · √((B_T+1)T) + bias`. Cleanest.
- *Cumulative `V_max`* with simulation lemma's N_h-multiplier dropped per Codex H3 fix (b). Headline becomes `Õ(V_max √((B_T+1) T))` everywhere; the implicit N_h is absorbed into V_max via cumulative range. Forces V_max to do double duty (range + horizon-extent).
- *Identity-as-headline* (Q2 from rc1 review, Codex M2 / paper-1+3 alignment): lead with the per-round identity as the headline result (which is unconditional), and present the cumulative rate as a downstream consequence with the bias-vanishes-in-Regime-A condition. This is the held strategy item.

### N2 — Numbering inconsistency between abstract and §1.1 contributions

- **Abstract** (line 211) lists four equal-status components: (i) two-gap, (ii) identity, (iii) tempo, (iv) loop.
- **§1.1 Contribution** (line 240) lists three numbered moves: (i) identity, (ii) tempo, (iii) loop — and demotes the two-gap diagnostic to "Connective tissue: the two-gap diagnostic" at line 252.
- **§4** (line 320) returns to four components, matching abstract.
- **§4.4 Necessity** (line 387) frames as "*bundle of compatible guarantees* (each conclusion supported by its own assumptions), not a single integrated theorem."

So the paper has two competing framings: "four equal components" (abstract, §4) vs. "three load-bearing + connective tissue" (§1.1) vs. "bundle, not integrated theorem" (§4.4). Reviewers will notice. This is the *concrete surface* of Codex's M2 / paper-1+3+pipeline-agent's converging "abstract presents monolithically" critique. Held for the strategy talk — the resolution depends on which framing the paper commits to.

### N3 — Stale cross-references to Theorems 4.2 and 4.3 (no such theorems in §4)

- Line 607 (in §B Lemma 1 proof): `\textbf{Two-sided regret bound (Theorem 4.2 in \Cref{sec-main-result}).}` — but there is no Theorem 4.2 in §4. The "two-sided regret bound" `Δ_min(1-e^{-D}) ≤ R(Q) ≤ V_max(1-e^{-D})` is stated in §4 Component 2 (line 330) as a paragraph derivation, never numbered.
- Line 613 (in §B): `\subsection{Proof of perturbative extension (Theorem 4.3 — ε-stochastic and softmax-regularized)}` — but there is no Theorem 4.3 in §4. The perturbative extension is mentioned at §4 line 331 and §5 line 421 but never stated as a numbered theorem.

These are migration-era leftovers — the original paper-draft had separate numbered theorems for each result; the restructure folded them into paragraph form in §4 / §5 but the proof-section labels in §B still reference the old numbering. **Fix options:**

- *(a) Restore numbering:* state the two-sided regret bound and the perturbative extension as numbered theorems in §4 (Theorem 4.2 and Theorem 4.3 respectively), with full hypotheses + conclusion. This reads stronger — the paper's *three* main-text theorems become its named units (composition, two-sided identity-bound, perturbative extension). Bigger structural strengthening, mild page-budget cost (~5–8 lines).
- *(b) Update proof headers:* remove the stale "Theorem 4.2 / 4.3" references in §B, replace with descriptive names ("Two-sided regret bound proof (Component 2 of §4)" / "Perturbative extension proof"). Smaller change, no narrative gain.

(a) is a real strengthening — formalizing the two-sided bound and perturbative extension as named theorems clarifies the contribution shape and gives reviewers explicit objects to discuss. Worth flagging as a candidate elevation alongside Gemini's universal-failure-class theorem (O1).

### N4 — H1 algebra error visible in §B Lemma 2 proof

The §B Lemma 2 proof (line 651–663) explicitly displays:

> `E[ΔV | δ_Σ] ≤ -2 𝒯_Σ V + ρ_Σ²` (line 661)
>
> "Whenever V > ρ_Σ²/(2 𝒯_Σ), the expected drift is negative; iterating gives ultimate boundedness with V ≤ R*² where R*² = ρ_Σ²/𝒯_Σ^{·2} to leading order, i.e., R* = ρ_Σ/𝒯_Σ." (line 662)

The first sentence ("V > ρ²/(2𝒯) gives negative drift") is correct — and *that* gives `V ≤ ρ²/(2𝒯)` as the ultimate-bounded value, hence `R* = ‖δ‖ ≤ ρ/√(2𝒯)`. The second clause "R*² = ρ²/𝒯^{·2}" doesn't follow from the first — the algebra jumps from `V ≤ ρ²/(2𝒯)` to `R*² = ρ²/𝒯²` without justification.

This is exactly Codex H1 — the displayed proof gives `R* ~ ρ/√𝒯` (mean-square form), but the conclusion claims `R* = ρ/𝒯` (deterministic-cross-term form). The two forms come from different disturbance models; the proof as written conflates them.

Already in TODO as H1 spike candidate. The on-page algebra error makes it more urgent — even without a strengthening attempt, the current proof has an inconsistent step.

### N5 — Direction-forcing argument (§3 line 311) is load-bearing, undersold

> "Our bounds use the *reverse* KL direction (with P = π*); the forward direction `D_KL(Q || π*)` is +∞ whenever Q has off-optimum mass and is therefore vacuous as a regret coordinate."

This is *the structural reason* the whole framework works — forward KL is genuinely vacuous at the deterministic-π* corner, so reverse KL isn't a stylistic choice but the only direction that gives a non-trivial coordinate. Currently framed as a one-line "convention" remark; could be promoted to "this is why the identity is the right object: forward KL would give +∞ here, so reverse KL is the *forced* direction once we commit to deterministic π*." Minor refinement, but the rhetoric undersells what it's saying.

## H1–H5 / M1 / M3 — confirmed against primary source

The Codex H1–H5 / M1 / M3 findings I integrated yesterday hold up cleanly on a holistic re-read. Specifically:

- **H1** (Lyapunov scaling, line 651–663): algebra error visible on the page — see N4 above. The drift inequality gives `R* ~ ρ/√𝒯`; the stated `R* = ρ/𝒯` doesn't follow. The strengthening direction (retain cross-term) is right; outcome regime-dependent.
- **H2** (bias term value-scale conversion, line 695): the per-state per-horizon-step lift in §B aggregates `(1 - p_id) log(1/q_0)` over N_h horizon steps to give `N_h (1-p_id) log(1/q_0)` per round. No `V_max` factor at this lift step. Then summed over T rounds at line 579 / 695 to give the `T`-scale linear bias term. The missing `V_max` is the units mismatch; the `min(1, log(1/q_0))` clip is the strengthening.
- **H3** (V_max double-count, line 565–567 and 301): §3 defines `V_max` as cumulative-horizon Q range (max - min over Q_O, the full-horizon expected value). Line 565 in the simulation-lemma proof bounds each per-step bracket by `V_max · TV`, which uses the cumulative range as a per-step bound (loose by factor up to N_h). Then summing over N_h steps gives `V_max · N_h · TV_bar` — at saturation, this is `~N_h · V_max = N_h² r_max`, while the trivial regret bound is `V_max = N_h r_max`. The bound is N_h× looser than trivial in the saturating regime.
- **H4** (B_T = optimum-change vs. stationary-segment, line 539, 481): notation paragraph in §A defines `B_T = |{t : a*_t ≠ a*_{t-1}}|`; proof step 2 of (v) at line 481 says "Within each block of length Δ_i the MDP is stationary." Same mismatch as TODO entry says.
- **H5** (A1 with deterministic UCB, line 887–888 in §D-algorithm): `E[1 - e^{-K_t}] = E[1 - Q_t(a^*)] = O(log t / (t Δ_min))` for "Thompson sampling and UCB." The identity holds at the K_t = ∞ limit; pointwise A1 violation as TODO notes.
- **M1** (sequential-ignorability framing): the abstract (line 211) and §1.1 (iii) (line 250) use varying degrees of headline framing. §6 line 510 ("On coupled-goal architectures") *does* surface the C2 caveat properly in the conclusion — but only there. The M1 fix is to push the C2 visibility forward to the abstract and §1.
- **M3** (q_0 condition in §4): line 331 mentions perturbative extension without naming the `Q ≥ q_0` condition; §5 line 421 does mention it. M3 fix is one-clause add to §4.

## §4.4 Necessity — confirmation as page-budget candidate

§4.4 (line 387–400) does exactly what Joseph and I anticipated: a self-contained reviewer-anticipation argument ("Without Component 2... Without Component 3..." etc.). 13 lines of body prose. Lifts cleanly to an appendix with a 1-2 sentence summary in main text. *On re-read, the case is strengthened:* the body's narrative arc actually flows better without §4.4 interrupting between Theorem 4.1's unpacking (§4.3) and §5's mechanism narrative — the ablation reads better as "we anticipated this objection" appendix material.

Minor refinement: the "Without Component 2" failure mode at line 392 is framed as "we'd use Pinsker or BH instead" — slightly understates what's lost. The §1.1 framing at line 254 had it sharper: "the metric layer loses *exactness*, and the 'behavior-cloning loss against optimal trajectory' interpretation of the cumulative coordinate vanishes." When §4.4 moves to appendix, port the §1.1 framing.

## §6 Conclusion observations

- Line 504 "On the optimal dependencies on N_h, S, A": acknowledges potential `√N_h` improvement via Bernstein-type concentration. Honest scope.
- Line 506 "On the continuous-variation extension": frames as open problem.
- Line 508 "On joint uniqueness": "We do *not* claim joint uniqueness across all three properties." Good non-claim.
- Line 510 "On coupled-goal architectures": surfaces M1 properly in conclusion.
- Line 512 "On strict tightness of the strategic-tempo threshold": "sufficient and sharp inside the diagonal sector model." Good scope-honest framing.

The four "On X" subsections in §6.1 each acknowledge a real limitation of the current result without overclaiming. This is the right voice for a theory paper. Don't touch in budget pass.

## What's working well — preserve in any cleanup pass

- The boxed identity at §1 line 232–233 is a strong rhetorical anchor.
- §2 Related Work organization by named lineages with explicit "same shape, different axis" / "we compose with non-stationarity" framing — reviewer-resonant.
- §3 Preliminaries strict minimalism — defers convention details to appendix.
- §4.3 Unpacking the conclusions — four emphasized observations, each declarative and honest.
- §6 four "On X" observations — scope-honest without overclaiming.
- Practitioner takeaways (§6.2) — three numbered actionable items.

## Summary triage relative to Codex/Gemini/now

**Confirmed:** all H1–H5 / M1 / M3 findings hold up under independent read. The §B Lemma 2 proof has an on-page algebra error visible at line 662 (Codex H1).

**New findings (N1–N5):**
- N1 (rate-form instability across abstract/intro/theorem/conclusion) — strengthens the case for committing to a single headline form. Tied to H6 / Q2 strategy item.
- N2 (numbering inconsistency abstract vs. §1.1) — concrete surface of M2. Held for strategy.
- N3 (stale Theorem 4.2 / 4.3 cross-refs in §B) — real fix needed; (a) restoring numbering is a real strengthening, (b) updating proof headers is the minimal fix.
- N4 (Lemma 2 proof algebra error) — already in TODO as H1, but on-page visibility upgrades urgency.
- N5 (direction-forcing rhetoric undersold) — minor.

**Page-budget candidates confirmed:**
- §4.4 relocation — re-read strengthens the case (narrative arc cleaner without the interruption).
- BH/Pinsker repetition tightening — confirmed five+ surface mentions in abstract/§1/§4/§5/§E/§6.

**No structural collapses recommended.** The Jin-style §4–§5 split is doing real narrative work and shouldn't be touched.
