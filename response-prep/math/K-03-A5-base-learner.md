# K-03 — does (A5)'s occupancy-weighted TV condition actually hold for UCRL2/UCBVI?

**Status: OPEN, unexamined.** This file is a brief to myself, not a result. Nothing here has been checked against App D or against the source papers.

## The question

(A5) needs, within each restarted stationary interval of length `L`:

    Σ_{t=1}^{L} E[ TV̄_t ]  ≤  2c √L,
    TV̄_t := (1/N_h) Σ_{h} E_{s_h ~ d_h^{Q_t}} [ TV( π*_t(·|s_h), Q_t(·|s_h) ) ]

i.e. an **occupancy-weighted total-variation** regret, not a value regret. Conclusion (v) rests on this, so if it doesn't hold for the named learners, the headline rate is unsupported for any concrete algorithm.

## Why it is not obviously free

Standard UCRL2 / UCBVI analyses bound **value** suboptimality — via optimism plus confidence-set width, summed over episodes. The object here is a **policy-space distance** at visited states. These are related but not interchangeable:

- Value-gap → TV: does **not** go through in general. A tiny value gap is compatible with `TV = 1` whenever two actions have nearly equal Q-values (the near-tie regime). This is the direction the paper needs, and it is the direction that fails without an extra assumption.
- The paper does have `Δ_min > 0` (isolated optimum, action gap) in its canonical scope, which is exactly the hypothesis that rescues the conversion: a mis-ranking costs at least `Δ_min` in value, so `Σ E[TV̄_t] ≤ (1/Δ_min) · Σ E[value gap] ≤ Õ(√L)/Δ_min`. **That is probably the intended argument** and it is plausibly correct.
- If so, the `2c√L` in (A5) hides a `1/Δ_min` factor, and the final rate carries a `Δ_min` dependence that the theorem statement as written may not surface. Whether that is stated, absorbed into `c`, or genuinely missing is the thing to check.

Note the deterministic-argmax case is cleaner: `TV(δ_{a*}, δ_{â}) ∈ {0,1}`, so `Σ E[TV̄_t]` literally counts occupancy-weighted mis-ranking events. Then the question is whether optimistic learners have `√L` *mis-ranking-count* bounds, which is a known-shape result under a gap assumption but is a different theorem from their regret bound.

## Check order

1. Read App D first-hand in the frozen `../submitted-neurips-2026.pdf`. Determine whether the `Δ_min` conversion is present, implicit, or absent.
2. If present: confirm the constant `c` in (A5) is honest about carrying `1/Δ_min`, and whether conclusion (v) should therefore display a `Δ_min` factor. If it should and doesn't, that is a real (if repairable) error in the headline rate and we should say so ourselves.
3. If absent: attempt the conversion (it looks routine given `Δ_min > 0`); if it works, the response answers the reviewer with the missing step and the paper is stronger for it. If it doesn't, state the gap.
4. Either way the answer is short in the response. The value here is mostly for the rewrite.

## Note on disposition

This paper is being rejected. That makes it *easier*, not harder, to be exact about a possible hole in its main theorem — there is no accept to protect, and carrying a silent gap into the resubmission is the only genuinely bad outcome. Record whatever is found, including "the objection was right."
