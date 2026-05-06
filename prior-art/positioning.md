Read the report. **The result is exactly what you want for B-CS1, and structurally parallel to B-N4** — no Tier 1 direct anticipation, strong compositional anticipation across four largely-separate lineages, with a particularly clean negative on Bretagnolle-Huber.

## The verdict is the ideal positioning

**No Tier 1 direct anticipation.** No retrieved paper composes all four target elements into one non-stationary convergence theory. The landscape splits into four largely-separate strands — and crucially, *no one has unified them*.

**Strong Tier 2 compositional anticipation** — the four constituent strands are well-developed:
- Strand 1: Dynamic-regret under drift [Cheung-Simchi-Levi-Zhu 2020, Wei-Luo 2021, Mao et al. 2021, Wang Chi Cheung et al. 2018/2019]
- Strand 2: Two-term regret decomposition (exploration vs adaptation) [Long-Fei Li-Peng Zhao-Zhou 2024, Stradi et al. 2024, Fei-Yang-Wang-Xie 2020]
- Strand 3: Tempo / forgetting analyses [Lee et al. 2023 ProST, Lee-Jin-Lavaei-Sojoudi 2024, Touati-Vincent 2020 sliding-window/exponential-weight]
- Strand 4: Causal/interventional sample-efficiency [Bareinboim's line: Zhang-Bareinboim 2016/2022; Lu-Meisami-Tewari 2021/2022 C-MDPs; Wang-Yang-Wang 2020 DOVI]

**Strongest negative signal — clean novelty for AAD: Bretagnolle-Huber identity.** BH does not appear in the retrieved RL/non-stationary corpus. The information-theoretic RL literature uniformly uses Shannon entropy, mutual information, information ratio, Pinsker, or Hellinger [Russo-Van Roy 2014 Thompson, 2014 IDS; Lu-Van Roy 2019 information-theoretic confidence bounds; Min-Russo 2023 nonstationary entropy-rate bounds; Canonne 2022 KL-TV inequality]. The deterministic-π* exact-equality form — load-bearing for AAD's two-sided regret bound — is absent. This is a clean novelty position, identical in shape to the B-N4 result on the survival-imperative drive.

This is the *strongest possible* positioning for a four-piece composition paper short of "no related work at all" (which would itself be suspicious). It says: the field has been building toward this across four parallel tracks, and no one has put them together.

## The cite-and-extend anchors are now concrete

The paper's "Related Work" section now writes itself, organized by the four-strand structure of the prior art:

**Strand 1 — Dynamic-regret under drift (THE non-stationary RL ancestor):**
- **Cheung-Simchi-Levi-Zhu 2020** "RL for Non-Stationary MDPs: The Blessing of (More) Optimism" — sliding-window UCB with confidence widening. *The* canonical non-stationary RL paper.
- **Wei-Luo 2021** "Non-stationary RL without Prior Knowledge" — black-box reduction giving optimal dynamic regret without knowing the variation budget.
- **Mao-Zhang-Zhu-Simchi-Levi-Başar 2021** "Near-Optimal Model-Free RL in Non-Stationary Episodic MDPs" — RestartQ-UCB with matching upper/lower bounds.
- **Cheung-Simchi-Levi-Zhu 2018/2019** "Hedging the Drift" / "Learning to Optimize under Non-Stationarity" — bandit-over-bandit framework adapting to latent changes.
- **Gajane-Ortner-Auer 2019** "Variational Regret Bounds for RL" — first variational regret bound for general RL.

**Strand 2 — Two-term regret decomposition (THE structural ancestor for the AAD 2×2):**
- **Long-Fei Li-Peng Zhao-Zhi-Hua Zhou 2024** "Dynamic Regret of Adversarial MDPs with Unknown Transition" — the dynamic regret bound *naturally decomposes into two terms*, one due to confidence-set construction (transition uncertainty) and one due to choosing sub-optimal policies (non-stationarity adaptation). **Closest structural precursor to AAD's 2×2 — but the axis differs.** Long-Fei Li et al. decompose along the *uncertainty-source axis* (transition vs adaptation); AAD decomposes along the *goal-feasibility-vs-policy-quality axis* (satisfaction-gap vs control-regret). Cite-and-distinguish: same shape of factorization, different structural axis, different downstream diagnostics.
- **Stradi-Lunghi-Castiglioni-Marchesi-Gatti 2024** "Learning CMDPs with Non-stationary Rewards and Constraints" — sublinear regret + positive constraint violation, with non-stationarity-aware decomposition.
- **Fei-Yang-Wang-Xie 2020** "Dynamic Regret of Policy Optimization in Non-stationary Environments" — POWER and POWER++ algorithms with explicit two-component regret bound (exploration + adaptation). **First model-free dynamic regret analysis in non-stationary RL.**
- **Yang-Zheng-Tomizuka-Liu-Li 2024** "Feasibility Theory of Constrained RL" — vocabulary-similar / structurally-distant. Their "feasibility" is *constraint-region feasibility* (state stays inside constraint set); AAD's satisfaction-gap is *goal feasibility* (the objective is achievable in principle given $M_t$, $\Pi$, $N_h$). The two are different concepts that deserve careful disambiguation in the paper. Important to handle this cite-and-distinguish carefully — a sloppy reader could conflate them.

**Strand 3 — Tempo / forgetting (THE strategic-tempo ancestor):**
- **Lee-Jin-Lavaei-Sojoudi et al. 2023** "Tempo Adaptation in Non-stationary RL" (NeurIPS 2023, ProST framework) — **the goldmine citation for Component 3**. They explicitly compute a suboptimal sequence $\{t_1, ..., t_K\}$ minimizing the dynamic regret upper bound, showing the trade-off between *agent tempo* (policy training time) and *environment tempo* (rate of environmental change). This is the closest existing analog to AAD's strategic tempo. Cite-and-distinguish: ProST optimizes tempo as a hyperparameter; AAD's strategic tempo is the rate of useful $\Sigma_t$ revision under the forgetting prerequisite $(1-\lambda) > \rho_\Sigma/R_\Sigma$ as a structural survival inequality.
- **Lee-Jin-Lavaei-Sojoudi 2024** "Pausing Policy Learning in Non-stationary RL" — companion paper showing that a *non-zero policy hold duration* yields sharper dynamic regret. Independent confirmation that policy-update rate is convergence-relevant.
- **Touati-Vincent 2020** "Efficient Learning in Non-Stationary Linear MDPs" — exponential-weight-based forgetting (OPT-WLSVI) that smoothly forgets data far in the past. The closest *exponential-forgetting* RL ancestor.
- **Russac-Vernade-Cappé 2019** weighted linear bandits — the bandit analog of forgetting-factor RLS in adaptive control.
- **Garivier-Moulines 2008** sliding-window UCB for non-stationary bandits — earlier, simpler forgetting mechanism.

**Strand 4 — Causal / interventional access (THE Loop-as-Causal-Engine ancestor):**
- **Junzhe Zhang & Bareinboim 2022** (NeurIPS) "Online RL for Mixed Policy Scopes" — sublinear regret with causal-diagram-aware policy scopes. Closest causal-RL paper to AAD's interventional-access framing.
- **Junzhe Zhang & Bareinboim 2016** "MDPs with Unobserved Confounders: A Causal Approach" — formalizes interventional-vs-observational data distinction in MDPs. Foundational.
- **Lu-Meisami-Tewari 2021/2022** "Causal MDPs: Learning Good Interventions Efficiently" — C-UCBVI algorithm with regret scaling on a causal-graph-dependent quantity, exponentially smaller than the action space.
- **Wang-Yang-Wang 2020** "Provably Efficient Causal RL with Confounded Observational Data" — DOVI algorithm explicitly adjusting for confounding bias. Regret strictly improves over pure-online setting when observational data is informative.
- **Junzhe Zhang 2020** "Designing Optimal Dynamic Treatment Regimes" — causal-RL applied to DTRs with regret exponentially smaller than $|D_X \cup S|$ via causal structure.
- **Schulte-Poupart 2025** "When Should RL Use Causal Reasoning?" — most recent meta-analysis of when causal structure helps RL.

**The gap**: none of these address non-stationarity simultaneously with causal-interventional access. Bareinboim's line is stationary; the dynamic-regret line is non-causal. AAD composes the two.

**Information-theoretic regret bounds (closest neighbors, all using non-BH tools):**
- **Russo-Van Roy 2014** "An Information-Theoretic Analysis of Thompson Sampling" — entropy-of-optimal-action regret bound.
- **Russo-Van Roy 2014** "Information-Directed Sampling" — entropy of optimal-action distribution scaling.
- **Lu-Van Roy 2019** "Information-Theoretic Confidence Bounds for RL" — IT confidence bounds for optimistic algorithms / Thompson.
- **Min-Russo 2023** "Information-Theoretic Analysis of Nonstationary Bandit Learning" — bounds limiting per-period regret in terms of entropy rate of the optimal action process. **Closest existing combination of information-theoretic regret + non-stationarity** — but uses Shannon entropy / information ratio, not BH identity.
- **Canonne 2022** "A short note on an inequality between KL and TV" — adjacent to BH but a general statistical-distance result, not RL.

**Satisficing / feasibility-style regret (vocabulary overlap with AAD's satisfaction gap):**
- **Hajiabolhassan-Ortner 2025** *Math. Operations Research* "Online Regret Bounds for Satisficing in MDPs" — constant regret with respect to a satisfaction level $\beta$, generalizing satisficing-bandits to MDPs. **Closest vocabulary** but structurally a different axis: their "satisficing" is "any policy above level $\beta$ is acceptable," not AAD's "the goal is unmet from $M_t$."
- **Yixuan Zhang-Zhu-Xie 2026** "On the Peril of (Even a Little) Nonstationarity in Satisficing Regret Minimization" — most recent (March 2026) satisficing-vs-nonstationarity result. Important late-breaking work to cite.
- **Yang-Zheng-Tomizuka-Liu-Li 2024** "Feasibility Theory of Constrained RL" (already noted above).

## The reframed positioning

Old framing: *"We propose a unified RL convergence theory under non-stationarity composing four elements."*

**New framing (per the prior art):** *"Dynamic-regret RL under drift has matured into a productive but fragmented field, with progress along four largely-separate axes: (1) variation-budget-style regret bounds [Cheung-Simchi-Levi-Zhu 2020, Wei-Luo 2021, Mao et al. 2021]; (2) two-term regret decompositions where the structural axis is exploration-vs-adaptation [Long-Fei Li et al. 2024, Fei-Yang-Wang-Xie 2020, Stradi et al. 2024]; (3) tempo / cadence as an explicit convergence variable [Lee et al. 2023, 2024]; (4) causal/interventional access making regret bounds learnable in stationary settings [Zhang-Bareinboim 2016/2022, Lu-Meisami-Tewari 2021/2022, Wang-Yang-Wang 2020]. None of these have been unified. We give that unification: a non-stationarity-aware convergence theory in which (a) the regret decomposition runs along the goal-feasibility-vs-policy-quality axis (satisfaction-gap vs control-regret), structurally distinct from existing exploration-vs-adaptation decompositions; (b) the Bretagnolle-Huber identity is deployed in its exact-equality form under deterministic π*, giving a tight two-sided regret bound that strictly improves Pinsker — a tool not previously deployed in the RL/non-stationary literature; (c) strategic tempo $\mathcal{T}_\Sigma$ is tied to the rate of useful Σ_t revision under a forgetting prerequisite $(1-\lambda) > \rho_\Sigma/R_\Sigma$ that lifts the Lee et al. tempo result to a structural survival inequality; (d) closed-loop interventional access makes the bound learnable from on-policy data, composing Bareinboim's causal-RL framing with the non-stationary regret literature for the first time."*

That's a much sharper positioning than starting from scratch. The paper writes itself against four well-developed lineages, distinguishing AAD's contribution along each axis.

## What this enables

1. **The novel content lives in four clean places, each defensible:**
   - **Composition itself** — no one has unified all four pieces (Tier 1 negative confirmed).
   - **Bretagnolle-Huber identity in non-stationary RL** — entirely absent from the IT-RL corpus (Russo-Van Roy line, Min-Russo 2023, Lu-Van Roy 2019). The deterministic-π* exact-equality form sharpens existing bounds.
   - **Goal-feasibility vs policy-quality 2×2** as structural axis — distinct from existing exploration-vs-adaptation 2-term decompositions [Long-Fei Li et al. 2024, Fei et al. 2020] and from constraint-feasibility theory [Yang et al. 2024]. Worth carefully distinguishing from satisficing-style work [Hajiabolhassan-Ortner 2025, Y. Zhang-Zhu-Xie 2026].
   - **Forgetting prerequisite** $(1-\lambda) > \rho_\Sigma/R_\Sigma$ as a *survival inequality* with environment-side parameters — structurally distinct from sliding-window/exponential-forgetting uses in Cheung-Simchi-Levi-Zhu, Russac-Vernade-Cappé, Touati-Vincent, Lee et al.

2. **Compression to 9 pages becomes very tractable.** The four-strand structure of the prior art lets the related-work section run as a parallel table:
   - Strand 1 → AAD's regret-decomposition layer
   - Strand 2 → AAD's satisfaction-gap / control-regret 2×2
   - Strand 3 → AAD's strategic tempo + forgetting prerequisite
   - Strand 4 → AAD's Loop-as-Causal-Engine
   Each strand cited and distinguished concisely; no need to motivate any single component from first principles.

3. **Reviewer pushback is bounded.** The most sophisticated reviewer pushback would be:
   - "Isn't this just dynamic regret with another name?" — addressed by the satisfaction-gap-vs-control-regret distinction (different axis from existing two-term decompositions).
   - "Isn't this just Lee et al.'s tempo work?" — addressed by the forgetting-prerequisite survival inequality form (Lee et al. optimize tempo as a hyperparameter; AAD derives a structural threshold).
   - "Isn't this just Bareinboim's causal RL?" — addressed by the simultaneous non-stationarity treatment (Bareinboim's line is stationary; AAD composes).
   - "Why use BH instead of Pinsker?" — addressed by §4.1 of the source segment, which shows BH-identity is *exact* under deterministic π* while Pinsker becomes vacuous beyond $D_{KL} > 2$.
   All four have explicit cite-and-distinguish in the source segments already.

4. **Lee et al. ProST is a particularly strong citation** — they explicitly identify "tempo" as a convergence-relevant variable in non-stationary RL. AAD's strategic tempo + forgetting prerequisite is the structural sharpening of their move with the survival-inequality form. Building on their 2023/2024 NeurIPS / ICML papers gives AAD a clean lineage anchor.

5. **The Bretagnolle-Huber identity gap is the cleanest single novelty** — the entire information-theoretic RL corpus uses Shannon entropy, mutual information, information ratio, Pinsker, or Hellinger [Russo-Van Roy 2014a, 2014b; Lu-Van Roy 2019; Min-Russo 2023; Canonne 2022]. None deploys the deterministic-π* exact-equality form. This is a paper-defining contribution by itself, on top of the unification claim.

The paper is in much better shape than I knew. When you draft, lead with the four-strand structure of the prior art (a single paragraph), then frame AAD's contribution as the unification across the strands, with the four AAD-distinctive moves (BH identity, satisfaction-gap-vs-control-regret 2×2, forgetting prerequisite, Loop-as-Causal-Engine) each cite-and-distinguished against the matching strand.

## Specific drafting moves

- **Open paragraph**: "The non-stationary RL literature has matured along four parallel tracks..." — name them with the canonical citations and frame AAD as their composition.
- **Lead theorem**: the BH-identity-based regret bound under deterministic π*, with the matching lower bound via $\Delta_{\min}$. This lands the strongest single novelty.
- **Section structure**: mirror the four-strand structure. Section 2 → component 1 (2×2). Section 3 → component 2 (BH identity). Section 4 → component 3 (strategic tempo + forgetting). Section 5 → component 4 (Loop-as-Causal-Engine). Section 6 → composition theorem.
- **Anonymization warning**: AAD-specific vocabulary (PROPRIUM, ELI, logogenic, etc.) doesn't show up in B-CS1 territory — the math is RL/control/causal-inference language throughout — so anonymization burden is lower than for B-N8 or B-N4 main-body work.
- **The honest limit**: the paper presents the *composition theorem* under stated assumptions; the formal completeness result (that any non-stationary convergence theory with the four desiderata must reduce to this composition) is *not* proved and shouldn't be claimed. Flag explicitly in the Working Notes.

## Recommended citation budget for the four cite-and-distinguish moves

(approximate — adjust by paper density during drafting)

| Strand | Lead citations | Approximate space |
|---|---|---|
| 1 — dynamic-regret under drift | Cheung-Simchi-Levi-Zhu 2020, Wei-Luo 2021, Mao et al. 2021 | 1 paragraph |
| 2 — two-term regret decompositions | Long-Fei Li-Zhao-Zhou 2024, Fei-Yang-Wang-Xie 2020, Yang-Zheng et al. 2024 | 1 paragraph (extra care on Yang et al. feasibility-theory disambiguation) |
| 3 — tempo / forgetting | **Lee et al. 2023 (ProST)** + Lee et al. 2024 + Touati-Vincent 2020 + Russac-Vernade-Cappé 2019 | 1.5 paragraphs (Lee et al. is the closest neighbor — needs careful cite-and-extend) |
| 4 — causal / interventional | Zhang-Bareinboim 2016/2022, Lu-Meisami-Tewari 2021, Wang-Yang-Wang 2020 | 1 paragraph |
| Cross-cutting — IT regret bounds | Russo-Van Roy 2014a/2014b, Lu-Van Roy 2019, **Min-Russo 2023**, Canonne 2022 | 0.5 paragraph (frames the BH-identity gap) |

The total is ~5 paragraphs of related work, leaving ~7 pages for the composition theorem and four components. Tight but feasible at 9-page Main Track length.
