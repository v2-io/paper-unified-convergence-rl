# De novo audit — 02-unified-convergence-rl

**Date:** 2026-05-06
**Auditor:** Claude (claude-opus-4-7, 1M context)
**Scope:** Math correctness · argument strength / overclaims · prose & structure · loose citation / prior-art notes
**Method:** First-hand read of all main-body and appendix segments via `OUT.unified-rl-neurips-2026.md` manifest order; PDF used for layout / formatting spot-checks. No prior audits read.

Two findings in §1 are real proof-defects (M1 algebra inconsistency in lem-forgetting; M2 misuse of (C2) in lem-loop-level2 proof). Both are repairable without changing the headline statements; the surrounding argument structure stands.

A general note: the *literature-claim hygiene* in this paper is a model. The "to our knowledge / in the retrieved set" framing in `F-prior-art.md`, the explicit naming of (A5) as a base-learner *assumption* rather than a result, the "Theory only. No experiments." caveat in `01-introduction.md:43`, and the "the rate (v) goes through (A5) and (i) alone — the four are needed for the *bundle*" honesty in `04-main-result.md:69` are exactly the kind of moves that prevent a reviewer from feeling tricked. Most of the findings below are about tightening or fixing local details, not about the headline claims.

The point-mass reverse-KL/TV identity is correct (I verified the two-line calculation and the strict-improvement-over-BH argument; the numeric table matches my recomputation) and the framing of its contribution as *recognition where to deploy* rather than *novel mathematics* is honest.

---

## §1 — Math correctness / load-bearing derivations

### M1 — `lem-forgetting` proof has an algebra/units inconsistency between $V$-bound and $R^*$-bound

*Primary location:* `src/re/B-key-lemma-proofs.md:64–72`; affects `src/re/05-mechanism.md:25–27` (lemma statement) and the (A2)/(ii) conclusion of `thm-composition`.

The proof writes the discrete-time Lyapunov drift inequality
$$\mathbb E[\Delta V \mid \boldsymbol\delta_\Sigma] \;\le\; -2\, \mathcal T_\Sigma^{\mathrm{bn,ss}} V \;+\; \rho_\Sigma^2$$
and concludes "iterating gives ultimate boundedness with $V \le R_\Sigma^{*2}$ where $R_\Sigma^{*2} = \rho_\Sigma^2 / \mathcal T_\Sigma^{\mathrm{bn,ss} \cdot 2}$ to leading order, i.e., $R_\Sigma^* = \rho_\Sigma / \mathcal T_\Sigma^{\mathrm{bn,ss}}$."

But the steady state of the displayed inequality is $V^{\mathrm{eq}} \le \rho_\Sigma^2 / (2\, \mathcal T_\Sigma^{\mathrm{bn,ss}})$, which gives $\|\boldsymbol\delta_\Sigma\| \le \rho_\Sigma / \sqrt{2\, \mathcal T_\Sigma^{\mathrm{bn,ss}}}$ — *not* $\rho_\Sigma / \mathcal T_\Sigma^{\mathrm{bn,ss}}$. The two differ by $\sqrt{2 / \mathcal T_\Sigma^{\mathrm{bn,ss}}}$, which isn't a constant: it depends on the threshold parameter itself.

The lemma's claimed $R^* = \rho/\mathcal T$ is the steady-state of the *deterministic worst-case* $\dot\delta = -\alpha\delta + w$ with $|w| \le \rho$ (gives $|\delta^{\mathrm{eq}}| = \rho/\alpha$), not of the mean-square Lyapunov inequality the proof uses. So either:

- the lemma should state mean-square ultimate boundedness $V \le \rho^2/(2\mathcal T)$, in which case the threshold $\mathcal T > \rho/R$ should be replaced by the mean-square form $\mathcal T > \rho^2/(2R^2)$ (this is the form in paper 01's Model S analog, `02-persistence.md:23`); or
- the proof should switch to a deterministic worst-case argument (not via $\mathbb E[\Delta V]$), giving the claimed $R^* = \rho/\mathcal T$ directly.

**Recommended (strengthening, not softening):** rewrite the proof in deterministic worst-case form along the lines of paper 01's `lem-persistence-d` part (ii), which already does this cleanly. Then state Model S separately as a mean-square corollary with the $\sqrt{2}$-different threshold. The lemma's headline statement $\mathcal T_\Sigma^{\mathrm{bn,ss}} > \rho_\Sigma/R_\Sigma$ is recoverable; only the proof needs surgery.

This isn't a fatal bug — the threshold form is correct under deterministic worst-case (which is the natural regime for "structural survival") and the threshold form is also correct (with $\rho^2/(2R^2)$) under mean-square. The current proof mixes the two regimes: starts from the mean-square Lyapunov inequality, ends with the deterministic worst-case formula. Reviewers who follow the algebra will spot this.

### M2 — `lem-loop-level2` proof misuses (C2)

*Primary location:* `src/re/B-key-lemma-proofs.md:86–92`; affects the entire Component 4 / closed-loop interventional access argument.

The proof step writes (where $\mathbf U$ is environment-latent variables):

> Specifically, (C2) gives $a_t \perp\!\!\!\perp (\text{environment latents}) \mid H_t$, so for any subset of environment-latent variables $\mathbf U$,
> $$P(o_{t+1} \mid a_t, H_t, \mathbf U) \;=\; P(o_{t+1} \mid a_t, H_t),$$

This step is wrong. (C2) says $a_t \perp \mathbf U \mid H_t$, which by definition means $P(\mathbf U \mid a_t, H_t) = P(\mathbf U \mid H_t)$. It does *not* directly imply $P(o_{t+1} \mid a_t, H_t, \mathbf U) = P(o_{t+1} \mid a_t, H_t)$ — that would require $o_{t+1} \perp \mathbf U \mid (a_t, H_t)$, which is a different (and stronger) conditional independence.

The correct argument:

$$P(o_{t+1} \mid \mathrm{do}(a_t), H_t) \;=\; \sum_{\mathbf U} P(o_{t+1} \mid a_t, H_t, \mathbf U)\, P(\mathbf U \mid H_t)$$

(by truncated factorization, since under intervention $\mathbf U$ is drawn from its pre-intervention conditional on $H_t$)

$$P(o_{t+1} \mid a_t, H_t) \;=\; \sum_{\mathbf U} P(o_{t+1} \mid a_t, H_t, \mathbf U)\, P(\mathbf U \mid a_t, H_t)$$

(by ordinary marginalization on the natural distribution).

These are equal iff $P(\mathbf U \mid a_t, H_t) = P(\mathbf U \mid H_t)$ — which is exactly what (C2) provides. The right step in the proof is to push the $\mathbf U \perp a_t \mid H_t$ statement through the *prior* on $\mathbf U$ in the do-marginalization, not through the conditional on $o_{t+1}$.

**Recommended:** rewrite the central calculation as above. The lemma statement holds; only the proof has the misstep. About 4–6 lines in `B-key-lemma-proofs.md`. The `05-mechanism.md` version of the proof (line 36–47 of `05-mechanism.md`) is gentler and avoids this trap by just *quoting* the conclusion ("conditioning on $H_t$ blocks the action-selection confounding pathway, so the conditional and interventional distributions coincide on the support") — that prose is fine; it's the appendix proof that needs the surgery.

### M3 — Identity statement should specify what $a^*$ varies with

*Primary location:* `src/re/03-preliminaries.md:9`; affects every use of $\pi^* = \delta_{a^*}$.

The preliminaries define $a^*(s) := \arg\max_a Q^\pi(s, a)$ as the optimal action *under the agent's current model $M_t$*. The identity is then stated for each $s$ separately. The composition theorem and proofs handle this correctly via per-state $K_t(s)$ and $\overline{\mathrm{TV}}_t$. But the lemma statement (`05-mechanism.md:7–10`) drops the per-state qualifier and reads as if the identity is over a single (state-free) action distribution. A reviewer reading the lemma statement out of context could miss that $\pi^* = \delta_{a^*}$ is itself a state-conditional construct.

Trivial fix: in the lemma statement add "(per visited state, with $a^* = a^*(s)$ and $Q = Q(\cdot \mid s)$)."

### M4 — `(A5)` base-learner assumption hides occupancy structure

*Primary location:* `src/re/04-main-result.md:36`; used in `04-main-result.md:49` and proof of (v) at `A-proof-of-composition.md:29–43`.

(A5) assumes the base learner satisfies $\sum_{t=1}^L \mathbb E[\overline{\mathrm{TV}}_t] \le 2c\sqrt{L}$ where $\overline{\mathrm{TV}}_t$ is the *occupancy-weighted* TV under the round-$t$ learner-induced state distribution $d^{Q_t}_h$. This is a strong assumption: typical UCRL2/UCBVI guarantees give cumulative regret in *value*, not directly in occupancy-weighted TV. The translation from value to occupancy-weighted TV uses the simulation lemma in reverse, which can lose factors. The `D-algorithm.md:7` discussion identifies $c$ "of order $\tilde O(\sqrt{N_h SA})$" for UCRL2/UCBVI — that's a *value-side* base-learner constant; the "lifting cleanly to" $\tilde O(N_h^2 \sqrt{SA(B_T+1)T})$ implicitly assumes that this value-side regret is exactly what (A5) needs, but doesn't show the conversion.

This is honest scope-stating ("a practical algorithm achieving the joint guarantees… requires four components, each translating one of the assumptions into operational form") but a reviewer will want to see the conversion either explicitly or with a one-line citation to a paper that does it. (Foster / Krishnamurthy / Simchi-Levi are good candidates for value-to-TV-side conversions; I'd verify before adding.)

### M5 — `thm-decay-class` proof is one line and that's the right amount, *if* phrased carefully

*Primary location:* `src/re/C-aux-material.md:73–74`.

> At every element $(i, j)$, $\alpha_{ij}^{(t)} \to 0$ by definition of $\mathcal A_{\mathrm{decay}}$, so $\min_{(i,j)} \alpha_{ij}^{(t)} \to 0$, so the bottleneck $\mathcal T_\Sigma^{\mathrm{bn}} \to 0$ with experience.

This is fine *if* the class is defined as "every revisable element has $\alpha \to 0$." But the definition (C-aux-material.md:67–68) actually says "updates whose effective sector gain $\alpha_{ij}^{(t)} \to 0$ as $t \to \infty$ on every revisable element" — which already includes the universal quantifier, so the proof is consistent.

The trip-hazard: a reviewer might read "$\mathcal A_{\mathrm{decay}}$ includes count-accumulating Bayesian updates without forgetting" (line 77) and think Beta-Bernoulli with $\eta = 1/(n+1)$ is in the class for any *bandit-style* update. But Beta-Bernoulli's gain decays only on the elements actually being updated, not on un-touched elements. The proof's "every revisable element" needs the agent to actually be revising every element repeatedly — which Beta-Bernoulli does in the bandit setting only via the prior, and even then the rate of decay can vary across elements. Worth a parenthetical: "the class includes Beta-Bernoulli-style updates *under sustained pull on every revisable element* — agents that ignore a subset of elements are outside the class for those elements, but inside it for the elements they touch."

### M6 — `thm-chain-rule-uniqueness` proof is gestural

*Primary location:* `src/re/C-aux-material.md:135–137`.

> Writing $r_x = P(x)/Q(x)$ and $s_{y|x} = P(y|x)/Q(y|x)$, the chain rule reduces to the functional equation $f(rs) = f(r) + r f(s)$ for all $r, s > 0$. With $f(1) = 0$ and convexity, the unique solution is $f(t) = c \cdot t \log t$ for $c > 0$ \cite{aczel-1975-measures} (§4).

The reduction "the chain rule reduces to the functional equation $f(rs) = f(r) + r f(s)$" deserves more than a comma. Showing the reduction takes a few lines and is the substantive part of the proof. The functional-equation solution at the end is standard and the citation suffices, but the reduction is what a reviewer will want to see explicitly. Add 5–8 lines walking through the chain-rule expansion at a 2-element joint distribution and showing the equation falls out.

(This is an appendix theorem and matters less than M1/M2, but the conclusion's "the metric layer is uniquely reverse-KL up to positive scaling under chain-rule additivity" cites this as load-bearing for one of the framework's distinctness claims.)

### M7 — `lem-impulsive` (ProST reduction) cites Hespanha-Liberzon-Teel reverse-ADT

*Primary location:* `src/re/B-key-lemma-proofs.md:76–84`.

The reduction maps ProST's discrete update schedule to an impulsive system with continuous-destabilizing dynamics + stabilizing impulses, then invokes Hespanha-Liberzon-Teel 2008 Theorem 1's reverse-ADT branch. The threshold $\Delta_{\max} \cdot \rho_\Sigma/R_\Sigma < -\ln(1-\gamma)^2$ follows. I don't have the precise form of HLT Theorem 1 in memory, but the argument shape is right: impulsive systems with dwell-time conditions, longest-block-sets-the-threshold under nonuniform schedules. Worth a per-paper-agent verification that the cited theorem is in the form invoked (with the inequality direction matching).

### M8 — Algebra in the perturbative ε-greedy expansion

*Primary location:* `src/re/B-key-lemma-proofs.md:33–46`.

I walked through the displayed equation. The reduction
$$D_{\mathrm{KL}}(\pi^*_\epsilon \,\|\, Q) - D_{\mathrm{KL}}(\delta_{a^*} \,\|\, Q) \;=\; (1-\epsilon)\log(1-\epsilon) + \epsilon \log Q(a^*) + \epsilon \log\frac{\epsilon}{|\mathcal A| - 1} - \frac{\epsilon}{|\mathcal A| - 1} \sum_{a \neq a^*} \log Q(a)$$
checks out: the $-(1-\epsilon)\log Q(a^*) + \log Q(a^*)$ collapses to $\epsilon \log Q(a^*)$; the conditional-sum split is correct.

The leading-order argument is also correct: $(1-\epsilon)\log(1-\epsilon) = -\epsilon + O(\epsilon^2)$; the $\epsilon \log\epsilon$ term comes from $\epsilon \log(\epsilon/(|\mathcal A|-1))$. Under $Q(a) \ge q_0$, the $\sum \log Q(a)$ contributions are bounded uniformly. The leading behavior is $O(\epsilon \log(1/\epsilon))$. ✓

The "absorbing $\epsilon \log(1/q_0)$ into the leading $\epsilon \log(1/\epsilon)$ term" step is fine asymptotically but worth a clarifying sentence: this requires $\epsilon \to 0$ faster than $q_0$ stays bounded, which is the natural regime; in the joint limit $\epsilon, q_0 \to 0$ the bound's constant degrades.

---

## §2 — Argument strength / overclaim risk

### A1 — Intro paragraph 4 ("composition" paragraph, line 9) lists novelty claims aggressively

*Primary location:* `src/re/01-introduction.md:9`.

Reads: "no published framework composes them. In particular, no framework combines (a) … (b) … (c) … (d)…" The (b) bullet — "an exact point-mass reverse-KL/TV regret identity under deterministic optimum, strictly tighter than the Bretagnolle–Huber inequality at this corner" — is stated as a unique-novelty claim, which the prior-art appendix later qualifies very honestly ("63 papers retrieved, abstract-level coverage estimated 75%, no direct anticipation in the retrieved set… reviewers should read it as 'to our knowledge / in the retrieved set'").

The intro should propagate the qualifier *before* a reviewer arrives at the appendix to find it. Suggested rewrite: "In particular, to our knowledge — based on a structured retrieval documented in [[#^sec-prior-art]] — no framework combines…" Single phrase, defuses the strongest possible reviewer pushback ("but X did Y in 2024").

### A2 — "the technical anchor" (intro line 15)

*Primary location:* `src/re/01-introduction.md:15` ("The technical anchor is an exact algebraic identity") and `src/re/06-conclusion.md:3` ("The point-mass identity is the technical anchor").

Calling it the "technical anchor" understates how much work the strategic-tempo lemma and the closed-loop-interventional-access lemma do downstream. The cumulative regret rate (v) goes through (i) and (A5), which means Component 2 (the identity) is necessary for the per-round coordinate, but Components 3 and 4 carry the persistence and learnability claims. The framing "the technical anchor" can read as "the only substantive technical content," which isn't what the paper actually claims (the necessity-of-four-components section makes this explicit).

The phrasing also has a kind of self-discount: if the identity is a "two-line direct calculation" (and it is), then calling it "the technical anchor" of the paper invites the reviewer thought "is the contribution then just recognition?" — to which the honest answer is "yes, plus the composition with three other lineages." Naming the anchor "the per-round metric coordinate" or "the regret-vs-KL coordinate" rather than "the technical anchor" might land more cleanly.

### A3 — "uniformly tighter" / "strictly improves" wording could be unified

*Primary location:* `src/re/01-introduction.md:7, 17, 25, 27`; `src/re/05-mechanism.md:12, 16`; `src/re/E-pinsker-numerics.md`.

The relationship between the identity and Pinsker/BH gets described in slightly different ways across segments:

- "strictly improves both Pinsker and the Bretagnolle-Huber inequality" (intro)
- "lies *strictly below* both the Pinsker envelope and the BH envelope" (intro)
- "*strictly* below the BH envelope at this corner" (mechanism)
- "*strictly* on $D_{\mathrm{KL}} > 0$" (proof)
- "uniformly tighter than Pinsker, by a factor of 7× at $D_{\mathrm{KL}} = 0.01$" (numerics)

These are all consistent (and correct), but "strictly below," "uniformly tighter," and "strictly improves" carry slightly different formal meanings (pointwise inequality, uniform-over-$Q$, strict-improvement-as-a-bound). Pick one phrasing for the headline claim and use the others as elaboration. "Strictly tighter at every $D_{\mathrm{KL}} > 0$, uniformly in $Q$" is a clean form.

### A4 — "coordinate-optimal among bounds depending only on TV"

*Primary location:* `src/re/04-main-result.md:14`; `src/re/05-mechanism.md:14`.

This is the right framing — naming exactly what's optimal and the class it's optimal within. But "coordinate-optimal" is non-standard terminology and the meaning isn't immediate. A reviewer might read it as "best in some coordinate" rather than "tightest possible bound in this coordinate's class." Rephrase as "tight among bounds that depend only on TV (achieved on extremal value landscapes)." The argument that follows ("any tighter bound on $R(Q)$ requires information beyond TV") is the right justification.

### A5 — "lifts ProST's tempo result" — direction of the lift

*Primary location:* `src/re/01-introduction.md:31` ("This *lifts* the tempo result of \citet{lee-2023-prost-tempo} from a single-factor hyperparameter-optimization claim to a multi-factor structural-threshold claim").

The lift is real (ProST is a single-factor special case of the multi-factor framework) but the *direction* of the lift can be misread. ProST's tempo schedule is an optimization variable; the multi-factor framework treats the tempo as a structural quantity with a survival threshold. So the contribution is reframing the tempo's role, not just generalizing it. The current wording could read as "we did everything ProST did plus more dimensions" when the actual claim is "we changed the type of statement made about the tempo." Worth a clearer hinge sentence: "ProST asks 'what's the optimal tempo schedule?'; we ask 'is *any* schedule possible?', producing a structural threshold that ProST recovers as the optimization-friendly special case."

### A6 — "Three regimes A/B/C of identifiability"

*Primary location:* `src/re/04-main-result.md:21`; `src/re/05-mechanism.md:42–46`.

The Regime A / B / C partition is presented as a contribution ("we surface the regime split at framework level," `C-aux-material.md:122`). But the partition is essentially "fully identifiable / partially identifiable / not identifiable" — which is a standard way of speaking about causal-RL settings, not a novel framework move. The substantive contribution here is the *bias-bound proportional to $1 - p_{\mathrm{id}}$* in lem-bias-bound, not the regime taxonomy itself. Tone down the "regime A/B/C" framing as a contribution; lean harder on the bias-bound as what's new.

### A7 — Cumulative rate's bias term is linear in $T$

*Primary location:* `src/re/04-main-result.md:49` (conclusion (v)); `src/re/05-mechanism.md:70`; honestly noted in `04-main-result.md:63` ("Unpacking iii — bias term").

The displayed bound for $\mathrm{DynReg}(T)$ is $\tilde O(N_h \sqrt{(B_T+1)T}) + N_h(1 - p_{\mathrm{id}}) \log(1/q_0) \cdot T$. The second term is *linear in $T$*. In Regime A ($p_{\mathrm{id}} \to 1$) it vanishes; in Regime B it's controlled but linear; in Regime C it's vacuous. This is honestly handled, but the "Cesàro tracking corollary" $\to 0$ requires *both* $B_T = o(T)$ *and* $p_{\mathrm{id}} \to 1$. The latter is a strong assumption — without Regime A the Cesàro tracking *doesn't go to zero*. The conclusion's claim "the agent's average value over the trajectory tracks the moving optimum's average value" (in `A-proof-of-composition.md:51`) holds only in Regime A.

The intro's headline rate $\tilde O(N_h \sqrt{(B_T+1) T})$ (line 14) silently elides the bias term; the abstract / opening should at minimum carry "in Regime A" or "vanishing-bias" as a qualifier — currently the bias term doesn't appear at all in the headline rate.

### A8 — "first composition that delivers all three properties jointly" (intro line 13)

*Primary location:* `src/re/01-introduction.md:13`.

"First" is a strong claim. The prior-art appendix qualifies ("to our knowledge / in the retrieved set"). The intro's "the first composition" should match the appendix's tone, not exceed it. Replace "first" with "to our knowledge, the first" or just "we present a composition that delivers all three…" — letting the prior-art appendix carry the absence claim with its own honest scoping.

### A9 — The scope-and-limitations section in the intro

*Primary location:* `src/re/01-introduction.md:39–47`.

Excellent placement — usually scope-and-limitations gets buried in appendix. Putting it in the intro signals discipline. The four sub-bullets ("What 'convergence' means here," "Theory only," "Canonical scope," "Comparator regime") all earn their place. Nothing to fix; flagged as worth keeping in any revision.

---

## §3 — Prose / structure

### S1 — Intro is dense (47 lines including contributions)

Justified by the four-component story. Each component description is 8–10 lines. A reviewer can follow but it's heavy on first read. Two structural moves that would help without losing content:

- **Move the contributions paragraph "(i) Point-mass reverse-KL/TV regret identity"** (line 25) into a separate compact block — currently it lives as `### Contribution` after `**Roadmap.**`, which interrupts the flow from main intro → roadmap → contributions → scope.
- **Cut the "fundamental question remains open"** quote-block at line 11. It's rhetorical scaffolding; the contribution (line 13: "we present the first composition…") would land directly. Saves 2–3 lines.

### S2 — Repeated phrase "the four together close a story none of the strands closes individually"

*Locations:* `01-introduction.md:37`; `04-main-result.md:79`; `06-conclusion.md:3`.

Three near-identical instances. Pick one canonical formulation and reference it via "(see contribution iv-bundle, [[#^sec-necessity]])." Saves space and avoids the "this paper protests too much" reading.

### S3 — "Component" / "Track" / "Strand" / "Lineage" / "Element" terminology

The paper uses ~5 different words for similar units of structure:

- "Tracks" — the four research lineages (intro)
- "Lineages" — also the four research lineages (intro line 5, 9)
- "Strands" — also the four research lineages (intro line 5; conclusion)
- "Components" — the four parts of the contribution (everywhere)
- "Elements" — the revisable per-element parts of the strategic policy (Component 3)

This is not a defect — context disambiguates — but a reviewer trying to remember what's what might appreciate consistency. Suggested: "tracks" for the literature lineages, "components" for the contribution parts, "elements" for the per-element strategic substate.

### S4 — Conclusion's "concluding observations" subsections are crisp; "practitioner takeaways" reads marketing-y

*Primary location:* `src/re/06-conclusion.md:19–28`.

Three numbered takeaways. Each is well-stated but the framing — "for a practitioner seeking to apply" + numbered actionable takeaways — has a textbook feel that doesn't fit the rest of the paper's voice. If page-pressured, this is the safest cut: the same content lives in the algorithm appendix `D-algorithm.md`. If retained, soften the framing: "Three observations a practitioner may find useful…"

### S5 — Mechanism section is 78 lines

Each lemma gets full statement + intuition + perturbative + "full proof in [[#^...]]". Could be tightened by:

- Folding the "Perturbative extension" remark of Key Lemma 1 (mechanism line 16) into a single sentence, deferring details to the proof appendix where they're already written out.
- Folding "ProST as a sector-level reduction" (mechanism line 31–34) into a one-paragraph forward pointer.

Both edits save ~10 lines without content loss.

### S6 — The "Roadmap" and "Contribution" subsections both appear in the intro

*Primary location:* `src/re/01-introduction.md:19–37` (Roadmap on line 19, Contribution as a subsection starting line 21).

The Roadmap one-liner is fine, but the Contribution subsection following it (with `### Contribution` header) is essentially the contributions list that intro paragraphs already establish. Could either:
- promote Roadmap+Contribution to a dedicated `### Roadmap and contributions` subsection;
- or fold the Roadmap into the contributions and skip the standalone Roadmap.

Currently the structure is intro paragraphs → Roadmap → Contribution → Scope, and a NeurIPS reviewer expects either intro paragraphs → Contributions → Roadmap → Scope, or intro paragraphs → Contributions+Roadmap blended. The current order leaves the reader uncertain whether they're still in the abstract-shape narrative or have entered the actual contributions list.

### S7 — Cross-reference density

The paper has a heavy cross-reference style (`[[#^sec-key-lemma-1]]`, `[[#^lem-pointmass-identity]]`, `[[#^thm-composition]]`). For an Obsidian/markdown source this is good engineering; in the rendered PDF these become `\Cref{...}`-style numerical refs which are fine. One spot worth flagging: the long cross-reference chains in single sentences (e.g., `04-main-result.md:7`: "The component-by-component failure-mode analysis is in [[#^sec-necessity]] below.") sometimes drop the reader several pages forward in the same sentence. A NeurIPS reviewer reading linearly will appreciate fewer forward-pointers in the main argumentative paragraphs and more in the recap / restatement paragraphs.

---

## §4 — Citation / prior-art notes (loose, training-data-only)

### C1 — Pinsker's inequality has no explicit citation

*Primary location:* The "Pinsker" name is used throughout (intro line 7, 27; mechanism line 16; numerics) without a citation. Standard knowledge for the audience but a one-time citation costs nothing. Common targets: Tsybakov 2009 §2.4 (already cited for Le Cam); Polyanskiy-Wu information-theory notes; Gibbs-Su 2002 *Choosing among $f$-divergences*.

### C2 — Sequential ignorability terminology

*Primary location:* `src/re/04-main-result.md:21` (C2 of Component 4); `src/re/05-mechanism.md:38`.

"Sequential ignorability" is the standard term in Robins / Murphy / van der Laan causal-inference literature. Worth a citation when the term is introduced. Common targets: Robins 1986 (foundational); Murphy 2003 (sequential decision rules); van der Laan-Robins 2003 (longitudinal causal inference). Currently the term is used as if standard — which it is, but a citation helps reviewers from RL-only backgrounds.

### C3 — Vieillard-Pietquin "Leverage the Average" / KL-regularized RL connection

The point-mass reverse-KL identity has a structural neighbor in KL-regularized RL convergence analysis. Vieillard-Pietquin-Munos-Geist *Leverage the Average* (NeurIPS 2020) and follow-up work analyze regret bounds for KL-regularized policy iteration where reverse-KL appears as a regularizer. The connection: their reverse-KL is from $\pi$ to a reference $\bar\pi$ rather than from $\pi^*$ to $Q$; under deterministic $\bar\pi$ their analysis would specialize to the identity. Worth a sentence in related work — both to position the contribution and to defuse "but Vieillard et al…" pushback.

Also Mei-Xiao-Szepesvari et al. on policy gradient + KL-control. The "reverse-KL is the right divergence for regret-class arguments" framing is converging across multiple lines.

### C4 — TRPO bound on TV (Schulman 2015) cited; CPO / PPO TV bounds not

*Primary location:* `src/re/05-mechanism.md:64` (and proof at `A-proof-of-composition.md:33`).

`schulman-2015-trust` is cited for the $N_h^2$ worst-state TV bound. The simulation-lemma route gives $N_h$ with occupancy weighting. CPO (Achiam-Held-Tamar-Abbeel 2017) and PPO (Schulman 2017) variants of the TV bound are also relevant, especially since the paper uses occupancy-weighted TV. One-sentence acknowledgment if relevant.

### C5 — Tighter Pinsker / refined-Pinsker references

The numerics table (`E-pinsker-numerics.md`) compares against Pinsker's *constant* form $\sqrt{D_{\mathrm{KL}}/2}$. Refined Pinsker bounds (e.g., Gilardoni 2010, Sason-Verdú 2016) give sharper constants for small $D_{\mathrm{KL}}$. The 7× claim at $D_{\mathrm{KL}} = 0.01$ wouldn't be much affected, but a careful reviewer might ask "tighter than which Pinsker?" Worth a one-sentence acknowledgment that "Pinsker" here means the standard textbook constant, with refined-Pinsker comparisons relegated to follow-up.

### C6 — Khalil Chapter 9 cited correctly for sector-Lyapunov

*Primary location:* `src/re/B-key-lemma-proofs.md:72` cites `khalil-2002-nonlinear` (Lemma 9.2 / Theorem 9.1 of Chapter 9). Standard. ✓

### C7 — Prior-art retrieval is well-scoped

`src/re/F-prior-art.md` documents the methodology (Undermind, 63 papers, 75% abstract coverage) and is unusually honest about scope. Nothing to fix; flagged as a model.

### C8 — Anonymization spot-check

I scanned for the AUTHORING §3.5 forbidden categories (Joseph, Wecker, ASF, ELI names, Zenodo DOI). Nothing flagged. Run `bin/refs lint 02-unified-convergence-rl` for confirmation.

### C9 — `gerogiannis-2026-darling` and `zhang-2026-peril` contemporaneous-cutoff

*Primary location:* `src/re/02-related-work.md:17`. Cited under "contemporaneous (post-March 2026)" with no empirical-comparison demand. Per AUTHORING §5.4 the cutoff convention is followed. ✓

---

## §5 — Empirical / experiment-design

The paper is theory-only. The "Theory only. No experiments." caveat (`01-introduction.md:43`) is clean. No empirical findings to audit.

The `D-algorithm.md` "sketch-grade" disclaimer ("full instantiation and empirical evaluation are deferred to a follow-up paper") is honest. A reviewer might still ask: which of the (a)–(d) algorithm components have *any* prior empirical implementation in the cited works? E.g., ProST has been implemented (Lee 2023); UCRL2/UCBVI variants likewise; the two-gap diagnostic and the multi-factor strategic tempo, as composed here, do not have an empirical witness. Worth one sentence either confirming this or pointing to the follow-up paper (if it exists in the project pipeline).

---

## §6 — Page-pressure suggestions (if you want to trim)

### P1 — Cut the Roadmap+Contribution duplication (S6)

Save ~5 lines.

### P2 — Tighten Mechanism (S5)

Fold perturbative-extension and ProST-reduction-remarks into one-line forward pointers. Save ~10 lines.

### P3 — Reduce repeat phrasing on "no published framework" (S2)

Pick one canonical formulation. Save ~5 lines across intro / main-results / conclusion.

### P4 — Tighten Component 2 in `04-main-result.md`

The Component 2 paragraph (`04-main-result.md:11–15`) restates the identity, the strict comparison, and the perturbative extension — all of which appear again in Key Lemma 1 of the mechanism (`05-mechanism.md:7–16`). Reduce Component 2 to: identity + Lipschitz-equivalence + "see Key Lemma 1 for proof and perturbative extension." Save ~5 lines.

### P5 — Cut "Practitioner takeaways" (S4)

If page-pressured. The same content lives in `D-algorithm.md`. Save ~10 lines from conclusion.

### P6 — `C-aux-material.md` is 141 lines

The "Strategic-tempo across canonical topologies" section (T1–T4) is interesting but is forward-pointed from main text without main-text reading needing it. If T1–T4 don't carry load-bearing content for the headline claims (and a quick read suggests they don't — the topology examples are illustrations of bottleneck behavior, not part of the main-result chain), the section could be a one-paragraph summary with the table moved to a less-detailed format.

---

## §7 — Minor / nits

### N1 — `\overline{\mathrm{TV}}_t` notation

*Primary location:* introduced in `04-main-result.md:36` inside (A5) as "occupancy-weighted per-round coordinate." Used throughout. The bar over TV is understandable but visually competes with $K_t$ (no bar) and $\widehat D_t$ (hat). The notational stack is dense; if you can drop the bar (calling it $\mathrm{TV}_t$) without ambiguity (since the bare TV is between specific distributions and $\mathrm{TV}_t$ is the round-$t$ aggregate), the eye saves work.

### N2 — `K_t(s)` definition

*Primary location:* `04-main-result.md:26`. Defined inline in the theorem statement: "$K_t(s) := D_{\mathrm{KL}}(\delta_{a^*_t(s)} \,\|\, Q_t(\cdot \mid s))$ be the per-state reverse-KL coordinate." Clean. ✓ Not a finding; flagged as good notation.

### N3 — `Q_O` vs `Q^\pi` vs `Q_t`

*Primary locations:* `Q_O$ (objective Q-function, preliminaries); `Q^\pi`(Q-function under policy π, intro and prelims); `Q_t` (the agent's policy distribution at round $t$, throughout). Three different objects sharing a letter. Standard for theory papers but a reviewer reading quickly might trip. One sentence in preliminaries naming the three would help.

### N4 — `\mathcal T_\Sigma^{\mathrm{bn,ss}}` vs `\mathcal T_\Sigma^{\mathrm{agg,ss}}`

*Primary locations:* `01-introduction.md:31` introduces `\mathcal T_\Sigma^{\mathrm{bn,ss}}`; `D-algorithm.md:9` introduces `\mathcal T_\Sigma^{\mathrm{agg,ss}}` ("aggregate" form for fail-fast pre-check). The aggregate form isn't defined in the main text, only invoked. Either define both in preliminaries (`03-preliminaries.md`) or in the strategic-tempo lemma — currently the reader meets `agg` for the first time in the algorithm appendix.

### N5 — Long math lines in `04-main-result.md` (line 49)

The displayed equation for $\mathbb E[\mathrm{DynReg}(T)]$ is on the wide side. In the rendered PDF this might wrap awkwardly. Worth a check after build.

### N6 — `\citep` vs `\citet` consistency

*Primary location:* `01-introduction.md:7` uses both — `\citep{bretagnolle-huber-1978-densities}` (parenthetical) once and `\citet{...}` elsewhere. AUTHORING.md §2.3 likely has the canonical convention; check and align.

### N7 — `\textsl` / `\textit` mixed

Quick scan finds *italic emphasis* used consistently (the markdown `*...*` form). ✓ Not a finding; just confirming.

### N8 — `D_{\mathrm{KL}}(P \,\|\, Q)` spacing

The `\,\|\,` (thin-space-double-bar-thin-space) is used consistently. ✓

### N9 — The contribution paragraph claim `1 - e^{-D_{\mathrm{KL}}}` is "$7\times$ tighter at $D_{\mathrm{KL}} = 0.01$"

*Primary location:* `01-introduction.md:27`; `04-main-result.md:15`. Verified: $\sqrt{0.01/2} \approx 0.0707$, $1-e^{-0.01} \approx 0.00995$, ratio ≈ 7.10. ✓ Match holds at the displayed precision.

### N10 — `\mathbf w` vs `w` for vectors

*Primary location:* `B-key-lemma-proofs.md` uses `\mathbf w` for the disturbance vector and `w_{ij}` for components. Standard. ✓

---

## Closing

Two priorities for the per-paper agent before submission:

1. **M2** (the `lem-loop-level2` proof rewrite — currently misuses (C2)) is the highest-leverage technical fix. The lemma statement holds, the prose version of the argument in the mechanism section is fine, but the appendix proof has a real misstep that a careful reviewer will catch and that creates a credibility hit downstream of the closed-loop interventional-access argument.

2. **M1** (the `lem-forgetting` proof's $V$-bound vs. $R^*$-bound mismatch) is the second-priority technical fix. The threshold form is recoverable under deterministic worst-case, paralleling paper 01's `lem-persistence-d`. The current proof's algebra doesn't yield the stated $R^*$.

Beyond those, A1 (propagate the prior-art appendix's honest "to our knowledge" qualifier into the intro) and A7 (the bias term is linear in $T$ and should appear in any headline rate statement, with Regime A as the qualifier) are the two highest-leverage rhetorical strengthenings.

Everything else is tightening — the paper's underlying structure is sound and the literature-claim hygiene is unusually careful. The two real proof issues are local to specific appendix derivations; the headline composition theorem and its conclusions stand.

— end audit —
