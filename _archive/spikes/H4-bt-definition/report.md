# Spike H4 — `B_T` as optimum-change count vs. kernel-stationary-segment count

**Status:** STRENGTHENING-FAILED (negative-with-payoff). The two notions are genuinely different, and on close inspection the *optimum-change-count* framing is wrong as the block-counter for the proof of Theorem 4.1(v). **Recommendation:** redefine `B_T` as the count of kernel-stationary-segment boundaries, keep the optimum-change-count interpretation as a corollary in the bandit / isolated-optimum / argmax-crossing-shift special case, and rewrite the §1 / §3 / §4 / §5 / §6 / §A passages accordingly. The strengthening attempt did surface one structurally interesting absorption argument that points to a *third* counter `B_T^{eff,Σ}` worth flagging as future work in §6.

---

## 0. Summary up front

The paper writes `B_T := |{t : a*_t ≠ a*_{t-1}}|` (preliminaries §3 line 7) and partitions [1, T] at *optimum-change* events (mechanism §5 line 66; appendix §A lines 5, 33–43), asserting "the MDP is stationary within each block." The two clauses do not compose:

1. **MDPs can change without changing a\*.** Reward shifts that lift all actions uniformly, transition-kernel shifts that move probability mass between non-optimal-action descendants, and value-function changes that preserve the argmax — all are real non-stationarities that contribute to within-block variation but do not register as `optimum-change` events.

2. **The base learner's `√L` guarantee in (A5) is a stationary-block guarantee in `(P, r)` space, not in `a*` space.** UCRL2 / UCBVI / Thompson-sampling build empirical confidence sets on `(P, r)` and the optimistic value depends on the sets being valid for a fixed `(P, r)` over the block. If `(P, r)` drifts mid-block, the running statistics are biased toward the pre-shift regime; the per-block bound `Σ_{t∈block} E[\overline{TV}_t] ≤ 2c√L` no longer holds and within-block regret can be `Ω(L)` until enough samples accumulate post-shift to overwhelm the running mean.

3. **(A2)'s strategic-tempo dynamics do not absorb this.** Component 3 governs the *agent's* mismatch state `δ_Σ ∈ ℝ^{|E|}` via Model (S); the disturbance `w` in Model (S) is a *budget* on the rate at which the agent's structured causal model is being perturbed. (A2) bounds `‖δ_Σ‖`. But the base learner in (A5) does not consult `δ_Σ` — it consults `(P_t, r_t)` directly via empirical confidence sets. The two layers are decoupled by the paper's "loose-coupling" design (this is a *strength* of the four-component structure, but the price is that (A2) cannot rescue (A5)).

The honest fix: redefine `B_T := |{t : (P_t, r_t) ≠ (P_{t-1}, r_{t-1})}|` (the count of piecewise-stationary segment boundaries). This is the standard counter in [Cheung-Simchi-Levi-Zhu 2020], [Auer-Gajane-Ortner 2019], [Mao 2021]. The headline rate `√((B_T+1) T)` is unchanged; only the *meaning* of B_T shifts. The §2 related-work positioning against piecewise-stationary literature actually lands *more cleanly* under this redefinition.

The optimum-change-count framing survives as a corollary in the bandit / isolated-optimum / argmax-crossing-shift case ("regime ARG"), which is exactly where the existing bandit-case sharpening (`O(log² T / Δ_min)` per-block rate) lives.

---

## 1. The current claim, in detail

- `src/re/03-preliminaries.md:7` — defines B_T as optimum-change count.
- `src/re/04-main-result.md:36` — (A5) asserts both (i) stationarity of (P_t, r_t) per block and (ii) restart-on-change, but doesn't specify *what* the restart fires on.
- `src/re/05-mechanism.md:66` — block decomposition reads the partition events as optimum-changes.
- `src/re/A-proof-of-composition.md:5,33-43` — the notation paragraph and Step 3 of the (v)-proof partition [1, T] at optimum-change events and apply Cauchy–Schwarz to `Σ_i √Δ_i ≤ √((B_T+1) T)`.

The Cauchy–Schwarz argument is correct *under any block-decomposition where the base learner's per-block √L guarantee holds*. What's wrong is the definition of B_T as `|{t : a*_t ≠ a*_{t-1}}|` — it partitions at the wrong events for the base learner's stationary-block guarantee to apply per block.

---

## 2. Concrete counterexamples — (P, r)-changes invisible to a*

### Counterexample A (cleanest — bandit, uniform reward shift)

Bandit, |A| = 2, action gap Δ = 0.1. Rewards `r_1(a*) = 0.6, r_1(a) = 0.5` for t ∈ [1, T/2], then `r_2(a*) = 0.9, r_2(a) = 0.8` for t ∈ [T/2 + 1, T]. Argmax a* is unchanged. `B_T^{a*-change} = 0`.

The mean rewards have shifted by 0.3 for *both* arms. UCB's running mean for a* halfway through the second segment is biased toward the pre-shift regime; the optimistic upper bound *falls below* the true value at the start of segment 2, breaking optimism. Within-block regret can be Ω(L).

This is the canonical reason Cheung-Simchi-Levi-Zhu introduced restart-on-change: they restart on (P, r)-change because uniform reward shifts (visible to the base learner via empirical means) need a restart even though the optimum is unchanged.

### Counterexample B (kernel-only shift, N_h = 2)

|S| = 2, |A| = 2, N_h = 2. From s_0, a* deterministically transitions to s_1 (reward 1); a transitions to s_0 with probability 1 - β_t, to s_1 with probability β_t. For t ∈ [1, T/2], β_1 = 0.1; for t ∈ [T/2 + 1, T], β_2 = 0.5. Argmax unchanged. `B_T^{a*-change} = 0`. But the kernel P has shifted; UCBVI's confidence set on (P, r) is miscalibrated.

The simulation lemma also breaks: `V^{π*}_t(s_0) - V^{Q_t}_t(s_0) ≤ V_max · N_h · \overline{TV}_t` uses *current* V_max(M_t), but V_max and the occupancy distribution `d^{Q_t}_h` are both kernel-dependent and shift mid-block.

### Counterexample C (sub-arm reward shift with argmax preserved)

|A| = 3. `r_1(a*) = 1, r_1(a_2) = 0.5, r_1(a_3) = 0.0` then a swap of the suboptimal arms: `r_2(a*) = 1, r_2(a_2) = 0.0, r_2(a_3) = 0.5`. Across the shift: a* stays optimal, Δ_min = 0.5 unchanged, V_max = 1 unchanged. But the identity of the second-best arm has swapped. A base learner that has accumulated "a_2 is better than a_3" (correctly, in segment 1) now plays a_2 more than a_3 (wrongly, in segment 2). UCB-style algorithms spend Ω(L) rounds re-exploring wrong-direction conclusions.

**Verdict on §2:** the optimum-change-count is provably *not* the right counter for (A5)'s base-learner guarantee. The right counter is the kernel-and-reward-stationary-segment count.

---

## 3. The strengthening attempt — does (A2) absorb within-block drift?

Per AGENTS.md §3.1, attempt the improbable first.

### 3.1 The proposed absorption argument

If (P, r) drifts within a block but a* is fixed, the drift contributes to (a) the strategic disturbance budget ρ_Σ in Model (S) and (b) the base learner's empirical statistics. The strengthening claim: under (A2), Component 3 keeps `‖δ_Σ‖` ultimately bounded, and the base-learner-side miscalibration is silently consumed by the per-element forgetting (1-λ_{ij}). So optimum-change is the right counter.

### 3.2 Why it fails — layer decoupling

Model (S) governs `δ_Σ ∈ ℝ^{|E|}` (vector of causal-model edge mismatches in the agent's representation). The base learner of (A5) is a *separate* algorithmic object (UCRL2, UCBVI, TS) maintaining empirical confidence sets on (P, r). These confidence sets are not parameterized by δ_Σ. Whether `‖δ_Σ‖` is bounded says nothing about whether the confidence sets have been recentered post-shift.

The paper's narrative about "loose coupling" between (A2)-strategic-tempo and (A5)-base-learner is structurally important: it lets the base learner be plug-and-play and lets the strategic substate's persistence be analyzed in its own dynamical-systems framework. But the price is that (A2) cannot rescue (A5).

### 3.3 Five tightening moves attempted, all fail

(i) **Treat δ_Σ as a confidence-set parameter.** Force the base learner's confidence radius to decay with (1-λ). Breaks the plug-and-play interface.

(ii) **Bound the simulation-lemma cost of within-block drift directly.** Adding `N_h · ‖P_t - P_{t-1}‖_TV` per round and summing over a block gives `N_h · V^{block}_kernel` — which is *exactly* the V_T continuous-variation term, i.e., the kernel-change-count framing rebuilt from scratch.

(iii) **Restrict to environments where kernel drift has bounded effect on a*-distinguishing structure.** Circular: this is the claim that environments are essentially a*-stationary. Counterexamples A–C are non-pathological MDPs that violate the restriction.

(iv) **Redefine a* as the argmax under the agent's (P̂_t, r̂_t) estimate.** Makes B_T agent-dependent, collapses the dynamic-regret comparator into a self-comparison.

(v) **Weighted kernel-change count.** Weight each kernel-change by `1[crosses argmax]`, partial credit for invisible changes. Counterexamples A–C show argmax-preserving changes still break the per-block √L guarantee — weights don't help; the base learner still needs the restart.

### 3.4 The one possible recovery — regime ARG

There is exactly one regime where the optimum-change-count framing is correct: **bandit case with isolated optimum and reward-only shifts that always cross argmax** ("regime ARG"). Specifically N_h = 1, every reward shift changes the argmax, Δ_min bounded uniformly below. In regime ARG, every (P, r)-shift is an a*-change and the two counters coincide.

But ARG is a very restrictive assumption. It rules out Counterexample A (uniform shifts), C (sub-arm swaps), and all N_h > 1 shifts of B's type. ARG can be the bandit-case framing, but cannot be the headline framing.

### 3.5 The interesting failed-spike payoff — `B_T^{eff,Σ}`

The strengthening attempt's failure points to a real refinement that's worth flagging.

There are two effective "block counts":
- `B_T^{kernel}` — what the base learner needs for its per-block guarantee (standard piecewise-stationary count).
- `B_T^{eff,Σ}` — the count of times `‖δ_Σ‖` exits its ultimate-bounded region. *Strictly less than* `B_T^{kernel}` whenever per-element forgetting absorbs within-(P, r)-block drift in the strategic substate. Under (A2) with margin, `B_T^{eff,Σ}` can be O(1) even with `B_T^{kernel} = Θ(T^{1/2})`.

This is an *agent-experienced* block count — the count of times the agent's strategic understanding has had to be qualitatively re-organized, regardless of how often the underlying environment shifted.

This is interesting because:
- It connects to a long-standing puzzle: agents in high-V_T slow-drift environments often outperform B_T-pessimistic predictions. The reason is that what matters is the *agent's effective* variation budget, not the environment's.
- It re-grounds (ii) "`‖δ_Σ‖` is ultimately bounded" as a *quantitative* statement about how much non-stationarity the agent absorbs without paying for it in regret.
- Under additional assumptions, the rate could become `√((B_T^{eff,Σ}+1) T)` — strictly better than `√((B_T^{kernel}+1) T)`.

Significant follow-up direction. Not a recovery of the optimum-change-count framing for the current paper.

---

## 4. The fallback — redefine B_T as kernel-change count

### 4.1 New definition

Replace `src/re/03-preliminaries.md:7`:

> A variation budget V_T measures continuous variation in transitions and rewards. We work in the *piecewise-stationary* specialization: time partitions into B_T + 1 maximal stationary intervals on which (P_t, r_t) is constant, with `B_T := |{t : (P_t, r_t) ≠ (P_{t-1}, r_{t-1})}|` counting segment boundaries. Optimum-changes `a*_t ≠ a*_{t-1}` can only occur at segment boundaries (since a* is determined by (P, r)); the converse is not generally true (a segment boundary need not change a*). Our results count segment boundaries, not optimum-changes.

### 4.2 Cascade through the segments

1. `src/re/03-preliminaries.md:7` — main definition swap.
2. `src/re/01-introduction.md:13` — replace "B_T counts optimum-change events" with "B_T counts piecewise-stationary segment boundaries".
3. `src/re/04-main-result.md:36` — (A5) text remains correct.
4. `src/re/04-main-result.md:61` — Comparator regime paragraph correct as-is. Add the optimum-change-count corollary remark below.
5. `src/re/05-mechanism.md:66` — replace "Partition [1, T] at optimum-change events" with "Partition [1, T] at piecewise-stationary segment boundaries."
6. `src/re/A-proof-of-composition.md:5` — update notation paragraph: `B_T = |{t : (P_t, r_t) ≠ (P_{t-1}, r_{t-1})}|`.
7. `src/re/A-proof-of-composition.md:33-43` — proof unchanged; Cauchy–Schwarz step is independent of the definition.
8. `src/re/06-conclusion.md:3` — replace headline phrase as in (2).
9. `src/re/02-related-work.md:5` — text already lands cleanly; no edit needed.

Total: 1 main definition + 4 phrase replacements + 2 new remarks. ~15 lines net.

### 4.3 New remark — optimum-change-count as corollary

Add after `src/re/04-main-result.md:61` (Comparator regime paragraph):

> *Optimum-change count.* When every kernel-change crosses the argmax — equivalently, when every (P, r)-shift is a*-visible — B_T reduces to the optimum-change count `|{t : a*_t ≠ a*_{t-1}}|`. In the bandit case with isolated optimum and uniform action-gap floor Δ_min, this reduction holds whenever reward shifts are above the gap. In general MDPs with N_h > 1, kernel changes that preserve the argmax (e.g., transition-probability shifts among non-optimal-action descendants, value-function-preserving rotations) are not visible at a*; the kernel-change count is the right counter.

### 4.4 Bandit-case sharpening preserved

The "Bandit case sharpening" remark at `src/re/05-mechanism.md:74` and `src/re/A-proof-of-composition.md:53` references the `O(log² T / Δ_min)` per-block rate. Under regime ARG (bandit, isolated optimum, reward shifts cross argmax), the kernel-change count and a*-change count coincide; existing derivation goes through unchanged.

### 4.5 Caveat on restart triggering

The base learner restart needs to fire on kernel-changes, not a*-changes. Operationally this is what restart-on-change-detection algorithms (Cheung et al. 2020 MASTER, Wei-Luo 2021 black-box, gerogiannis-2026 DARLING) already do — they detect *kernel-change* events via concentration on (P, r) estimates, not via optimum-change. So the assumption is consistent with what the field has built.

---

## 5. Why not V_T (the third spike outcome)

The directive offered switching to continuous-variation V_T as a third legitimate option. The right verdict: **stay in B_T-land**.

The paper's current `√((B_T+1) T)` is a `T^{1/2}` piecewise-stationary rate. Mao 2021's `T^{2/3}` continuous-variation rate is a different regime. Switching to V_T would require re-doing the proof's Cauchy–Schwarz argument with adaptive-window analysis (Wei-Luo 2021 style) or restart-detection (DARLING) — V_T cannot be partitioned at discrete events. The paper's contribution structure (composition with a base learner satisfying (A5)) assumes discrete-segment structure.

The §6 conclusion at `src/re/06-conclusion.md:11` already correctly flags V_T as open future work. The H4 fix doesn't change this.

---

## 6. Optional new direction in §6 conclusion — `B_T^{eff}`

Add to `src/re/06-conclusion.md` near line 11:

> *On agent-experienced block count.* The kernel-change count B_T of the rate (v) is an *environment-side* quantity. The strategic-tempo guarantee (ii) suggests an *agent-experienced* block count `B_T^{eff}` — the number of times `‖δ_Σ‖` exits its ultimate-bounded region — could be strictly smaller than B_T in regimes where per-element forgetting absorbs within-segment drift. Whether (v) sharpens to `√((B_T^{eff}+1) T)` requires algorithmic design coupling the base learner's confidence-set widening with the strategic substate, deferred to follow-up. The result would explain why agents in high-V_T slow-drift environments often outperform B_T-pessimistic predictions.

---

## 7. Working notes — five strengthening attempts, all failed

1. **δ_Σ absorbs disturbance directly.** Failed: layer decoupling.
2. **Extend simulation lemma to within-block drift.** Failed: produces `N_h · V^{block}_kernel` — exactly the V_T framing.
3. **Assume kernel only shifts at a*-changes.** Failed: regime ARG only.
4. **Redefine a* as agent's empirical argmax.** Failed: collapses the dynamic-regret comparator.
5. **Weighted kernel-change count.** Failed: argmax-preserving changes still break the per-block √L guarantee.

---

## 8. Strategic call

- **Redefine B_T as kernel-change count.** ~15 lines diff across 8 segments, structurally clean, related-work positioning becomes more consistent with [Cheung 2020], [Auer-Gajane-Ortner 2019], [Mao 2021] who all use kernel-change-count.
- **Optimum-change framing survives as a bandit-special-case corollary** (regime ARG). Honest framing for the bandit-case sharpening, not for the headline.
- **`B_T^{eff,Σ}` direction** worth flagging in §6 future work — the most interesting structural payoff of the failed strengthening attempt.
- **The strengthening-before-soften principle was honored.** Five distinct attempts; each failed for substantive (not effort-based) reasons; failures are documented so they don't get re-attempted without new evidence.

The paper's contribution is not weakened. Headline rate, four-component composition, per-round identity, and strategic-tempo persistence inequality all carry through unchanged. Only the within-B_T framing is corrected.

---

## 9. Open questions for the integration agent (non-blocking)

- Use "maximal stationary segments" phrasing in §3, cleaner than the indicator-of-inequality form for B_T.
- Whether to add a one-line "what B_T is *not*" remark in §3 to head off reviewers who'd otherwise import the optimum-change interpretation. Field-standard one is the kernel-change count, so the bar is low.
- Whether the bandit-case sharpening's regime-of-validity (regime ARG) is worth promoting from a remark to a numbered corollary.
