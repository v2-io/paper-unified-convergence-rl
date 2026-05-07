## Pinsker vs.\ Point-Mass Identity Numerical Comparison ^sec-pinsker-numerics

We compare $V_{\max}(1 - e^{-D_{\mathrm{KL}}})$ (point-mass identity bound, exact under deterministic $\pi^*$ per [[#^thm-twosided-regret]]) with $V_{\max}\sqrt{D_{\mathrm{KL}}/2}$ (Pinsker bound, also valid under deterministic $\pi^*$ but loose), $V_{\max}\sqrt{1 - e^{-D_{\mathrm{KL}}}}$ (Bretagnolle--Huber inequality, the general envelope below which the identity sits at this corner), and the trivial envelope $V_{\max}$. Set $V_{\max} = 1$ for normalization.

| $D_{\mathrm{KL}}$ | $1 - e^{-D_{\mathrm{KL}}}$ (identity) | $\sqrt{1 - e^{-D_{\mathrm{KL}}}}$ (BH) | $\sqrt{D_{\mathrm{KL}}/2}$ (Pinsker) | $\min(\sqrt{D_{\mathrm{KL}}/2}, 1)$ | Pinsker / identity ratio |
|---|---|---|---|---|---|
| 0.01 | 0.00995 | 0.0998 | 0.0707 | 0.0707 | 7.10× |
| 0.1 | 0.0952 | 0.308 | 0.224 | 0.224 | 2.35× |
| 0.5 | 0.393 | 0.627 | 0.500 | 0.500 | 1.27× |
| 1.0 | 0.632 | 0.795 | 0.707 | 0.707 | 1.12× |
| 2.0 | 0.865 | 0.930 | 1.000 | 1.000 | 1.16× (Pinsker = trivial) |
| 4.0 | 0.982 | 0.991 | 1.414 | 1.000 | 1.02× (Pinsker vacuous) |
| 10.0 | 0.99995 | 0.99998 | 2.236 | 1.000 | 1.00× (Pinsker fully vacuous) |

The point-mass identity bound is uniformly tighter than Pinsker, by a factor of $7\times$ at $D_{\mathrm{KL}} = 0.01$ and converging to $1$ as $D_{\mathrm{KL}}$ grows large (where both saturate at $V_{\max}$). It is also uniformly tighter than the BH inequality $\sqrt{1 - e^{-D_{\mathrm{KL}}}}$ — the relation $x < \sqrt{x}$ on $(0, 1)$ — although both saturate together at $V_{\max}$ for large $D_{\mathrm{KL}}$, where BH stays informative while Pinsker becomes vacuous (exceeds the trivial $V_{\max}$ envelope) for $D_{\mathrm{KL}} > 2$.

The matching lower bound $\Delta_{\min}(1 - e^{-D_{\mathrm{KL}}})$ has the same shape, scaled by $\Delta_{\min}/V_{\max}$. In the extremal value landscape ($\Delta_{\min} = V_{\max}$), the upper and lower bounds coincide and the regret is *identified exactly* by the KL coordinate. For typical landscapes, regret and KL are Lipschitz-equivalent with constants $\Delta_{\min}/V_{\max}$ and $1$.
