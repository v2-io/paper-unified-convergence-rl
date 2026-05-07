## Conclusion ^sec-conclusion

The four-component composition delivers three jointly-novel properties — non-stationarity persistence, explicit metric structure on policy space, on-policy learnability — and a Best-of-Both-Worlds rate $\tilde O(\min\{V_{\max} N_h \sqrt{(B_T+1) T},\, V_{\max} N_h (V_T+1)^{1/3} T^{2/3}\})$ adapting between $B_T$ and $V_T$ via Wei-Luo MASTER wrapping ([[#^sec-proof-composition]]). The $V_T$ exponent is information-theoretically near-optimal at the deterministic-$\pi^*$ corner per \cite{besbes-gur-zeevi-2014-stochastic}; the per-round identity sharpens constants and per-round form.

**On coupled-goal architectures.** (C2) presupposes architectural separation between $M_t$ and the goal state — goal-conditioned LLM policies violate this and need additional machinery (\cite{bruineberg-dolega-dewhurst-baltieri-2022-bbs} as starting point).

**Open directions.** An *agent-experienced* variation budget $V_T^{\mathrm{eff},\Sigma}$ or identity-coordinate variation $V_T^{(K)}$ would explain why agents in slow-drift environments outperform $B_T$-pessimistic predictions; Bernstein-type concentration \cite{azar-2017-minimax} could shave one $\sqrt{N_h}$ factor from the lifted rate; joint uniqueness across all three properties and strategic-tempo beyond Model (Σ) remain open ([[#^thm-chain-rule-uniqueness]]).

**Practitioner takeaway.** Use $\mathcal T_\Sigma^{\mathrm{agg,ss}} > |E|\rho_\Sigma/R_\Sigma$ as a fail-fast pre-check; route corrective action via the 2$\times$2 diagnostic; apply the per-round identity wherever deterministic-$\pi^*$ scope holds.
