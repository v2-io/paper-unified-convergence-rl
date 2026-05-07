# M2 — `lem-loop-level2` proof misuses (C2) — spike report

**Outcome (one line up front).** **CRACKED with structural payoff.** Opus's sketch is correct as far as it goes — applying (C2) to the *prior on `U`* in the truncated factorization gives the right ~5-line proof. But pushing harder reveals more: (a) the (C1)–(C3) decomposition is genuinely over-decomposed for the *identification* claim the lemma makes — only (C2) is load-bearing; (C1) reduces to a trivial realized-action positivity that holds by construction, and (C3) is *not used at all* in the identification step (it loads only for *estimation* in §F's bias bound, which uses agent-policy-queryability, not behavior-policy-knownness); (b) (C2) itself is stated in the wrong form — `a_t ⊥ U | H_t` is *one* equivalent of sequential ignorability, but the equivalent the proof actually needs is `o_{t+1} ⊥ a_t | H_t` in the mutilated graph $G_{\overline{a_t}}$ (or equivalently the potential-outcome form $a_t \perp Y^{(a_t)} | H_t$); (c) under the paper's singular-trajectory + agent-as-policy architecture, "known action mechanism" is *automatic*, not an imposed condition.

The §1 / §4 framing of "interventional under sequential ignorability" (Codex M1 / Opus A8 concern) lands cleaner under this restructure: the headline assumption can become a *single* named structural condition rather than a three-part bundle in which two parts do no work in the identification proof.

---

## 1. The corrected proof — three equivalent forms

Three equivalent formulations of "no unobserved confounding given $H_t$." Each gives a clean ~3–5 line proof. Differences are notational/conceptual, not mathematical.

### Form A — Truncated factorization (Opus's sketch, expanded)

Let $\mathbf{U}$ denote the variables not in $H_t \cup \{a_t\}$ that influence $o_{t+1}$. By the truncated-factorization formula (Pearl 2009, Theorem 3.4.1):

$$P(o_{t+1} \mid \mathrm{do}(a_t), H_t) = \sum_{\mathbf{U}} P(o_{t+1} \mid a_t, \mathbf{U}, H_t) \cdot P(\mathbf{U} \mid H_t),$$

— intervention deletes the $H_t \to a_t$ edge, so $\mathbf{U}$'s distribution is conditioned on $H_t$ alone. By ordinary marginalization,

$$P(o_{t+1} \mid a_t, H_t) = \sum_{\mathbf{U}} P(o_{t+1} \mid a_t, \mathbf{U}, H_t) \cdot P(\mathbf{U} \mid a_t, H_t).$$

These coincide iff $P(\mathbf{U} \mid a_t, H_t) = P(\mathbf{U} \mid H_t)$ — i.e., iff $a_t \perp \mathbf{U} \mid H_t$. This is (C2)'s sequential-ignorability content applied to the *prior* on $\mathbf{U}$ in the do-marginalization, *not* to the conditional on $o_{t+1}$. ∎

The misstep in the current proof is the *direction* of the ⊥ application: it tries to push the conditional independence through $P(o_{t+1} \mid a_t, H_t, \mathbf{U})$ — which would require a different (and stronger) independence; the correct move pushes through $P(\mathbf{U} \mid a_t, H_t)$, which is *exactly* the definitional content of $a_t \perp \mathbf{U} \mid H_t$.

### Form B — Pearl's Rule 2 of do-calculus (cleanest)

Pearl 2009, Theorem 3.4.1, Rule 2 — *action/observation exchange*:

> $P(y \mid \mathrm{do}(x), z) = P(y \mid x, z)$ if $(Y \perp X \mid Z)_{G_{\overline{X}}}$,

where $G_{\overline{X}}$ is $G$ with all incoming arrows to $X$ removed. Applied here: $P(o_{t+1} \mid \mathrm{do}(a_t), H_t) = P(o_{t+1} \mid a_t, H_t)$ if $(o_{t+1} \perp a_t \mid H_t)_{G_{\overline{a_t}}}$. In the singular-trajectory + agent-as-policy graph, the only arrows into $a_t$ come from $H_t$; deleting them severs $a_t$ from any back-door path through unobserved confounders. Whether $H_t$ d-separates $a_t$ from $o_{t+1}$ in $G_{\overline{a_t}}$ is exactly what "no unobserved confounding given $H_t$" asserts.

This form is shorter, more directly tied to the cited Pearl 2009 reference, and avoids the latent-variable-marginalization machinery that the current proof gets tangled in.

### Form C — Potential-outcome form (Robins/Murphy)

Sequential ignorability in Robins/Murphy: $a_t \perp Y^{(a_t)} \mid H_t$ where $Y^{(a_t)}$ is the potential outcome under intervention $a_t$. Under SUTVA + consistency,

$$P(o_{t+1} \mid \mathrm{do}(a_t), H_t) = P(Y^{(a_t)} \mid H_t) = P(Y^{(a_t)} \mid a_t, H_t) = P(o_{t+1} \mid a_t, H_t). \quad \square$$

### Recommendation

**Use Form B as the primary proof, with one-sentence pointers to Forms A and C.** Form B is shortest, most directly tied to the cited Pearl 2009 reference, and avoids the latent-variable-marginalization machinery. The current proof line at `B-key-lemma-proofs.md:90` already invokes "Pearl's $\mathrm{do}$-calculus"; switching to Rule 2 directly is a *contraction*, not an expansion.

---

## 2. The (C1)–(C3) decomposition is over-decomposed for *identification*

This is the structural finding most valuable beyond Opus's sketch.

### (C1) Positivity — only realized-action positivity is needed

Current (C1): $\pi_t(a \mid H_t) \ge p_{\min} > 0$ on $H_t$'s support *for the actions of interest* — a full-support condition.

For the identity $P(o_{t+1} \mid \mathrm{do}(a_t), H_t) = P(o_{t+1} \mid a_t, H_t)$, only positivity *at the realized action $a_t$* is needed, and that is **automatic**: $a_t$ is drawn from $\pi_t(\cdot \mid H_t)$, so $\pi_t(a_t \mid H_t) > 0$ tautologically.

Full-support positivity loads in two places, *neither* of which is the loop-Level-2 lemma:
- *Off-policy / counterfactual identification* (estimating $P(o \mid \mathrm{do}(a))$ for $a$ the agent didn't take). The lemma claims on-policy access at the realized $a_t$ only.
- *§F bias bound* — but that uses a *two-point* $Q_t \ge q_0$ at $\{a^*_{\mathrm{ag}}, \tilde a^*\}$, a distinct condition from full-support behavior-policy positivity.

So (C1)'s full-support positivity is *not load-bearing* for the loop-Level-2 identity.

### (C2) Sequential ignorability — load-bearing, but stated in the wrong form

Current statement: "$H_t$ blocks unobserved confounding of the action mechanism, so $a_t \perp\!\!\!\perp (\text{environment latents}) \mid H_t$."

The form $a_t \perp \mathbf{U} \mid H_t$ is *one* equivalent of sequential ignorability — but the wrong handle for the proof. The proof tries to use it as if it gave $o_{t+1} \perp \mathbf{U} \mid (a_t, H_t)$, a different (and stronger) condition.

**Recommended restatement of (C2):** "*No unobserved confounding given $H_t$* — i.e., $H_t$ d-separates $a_t$ from $o_{t+1}$ in the mutilated graph $G_{\overline{a_t}}$ (equivalently, $a_t \perp Y^{(a_t)} \mid H_t$ in potential-outcome notation)."

Names the condition in graph-theoretic terms (what the proof actually uses); parenthetically gives the potential-outcome equivalent (what causal-inference reviewers recognize as "sequential ignorability"). The misleading form $a_t \perp \mathbf{U} \mid H_t$ is dropped or relegated to a third-place equivalent.

### (C3) Known action mechanism — *not used in the identification step*

Current statement: "the behavior policy $\pi_t(a \mid H_t)$ is known."

This is **not used** in the identity proof. The identity is a structural claim: under (C2), $P(o_{t+1} \mid a_t, H_t) = P(o_{t+1} \mid \mathrm{do}(a_t), H_t)$, regardless of whether we know $\pi_t$.

Where (C3) actually loads:
- *Off-policy estimation* (IPW reweighting needs $\pi_t$). Not what the lemma claims.
- *§F bias bound.* The agent's per-state $K_t(s) = -\log Q_t(a^*_{\mathrm{ag},t}(s) \mid s)$ is computed by querying $Q_t$ — the *agent's* policy. **Automatic** when the agent IS the policy (the paper's framing); architectural, not a substantive imposed condition.

A possible reading: (C3) is stated *for emphasis* — to flag that the framework distinguishes "agent's own policy" from "behavior policy." But within the paper's architecture that distinction collapses. (C3) reduces to "the agent has internal access to $Q_t$" — definitional.

### The substantive condition: $H_t$-sufficiency

The substantive condition is single. Call it **$H_t$-sufficiency**: $H_t$ d-separates $a_t$ from $o_{t+1}$ in $G_{\overline{a_t}}$, equivalently $a_t \perp Y^{(a_t)} \mid H_t$. Under $H_t$-sufficiency alone (plus realized-action positivity, which is automatic): $P(o_{t+1} \mid \mathrm{do}(a_t), H_t) = P(o_{t+1} \mid a_t, H_t)$.

(C1) and (C3) become *remarks* documenting where realized-action positivity and agent-policy-queryability come from — automatic in the paper's architecture, but worth flagging.

### Why this matters beyond the proof — Codex M1 / Opus A8

Headline framing at `01-introduction.md:33` and `04-main-result.md:21` ("interventional by construction" / "interventional under (C1)–(C3)") was flagged for eliding the C2 caveat. Under this restructure, the cleanest headline becomes:

> "interventional under $H_t$-sufficiency" (or, equivalently, "interventional under sequential ignorability")

— a *single named structural condition*, not a three-part bundle. The §1 / §4 prose then says: "(C1) reduces to realized-action positivity (automatic) and (C3) reduces to agent-policy-queryability (architectural); the substantive condition is (C2) $H_t$-sufficiency." This *strengthens* the framing rather than softening it: instead of "we impose three conditions" (which a skeptical reviewer reads as "three things that could fail"), it becomes "we impose one structural condition; two ancillary requirements are automatic in our architecture."

---

## 3. The case against collapsing — why one might preserve (C1)–(C3)

**Argument 1: Estimation vs. identification distinction.** The lemma makes an *identification* claim; downstream uses (e.g., §F bias bound) make *estimation* claims. (C1) full-support and (C3) known-policy are load-bearing for *estimation* — just not for the identity in this lemma. If the framework wants to make off-policy / counterfactual claims later, (C1) and (C3) need to be available as named conditions.

*Counter-counter:* Framework as currently stated does *not* make off-policy claims. §F's two-point $q_0$ is a *separate, weaker* condition than (C1)'s full-support. Demoting (C1)/(C3) to remarks for the loop-Level-2 lemma doesn't preclude reintroducing them as named conditions if a future paper makes off-policy claims.

**Argument 2: Reviewer expectations.** Causal-inference reviewers expect (C1) positivity, (C2) sequential ignorability, (C3) known propensities/consistency as a standard package. Deviation might trigger pushback.

*Counter-counter:* Standard list comes from the *estimation* setting (where IPW / G-computation needs all three). For the identification claim only (C2) is load-bearing; a careful reviewer will see this. Naming exactly to what conditions do is more honest.

**Argument 3: §F bias bound's $q_0$ condition.** Isn't this the same as (C1)?

*Counter-counter:* (C1) is on the *behavior policy* (propensity $\pi_t$); §F's $q_0$ is on the *agent's internal policy* $Q_t$ at the agent's identified optimum and the true optimum. In the architecture they collapse — but the *form* differs (full-support on action set vs. two-point on argmax candidates). Naming both as "(C1) positivity" risks readers conflating them.

**Honest assessment.** Argument 1 is the strongest counter. Per-paper agent may want to keep (C1)–(C3) named for forward-compatibility. The conservative move:
- Keep (C1)–(C3) as named conditions.
- *Add a parenthetical remark* in lemma statement and §B proof: "(C1) and (C3) are automatic under singular-trajectory + agent-as-policy architecture; the substantive condition for identification is (C2)."
- Restate (C2) using d-separation / potential-outcome form (Form B/C).
- Fix the proof per Form B.

This is the most conservative version of the strengthening — preserves the named-conditions list (forward-compatible, reviewer-expected) while making clear what's load-bearing.

The maximally-strong version (collapse to single condition) is cleaner but requires cascading edits across §1 introduction, §4 main result, §6 conclusion, the algorithm appendix, and the §F bias bound prose. The conservative version is a one-segment fix.

---

## 4. What to actually edit

### 4.1 `src/re/B-key-lemma-proofs.md:86-92` — proof rewrite (~6 lines)

**Current (broken):** see source.

**Proposed (Form B):**
```
By Pearl's do-calculus (Theorem 3.4.1, Rule 2 — action/observation exchange):
  P(o_{t+1} | do(a_t), H_t) = P(o_{t+1} | a_t, H_t)   if (o_{t+1} ⊥ a_t | H_t)_{G_{\overline{a_t}}},
where G_{\overline{a_t}} is the graph G with all incoming arrows to a_t removed. In the singular-trajectory + agent-as-policy graph of [[#^sec-preliminaries]], the only arrows into a_t come from H_t (the agent's policy is π_t(a_t | H_t)); deleting them severs the only confounding path from a_t to o_{t+1}. Whether H_t d-separates a_t from o_{t+1} in G_{\overline{a_t}} is precisely the content of (C2). Thus P(o_{t+1} | do(a_t), H_t) = P(o_{t+1} | a_t, H_t) on the support where π_t(a_t | H_t) > 0 (automatic since a_t was drawn from π_t).
```

Roughly the same length; the proof is now correct.

Optional ~2-line addition for Robins/Murphy-trained readers:
```
*Remark on equivalent forms.* The d-separation condition is equivalent to the potential-outcome form a_t ⊥ Y^{(a_t)} | H_t (sequential ignorability in the Robins/Murphy sense), and to the truncated-factorization condition P(U | a_t, H_t) = P(U | H_t) when U are the latent variables jointly affecting a_t and o_{t+1}.
```

### 4.2 `src/re/05-mechanism.md:35-47` — lemma statement restatement (~1-2 lines)

**Current (C2):** `(C2) *sequential ignorability* — H_t blocks unobserved confounding of the action mechanism, so a_t ⊥ (environment latents) | H_t;`

**Proposed:** `(C2) *sequential ignorability* — H_t d-separates a_t from o_{t+1} in the mutilated graph G_{\overline{a_t}} (equivalently, a_t ⊥ Y^{(a_t)} | H_t in potential-outcome notation; or P(U | a_t, H_t) = P(U | H_t) for the latent confounders U);`

### 4.3 `src/re/04-main-result.md:21` — Component 4 prose

Current wording at line 21 is *gentler than* §5/§B versions and doesn't make the directional misclaim. Could optionally tighten; probably not worth the edit.

### 4.4 `src/re/01-introduction.md:33` — headline framing (Codex M1 / Opus A8)

Already filed in TODO as M1. Two restatements possible:
- **Conservative (recommended):** "interventional under (C1)–(C3) of [[#^sec-key-lemma-3]] (sequential ignorability + ancillary positivity / known-policy conditions)."
- **Aggressive:** "interventional under H_t-sufficiency (a single structural condition; see [[#^sec-key-lemma-3]])."

Aggressive requires the §3 maximally-strong restructuring + cascade. Conservative pairs naturally with §4.1 / §4.2 above.

### 4.5 `src/re/06-conclusion.md:15` — coupled-goal architectures remark

Add concrete statement of why coupled-goal violates (C2) — ~4 lines.

---

## 5. Failed strengthening attempts (record per AGENTS §3.1)

### Attempt 1: Make (C2) automatic from singular-trajectory — FAILED

Spent time trying to argue §3 *singular-trajectory* automatically gives (C2). It doesn't. Singular-trajectory says we have a single non-forkable causal trajectory — a metaphysical commitment about the *world's* causal structure. (C2) constrains the *agent's information state architecture*. They're orthogonal.

Mildly disappointing — would have been beautiful if §3 alone gave Component 4. But the architecture-side condition does real work; naming it explicitly is more useful than hiding it under a metaphysical commitment.

### Attempt 2: Eliminate (C2) by augmenting H_t — VACUOUS

If H_t is defined as "all variables that affect a_t and o_{t+1}," then (C2) is vacuous by definition. But then H_t includes latent variables and is unobservable; the agent can't condition on it. Lemma becomes a tautology that doesn't apply to any actual agent. The right framing keeps H_t as the *agent's accessible information state* and treats (C2) as a constraint on the agent's architecture.

### Attempt 3: Generalize beyond Pearl's hierarchy to causal-DAG-free framings — EQUIVALENT, NOT STRONGER

Considered whether the lemma generalizes to decision-theoretic regime-indicator framings (Robins/Dawid). In that framing, "intervention" is a regime indicator F = f; the analog of (C2) is a_t ⊥ F | H_t. The proof goes through similarly. So the lemma is robust to the framing choice.

### Attempt 4: Tighten via explicit identification of U as exogenous noise — TECHNICALLY CORRECT, ADDS UNNEEDED MACHINERY

Tried to argue U should be the exogenous noise terms of the NPSEM. Technically correct but adds machinery the paper doesn't need. The d-separation form (Form B) doesn't require naming U at all. Don't bother.

### Attempt 5: Search for a stronger conclusion (off-policy on the same data) — POSSIBLE BUT BEYOND PAPER'S CLAIMS

Under (C1)+(C2)+(C3), one *can* extend the lemma's claim to off-policy identification of P(o_{t+1} | do(a)) for arbitrary a ∈ A — and *all three* conditions become load-bearing. But the paper's framework uses per-state K_t(s) at the agent's *identified* optimum on the data the agent generated. Going off-policy would require infrastructure (IPW estimators, off-policy bias bounds) the paper doesn't present. Technically possible; beyond what the paper claims; don't bother.

---

## 6. Summary: what to do

**Tier 1 — required (proof is wrong as written):**
- Edit §B central calculation per §4.1 (~6 lines, replacing broken proof with Pearl Rule 2 form).
- Edit §5 lemma-statement (C2) wording per §4.2 (~1-2 line restatement using d-separation / potential-outcome equivalent).

**Tier 2 — strongly recommended (compounds the fix):**
- Edit §6 coupled-goal-architectures remark per §4.5 (concrete statement of why coupled-goal violates (C2), ~4 lines).
- TODO M1 fix at §1 / §4 (already filed): "interventional by construction" → either "under (C1)–(C3)" (conservative) or "under H_t-sufficiency" (aggressive).

**Tier 3 — strategic-call territory:**
- Decide aggressive (collapse to single H_t-sufficiency) vs. conservative (keep three conditions with parenthetical "ancillary; substantive content is (C2)" remark) restructuring. Recommendation: conservative for this submission cycle.

**Tier 4 — optional (length-budget permitting):**
- Add Robins/Murphy + Pearl-track equivalence remark per §4.1's optional addition (~2 lines). Defuses both reviewer tracks. Skip if page-pressed.

The lemma's conclusion stands. The structural finding — that the (C1)–(C3) decomposition is over-decomposed and only (C2) is load-bearing for the identification claim, with (C2) itself stated in a misleading form — is the value-add beyond Opus's sketch. The Codex M1 / Opus A8 §1/§4 framing concern lands cleaner: a *single* substantive condition is more honest than a three-part bundle in which two parts do no work in the lemma's central calculation.

---

## 7. Questions for owner / strategic call

- **Aggressive vs. conservative restructuring.** Worth a brief discussion: collapse (C1)–(C3) into single H_t-sufficiency, or keep three-part with parenthetical "(C1) and (C3) automatic"? Aggressive is cleaner; conservative is faster. Lean conservative for this cycle.
- **Robins/Murphy equivalence remark.** Add the parenthetical or skip? Two lines vs. defusing one reviewer track.
- **Bib database.** §4.1 optional remark would cite Robins 1986; Murphy 2003 and Dawid 2000 may need `bin/refs add` before any final build. Verify before committing.
