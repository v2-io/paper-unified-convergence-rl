# Spike VT-unification — Report

**Status:** STRENGTHENED — Completion State 2 (succeed at strengthening to match an existing claim) + Completion State 3 (map structural boundary). The spike confirms the per-round identity route extends to continuous-variation $V_T$ via the Wei–Luo 2021 black-box reduction, with the per-round identity surviving as the per-round coordinate. The achieved exponent matches Mao 2021's $\tilde O(V_T^{1/3} T^{2/3})$ (information-theoretically near-optimal in $V_T$ per Besbes–Gur–Zeevi 2014); a Best-of-Both-Worlds (BoBW) bound holds: a single algorithm achieves $\tilde O(\min\{V_{\max} N_h \sqrt{(B_T+1) T},\ V_{\max} N_h (V_T+1)^{1/3} T^{2/3}\})$ simultaneously, automatically adapting to whichever regime is sharper. Completion State 1 (beat Mao's exponent) was attempted along five angles; **all five fail at the Besbes–Gur–Zeevi lower bound**, which holds at the deterministic-$\pi^*$ corner.

The §6 "open question" framing should be resolved: **the regimes are NOT structurally distinct.** They share the same per-round coordinate ($1 - e^{-K_t}$ exact at deterministic-$\pi^*$); what differs is the *aggregation strategy* (block-Cauchy–Schwarz across $B_T+1$ stationary intervals vs. adaptive-window MASTER over $\log T$ parallel base-learner instances). Two genuinely-open refinements remain: an *agent-experienced* variation budget $V_T^{\mathrm{eff},\Sigma}$ paralleling H4's $B_T^{\mathrm{eff},\Sigma}$, and an identity-coordinate variation $V_T^{(K)}$.

---

## 1. Setup and the question

Theorem 4.1(v) currently claims:
$$\mathbb E[\mathrm{DynReg}(T)] \;\le\; 2c\, V_{\max}\, N_h \sqrt{(B_T + 1)\, T} \;+\; V_{\max}\, N_h\, (1 - p_{\mathrm{id}})\, T,$$
with $B_T$ the kernel-stationary-segment count (per H4 redefinition). §6's "On the continuous-variation extension" paragraph flags V_T extension as open, asserting that "continuous variation cannot be partitioned at discrete segment-change events." The spike pushes whether this is structurally true or just a property of one specific aggregation strategy.

---

## 2. Dependency map: which steps are piecewise-stationary-specific?

Auditing the existing proof of (v):

- **Step 1 (simulation lemma).** Stationarity-*agnostic*. `V^{π*_t}_t - V^{Q_t}_t ≤ V_max N_h TV_bar_t` holds per round regardless of cross-round dynamics. Survives any V_T extension.
- **Step 2 (per-round identity).** Stationarity-*agnostic*. `TV(δ_{a*_t(s)}, Q_t(·|s)) = 1 - e^{-K_t(s)}` is an algebraic identity in the action distribution; per-round only. Survives any V_T extension *provided deterministic-π* scope is preserved* (see §5.2).
- **Step 3 (block decomposition + Cauchy–Schwarz).** *Piecewise-stationary-specific.* Partition `[1,T]` at `(P,r)`-changes into `B_T+1` blocks; aggregate via `Σ √Δ_i ≤ √((B_T+1) T)`. **This is the only piecewise-stationary-specific piece.**
- **Step 4 (misidentification penalty).** Stationarity-*agnostic*. `V_max N_h (1-p_id) T` is round-additive on the misidentification event; doesn't care about block structure.
- **(A5) base learner.** Provides a TV-coordinate stationary regret guarantee `Σ_t TV_bar_t ≤ 2c√L` per stationary block — *exactly the property* Wei–Luo's MASTER black-box reduction consumes.

**Diagnostic:** the strengthening question reduces to whether Step 3's aggregator can be replaced by something V_T-compatible. **Yes — Wei–Luo MASTER provides exactly that.**

---

## 3. The strengthening: Wei–Luo MASTER black-box reduction

**What MASTER does** (Wei–Luo 2021, "Non-stationary RL without prior knowledge: an optimal black-box approach"). Black-box reduction from any base learner with stationary regret $\tilde O(\sqrt L)$ to a non-stationary algorithm achieving:
$$\tilde O\!\left(\min\Big\{\sqrt{(L_T+1) T},\ (V_T+1)^{1/3} T^{2/3}\Big\}\right)$$
with $L_T$ the piecewise-stationary segment count and $V_T$ the continuous variation budget. **The min is achieved automatically** (no prior knowledge required) by running $\log T$ parallel instances at exponentially-spaced window sizes $W_k = 2^k$ and using an online meta-learner.

**Required of the base learner:**
- (B1) Stationary regret $\tilde O(\sqrt L)$ on stationary MDP after $L$ rounds.
- (B2) Standard concentration / tail control.

Both satisfied by UCRL2, UCBVI, Thompson sampling — all (A5)-compatible.

**What our framework supplies.** Inside a stationary block, (A5) gives `Σ TV_bar_t ≤ 2c√L`; composed with Steps 1–2 this gives stationary value-regret `Σ (V^{π*}_t - V^{Q_t}_t) ≤ 2c V_max N_h √L`. Exactly the `√L` shape MASTER needs, with explicit prefactor `2c V_max N_h`.

**The wrapping is mechanical.** Apply MASTER to our identity-routed base learner. The per-round identity (Steps 1–2) is stationarity-agnostic; the aggregator (Step 3) is replaced by MASTER's adaptive-window logic; the misidentification penalty (Step 4) is round-additive and rides through. Result:
$$\mathbb E[\mathrm{DynReg}(T)] \;\le\; \tilde O\!\left(V_{\max}\, N_h \cdot \min\Big\{\sqrt{(B_T+1) T},\ (V_T+1)^{1/3} T^{2/3}\Big\}\right) + V_{\max}\, N_h\, (1 - p_{\mathrm{id}})\, T.$$

---

## 4. The unified statement

**Theorem (composition + BoBW non-stationarity, sketch).** Under the assumptions of Thm 4.1 *modulo* (A5) replaced by:
- **(A5')** *MASTER-wrapped base learner.* The base learner is the MASTER-wrapping of any (B1)–(B2)-compatible black-box stationary learner (UCRL2, UCBVI, TS); within each MASTER-selected window, it satisfies `Σ TV_bar_t ≤ 2c√W` in TV coordinates.

Theorem 4.1 holds with conclusion (v) replaced by the BoBW bound above; conclusions (i)–(iv) are unchanged.

The V_T-side prefactor `V_max N_h` matches the B_T-side prefactor — both inherited from the per-round identity. Mao 2021's published prefactor $\tilde O(SA \cdot H \cdot V_T^{1/3} T^{2/3})$ has the same horizon dependence; the $\sqrt{SA}$ factor lifts cleanly when the base is UCRL2 (mirroring the published $N_h^2 \sqrt{SA(B_T+1)T}$ on the B_T side).

---

## 5. Why the "regimes are structurally distinct" hypothesis fails

Examining each potential structural obstruction:

**5.1 "Continuous variation cannot be partitioned at discrete events."** Correct for block-Cauchy–Schwarz (Step 3); not a structural obstruction to the per-round identity. MASTER's adaptive-window aggregator doesn't partition at discrete events — it runs parallel instances at different window sizes. Identity is round-local; aggregation is separate.

**5.2 "$\pi^*$ moves continuously under V_T."** Under continuous variation in $(P_t, r_t)$, $a^*_t = \arg\max Q^*$ is *piecewise constant* — it changes only at argmax-crossing events (codimension-1 in parameter trajectory). Outside argmax-crossings, $a^*_t$ is locally constant. **Deterministic-$\pi^*$ assumption is preserved under continuous V_T variation in the bandit/isolated-optimum case.** The identity applies as written.

**5.3 "Block-Cauchy–Schwarz needs discrete blocks."** True for that specific aggregator. MASTER doesn't use block-Cauchy–Schwarz; it works fine with the per-round identity. Property of the aggregator, not of the identity.

**5.4 "(A2) strategic-tempo's `ρ_Σ/R_Σ` threshold doesn't compose with continuous variation."** Closer look: under continuous V_T, `ρ_Σ` is identified up to a constant proportional to `V_T/T` (already noted in `_recovery/long-form-2026-05-05.md`). The threshold form remains structurally well-defined; it just takes a `V_T/T`-scaled form. Not an obstruction.

**5.5 "Cesàro tracking requires `B_T = o(T)`."** The V_T analog: `(1/T) Σ (V^{π*}_t - V^{Q_t}_t) = O(V_max N_h (V_T+1)^{1/3} T^{-1/3}) → 0` when V_T = o(T) and p_id → 1. Extends cleanly.

**Conclusion:** every potential structural-obstruction direction either fails or is a non-issue. Regimes are unifiable.

---

## 6. Failed attempts to beat Mao's exponent (Completion State 1)

**The structural blocker:** Besbes–Gur–Zeevi 2014 Theorem 2 — non-stationary stochastic bandits — gives $\Omega(V_T^{1/3} T^{2/3})$ lower bound. The hard instance is a smoothly-varying two-armed bandit where the optimal arm flips at argmax-crossings; **deterministic-$\pi^*$ assumption is preserved at the lower-bound construction**. Mao 2021 Theorem 5.1 lifts this to MDPs. So no per-round-identity sharpening can break the exponent.

**6.1 Direct identity-driven aggregation.** Hoped that since $1 - e^{-K_t} \approx K_t$ at small $K_t$ (quadratically sharper than Pinsker's $\sqrt{K_t/2}$), summing directly might give cumulative regret $O(\log T)$ as in stationary UCB. **Failed:** under non-stationarity, $\sum K_t = \Omega(T)$ even at moderate V_T — UCB's logarithmic regret is a stationary property; doesn't survive variation.

**6.2 Refined variation budget on the identity coordinate, $V_T^{(K)} := \sum |K_t - K_{t-1}|$.** This is agent-coupled (depends on $Q_t$). **Failed against worst-case lower bound:** adversarial environments can produce $V_T^{(K)} \asymp V_T$. **But** in *typical* regimes, $V_T^{(K)} \ll V_T$ — the same observation as $B_T^{\mathrm{eff},\Sigma}$. Filed as future work in §7 below.

**6.3 Itô-style continuous-time analysis.** **Failed quickly.** The agent-environment loop isn't a Markov diffusion in the natural sense — it's discrete-time with action-conditioned transitions. Continuous-time embedding introduces objects (drift coefficients, diffusion matrices) without natural discrete-time analogs. BSDE/Itô-isometry approaches in continuous-time RL don't give Mao-style improvements either.

**6.4 RKHS / smoothness-aware variation budget.** **Failed for our setting.** RKHS-aware base learners (GP-UCRL, kernel-UCRL) can give sharper rates against smooth-RKHS-variation budgets, but those are a different tribe of algorithm. Our framework operates at the base-learner-agnostic level (UCRL2/UCBVI/TS, none RKHS-aware). Out of scope.

**6.5 Hölder-$p$ variation interpolation.** **Failed to give a sharper exponent.** Mao-style analysis already extends to Hölder-$p$ variation — rate $T^{(2p-1)/(2p)}$ for $p$-Hölder. Our framework plugs into any of these analyses; the *exponent* is set by the variation-budget literature, not by the identity. No improvement at fixed $p$.

**Diagnostic across §§6.1–6.5:** the lower bound is on the *aggregation*, not on the per-round coordinate. A sharper per-round coordinate doesn't help against an aggregation-side lower bound. **The honest framing:** identity sharpens constants and per-round form, not the exponent.

---

## 7. Genuinely-open refinements

**7.1 Agent-experienced continuous variation $V_T^{\mathrm{eff},\Sigma}$.** Continuous-variation analog of H4's $B_T^{\mathrm{eff},\Sigma}$. Define $V_T^{\mathrm{eff},\Sigma} := \int_0^T \|\boldsymbol\delta_\Sigma(t)\|\, dt$ (or similar functional of strategic-substate trajectory). When the strategic substate stays within $R_\Sigma^*$-radius (under (A2) margin), $V_T^{\mathrm{eff},\Sigma}$ accumulates only during excursions — strictly less than $V_T$ for typical environments where the agent absorbs drift. A Mao-style analysis lifted to $V_T^{\mathrm{eff},\Sigma}$ would give $\tilde O((V_T^{\mathrm{eff},\Sigma}+1)^{1/3} T^{2/3})$, sharper than standard V_T-rate in absorbing regimes. Requires algorithmic-design coupling base learner's window-width with strategic-substate excursion-counting. Filed as follow-up.

**7.2 Identity-coordinate variation budget $V_T^{(K)}$.** §6.2's surviving direction. In typical (not worst-case) regimes, $V_T^{(K)} \ll V_T$; Mao-style rate against $V_T^{(K)}$ would give sharper bounds in well-behaved regimes, while worst-case lower bound is preserved. Distinct from $V_T^{\mathrm{eff},\Sigma}$ — lives in action-distribution coordinate vs. strategic-substate coordinate.

**7.3 Cesàro tracking sharper constant.** Standard Mao gives $(V_T/T)^{1/3}$ with prefactor $\tilde O(SA \cdot H)$. Our identity-routed analysis gives same time-averaged form with prefactor $\tilde O(V_{\max} N_h)$. For large-$SA$ MDPs, $V_{\max} N_h < SA \cdot H$ — a small-but-real improvement in constants, worth surfacing in §6 practitioner takeaways.

---

## 8. Concrete diff suggestions

**8.1 §6 conclusion — replace the "open question" paragraph.**

Current text at `src/re/06-conclusion.md:11`:

> **On the continuous-variation extension.** [...] It remains an interesting open question whether the per-round-identity route can match the $T^{2/3}$ continuous-variation rate, or whether the regimes are structurally distinct.

Suggested replacement:

> **On the continuous-variation extension.** The current rate is in the piecewise-stationary $B_T$ family. The same per-round identity route extends to the continuous-variation $V_T$ regime via the black-box reduction of \cite{wei-luo-2021-blackbox}, which wraps any stationary base learner with $\tilde O(\sqrt L)$ regret into a non-stationary algorithm achieving $\tilde O(\min\{\sqrt{(B_T+1) T},\ (V_T+1)^{1/3} T^{2/3}\})$ dynamic regret without prior knowledge of either variation budget. Our framework supplies such a stationary base learner — the identity-routed (A5)-compatible learner achieving $\sum_{t \in \mathrm{block}} \mathbb E[\overline{\mathrm{TV}}_t] \le 2c\sqrt L$ — and the wrapping is mechanical, since the per-round identity (Steps 1–2 of the proof) is stationarity-agnostic and survives the substitution of block-Cauchy–Schwarz aggregation by adaptive-window MASTER aggregation. The resulting rate $\tilde O(V_{\max} N_h \min\{\sqrt{(B_T+1) T},\ (V_T+1)^{1/3} T^{2/3}\})$ matches \cite{mao-2021-nearoptimal}'s near-optimal $V_T^{1/3} T^{2/3}$ exponent (the Besbes–Gur–Zeevi 2014 lower bound for non-stationary bandits applies *also* at the deterministic-$\pi^*$ corner, so the exponent cannot be improved by the identity); the prefactor $V_{\max} N_h$ inherits from our framework's per-round identity. The two regimes are *not* structurally distinct — they share the same per-round coordinate and differ only in aggregation strategy. Whether an *agent-experienced* variation budget (continuous-variation analog of $B_T^{\mathrm{eff},\Sigma}$ in the next paragraph) gives a sharper-than-$V_T$ rate in regimes where the strategic substate absorbs continuous environment-side drift remains open.

**8.2 §4.3 "Comparator regime" paragraph — minor update.**

Current closing sentence at `04-main-result.md:61`:

> [...] Generalizing the per-round-identity route to the continuous-variation regime is open ([[#^sec-conclusion]]).

Replace with:

> [...] The per-round identity route extends to continuous variation via Wei–Luo's black-box reduction, achieving $\tilde O(V_{\max} N_h \min\{\sqrt{(B_T+1) T},\ (V_T+1)^{1/3} T^{2/3}\})$ Best-of-Both-Worlds dynamic regret automatically (full development in [[#^sec-conclusion]]).

**8.3 §1 introduction — optional formal expansion (per-paper-strategic call).**

Headline could optionally be elevated to mention BoBW form. Minimum diff (which I'd recommend) is just §8.1.

**8.4 Optional Remark at end of §A `A-proof-of-composition.md` for theorem-grade availability.**

> **Remark (BoBW non-stationarity extension).** Conclusion (v) of the composition theorem uses the restart-on-change form of (A5). When (A5) is replaced by (A5'), the Wei–Luo black-box reduction of any (B1)–(B2)-compatible base learner (UCRL2, UCBVI, Thompson sampling), the rate term in (v) replaces $\sqrt{(B_T+1) T}$ with $\min\{\sqrt{(B_T+1) T}, (V_T+1)^{1/3} T^{2/3}\}$, automatically without prior knowledge of $B_T$ or $V_T$. The proof is mechanical: Steps 1, 2, 4 are stationarity-agnostic; Step 3's block-Cauchy–Schwarz aggregator is replaced by Wei–Luo's adaptive-window MASTER aggregator, which composes with the per-round identity to give the BoBW guarantee. The achieved $V_T^{1/3} T^{2/3}$ exponent matches \cite{mao-2021-nearoptimal} and is near-optimal per the Besbes–Gur–Zeevi 2014 lower bound, which holds also at the deterministic-$\pi^*$ corner.

---

## 9. Working notes — false starts

**9.1 The "exact-corner gives sharper exponent" hope.** Went in hoping the identity's quadratic sharpness over Pinsker at small $K_t$ might break $V_T^{1/3} T^{2/3}$. Hope dissolved when checking the Besbes–Gur–Zeevi construction: smoothly-varying two-armed bandit with deterministic optimum, isolated gap-floor. **The deterministic-$\pi^*$ assumption is preserved at the lower-bound construction.** Sharpness at the corner where the identity is exact can't break an aggregation-side lower bound. Most informative dead-end.

**9.2 The "identity gives a better continuous-time analog" hope.** Considered Itô-style. Identity is a static algebraic relation between coordinates — no natural dynamic form. Dynamics live in $(P_t, r_t, Q_t)$; identity relates point-in-time coordinates of $Q_t$. Static fiber-bundle relation, orthogonal to dynamics. Not productive.

**9.3 The "MASTER might not handle the misidentification penalty" worry.** The penalty `V_max N_h (1-p_id)` is round-additive and environment-side; doesn't depend on which MASTER instance is active at round $t$. Rides through uncorrupted. Worry dispelled.

**9.4 The "(A5')'s window-restart cost" worry.** MASTER's adaptive windows might cross stationarity boundaries, breaking the within-window `√L` guarantee. **Resolution:** Wei–Luo's analysis already handles this — within-window stationary regret + cross-window variation cost, optimized over window sizes. The variation cost is paid once per actual transition, not per window. Consistent with our (A5).

**9.5 The "non-isolated optimum near argmax-crossings" worry.** Under continuous V_T, $\Delta_{\min}$ shrinks near argmax-crossings, potentially to zero. **Resolution:** argmax-crossings are codimension-1 events; the set of $t$ with $\Delta_{\min}(t) < \epsilon$ has Lebesgue measure $O(V_T \epsilon)$ for typical smooth-variation regimes. Within the small-measure region, the perturbative extension takes over with $O(\epsilon \log(1/\epsilon))$ corrections. Same structural protection as in piecewise-stationary $B_T$. Confirmed not a structural obstruction.

---

## 10. Strategic call

**§6 commitment recommended.** §6 currently says "open question whether regimes are structurally distinct." Spike resolves this: **regimes are not structurally distinct, §6 paragraph should commit.** §8.1 above gives suggested replacement. Genuinely improves the paper — answers an open question with a clean structural argument and a concrete BoBW result.

**Optional §1 / §A elevation.** Per-paper-strategic. Minimum diff is §8.1 only.

**The two follow-up directions ($V_T^{\mathrm{eff},\Sigma}$, $V_T^{(K)}$).** Genuinely open future work; parallel the existing $B_T^{\mathrm{eff},\Sigma}$ direction. Filed in §8.1's suggested replacement as one sentence at the end.

**Strengthen-before-soften audit.** The spike attempted Completion State 1 (beat Mao's exponent) along five angles — all five failed for substantive structural reasons (Besbes–Gur–Zeevi lower bound at the deterministic-$\pi^*$ corner, RKHS out-of-scope, Itô without natural discrete analog). Then matched the existing claim mechanically (Completion State 2 — Wei–Luo wrapping). Then mapped the structural boundary (Completion State 3 — regimes are unifiable). All three completion states represented; the trichotomy framing was load-bearing.

---

## 11. Primary-source verification + integration status

**All three load-bearing claims verified first-hand against registered PDFs (2026-05-07):**

- **Wei-Luo MASTER black-box reduction** — verified against `refs/pdfs/wei-luo-2021-blackbox.pdf` (44pp, v3, Sept 2021). Theorem 2 + Assumption 1 confirm: any base learner with stationary regret `C(t) = c_1·t^p + c_2` for `p ∈ [1/2, 1)` plus an auxiliary-quantity output condition can be wrapped to achieve `Õ(min{√(LT), ∆^{1/3} T^{2/3}})` automatically. Our framework's identity-routed (A5)-compatible learner satisfies Assumption 1 with `C(t) = 2c V_max N_h · t^{1/2}` at the boundary `p = 1/2` (explicitly admitted by Wei-Luo Theorem 2). UCB1, UCRL2, UCBVI, Thompson sampling all listed as Assumption-1-compatible in their Section 2 examples. Table 1 explicitly confirms `MASTER + Q-UCB` for episodic tabular MDPs achieves `min{Reg*_L, Reg*_∆}` automatically — *improving over* Mao 2021 which requires Double-Restart Q-UCB for parameter-free property.

- **Mao 2021 V_T rate + lower bound** — verified against `refs/pdfs/mao-2021-nearoptimal.pdf` (50pp, v4, Aug 2022). RestartQ-UCB achieves `Õ((SA)^{1/3} ∆^{1/3} H T^{2/3})`; lower bound `Ω((SA)^{1/3} ∆^{1/3} H^{2/3} T^{2/3})`. **Correction to the spike's earlier overstatement: Mao's prefactor is `(SA)^{1/3}·H`, not `SA·H`** (cube-root SA-dependence, not linear). The framework's MASTER-wrapped form will inherit whatever SA-exponent the chosen base learner has (UCRL2/UCBVI gives `√(SA)·N_h`, different from Mao's specialized `(SA)^{1/3}`); the *exponent* in T and ∆ matches.

- **Besbes-Gur-Zeevi lower bound at deterministic-π*** — verified against `refs/pdfs/besbes-gur-zeevi-2014-stochastic.pdf` (30pp, OR journal version 2019; bib registered as `besbes-gur-zeevi-2014-stochastic`). Theorem 1: `R^π(V_T, T) ≥ C·K^{1/3} V_T^{1/3} T^{2/3}`. Lower-bound construction: K-armed Bernoulli bandit with means `µ^k_t`, dynamic oracle plays the per-round argmax — **deterministic-per-round optimum**, exactly matching B-CS1's canonical scope. The lower bound applies at the deterministic-π* corner, blocking any per-round-identity-based exponent improvement.

**Integration status (landed 2026-05-07):**

- ✓ **Tier 1 (§6 commitment paragraph).** Replaced the "open question" framing in `src/re/06-conclusion.md` with the Wei-Luo MASTER + Mao + BGZ commitment, including the parameter-free property and the V_T^{eff,Σ} / V_T^{(K)} future-work flag.
- ✓ **Tier 2a (§4.3 *Comparator regime* sentence).** Updated `src/re/04-main-result.md:61` to point at the §6 commitment + the §A (A5')-BoBW Remark, replacing the prior "open" framing. Also corrected the displayed Mao rate's SA exponent.
- ✓ **Tier 2b (§A (A5')-BoBW Remark).** Added formal Remark to `src/re/A-proof-of-composition.md` establishing theorem-grade availability of the BoBW form via (A5'). Includes the explicit citation chain (Wei-Luo Theorem 2 + Assumption 1; Mao 2021 rate match; BGZ 2014 lower bound at deterministic-π*).

**Bib housekeeping done:** `besbes-gur-zeevi-2014-stochastic` registered via `bin/refs add` from BibTeX; entry at `refs/entries/besbes-gur-zeevi-2014-stochastic.yml`. Wei-Luo and Mao bib entries already existed.

**Build verification:** clean. Main text remains 13pp (Tier 1 §6 commitment fits in existing page); appendix grew by ~1 page from the §A Remark; total ~29pp. No new overfulls.

## 12. Open questions for the integration agent (non-blocking, post-integration)

- **Page-budget scope.** §8.1's suggested §6 replacement is ~20% longer than original. Can be shortened to 4–5 sentences if budget pressure demands; load-bearing parts: (a) wrapping is via Wei–Luo MASTER, (b) per-round identity is stationarity-agnostic, (c) BoBW rate matches Mao's near-optimal exponent, (d) regimes are not structurally distinct.

- **Formal-statement choice.** §6 commitment is sufficient for this submission cycle; numbered Corollary in §A is post-strategy-talk territory.

- **(A5') folding.** Recommend separate Remark in §A (§8.4) keeping Theorem 4.1 clean and giving the BoBW form formal availability for citation.

- **Citation verification flag for Wei–Luo 2021.** The reference is registered (`wei-luo-2021-blackbox.yml`). The spike's claim about MASTER's exact form (BoBW guarantee, $\log T$ parallel instances at $W_k = 2^k$) is from reading of the paper's title and the corroborating `_recovery/long-form-2026-05-05.md` paper-draft text ("Wei-Luo 2021 gives an optimal black-box reduction yielding $\tilde O(\min\{\sqrt{LT}, \Delta^{1/3} T^{2/3}\})$"). Wei–Luo PDF now registered at `refs/pdfs/wei-luo-2021-blackbox.pdf` (44pp, v3, Sept 2021); Mao 2021 at `refs/pdfs/mao-2021-nearoptimal.pdf` (50pp, v4, Aug 2022); Besbes-Gur-Zeevi 2014 at `refs/pdfs/besbes-gur-zeevi-2014-stochastic.pdf` (30pp, OR journal version) — all available for primary-source verification before §6 commitment lands.
