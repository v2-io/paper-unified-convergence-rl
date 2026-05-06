# OUT.neurips-2026-paper.md — 9-page-budget assembly manifest

*Subset selection of `src/` segments for the NeurIPS 2026 Main Track 9-page budget. Same segments as `OUT.full-paper.md`; appendix rows commented out via `<!-- ... -->` for sections that don't fit the budget per AUTHORING §7.2 (reuse-over-re-edit; trim by manifest-level omission rather than within-segment editing).*

*Migration-agent draft — initial cut. Per the source paper's `OUTLINE.md` Pass-5 audit, "bulk of the paper is load-bearing; cutting will not get to 9pp; compression of expository sections is the path." Manifest-level appendix omission alone is insufficient — within-segment compression of §3.3, §4.4, §4.6, §6.3, §7 lead, §7.2, §9.2, §9.3 is the per-paper-agent territory post-migration. The cuts encoded below are a starting point; the per-paper agent should refine.*

## Body

| § | Type | Slug | Title | Stage |
|---|------|------|-------|-------|
| 1 | Section | [intro](src/01-introduction.md) | Introduction | draft |
| 2 | Section | [setup](src/02-setup.md) | Setup | draft |
| 3 | Section | [two-gap](src/03-two-gap-diagnostic.md) | Component 1 — The Two-Gap Diagnostic | draft |
| 4 | Section | [identity](src/04-pointmass-identity.md) | Component 2 — Point-Mass Reverse-KL/TV Identity | draft |
| 5 | Section | [tempo](src/05-strategic-tempo.md) | Component 3 — Strategic Tempo + Forgetting | draft |
| 6 | Section | [loop](src/06-loop-level2.md) | Component 4 — Closed-Loop Interventional Access | draft |
| 7 | Section | [composition](src/07-composition.md) | Composition Theorem | draft |
| 8 | Section | [related](src/08-related-work.md) | Related Work | draft |
| 9 | Section | [conclusion](src/09-limitations-conclusion.md) | Limitations and Conclusion | draft |

## References

| § | Type | Slug | Title | Stage |
|---|------|------|-------|-------|
| – | Bibliography | [refs](src/10-references.md) | References | draft |

## Appendices

*Appendix A (supporting material) and Appendix F (bias bound) are kept — A backs §3 / §4 / §5 derivation steps and §A.6 carries the perturbative-extension proof; F is the load-bearing proof for [[#^thm-composition]](v) conclusion (iii). Appendix G (proof sketches) kept for Theorem 5.1 and Theorem 7.1(v) main proofs.*

*Appendix B (Pinsker numerics) is decorative — body §4.4 already states the strict-improvement claim; numerical table is supplementary. Appendix C (chain-rule uniqueness) is supporting — body §4.6 already states reverse-KL is uniquely chain-rule additive; the explicit proof is supplementary. Appendix D (prior-art summary) is methodological — body §8 already does the cite-and-distinguish. Appendix E (algorithm sketch) is implementation-flavored — body §7 remarks already mention the bandit and trajectory-level rates; the full algorithm sketch is supplementary.*

*Per-paper agent: re-evaluate which appendix cuts are right for the final 9pp form. The set below is a first cut.*

| § | Type | Slug | Title | Stage |
|---|------|------|-------|-------|
| A | Appendix | [supporting](src/A-supporting-material.md) | Supporting Material for the Main Components | draft |
<!-- | B | Appendix | [pinsker](src/B-pinsker-numerics.md) | Pinsker vs.\ Point-Mass Identity Numerical Comparison | draft | -->
<!-- | C | Appendix | [chain-rule](src/C-chain-rule-uniqueness.md) | Chain-Rule Uniqueness of Reverse-KL | draft | -->
<!-- | D | Appendix | [prior-art](src/D-prior-art-summary.md) | Prior-Art Retrieval Summary | draft | -->
<!-- | E | Appendix | [algo](src/E-algorithm-sketch.md) | Sketch of an Algorithm Achieving Theorem 7.1 | draft | -->
| F | Appendix | [bias](src/F-bias-bound.md) | Bias Bound for the KL-Coordinate Under Partial Identifiability | draft |
| G | Appendix | [proofs](src/G-proof-sketches.md) | Proof Sketches for Theorem 5.1 and Theorem 7.1(v) | draft |
