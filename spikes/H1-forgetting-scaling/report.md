# H1 / M1 / N4 — Strategic-tempo Lyapunov-proof scaling — spike report

**Status: CRACKED (strengthening recovers `R_Σ* = ρ_Σ/𝒯_Σ`).** Plus a cross-paper Model-(S) naming-collision call.

## TL;DR

The headline form `R_Σ* = ρ_Σ / 𝒯_Σ` is recoverable as written. The audit findings (Codex H1, self-N4, Opus M1) are correct that the displayed proof at `src/re/B-key-lemma-proofs.md:64-72` does not actually deliver it — but the gap is not a fundamental modeling problem. It is a one-line algebra hop where two standard moves got conflated: the proof's `E[ΔV] ≤ -2𝒯V + ρ²` inequality is the *mean-square* reduction (under an implicit zero-mean assumption that's not in the model statement), and that inequality honestly delivers only `R_Σ* ≤ ρ_Σ / √(2𝒯_Σ)`. The deterministic-disturbance form stated in §5 — `|w_ij| ≤ ρ_Σ/|E|^{1/2}`, `Σ w_ij² ≤ ρ_Σ²`, no zero-mean assumption — is what gives the linear `ρ/𝒯` scaling, but to extract it you have to keep the cross-term `2δ^⊤w` and bound it via Cauchy–Schwarz, *not* expand `‖δ - F + w‖²` and drop into `E[‖w‖²]`.

The fix is squarely in the second bucket of the brief's trichotomy — **strengthen the math to match the existing claim**. Paper-01's `lem-persistence-d`(i) at `01-tragedy-confident-agent/src/02-persistence.md:21` has the canonical continuous-time form; the discrete-time analog uses an η-AM-GM expansion. Both go through cleanly under Model (S) as currently stated. Sharpness in (ii) of paper-01 also transfers — it gives the same `ρ/𝒯` threshold from the *adversarial-disturbance* lower-bound side, matching the existing "Sharpness" paragraph at line 74.

The third trichotomy outcome ("two corollaries — deterministic with `𝒯 > ρ/R`, mean-square with `𝒯 > ρ²/(2R²)`") is also worth surfacing — *not* as a fallback for failed strengthening but as a structural pointer for cross-paper consistency: paper-01 already does this dichotomy explicitly at `02-persistence.md:23` and adopting the same convention in B-CS1 would unify both papers.

## Trichotomy mapping

- **Tighter than `ρ/𝒯` under sharpened conditions** — *not found, structurally not expected.* Paper-01's `lem-persistence-d`(ii) shows `ρ/𝒯` is sharp from the adversarial-disturbance side; an adversarial `w` aligned with `δ` saturates Cauchy–Schwarz. Tightening would require restricting the disturbance class (e.g., disturbances always orthogonal to `δ` — not natural here).
- **Strengthening to recover `ρ/𝒯` honestly** — *succeeded.* Load-bearing finding. The discrete-time deterministic Lyapunov-with-cross-term argument gives `R_Σ* = ρ_Σ/𝒯_Σ^{bn,ss}`, matching the theorem statement and `(A2)`'s `𝒯_Σ > ρ_Σ/R_Σ`.
- **Fundamental incompatibility forcing model split** — *partially relevant, worth surfacing.* The deterministic and mean-square models genuinely give different scalings (`ρ/𝒯` vs. `ρ/√(2𝒯)`). Paper-01 names this dichotomy explicitly. B-CS1 currently uses the deterministic form in the model statement and the mean-square form in the proof, silently. Whichever direction the unification goes (probably deterministic, since that's what (A2) and downstream rely on), it should be said.

## Working notes — angles tried

### 1. First-hand verification of the algebra hop

The proof at `B-key-lemma-proofs.md:64-72` runs:

1. *(line 65)* `E[ΔV | δ] = -2δ^⊤F + E[‖w‖²]`.
2. *(line 67)* `δ^⊤F ≥ 𝒯·‖δ‖² = 𝒯V`.
3. *(line 68)* `E[‖w‖²] ≤ ρ_Σ²`.
4. *(line 71)* `E[ΔV] ≤ -2𝒯V + ρ_Σ²`.
5. *(line 72)* "Whenever `V > ρ²/(2𝒯)` the drift is negative; iterating gives ultimate boundedness with `V ≤ R*²` where `R*² = ρ²/𝒯²` to leading order, i.e., `R* = ρ/𝒯`."

The hop is between sub-line-72-clause-1 and sub-line-72-clause-2. The drift-negative threshold `V > ρ²/(2𝒯)` is correctly derived from `-2𝒯V + ρ² < 0`. But ultimate-boundedness then gives `V* ≤ ρ²/(2𝒯)`, *not* `ρ²/𝒯²`. Iterating `V⁺ ≤ V - 2𝒯V + ρ² = (1-2𝒯)V + ρ²`: steady state `V* = ρ²/(2𝒯)`, giving `‖δ‖* ≤ ρ/√(2𝒯)`. The "to leading order" hedge does not make the inequality tighter — `ρ²/𝒯²` is strictly larger than `ρ²/(2𝒯)` once `𝒯 < 1/2` (the regime of interest), so going *up* from the inequality is not "leading order," it's wrong direction.

So the audit findings are confirmed: as written, line 71 gives `ρ/√(2𝒯)`, not `ρ/𝒯`.

### 2. The implicit zero-mean assumption

Line 65 has a silent step. Starting from `δ⁺ = δ - F(δ) + w` (Model (S)) with `V = ‖δ‖²`:

```
V⁺ - V = ‖δ - F + w‖² - ‖δ‖²
       = -2δ^⊤F + 2δ^⊤w + ‖F - w‖²
       = -2δ^⊤F + 2δ^⊤w + ‖F‖² - 2F^⊤w + ‖w‖²
```

The line-65 form `E[ΔV | δ] = -2δ^⊤F + E[‖w‖²]` requires:

- `E[w | δ] = 0` to kill the `2δ^⊤w` cross-term.
- `‖F‖²` dropped as `O(𝒯²V)` (legitimate for small `𝒯`, makes line 65 an inequality not an equality — minor).
- `E[F^⊤w] = 0` via zero-mean.

But the model statement at `05-mechanism.md:20-22` is *deterministic*: `|w_ij| ≤ ρ_Σ/|E|^{1/2}` pointwise and `Σ w_ij² ≤ ρ_Σ²` pointwise. There is no zero-mean assumption. In the standard adversarial-disturbance reading, `w` is allowed to be the worst-case, including aligned with `δ` — for which `δ^⊤w = +‖δ‖ρ` (Cauchy–Schwarz upper bound), not zero.

Line 65 silently shifts from the deterministic Model (S) at line 20–22 to a stochastic zero-mean reading. This is the model-swap the brief warned about.

### 3. Deterministic Lyapunov-with-cross-term — continuous time (cleanest version)

For continuous-time idealization `δ̇ = -F(δ) + w`, with `V = ‖δ‖²`:

```
V̇ = 2δ^⊤δ̇ = -2δ^⊤F + 2δ^⊤w
```

By the bottleneck sector inequality (line 67), `δ^⊤F ≥ 𝒯_Σ^{bn,ss} ‖δ‖²`. By Cauchy–Schwarz with `‖w‖ ≤ ρ_Σ`:

```
|2δ^⊤w| ≤ 2‖δ‖·‖w‖ ≤ 2ρ_Σ‖δ‖
```

Combining:

```
V̇ ≤ -2𝒯_Σ‖δ‖² + 2ρ_Σ‖δ‖ = -2‖δ‖·(𝒯_Σ‖δ‖ - ρ_Σ)
```

Strictly negative whenever `‖δ‖ > ρ_Σ/𝒯_Σ`. Trajectory ultimately bounded by `R_Σ* = ρ_Σ/𝒯_Σ`. **Threshold: `𝒯_Σ > ρ_Σ/R_Σ` ⟺ `R_Σ* ≤ R_Σ`.** Matches the headline. This is paper-01's `lem-persistence-d`(i), modulo `V = ‖δ‖²` vs. `V = ½‖δ‖²` factor-of-two convention.

### 4. Deterministic Lyapunov-with-cross-term — discrete time

The model in B-CS1 is discrete-time, so the continuous-time form is technically a mismatch — though it's a fine approximation. For discrete-time proper, use AM–GM:

```
V⁺ = ‖δ - F + w‖² = ‖δ - F‖² + 2(δ - F)^⊤w + ‖w‖²
```

In the diagonal architecture `‖δ - F‖² = Σ(1 - α_ij)²δ_ij²`. When `α_ij ∈ [0, 1]`, `(1 - α)² ≤ (1 - α)`, so `‖δ - F‖² ≤ (1 - 𝒯_Σ)V`. AM–GM with parameter η > 0:

```
2(δ - F)^⊤w ≤ η‖δ - F‖² + (1/η)‖w‖²
```

With `‖w‖² ≤ ρ_Σ²`:

```
V⁺ ≤ (1 + η)(1 - 𝒯_Σ)V + (1 + 1/η)ρ_Σ²
```

Pick `η = 𝒯_Σ/(2(1 - 𝒯_Σ))`. Then `(1 + η)(1 - 𝒯_Σ) = 1 - 𝒯_Σ/2`, and `(1 + 1/η) ≈ 2/𝒯_Σ` for small `𝒯_Σ`. Steady-state `V* = (1 + 1/η)ρ_Σ²/(𝒯_Σ/2) ≈ 4ρ_Σ²/𝒯_Σ²`, giving `‖δ‖* ≤ 2ρ_Σ/𝒯_Σ`. Same scaling, constant-of-2.

The constant can be tightened by optimizing η; the scaling is identical. Cleanest path: state continuous-time as the structural form, treat discrete as corollary up to absorbable constants. (A2) is a *threshold* condition — constant on `R*` shifts the threshold by a constant, easily absorbed into the abstract `ρ_Σ` budget.

### 5. Sharpness via comparison principle

The "Sharpness" paragraph at line 74 currently asserts an adversarial element-concentration argument informally. Paper-01's `lem-persistence-d`(ii) gives the formal version: with `w*(t) = ρ δ(t)/‖δ(t)‖` (admissible since `‖w*‖ = ρ`):

```
d‖δ‖/dt ≥ -𝒯‖δ‖ + ρ
```

Comparison principle against `μ̇ = -𝒯μ + ρ` gives `‖δ(t)‖ ≥ μ(t)`, and `μ(t)` crosses any `R < ρ/𝒯` in finite time. In the diagonal-correction model, the bottleneck-element refinement is: pick `w` and `δ` both supported on `(i*, j*)` where `α_ij` is minimized; this saturates the sector inequality on the contracting side and aligns with `w` on the disturbance side simultaneously. So sharpness transfers cleanly and can be made rigorous in a sentence.

### 6. Mean-square corollary — the legitimate zero-mean reading

For completeness: if `w` is taken to be zero-mean random with `E[‖w‖²] ≤ ρ_Σ²` (distinct model from current Model (S)):

```
E[V⁺ | δ] ≤ (1 - 2𝒯 + 𝒯²)V + ρ² ≈ (1 - 2𝒯)V + ρ²
```

Steady state `E[V*] = ρ²/(2𝒯)`, giving `√(E[‖δ‖²]) ≤ ρ/√(2𝒯)` and threshold `𝒯 > ρ²/(2R²)`. *Real result*, not a fallback. Applies in genuinely-stochastic-noise regimes. For B-CS1's downstream — non-stationarity-induced drift in the optimum — the deterministic worst-case is the more natural model since drift can be "adversarial" in the sense of consistently moving mismatch in one direction.

### 7. Regime split (small-𝒯 vs. large-𝒯)

The brief asked whether different scalings emerge in different regimes:

- **Small `𝒯_Σ ≪ 1`** — continuous-time analysis is the right idealization; `ρ/𝒯` holds; discrete-time AM–GM gives same scaling with absorbable constants.
- **`𝒯_Σ` approaching 1** — discrete-time needs `α_ij < 1`; if `α_ij ≥ 1` the element overshoots zero and the discrete-time model breaks. In practice `𝒯_Σ ≤ 1` is structural (it's `min(ν · ι · (1-λ))` each in `[0,1]`), so this regime is not relevant.
- **No different scaling emerges in different regimes for the deterministic model.** The mean-square model gives `ρ/√(2𝒯)` uniformly. So "different regime, different scaling" is *not* what's happening — what's happening is *different model, different scaling*.

### 8. Cross-check against Khalil Chapter 9

Khalil Ch. 9 (the textbook the proof cites) handles both deterministic and stochastic disturbances. Theorem 9.1 / Lemma 9.2 cover the deterministic case where disturbance is bounded but otherwise arbitrary; cross-term retention is part of the standard argument there. The B-CS1 proof cites Khalil but executes a different derivation — citation cover-page is correct (Khalil *does* give this kind of result) but the specific derivation doesn't follow Khalil's actual technique. Recommend citing Khalil more precisely (Section 9.2 / comparison-principle argument) when the rewrite lands.

### 9. Convention drift to flag

Paper-01 uses `V = ½‖δ‖²` (`02-persistence.md:21`); B-CS1 uses `V = ‖δ‖²` (`B-key-lemma-proofs.md:64`). Harmless — factor-of-2 in displayed inequalities — but if both papers are read together, the reader hits "wait, why's this different by 2?" Worth a one-line acknowledgment.

## Recommended diff for `B-key-lemma-proofs.md` lines 64–74

Replace the displayed proof with a deterministic-cross-term derivation mirroring paper-01's `lem-persistence-d`(i). Sketch (final wording is the integration agent's call):

> Within Model (S) of [[#^sec-key-lemma-2]] with diagonal correction architecture, in the continuous-time idealization `δ̇ = -F(δ) + w`, the Lyapunov function `V = ‖δ‖²` satisfies `V̇ = -2δ^⊤F(δ) + 2δ^⊤w`. By the diagonal architecture and bottleneck sector inequality, `δ^⊤F(δ) = Σ α_ij^{ss}δ_ij² ≥ min α_ij^{ss}·‖δ‖² = 𝒯_Σ^{bn,ss}V`, with the bound tight on the bottleneck-element indicator. By Cauchy–Schwarz with `‖w‖ ≤ ρ_Σ`, `|2δ^⊤w| ≤ 2‖δ‖ρ_Σ`. Combining: `V̇ ≤ -2‖δ‖(𝒯_Σ^{bn,ss}‖δ‖ - ρ_Σ)`. The drift is strictly negative whenever `‖δ‖ > ρ_Σ/𝒯_Σ^{bn,ss}`; trajectory ultimately bounded by `R_Σ* = ρ_Σ/𝒯_Σ^{bn,ss}`. Standard Khalil sector-Lyapunov ultimate-boundedness, comparison-principle form `[Section 9.2]{khalil-2002-nonlinear}`. So whenever `𝒯_Σ^{bn,ss} > ρ_Σ/R_Σ` we have `R_Σ* ≤ R_Σ` and the modeled mismatch is ultimately bounded within the strategic reserve. The discrete-time form recovers the same scaling up to absorbable constants via AM–GM expansion of `‖δ - F + w‖²`. *Mean-square corollary:* if `w` is zero-mean stochastic with `E[‖w‖²] ≤ ρ_Σ²`, the cross-term vanishes in expectation and the bound is `√E[V*] ≤ ρ_Σ/√(2𝒯_Σ^{bn,ss})` under threshold `𝒯_Σ > ρ_Σ²/(2R_Σ²)` — the strict mean-square form. The deterministic-disturbance form is operative for [[#^thm-composition]](A2) since non-stationarity drift is adversarial-aligned rather than zero-mean.

Sharpness paragraph upgrade (line 74): use the comparison-principle witness from paper-01 — `w*(t) = ρ_Σ δ(t)/‖δ(t)‖` saturates Cauchy–Schwarz, gives `d‖δ‖/dt ≥ -𝒯_Σ‖δ‖ + ρ_Σ`, comparison-principle yields finite exit time when `𝒯_Σ < ρ_Σ/R_Σ`. Bottleneck-element concentration is the natural sharpness witness in the diagonal-correction case.

### Downstream propagation — does anything else change?

Verified by spot-check:

- **Theorem 4.1(A2)** (`04-main-result.md:30`): threshold `𝒯_Σ > ρ_Σ/R_Σ` — **unchanged**.
- **Theorem 4.1(ii)** (`04-main-result.md:42`): `R_Σ* = ρ_Σ/𝒯_Σ` — **unchanged**.
- **Headline §1** (`01-introduction.md:31`): same threshold — **unchanged**.
- **Component 3** (`04-main-result.md:18`): same threshold — **unchanged**.
- **§5 intuition paragraph** (`05-mechanism.md:22-29`): "diagonal correction"/"Khalil's classical sector-Lyapunov template" — **unchanged**, optionally note continuous-time idealization.

So **proof rewrite is local; no downstream propagation needed**. Cheapest-possible fix consistent with honest math.

## Cross-paper consistency note (strategic call for Joseph)

Paper-01 (`01-tragedy-confident-agent`) handles the deterministic/mean-square dichotomy explicitly:

- *Model D* (deterministic): `R* = ρ/α`, threshold `α > ρ/R`. Pathwise persistence. (`02-persistence.md:8-10`)
- *Model S* (mean-square Gaussian): `R*_S = σ_w √(n/(2α))`, threshold `α > nσ_w²/(2R²)`. Mean-square only. (`02-persistence.md:23`)
- Explicit warning: *"Model S 'persistence' should be read as mean-square boundedness or finite-horizon high-probability survival; pathwise infinite-horizon `T_exit = ∞` is a Model D claim."* (line 23)

B-CS1 currently *names* "Model (S)" (`05-mechanism.md:22`) but runs the proof under a stochastic-zero-mean reading and produces quadratic-threshold scaling, while *stating* the linear-threshold (Model D) form everywhere downstream. Collision is silent — no in-text acknowledgment of which model is operative.

**The naming collision is the substantive cross-paper question.** Paper-01 uses S for the *stochastic* model; B-CS1 uses (S) for what's effectively paper-01's *deterministic* (Model D). Two options:

- **Option A**: Rename B-CS1's "Model (S)" to "Model (D)" or "Model (Σ)" or just "the diagonal sector model" to avoid collision and signal the deterministic-disturbance reading.
- **Option B**: Keep "Model (S)" but make the deterministic reading explicit ("Model (S) treats `w` as deterministic with `‖w‖ ≤ ρ_Σ`; the stochastic mean-square variant is recovered as a corollary").

Joseph: cross-paper coordination decision, not something the spike can settle alone.

## Summary of findings

- **Core**: Headline `R_Σ* = ρ_Σ/𝒯_Σ` is recoverable. Proof needs rewrite using deterministic Lyapunov-with-cross-term (Cauchy–Schwarz on `2δ^⊤w`), not implicit-zero-mean expansion. Local fix (lines 64–74); no downstream propagation. Template: paper-01's `lem-persistence-d`(i) at `01-tragedy-confident-agent/src/02-persistence.md:21`.
- **Sharpness**: Line 74 paragraph correct in spirit but informal; can be made rigorous via paper-01's comparison-principle witness in `lem-persistence-d`(ii). One-paragraph upgrade.
- **Cross-paper**: Naming collision (B-CS1 "Model (S)" vs. paper-01 "Model S" naming opposite things) flagged for Joseph's decision.
- **Mean-square corollary**: Adding the `ρ/√(2𝒯)` form as a one-line remark (real result for genuinely-stochastic-noise regimes) is recommended — gives reader the dichotomy without changing operative form.
- **Audit findings disposition**: Codex H1, self-N4, Opus M1 confirmed correct; the strengthening direction the brief outlined (retain cross-term, Cauchy–Schwarz) is the right one and goes through.
- **Trichotomy outcome**: Bucket 2 (strengthen to match existing claim). Bucket 1 (tighter than `ρ/𝒯`) structurally not available — paper-01's sharpness rules it out. Bucket 3 (model split) partly relevant — it's a structural pointer worth surfacing alongside the strengthened proof, not a fallback.
- **Effort estimate**: ~1–2 paragraphs in `B-key-lemma-proofs.md`, optionally ~1 line in `05-mechanism.md`. Cross-paper convention question separate.
