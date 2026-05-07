# H5 Spike — A1 Satisfaction by Deterministic Base Learners

**Headline outcome:** *strengthening succeeded* in the first form anticipated by the brief — A1 is **too strong**. It is not load-bearing in the proof of conclusion (i), and not load-bearing in the *expectation*-form derivation of conclusion (v). The identity that A1 was apparently designed to support extends *unconditionally* through the `K_t = +∞` limit under the convention `e^{-∞} = 0`. Vanilla deterministic UCB / UCBVI / UCRL2 are compatible with the framework once A1 is replaced by a vacuous extended-real reading (or removed entirely).

The spike *also* surfaced an independent finding worth flagging back: the bandit-case rate `E[1 - e^{-K_t}] = O(log t / (t·Δ_min))` claimed at `05-mechanism.md:74` and `D-algorithm.md:5-7` is off by one factor of Δ_min. The correct probability-of-suboptimal-pull rate for UCB is `O(log t / (t·Δ_min²))`, and the V_max·TV chain therefore yields cumulative regret `O((B_T+1)log²(·)/Δ_min²)` rather than `/Δ_min`. The framework's TV-side bound is structurally looser than direct gap-aware UCB analysis by exactly one factor of Δ_min. This is genuinely separate from H5 but lives in the adjacent paragraphs.

---

## 1. The failure mode, verified against primary source

(A1) at `04-main-result.md:28`:
> **(A1) Metric on policy space.** Q_t(a*_t | s) > 0 at every visited state and round.

Vanilla deterministic UCB (UCB1, Auer-Cesa-Bianchi-Fischer 2002) deploys a Dirac at the UCB-argmax: `Q_t = δ_{a_t^{UCB}}`. Whenever `a_t^{UCB} ≠ a*`, `Q_t(a*) = 0` and (A1) is violated *pointwise*. Same for UCBVI (Azar-Osband-Munos 2017) and UCRL2 (Auer-Jaksch-Ortner 2010): both deploy at the optimism-argmax.

Under the natural extension, `Q_t(a*) = 0` gives `K_t = -log Q_t(a*) = +∞`. The identity `TV(δ_{a*}, Q_t) = 1 - e^{-K_t}` extends through the limit as `1 = 1 - 0`, matching `TV(δ_{a*}, δ_{a^{UCB}}) = 1` directly. The expectation `E[1 - e^{-K_t}] = E[1 - Q_t(a*)] = P(a_t^{UCB} ≠ a*)` is finite and bounded by textbook UCB suboptimal-pull rates.

So the *pointwise* reading of (A1) is what fails. The *expectation* form holds.

## 2. Audit of every load-bearing use of (A1) in the proof

I traced every appearance of (A1) and `Q_t(a*) > 0` through the source.

**(A) Lemma 1 statement** (`05-mechanism.md:7-10`). The hypothesis `Q(a*) > 0` is what makes D_KL finite-valued; the identity `TV = 1 - e^{-D_KL}` *holds even when Q(a*) = 0*, in the extended-real sense (`D_KL = +∞`, `e^{-∞} = 0`, `TV = 1`). At `Q(a*) = 0`: `TV(δ_{a*}, Q) = 1 - 0 = 1` and `1 - e^{-(+∞)} = 1 - 0 = 1`. ✓

**(B) Proof of conclusion (i)** (`A-proof-of-composition.md:7-11`). The TV-regret bounds (`Δ_min · TV ≤ R(Q_t | s) ≤ V_max · TV`) *do not depend on (A1)*. They hold for any policy Q including Diracs at suboptimal actions. At `Q = δ_{a^{UCB}}` with `a^{UCB} ≠ a*`: `R(Q) = Δ(a^{UCB}) ∈ [Δ_min, V_max]`, and the bounds give `Δ_min · 1 ≤ R(Q) ≤ V_max · 1`, both correct. The identity substitution then reads `Δ_min(1 - e^{-K_t}) ≤ R(Q_t) ≤ V_max(1 - e^{-K_t})`; with `K_t = +∞`, this is `Δ_min · 1 ≤ R(Q_t) ≤ V_max · 1`, matching textbook. **(A1) is not load-bearing in the proof of (i).**

**(C) Proof of (v) Step 2** (`A-proof-of-composition.md:35-37`). Uses the per-state identity. Same analysis as (A): equality `1 - Q_t(a*) = 1 - e^{-K_t}` is unconditional with `e^{-∞} = 0`. **Not load-bearing.**

**(D) Proof of (v) Steps 1, 3** (sim lemma, Cauchy-Schwarz). Neither uses (A1). **Not load-bearing.**

**(E) Proof of (v) Step 4 / Lemma 4 bias bound.** *Separate* from (A1). Lemma 4 requires `Q_t ≥ q_0` at *both* a*_{ag,t}(s) and tilde-a*_t(s) — the *two-point* support. For deterministic UCB / UCBVI / UCRL2, both are 0 generically. But the bias bound is `(1 - p_id) · log(1/q_0)`. In **Regime A**, `p_id = 1` and the misidentification event has probability 0; the bias is identically 0 by Lemma 4's matching-event case. The `log(1/q_0)` factor never gets multiplied by anything nonzero. Regime A is safe.

**(F) Conclusion (iii) statement** (`04-main-result.md:44`). Regime-conditional: informative in Regime A (always 0); informative in Regime B *only* under explicit q_0 (e.g., ε-smoothed UCB); vacuous in Regime C and for deterministic UCB outside Regime A. Matches existing "Vanishes in Regime A" qualifier.

**Audit summary.** (A1) appears *only* in conclusion (i) and as a parenthetical-restatement-of-Lemma-1's-hypothesis in Step 2 of the proof of (v). In neither place is it doing nontrivial work — both pass through unconditionally under the extended-real convention. Lemma 4's q_0 condition is *distinct* from (A1) and has its own well-known regime-dependent character.

## 3. Three completion states from the brief: which succeeds?

**3.1 Completion state 1: A1 is too strong, weaken it.** **This is what the audit lands on.** The proof goes through unconditionally under the extended-real convention `e^{-∞} = 0`. The identity holds for *all* policies Q (including Diracs at suboptimal actions, including any K ∈ [0, ∞]). The only place pointwise positivity does any work is *inside* Lemma 1's stated hypothesis — purely to keep D_KL finite-valued as an unextended real. Once the natural limit is accepted (which is *forced* by the formula), the hypothesis becomes vacuous.

**3.2 Completion state 2: Redefine Q_t as internal planning distribution.** Viable but *strictly weaker as a strengthening* — adds machinery (separate "internal" Q_t distinct from deployed policy) and requires arguing deployed regret matches internal regret up to controllable smoothing cost. With state 1 working as cleanly, this is unnecessary.

**3.3 Completion state 3: Pointwise A1 structurally needed; restrict scope.** **Does not hold up under the audit.** Proof of (i) does not use pointwise A1 at any step that's tight. The structural boundary the brief asked us to map *would have been* at conclusion (iii)'s bias bound — Lemma 4's q_0 condition genuinely fails for deterministic learners outside Regime A. But it's not a boundary owed to (A1); it's owed to Lemma 4. They're independent.

## 4. Recommended diff

**At `04-main-result.md:28`** — replace (A1) with the extended-real form:

```
- > **(A1) Metric on policy space.** Q_t(a*_t | s) > 0 at every visited state and round.
+ > **(A1) Metric extension.** The identity TV(δ_{a*_t(s)}, Q_t(·|s)) = 1 - e^{-K_t(s)} of [[#^lem-pointmass-identity]] is read in the extended real sense: when Q_t(a*_t(s)|s) = 0, K_t(s) = +∞ and 1 - e^{-K_t(s)} = 1 recovers the trivial bound R(Q_t|s) ≤ V_max. No pointwise positivity is required; vanilla deterministic UCB / UCBVI / UCRL2 deployed at the optimism-argmax are in scope.
```

(Or drop A1 entirely from the assumption list and put the extension remark inline at conclusion (i)'s justification.)

**At `05-mechanism.md:7-10`** — relax Lemma 1's hypothesis with extended-real qualifier.

**At `A-proof-of-composition.md:9`** — drop the "(which is (A1))" parenthetical.

**At `B-key-lemma-proofs.md:7`** — small qualifier in Lemma 1 proof.

**At `D-algorithm.md:5-7`** — example list now unconditional in bandit case; clarifier.

## 5. Side finding: bandit rate constant — one Δ_min off

While auditing (A1)'s proof use, I noticed the rate `E[1 - e^{-K_t}] = O(log t / (t · Δ_min))` doesn't match standard UCB analysis:

- `E[1 - e^{-K_t}] = E[1 - Q_t(a*)]`. For deterministic UCB this is `P(a_t^{UCB} ≠ a*) = Σ_{a≠a*} P(a_t = a)`.
- Auer 2002 / Lattimore-Szepesvári Theorem 7.1: `E[N_a(T)] ≤ 8 log T / Δ_a² + O(1)` for UCB1.
- Per-round suboptimal-pull probability: `P(a_t = a) = O(log t / (t Δ_a²))`.
- Sum over suboptimal arms: `P(a_t ≠ a*) ≤ K · O(log t / (t Δ_min²))`. **One factor of Δ_min more than the paper's claim.**

Conversely, *cumulative regret* of UCB *is* `O(K log T / Δ_min)` — one Δ_min — because each suboptimal pull weighs Δ(a) and `Δ(a) · 1/Δ(a)² = 1/Δ(a)`. So:
- Cumulative regret rate (literature): `O(log T / Δ_min)`.
- Cumulative `V_max · TV` rate from this framework: `O(V_max · log² T / Δ_min²)`.

The framework's `V_max · TV` chain *is structurally looser* than direct gap-aware UCB analysis by exactly one factor of Δ_min. This is **not** an error in the framework; it's a structural feature of the TV-regret upper bound. The TV bound treats all suboptimal mass uniformly, weighing each by V_max; UCB's gap-aware analysis weighs each by Δ(a). The Lipschitz envelope `Δ_min · TV ≤ R(Q) ≤ V_max · TV` makes this gap visible: the upper bound (which the framework uses) is loose by V_max/Δ_min relative to the *true* regret on UCB-style action-gap-aware policies.

**Honest accounting:** ~90% confident in the `Δ_min²` correction. Standard textbooks (Lattimore-Szepesvári Bandit Algorithms 2020, Ch. 7-8; Bubeck-Cesa-Bianchi 2012; Auer-Cesa-Bianchi-Fischer 2002 Theorem 1) all give `E[N_a(T)] = O(log T / Δ_a²)` for UCB1. Integration agent should spot-check against a textbook before applying the diff to `D-algorithm.md`.

## 6. What this spike doesn't try to do

- **Trajectory-level (UCRL2/UCBVI) analog.** Focused on bandit case (N_h = 1). For UCRL2/UCBVI in piecewise-stationary MDPs, the lifted rate `Õ(N_h² √(SA(B_T+1)T))` at `D-algorithm.md:7` should be checked similarly (and against H3, the V_max double-count). The (A1) resolution carries over directly.
- **Off-policy / RLHF / GRPO settings** where Q_t is genuinely stochastic — (A1) holds pointwise without controversy.
- **ε-smoothed UCB analyses** — viable but adds complexity without payoff over completion state 1.
- **IDS treatment** — already noted as future work at `D-algorithm.md:7`; unaffected by H5.

## 7. Summary recommendation

1. **Adopt completion state 1.** Reformulate (A1) as the extended-real convention `e^{-∞} = 0`, making it vacuous (or remove it entirely). Vanilla deterministic UCB / UCBVI / UCRL2 are then in scope without modification.
2. **Apply the four small diffs in §4** to `04-main-result.md:28`, `05-mechanism.md:7-10`, `A-proof-of-composition.md:9`, and `B-key-lemma-proofs.md:7`.
3. **Side finding:** the bandit-case rate constant at `D-algorithm.md:7` should be `O(log² T / Δ_min²)` from the V_max·TV chain. The framework's TV-side bound is structurally one Δ_min looser than direct gap-aware UCB analysis.
4. **No restriction of the example list at `D-algorithm.md:5-7` is needed.** Vanilla deterministic UCB joins Thompson sampling on the example list.
5. **Outcome status: SUCCEEDED beyond the existing claim.** Per AGENTS §3.1 — strengthening attempt confirmed the brief's first completion state.

---

## Working notes (preserved alongside conclusion)

**Initial mental model that almost led astray.** First read of the brief, my instinct was to take completion state 2 (redefine Q_t internally) seriously because "internal sampling distribution for UCRL2" sounded like a familiar pattern from the optimism literature. Actually walking through the proof of (i) revealed completion state 1 working cleanly enough that state 2 was unnecessary. Exactly the §3.1 pattern: the obvious move (add machinery) felt productive in planning; the harder, less-obvious move (look at whether the proof actually uses the assumption) gave the better outcome.

**Where I doubted the strengthening would land.** Around halfway through reading the proof of (i), I expected to find a step where pointwise positivity was tightening a `log Q(a*)` term that wouldn't reduce in the extended-real sense. The proof at `A-proof-of-composition.md:9-11` has no such step — it goes through the TV-regret-bound chain and substitutes the identity. The substitution is robust to `K_t = +∞`.

**Where I almost folded in the bandit rate constant as part of H5.** The Δ_min vs. Δ_min² issue surfaced while checking whether the bandit-case sharpening claim "matches" the literature for vanilla UCB. It's separate from (A1) — even with (A1) as stated, the rate would be off by one Δ_min — but it lives in the same paragraphs, so I noticed it while doing the (A1) audit.

**On the "two-point support" condition (Lemma 4).** Spent some time wondering whether (A1)'s pointwise positivity might be related to Lemma 4's two-point q_0 condition. They're not the same:

| Condition | Where | What | Required |
|---|---|---|---|
| (A1) pointwise | `04-main-result.md:28` | Q(a*) > 0 | Identity at finite K_t (not actually needed) |
| Lemma 4 two-point | `05-mechanism.md:52` | Q(a*_{ag}) ≥ q_0 and Q(tilde-a*) ≥ q_0 | Bias bound (1-p_id) log(1/q_0) |
| Perturbative full-support | `B-key-lemma-proofs.md:27` | Q(a) ≥ q_0 for all a | ε-greedy and softmax extensions |

H5 addresses (A1) only. Lemma 4 and the perturbative extension's conditions have their own well-defined operational scope.

**Note on framing for the paper.** The strengthening from completion state 1 is small in surface area (a few line edits) but has a meaningful framing payoff: the paper currently reads as if (A1) is a load-bearing assumption restricting the scope to randomized-Q learners; after the strengthening, the framework is unconditional in the bandit case and Dirac-deploying base learners (UCB, UCBVI, UCRL2) are first-class citizens. This addresses the implicit reviewer concern "why use this framework when I'm running vanilla UCB?" — the answer is "you can; (A1) is a notation extension, not a real restriction."

**A word on Δ_min² vs. Δ_min and the strengthening principle.** The Δ_min² correction is a *softening* of the headline rate, not a strengthening. Per AGENTS §3.1, I tried to find a strengthening — a way to recover `O(log² T / Δ_min)` rate via the TV chain. I could not. The `V_max · TV` upper bound is structurally looser than gap-aware analysis by one Δ_min; this is not tightenable, it's a fundamental feature of upper-bounding `Σ Q(a) Δ(a)` by `V_max · Σ Q(a)`. Honest characterization: the framework gives the structural / unifying analysis but doesn't out-tight direct UCB analysis on the rate constant. Fine framing — the paper's contribution is unification + persistence + per-round identity, not a sharper bandit constant.
