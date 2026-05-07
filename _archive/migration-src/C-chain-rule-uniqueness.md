## Chain-Rule Uniqueness of Reverse-KL ^sec-chain-rule-uniqueness

> [!theorem] Chain-rule uniqueness (Hobson 1969; Csiszár 1991) ^thm-chain-rule-uniqueness
> Let $D_f(P \,\|\, Q) = \sum_x Q(x) f(P(x)/Q(x))$ be a smooth $f$-divergence with $f$ convex and $f(1) = 0$. The chain rule
> $$D_f(P_{XY} \,\|\, Q_{XY}) \;=\; D_f(P_X \,\|\, Q_X) \;+\; \mathbb E_{P_X}\!\left[D_f(P_{Y|X} \,\|\, Q_{Y|X})\right]$$
> holds for all joint distributions if and only if $f(t) = c \cdot t \log t$ for some $c > 0$ — i.e., $D_f$ is reverse-KL up to positive scaling.

> [!proof]
> Writing $r_x = P(x)/Q(x)$ and $s_{y|x} = P(y|x)/Q(y|x)$, the chain rule reduces to the functional equation $f(rs) = f(r) + r f(s)$ for all $r, s > 0$. With $f(1) = 0$ and convexity, the unique solution is $f(t) = c \cdot t \log t$ for $c > 0$ \cite{aczel-1975-measures} (§4).

**References.** Hobson 1969 ("A new theorem of information theory," *J.\ Stat.\ Phys.*); Csiszár 1991 ("Why least squares and maximum entropy?" *Annals of Statistics*; Theorem 3 corollary, Theorem 5); Shore-Johnson 1980 ("Axiomatic derivation of the principle of maximum entropy," *IEEE Trans.\ Info.\ Theory*, system-independence axiom); Sanov 1957 (large-deviation rate function); Aczél-Daróczy 1975 (functional-equation machinery).

These references give *structurally equivalent reformulations* of the same axiom. The Cauchy functional equation each reduces to is the common content. No known uniqueness route outside the independence-on-sub-problems family exists.

**Why other family members fail the chain rule.** Concrete counterexample for $\chi^2$: take $Q_X$ uniform on $\{x_1, x_2\}$, $P_X = (3/4, 1/4)$, $Q(y|x)$ uniform, $P(y|x) = (3/4, 1/4)$. Direct calculation gives $\chi^2(P_{XY} \,\|\, Q_{XY}) = 9/16$ while $\chi^2(P_X \,\|\, Q_X) + \mathbb E_{P_X}[\chi^2(P_{Y|X} \,\|\, Q_{Y|X})] = 1/4 + 1/4 = 8/16$. Non-additive. Rényi-$\alpha$ for $\alpha \neq 1$ fails analogously; squared Hellinger likewise fails.
