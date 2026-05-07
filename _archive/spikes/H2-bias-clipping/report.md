# H2 — Bias-term value-scale conversion + clipping spike report

**Status: CRACKED — strengthening succeeds beyond the parent's anticipated `min(1, log(1/q₀))` clip. The `log(1/q_0)` factor should not appear in conclusion (v) at all; the corrected bias term is `V_max · N_h · (1 - p_id) · T`.**

The parent context expected the strengthening would land at `V_max · N_h · (1 - p_id) · min(1, log(1/q_0)) · T` — a clipped form that tightens for q_0 < 1/e. The actual answer is structurally sharper: the `log(1/q_0)` comes from routing the misidentification penalty through Lemma 4's KL-coordinate bound, but **Lemma 4 bounds a different quantity** (the agent's KL self-readout bias) than what (v) needs (the value-coordinate misidentification penalty). A direct value-side derivation via the simulation lemma gives `V_max(1 - p_id)` per state-step with no log factor and no clip needed.

The honest framing: the current proof in §A Step 4 has a *category error*, not just a units error. The fix is the simpler value-side derivation; Lemma 4 stays put as the right tool for conclusion (iii) (KL-readout computability for the diagnostic).

---

## 1. The structural diagnosis

### 1.1 What conclusion (v) needs

Conclusion (v) of Theorem 4.1 bounds the trajectory-level dynamic regret:

`E[DynReg(T)] = E[ Σ_t (V^{tilde-π*}_t - V^{Q_t}_t) ]`

where `tilde-π* = δ_{tilde-a*}` is the **true** optimum (a property of the MDP) and `Q_t` is the agent's policy. The agent does *not* know `tilde-a*_t` in general; it has access only to its identified `a*_{ag,t}`.

The natural decomposition (per-round, suppressing h and t):

`V^{tilde-π*}(s_0) - V^Q(s_0) = [V^{tilde-π*}(s_0) - V^{π*_ag}(s_0)] + [V^{π*_ag}(s_0) - V^Q(s_0)]`

where `π*_ag(·|s) := δ_{a*_ag(s)}`.

The second term is what (A5)'s base-learner regret guarantee delivers (after applying the simulation lemma + Key Lemma 1 identity at the agent's identified `a*_ag`). It chains through the `√((B_T+1)T)` block-Cauchy–Schwarz argument and gives the rate term `2c V_max N_h √((B_T+1) T)`.

The first term — the **misidentification penalty** — is what step 4 of the rate combination is trying to bound. This is what gets the `N_h(1-p_id) log(1/q_0) T` in the current (v).

### 1.2 What Lemma 4 actually bounds

Lemma 4 at `B-key-lemma-proofs.md:94-102` bounds:

`|D̂_t(s) - D^true_t(s)| ≤ 1[misid] · log(1/q_0)`

where `D̂_t(s) = -log Q_t(a*_ag(s) | s)` and `D^true_t(s) = -log Q_t(tilde-a*(s) | s)`.

This is a difference between **two KL self-readouts of the same Q_t** evaluated at two different argmax candidates. It quantifies how much the agent's KL-coordinate readout would change if the agent learned that its identified optimum is wrong. It is **not** a bound on the value gap between `tilde-π*` and `π*_ag`.

Concretely: `D̂_t` and `D^true_t` differ because `Q_t(a*_ag)` and `Q_t(tilde-a*)` may differ. The `log(1/q_0)` ceiling is the **range of log Q_t** on the support condition — a property of the agent's policy distribution, not of the value function.

### 1.3 The category error

Step 4 of the §A composition proof at `A-proof-of-composition.md:45` writes:

> *Step 4: Bias term via per-state, per-horizon-step lift of Key Lemma 4.* … the per-round bias contribution to the trajectory-level value gap is at most `N_h (1 - p_id) log(1/q_0)`.

The phrase "the per-round bias contribution to the trajectory-level value gap is at most" equates a *KL-coordinate* bound with a *value-coordinate* contribution. The two have different units. The currently-stated bias term inherits this units mismatch: `log(1/q_0)` in KL-coordinates becomes a value-coordinate quantity by fiat, with neither a `V_max` multiplier nor a `1-e^{-D}` map.

The parent context's framing of this as a "units mismatch fixable by adding V_max + a clip" is partial. It would be a units mismatch *if* the misidentification penalty in value coordinates genuinely needed to be routed through the KL coordinate. But it doesn't — there's a direct value-side derivation that bypasses Lemma 4 entirely.

---

## 2. The direct value-side derivation

### 2.1 Simulation lemma applied to the misidentification penalty

For any two policies π, π' on a finite-horizon non-stationary MDP, the performance-difference / simulation lemma gives

`V^{π'}_t(s_0) - V^π_t(s_0) = Σ_{h=0}^{N_h-1} E_{s_h ~ d^π_h(·|s_0)} [ < π'(·|s_h) - π(·|s_h), Q^{π'}_h(s_h, ·) > ]`

Apply with `π' = tilde-π*_t = δ_{tilde-a*_t}` and `π = π*_{ag,t} = δ_{a*_ag,t}`. Both are point-mass, so the per-step bracket becomes

`< δ_{tilde-a*(s_h)} - δ_{a*_ag(s_h)}, Q^{tilde-π*}_h(s_h, ·) > = Q^{tilde-π*}_h(s_h, tilde-a*(s_h)) - Q^{tilde-π*}_h(s_h, a*_ag(s_h))`

This is a Q-value difference at fixed state s_h between two actions. By boundedness of Q over the trajectory horizon (`Q^{tilde-π*}_h ∈ [0, V_max]` where V_max is the §3-defined cumulative-Q range):

`0 ≤ Q^{tilde-π*}_h(s_h, tilde-a*) - Q^{tilde-π*}_h(s_h, a*_ag) ≤ V_max · 1[tilde-a*(s_h) ≠ a*_ag(s_h)]`

The lower bound (zero) is by optimality of `tilde-a*`. The upper bound is by the Q-range with the indicator capturing the misidentification event.

Taking expectations:

`E[Q^{tilde-π*}_h(s_h, tilde-a*) - Q^{tilde-π*}_h(s_h, a*_ag)] ≤ V_max · (1 - p_id(s_h))`

Aggregating over the N_h horizon steps (under the floor `p_id := min_s p_id(s)`):

`E[V^{tilde-π*_t}_t(s_0) - V^{π*_{ag,t}}_t(s_0)] ≤ V_max · N_h · (1 - p_id)`

Summing over T rounds:

`Σ_{t=1}^T E[V^{tilde-π*_t}_t - V^{π*_{ag,t}}_t] ≤ V_max · N_h · (1 - p_id) · T`

**No log(1/q_0). No clip. No reference to Lemma 4.** The factor V_max multiplies the expected misidentification fraction directly because the value gap on the misidentification event is bounded by the Q-range, period.

### 2.2 The corrected (v)

`E[DynReg(T)] ≤ 2c · V_max · N_h · √((B_T+1) T) + V_max · N_h · (1 - p_id) · T`

Both terms are now in value coordinates, with V_max as the leading constant, and the bias term is strictly tighter than both:
- the currently-stated `N_h(1-p_id) log(1/q_0) · T` (units error);
- the parent-anticipated `V_max N_h (1-p_id) min(1, log(1/q_0)) · T` (overcounted via Lemma 4 detour).

### 2.3 Why the parent's clip was the wrong target

The parent expected the strengthening would route through the 1-Lipschitz `1-e^{-D}` map applied to Lemma 4's KL bound, then multiply by V_max and clip at V_max. This bounds the **TV difference between two readout proxies of Q_t**, *not* the value gap between `tilde-π*` and `π*_ag`. The two are unrelated upper bounds on different quantities living on different scales.

**The TV-based proxy.** `TV(δ_a, δ_b) = 1[a ≠ b]` for any two actions a, b. The TV between two point masses is a pure indicator — *no log(1/q_0) involved*. The KL bound in Lemma 4 cannot be the right vehicle to convert to a TV between point masses, because **the TV between two point masses is already 0 or 1**, regardless of the underlying Q_t.

**The fundamental confusion in the parent's framing** was treating Lemma 4's `|D̂ - D^true|` as if it were a bound on the KL between `δ_{a*_ag}` and `δ_{tilde-a*}`. It is not. Lemma 4 instead bounds the difference of *two reverse-KLs of Q_t* against two distinct reference point masses — a different object.

---

## 3. The four-place patch

### 3.1 `src/re/05-mechanism.md:70` — Step 4 of rate combination

Replace the Lemma-4-routed framing with the value-side simulation-lemma framing.

### 3.2 `src/re/04-main-result.md:48-49` — Theorem 4.1(v) statement

Replace `N_h(1 - p_id) log(1/q_0) · T` with `V_max · N_h · (1 - p_id) · T`.

### 3.3 `src/re/A-proof-of-composition.md:45-49` — Step 4 + Combining

Replace Step 4 with the value-side simulation-lemma argument. Pure indicator decomposition on the per-step bracket; no Lemma 4 reference.

### 3.4 `src/re/04-main-result.md:63` — §4.3 unpacking paragraph

Update to describe the bias term as the value-coordinate misidentification penalty, bounded by `V_max` on the misidentification event of probability `1 - p_id`.

(Lemma 4 stays in the paper unchanged; only its role in (v) is reassigned. Conclusion (iii) at `04-main-result.md:44` still cites Lemma 4 correctly for the KL-readout bound — that's its native object.)

---

## 4. Sanity checks

### 4.1 Saturation regime check

In Regime C (p_id → 0), the new bias term saturates at `V_max N_h · T` — the trivial value envelope. The currently-stated `N_h log(1/q_0) · T` form, by contrast, can *exceed* the trivial envelope `V_max N_h T` when `log(1/q_0) > V_max` (e.g., `q_0 < e^{-V_max}`) — non-trivially **vacuous beyond the trivial bound**. The fix removes this pathology.

### 4.2 Independence of q_0

The new bound has no dependence on q_0. This is honest: the support condition `Q_t(·|s) ≥ q_0` at the two argmax candidates is needed for Lemma 4's *KL-readout* bound (conclusion (iii)) — because `D̂_t` and `D^true_t` are KL coordinates that diverge if q_0 → 0. The *value-side* misidentification penalty doesn't need q_0 at all.

This actually **relaxes** the assumption set for (v): the support condition `Q_t ≥ q_0` at the two candidates is no longer needed for the rate term itself; only conclusion (iii) (KL-coordinate computability) requires it.

### 4.3 Connection to UCRL2/UCBVI lifting

The lifted UCRL2/UCBVI rate `Õ(N_h^2 √(SA(B_T+1)T))` at `04-main-result.md:65` and `06-conclusion.md:9` carries through unchanged — the rate term is the only part that lifts; the bias term is a separate T-linear contribution.

### 4.4 Interaction with H3 (V_max convention)

H3 flags that the §3 cumulative-Q convention for V_max creates a horizon-double-count when multiplied by N_h in the simulation-lemma step. Under H3 fix (a) — redefining V_max as per-step value range (≤ 1 here) — the new bias term reads `V_max · N_h · (1-p_id) · T` with V_max ≤ 1 per step and N_h as the horizon factor, exactly matching the rate term's structure.

**Recommendation: H3 fix (a) is the cleaner global convention**, and under it the bias term's `V_max · N_h` prefactor reads transparently. H2 fix and H3 fix (a) should land in the same integration pass for consistency.

---

## 5. Failed strengthening attempts (record per AGENTS §3.1 / §5.4)

### 5.1 Sharpening via small-gap-correlated misidentification

**Hypothesis.** Misidentification events tend to occur on small-action-gap states/actions. If true, `E[V_misid | misid] ≤ c · Δ_min` — bias term sharpens to `c Δ_min N_h (1-p_id) T`, much sharper for `Δ_min ≪ V_max`.

**Why it doesn't land cleanly.** The hypothesis is plausible for stochastic-bandit base learners (UCB: `P[misid for a] ≤ 2 exp(-2 n_t Δ(a)^2)`, so misidentification mass concentrates on small-gap arms). But it requires a per-base-learner argument; the current (A5) abstracts the base learner via `E[\overline{TV}_t] ≤ 2c√L`, which doesn't expose this structure. To get the Δ_min improvement, one would add an (A5ʹ) misidentification-quality assumption.

**Status:** Documented; deferred. Worth a parenthetical in §5 step 4 noting that under additional bandit-style misidentification structure, V_max in the bias term sharpens to `c' Δ_min`. Not the primary fix.

### 5.2 Sublinear aggregation via time-varying p_id

**Hypothesis.** As data accumulates, `p_id,t → 1`; the bias becomes sublinear in T.

**Why it doesn't land cleanly.** (1) Current (A3) takes p_id as a fixed regime parameter. (2) (A5)'s block-restart structure resets the base learner at each block boundary, so cross-block decay washes out within-block improvement. The piecewise-stationary B_T framing is structurally incompatible without restructuring (A3) and (A5).

**Status:** Out of scope. Future work — ties to the bandit-case logarithmic-rate sharpening at `05-mechanism.md:74`.

### 5.3 Replacing the 1-e^{-D} Lipschitz route with a forward-direction conversion

**Hypothesis.** A different KL→value conversion (e.g., Jensen on a convex transform) might give a non-trivial bound on the misidentification penalty in value coordinates.

**Why it doesn't land.** Any KL-coordinate bound on `|D̂ - D^true|` bounds a difference of log-Q values at distinct argmax candidates — it cannot be a bound on the value gap between `tilde-π*` and `π*_ag` without further structural assumptions linking the agent's KL self-readout to the value function. The two are independent quantities.

**Status:** Confirms §1.3 — Lemma 4 is structurally the wrong vehicle. The direct value-side derivation in §2 is the only clean path.

---

## 6. Optional sharpening: Δ_max instead of V_max

A small additional refinement: the per-step bound uses V_max as the Q-range, but a tighter form is

`Q^{tilde-π*}_h(s_h, tilde-a*) - Q^{tilde-π*}_h(s_h, a*_ag) ≤ max_{a ≠ tilde-a*} Δ(a; s_h) =: Δ_max(s_h)`

the worst non-optimal action gap at s_h. Under a worst-state floor `Δ_max := max_s Δ_max(s)`, the bias is `Δ_max · N_h · (1-p_id) · T` — strictly tighter when `Δ_max < V_max`. Headline form keeps V_max for parity with the rate term's V_max prefactor.

---

## 7. Summary

**Strengthening status: CRACKED.** The right correction is

`E[DynReg(T)] ≤ 2c · V_max · N_h · √((B_T+1) · T) + V_max · N_h · (1-p_id) · T`

derived directly from a value-side simulation-lemma argument. **Strictly tighter** than:

- the currently-stated `N_h(1-p_id) log(1/q_0) · T` form (units error + can exceed trivial value envelope for small q_0);
- the parent's anticipated `V_max N_h (1-p_id) min(1, log(1/q_0)) · T` form (still routed through Lemma 4 unnecessarily; carries spurious log factor for q_0 ∈ (e^{-1}, 1)).

**Also:** the support condition `Q_t ≥ q_0` at the two argmax candidates is no longer needed for the rate term. Only conclusion (iii) (KL-readout computability) requires it. Rate (v) is provable under a strictly weaker assumption set.

**Diff scope:** Four locations. About 30 lines of replacement total.

**Lemma 4 stays.** Its native role is conclusion (iii)'s KL-coordinate readout bound. The §A composition proof's Step 4 was reaching for the wrong tool; the right tool is a trivial value-side indicator decomposition.

**Cross-spike interaction:** H3 (V_max convention) interacts cleanly — under H3 fix (a) (per-step convention), the bias term's `V_max · N_h` prefactor mirrors the rate term's `V_max · N_h · √((B_T+1) T)` in horizon-aggregation structure.

**Pattern observation (per AGENTS §3.1).** Codex's H2 finding originally came in as a softening recommendation ("the bound could exceed the trivial value range"). The parent context's strengthening framing pulled it back to a clip — itself a real strengthening. The actual answer is a third, sharper read: the conversion route was structurally the wrong vehicle, and a direct derivation gives a tighter bound *with no clip needed at all*. Three reads of "what does this finding mean": (1) softening, (2) clipped strengthening, (3) structural-fix strengthening. Each successive read finds more value. The AGENTS §3.1 instinct ("attempt the improbable / hardest thing first") delivered the final read.
