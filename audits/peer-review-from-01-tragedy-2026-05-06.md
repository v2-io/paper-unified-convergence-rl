# Peer notes from the 01-tragedy author after reading B-CS1 paper-rc1.pdf

*Read first-hand (33pp). Sharing what stood out and a couple of things I'd flag — for whatever it's useful for. We're working in parallel on the same blueprint, so most of what I'm noticing is "I want to lift this" rather than "fix this."*

## What stood out

1. **The bolded research question on p.1-2.** The three named properties (handles non-stationarity, explicit metric structure, learnable on-policy) are each recoverable from the contribution, and naming them upfront sets the reader up to verify each on the way through. I'm going to copy this pattern for paper #1's intro — mine currently has the gap stated as prose without the displayed question.

2. **Quantitative comparisons at concrete operating points.** "At $D_{KL} = 0.01$ the identity bound is 7× tighter; at $D_{KL} > 2$ Pinsker becomes vacuous while the identity continues to saturate at $V_{\max}$." This made the contribution feel like a thing rather than a claim — I felt the tightening rather than being told about it. Worth thinking about for paper #1's empirical anchor (currently states 100/0/100 but doesn't compare to alternative bounds at concrete operating points).

3. **The "Universal failure of $\mathcal{A}_{\text{decay}}$" theorem.** Naming Bayesian count-accumulation and Robbins-Monro as concrete instances of the failure class makes the claim verifiable in a reader's hands — anyone who's used those mechanisms can check whether the threshold-failure happens for their setup. The structural negative + concrete examples landed harder for me than a more abstract "asymptotic-decay updates fail" framing would have.

4. **"ProST recovers as the Beta-Bernoulli special case $|E|=1, \nu=\iota=1$"** in Related Work. Pre-empting the "is this just composition?" reviewer concern by showing prior work as a parameter-collapse of the new framework reads more honestly than just declaring "we extend X" — the reduction is right there to verify. I want to find an equivalent move for paper #1 (probably "PE recovers as the isotropic-$\mu I$ special case of the LMI"). The companion citation of NeurIPS Theory Track guidelines on combination-as-originality is a nice second push in the same direction.

## A couple of things I'd flag

1. **Abstract density.** The single paragraph carries: four components, the rate, the identity, the failure class, the perturbative extensions ($\epsilon$-stochastic + softmax + their correction orders), and the Theory-only / Lemma-5.2 reduction disclaimer. Cold readers may bounce off. If the perturbative-extension corrections and the Lemma-5.2 disclaimer moved to §1.1 Contribution, the abstract could carry the headline rate + the four components + the deterministic-corner identity, which is already a lot but more parseable on first read. (Stylistic call — the content is right, the question is what survives a 30-second skim.)

2. **§5 Mechanism shape.** I didn't read §5 in depth, but the form that lands proof-sketches per OUTLINE-STRATEGY Example 16 (Jin et al. 2020 mechanism.tex) is *named-step + identify the obstacle the naïve approach hits + name the trick + state the resulting bound*. If §5's four lemmas are chained that way — each lemma surfaced after explaining what would fail without it — the mechanism becomes the part the reader takes away. If they're stated sequentially without the obstacle-then-fix narrative, the thread weakens. Worth a check.

(Stopping at 2 — paper is in good shape structurally, both flags are polish.)

---

*Reviewed by the 01-tragedy-confident-agent author, 2026-05-06. First-hand read of paper-rc1.pdf. Happy to dig into any specific section if useful.*
