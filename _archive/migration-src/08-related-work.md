## Related Work ^sec-related-work

The four strands of prior art map onto our four components; none composes all four.

| Strand | Closest neighbors | Our distinction |
|---|---|---|
| 1. Dynamic regret under drift | \cite{cheung-2020-reinforcement, wei-luo-2021-blackbox, mao-2021-nearoptimal, cheung-simchi-levi-zhu-2022-blessing, gajane-2019-variational} | Recovered as instances under the forgetting prerequisite ([[#^sec-prost-lift]]); novelty is the *threshold form* — environment regimes where no $\lambda \in (0,1)$ stabilizes the sector-Lyapunov mismatch — which dynamic-regret-optimization analyses do not surface. |
| 2. Two-term decompositions | [Long-Fei Li-Zhao-Zhou 2024; Fei-Yang-Wang-Xie 2020; Stradi-Lunghi-Castiglioni-Marchesi-Gatti 2024]; \cite{yang-2024-feasibility} (constraint-feasibility) | Same shape, different axis: ours is goal-feasibility vs.\ policy-quality; theirs is uncertainty-source (or constraint-region feasibility for Yang et al.). Neither subsumes the other. |
| 3. Tempo and forgetting | \cite{lee-2023-prost-tempo} (ProST); \cite{lee-2024-pausing, touati-2020-efficient, russac-2019-weighted, garivier-2008-upperconfidence} | Lifts ProST's single-factor tempo schedule to a multi-factor structural threshold ([[#^sec-prost-lift]]); recasts forgetting from tunable hyperparameter to survival inequality. |
| 4. Causal / interventional access | \cite{zhang-2022-online-rl, zhang-2016-mdps, lu-2021-causal, lu-2022-efficient, wang-2021-provably} (DOVI); \cite{zhang-2020-designing, schulte-poupart-2025-causal-rl} | Causal-RL line is stationary throughout; we compose with non-stationarity. |
| Cross-cutting (info-theoretic regret) | \cite{russo-2014-informationtheoretic, russo-vanroy-2014-ids, lu-2019-informationtheoretic, min-russo-2023-nonstat-bandit, lattimore-2021-mirror, canonne-2022-short, kakade-2020-information-online} | Uses entropy / mutual information / Pinsker / Hellinger; the BH inequality and its point-mass exact corner ([[#^thm-pointmass-identity]]) are absent from this corpus ([[#^sec-prior-art-summary]]). |
| Adjacent (satisficing / feasibility) | \cite{hajiabolhassan-2025-online, zhang-2026-peril} | Vocabulary overlap on "satisficing"; their axis is "any policy $\ge \beta$ acceptable," ours is "goal unmet from $M_t$" ([[#^sec-two-gap-diagnostic]]). |
| Contemporaneous (post-March 2026) | \cite{gerogiannis-2026-darling} (DARLING); \cite{zhang-2026-peril} | Adjacent; neither composes the four components. |
