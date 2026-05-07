# De Novo Audit: A Unified Convergence Theory for Non-Stationary RL

**Date:** 2026-05-06
**Auditor:** Gemini CLI

## 1. Space Reduction & Restructuring (Addressing the >9pp limit)
The paper is dense with four major components and needs compression.
- **Relocate Ablation (Section 4.4):** Section 4.4 ("Necessity of the four components") is an excellent ablation argument, but it consumes valuable main-text space. Since the composition itself is the primary contribution, this ablation could be moved to the appendix, leaving just a single summarizing sentence in the main text.
- **Merge Sections 4 and 5:** Section 5 ("Mechanism") restates the four key lemmas and provides intuitions. Since the formal lemmas are the backbone of the proofs, consider stating the formal lemmas directly in Section 4 alongside the components they support. Section 5 could then be condensed into a single cohesive proof sketch for Theorem 4.1. This eliminates structural redundancy and will likely save over a page.

## 2. Claim Strengthening & Technical Focus
Following the project's "strengthen before softening" mandate:
- **Elevate the Perturbative Extension:** The point-mass reverse-KL/TV identity (Lemma 5.1) is sharp at the deterministic-$\pi^*$ corner. The text notes it extends perturbatively to $\epsilon$-greedy and softmax-regularized policies, but treats this almost as a secondary remark. Since most practical RL uses these exploration schemes, formalizing the perturbative extension (with its exact analytic correction terms) as a main-text Theorem would significantly strengthen the paper's real-world applicability.
- **Highlight the Universal Failure Class:** The claim that gain-decay updates universally fail the persistence threshold (Theorem C.2) is a profound structural result. Currently, it is somewhat buried in the description of Component 3 and Appendix C.6. Elevating this universal failure class to a formal theorem in the main text would strengthen the paper's impact on adaptive control practice.