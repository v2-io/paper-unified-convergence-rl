## Related Work ^sec-related-work

The four strands of prior art map onto our four components; none composes all four.

| Strand | Closest neighbors | Our distinction |
|---|---|---|
| 1. Dynamic regret under drift | [Cheung-Simchi-Levi-Zhu 2020; Wei-Luo 2021; Mao-Zhang-Zhu-Simchi-Levi-Başar 2021; Cheung-Simchi-Levi-Zhu 2022; Gajane-Ortner-Auer 2019] | Recovered as instances under the forgetting prerequisite ([[#^sec-prost-lift]]); novelty is the *threshold form* — environment regimes where no $\lambda \in (0,1)$ stabilizes the sector-Lyapunov mismatch — which dynamic-regret-optimization analyses do not surface. |
| 2. Two-term decompositions | [Long-Fei Li-Zhao-Zhou 2024; Fei-Yang-Wang-Xie 2020; Stradi-Lunghi-Castiglioni-Marchesi-Gatti 2024]; \cite{yang-2024-feasibility} (constraint-feasibility) | Same shape, different axis: ours is goal-feasibility vs.\ policy-quality; theirs is uncertainty-source (or constraint-region feasibility for Yang et al.). Neither subsumes the other. |
| 3. Tempo and forgetting | [Lee et al.\ 2023 ProST; Lee et al.\ 2024; Touati-Vincent 2020; Russac-Vernade-Cappé 2019; Garivier-Moulines 2008] | Lifts ProST's single-factor tempo schedule to a multi-factor structural threshold ([[#^sec-prost-lift]]); recasts forgetting from tunable hyperparameter to survival inequality. |
| 4. Causal / interventional access | [Junzhe Zhang-Bareinboim 2022, 2016; Lu-Meisami-Tewari 2021, 2022; Wang-Yang-Wang 2021 DOVI; Junzhe Zhang 2020; Schulte-Poupart 2025] | Causal-RL line is stationary throughout; we compose with non-stationarity. |
| Cross-cutting (info-theoretic regret) | [Russo-Van Roy 2014a, 2014b; Lu-Van Roy 2019; Min-Russo 2023; Lattimore-György 2021; Canonne 2022; Kakade-Krishnamurthy-Lowrey-Ohnishi-Sun 2020] | Uses entropy / mutual information / Pinsker / Hellinger; the BH inequality and its point-mass exact corner ([[#^thm-pointmass-identity]]) are absent from this corpus ([[#^sec-prior-art-summary]]). |
| Adjacent (satisficing / feasibility) | [Hajiabolhassan-Ortner 2025; Y. Zhang-Zhu-Xie 2026] | Vocabulary overlap on "satisficing"; their axis is "any policy $\ge \beta$ acceptable," ours is "goal unmet from $M_t$" ([[#^sec-two-gap-diagnostic]]). |
| Contemporaneous (post-March 2026) | [Gerogiannis-Huang-Veeravalli 2026 DARLING; Y. Zhang-Zhu-Xie 2026] | Adjacent; neither composes the four components. |
