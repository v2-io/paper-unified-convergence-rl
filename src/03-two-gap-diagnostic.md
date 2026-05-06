## Component 1 — The Two-Gap Diagnostic ^sec-two-gap-diagnostic

### The two gaps ^sec-the-two-gaps

Let $\Pi$ denote the policy class available to the agent and let $V_O^{\min}$ denote the threshold trajectory value above which the objective is considered "met" — a property of the objective, set by the application domain (for constraints: all satisfied; for utility maximization: a minimum acceptable value).

**Objective attainability.**
$$A_O(M_t;\, \Pi, N_h) \;:=\; \sup_{\pi \in \Pi} V_O(M_t, \pi;\, N_h).$$
$A_O$ is the best achievable value given the agent's beliefs, available policies, and horizon.

**Satisfaction gap** (goal-feasibility diagnostic):
$$\delta_{\mathrm{sat}} \;:=\; V_O^{\min} - A_O(M_t;\, \Pi, N_h).$$
$\delta_{\mathrm{sat}} > 0$ means the objective is *unmet* under the best available policy. The signal does not automatically mean the goal is wrong: it means the goal is unmet *given* $(M_t, \Pi, N_h)$.

**Control regret** (policy-quality diagnostic):
$$\delta_{\mathrm{regret}} \;:=\; A_O(M_t;\, \Pi, N_h) - V_O(M_t, \pi_{\mathrm{current}};\, N_h) \;\ge\; 0.$$
$\delta_{\mathrm{regret}}$ measures the gap between the best available policy improvement and the agent's current policy. It is non-negative by the supremum definition of $A_O$.

The two quantities answer different diagnostic questions: $\delta_{\mathrm{sat}}$ asks "is the goal unattainable from here?"; $\delta_{\mathrm{regret}}$ asks "is the current policy suboptimal?" These can be true simultaneously, neither, or in either combination. The decomposition runs along a *goal-feasibility-vs-policy-quality axis* — distinct from constraint-region feasibility [Yang-Zheng-Tomizuka-Liu-Li 2024] (state stays in constraint set; our $\delta_{\mathrm{sat}}$ is objective threshold attainable from $M_t$) and from satisficing-MDP [Hajiabolhassan-Ortner 2025; Y. Zhang-Zhu-Xie 2026] ("any policy $\ge \beta$ acceptable"; our $\delta_{\mathrm{sat}}$ signals goal-relative infeasibility, not policy-relative satisficing).

### The $2{\times}2$ disambiguation ^sec-2x2-disambiguation

The $2{\times}2$ cross of the two gaps is the load-bearing diagnostic:

| | $\delta_{\mathrm{sat}} \le 0$ (attainable) | $\delta_{\mathrm{sat}} > 0$ (unmet) |
|---|---|---|
| $\delta_{\mathrm{regret}} \approx 0$ (near-optimal policy) | **Success**: goal achievable, policy good | **Capability limit**: optimally pursuing an unmet goal |
| $\delta_{\mathrm{regret}} \gg 0$ (suboptimal policy) | **Strategy problem**: goal achievable, policy poor | **Both**: goal hard *and* policy weak |

Each cell prescribes a different corrective action:

- **Success.** No corrective action.
- **Strategy problem.** Revise the policy. The signal is informative about *which* policy revision: the strategic-calibration residual decomposes $\delta_{\mathrm{regret}}$ over the policy's structural elements (Appendix A).
- **Capability limit.** $\delta_{\mathrm{regret}} \approx 0$ rules out *the policy* as the source of failure. The remaining candidates are the model ($M_t$ may be wrong about feasibility), the policy class ($\Pi$ may be too narrow), the horizon ($N_h$ may be too short), or the objective itself (revising $O$ as last resort). The order matters: $M_t$ first, then $\Pi$ and $N_h$, then $O$.
- **Both.** Revise the policy first (cheaper, more likely to be the issue), then re-evaluate $\delta_{\mathrm{sat}}$.

### Convention dependence ^sec-convention-dependence

$\delta_{\mathrm{sat}}, \delta_{\mathrm{regret}}$ depend on the continuation convention $\pi_{\mathrm{cont}}$. Default is one-step improvement (C1, [[#^sec-setup]]); receding-horizon (C2) and Bellman (C3) tighten the diagnostic at increasing cost. The monotonicity $\delta_{\mathrm{sat}}^{\mathrm B} \le \delta_{\mathrm{sat}}^{\mathrm{RH}} \le \delta_{\mathrm{sat}}^{(1)}$ (with reversed inequality on $\delta_{\mathrm{regret}}$) is proved in [[#^sec-convention-hierarchy]]. The $2{\times}2$ structure is preserved across all three; only the inferential force of "infeasible" vs.\ "locally stuck" varies. Analyses must state the convention.
