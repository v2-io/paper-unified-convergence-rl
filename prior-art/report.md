# Unified non-stationary convergence theory prior art

##### [**Undermind**](https://undermind.ai)

---

**Research Goal:** Investigate whether any prior academic framework across reinforcement learning, bandits, online learning, adaptive control, decision theory, causal decision-making, and active-inference-adjacent literatures explicitly unifies a non-stationarity-aware convergence theory by composing all four of the following elements in a single derivation chain: (1) a two-gap diagnostic that separates environmental or goal-feasibility limitation from agent suboptimality (satisfaction-gap versus control-regret), ideally with a 2×2 disambiguation; (2) use of the Bretagnolle-Huber identity in regret analysis, especially whether the deterministic-optimum case has been used to obtain an exact-equality form rather than only inequality-based bounds; (3) a strategic-tempo or closely analogous quantity tying convergence to the rate of useful policy or strategy revision and to information acquisition under forgetting or drift; and (4) a closed-loop causal or interventional account in which on-policy interaction provides the data needed to make regret bounds learnable rather than merely analytically provable. Use a split standard. For direct anticipation, require an explicit composition of all four elements within one framework, allowing renamed equivalents only when the full four-part structure is clearly present. For closest prior art, include frameworks that explicitly compose three of the four elements with the fourth only adjacent, and distinguish these from looser adjacency where ingredients are present only in isolation. Pay particular attention to: whether any prior work rigorously formalizes a goal-feasibility versus policy-quality decomposition in RL, constrained or safe RL, decision theory, or active-inference-adjacent work; whether Bretagnolle-Huber has been used in RL or online-learning regret bounds and whether the deterministic-optimum exact-equality form has been deployed; whether adaptive-control or drifting-environment literatures contain structural analogs of a tempo or forgetting-survival prerequisite; and whether any causal RL or causal decision-making work uses closed-loop interventional access to argue that regret bounds are learnable from on-policy data. The desired outcome is a structured prior-art map that separates direct anticipation, closest 3-out-of-4 candidates, and adjacent literatures, with an evidence register that distinguishes full-text support from abstract-level support and an honest statement of search scope and remaining uncertainty.

*Found 63 papers · May 4, 2026 · Estimated coverage of relevant papers: 75%*

## Summary of Results

No retrieved paper explicitly composes all four target elements into one non-stationary convergence theory; the landscape instead splits into largely separate strands: dynamic-regret under drift \[1\], \[2\], \[3\], \[4\], feasibility/satisficing decompositions \[5\], \[6\], tempo/forgetting analyses \[7\], \[8\], \[9\], \[10\], and causal/interventional sample-efficiency arguments \[11\], \[12\], \[13\], \[14\], \[15\].

#### Direct anticipation

- **None found** in the retrieved set.
- In particular, no abstract-level evidence shows a single derivation chaining: feasibility-vs-suboptimality decomposition, Bretagnolle-Huber, tempo/forgetting, and closed-loop causal learnability.

#### Closest candidates

- **Two-term non-stationarity decompositions:** dynamic regret split into exploration/transition-estimation and adaptation/suboptimal-policy terms in \[16\], \[17\]. This is the clearest structural precursor to a “control-regret” factorization, but not a feasibility/satisfaction-gap theory.
- **Tempo as an explicit convergence variable:** \[7\], \[8\] make policy-update timing itself an optimization variable, linking regret to policy revision rate under environmental change; \[9\], \[10\] supply the forgetting analogue via exponential weighting/sliding windows.
- **Closed-loop causal/interventional learning:** \[11\], \[12\], \[13\], \[14\] argue that active intervention or causal structure sharpens regret/sample complexity from on-policy interaction, but not in a non-stationary unified theory.

#### Missing element with strongest negative signal

- **Bretagnolle-Huber** does not appear in the retrieved RL/non-stationary abstracts; information-theoretic analyses rely instead on entropy, information ratio, or Hellinger/KL tools \[18\], \[19\], \[20\], \[21\]. No evidence here of the deterministic-optimum exact-equality use.

#### Scope / evidence status

- Support above is **abstract/snippet-level** from the retrieved papers only; full-text checking is still needed mainly for hidden use of Bretagnolle-Huber and any implicit feasibility/control-regret decomposition inside proofs.

## Paper Catalog (63 papers)

|  | Year | Cit/yr | Title | Authors | Journal |
|---:|:--:|:--:|:---|:---|:---|
| 1 | 2019 | 9.8 | Variational Regret Bounds for Reinforcement Learning ([link](https://www.semanticscholar.org/paper/16cdf928ac852257497f97c19f71eac802aab37d)) | Pratik Gajane, R. Ortner, and P. Auer | Conference on Uncertainty in Artificial Intelligence |
| 2 | 2024 | 0.5 | The Feasibility Theory of Constrained Reinforcement Learning: A Tutorial Study ([link](https://doi.org/10.1108/FTSYS-03-2026-001)) | Yujie Yang, Zhilong Zheng, Masayoshi Tomizuka, Changliu Liu, and S. Li |  |
| 3 | 2021 | 25 | Non-stationary Reinforcement Learning without Prior Knowledge: An Optimal Black-box Approach ([link](https://www.semanticscholar.org/paper/dc902589d77374364e1540885e6267939cc0d17c)) | Chen-Yu Wei and Haipeng Luo | ArXiv |
| 4 | 2020 | 2.8 | Online learning with dynamics: A minimax perspective ([link](https://www.semanticscholar.org/paper/59101599546a2e27deac59eefbd8627f2a653025)) | K. Bhatia and Karthik Sridharan | ArXiv |
| 5 | 2020 | 19 | Reinforcement Learning for Non-Stationary Markov Decision Processes: The Blessing of (More) Optimism ([link](https://www.semanticscholar.org/paper/5b3e68658c99ed9c461a909b16b862221946d6ad)) | Wang Chi Cheung, D. Simchi-Levi, and Ruihao Zhu | ArXiv |
| 6 | 2023 | 6.7 | Provably Efficient Model-Free Algorithms for Non-stationary CMDPs ([link](https://doi.org/10.48550/arXiv.2303.05733)) | Honghao Wei, A. Ghosh, N. Shroff, Lei Ying, and Xingyu Zhou | International Conference on Artificial Intelligence and Statistics |
| 7 | 2024 | 1.4 | Dynamic Regret of Adversarial MDPs with Unknown Transition and Linear Function Approximation ([link](https://doi.org/10.1609/aaai.v38i12.29261)) | Long-Fei Li, Peng Zhao, and Zhi-Hua Zhou | AAAI Conference on Artificial Intelligence |
| 8 | 2023 | 11 | Provably Efficient Primal-Dual Reinforcement Learning for CMDPs with Non-stationary Objectives and Constraints ([link](https://doi.org/10.1609/aaai.v37i6.25900)) | Yuhao Ding and J. Lavaei | ArXiv |
| 9 | 2022 | 2.8 | Provably Efficient Primal-Dual Reinforcement Learning for CMDPs with Non-stationary Objectives and Constraints ([link](https://www.semanticscholar.org/paper/1248dd2f6c2972e399ce11fc5768692e83c14a19)) | Yuhao Ding and J. Lavaei |  |
| 10 | 2020 | 12 | Dynamic Regret of Policy Optimization in Non-stationary Environments ([link](https://www.semanticscholar.org/paper/7c04e2a43c387b1fc9b4a1b603f6b9a19cd3cfd7)) | Yingjie Fei, Zhuoran Yang, Zhaoran Wang, and Qiaomin Xie | ArXiv |
| 11 | 2020 | 6.0 | Efficient Learning in Non-Stationary Linear Markov Decision Processes ([link](https://www.semanticscholar.org/paper/75d439faf8af5b9080a8955d85da9ca49929fc01)) | Ahmed Touati and Pascal Vincent | ArXiv |
| 12 | 2021 | 4.8 | Optimistic Policy Optimization is Provably Efficient in Non-stationary MDPs ([link](https://www.semanticscholar.org/paper/4a3ea0697e10b97a191d465fb465b1c1a035cd98)) | Han Zhong, Zhuoran Yang, Zhaoran Wang, and Csaba Szepesvari | ArXiv |
| 13 | 2023 | 1.9 | Tempo Adaptation in Non-stationary Reinforcement Learning ([link](https://doi.org/10.52202/075280-0362)) | Hyunin Lee et al. | Advances in Neural Information Processing Systems 36 |
| 14 | 2024 | 2.1 | Pausing Policy Learning in Non-stationary Reinforcement Learning ([link](https://doi.org/10.48550/arXiv.2405.16053)) | Hyunin Lee, Ming Jin, Javad Lavaei, and S. Sojoudi | International Conference on Machine Learning |
| 15 | 2021 | 9.2 | Near-Optimal Model-Free Reinforcement Learning in Non-Stationary Episodic MDPs ([link](https://www.semanticscholar.org/paper/190a29a8699a82d780f0de0bcd5bdb52626d3c72)) | Weichao Mao, K. Zhang, Ruihao Zhu, D. Simchi-Levi, and T. Başar | International Conference on Machine Learning |
| 16 | 2014 | 4.7 | Online Markov Decision Processes With Kullback–Leibler Control Cost ([link](https://doi.org/10.1109/TAC.2014.2301558)) | Peng Guan, M. Raginsky, and R. Willett | IEEE Transactions on Automatic Control |
| 17 | 2024 | 2.6 | Learning Constrained Markov Decision Processes With Non-stationary Rewards and Constraints ([link](https://doi.org/10.48550/arXiv.2405.14372)) | Francesco Emanuele Stradi, Anna Lunghi, Matteo Castiglioni, A. Marchesi, and Nicola Gatti | ArXiv |
| 18 | 2023 |  | Tempo Adaption in Non-stationary Reinforcement Learning ([link](https://doi.org/10.48550/arXiv.2309.14989)) | Hyunin Lee et al. | ArXiv |
| 19 | 2020 | 5.9 | Nonstationary Reinforcement Learning with Linear Function Approximation ([link](https://www.semanticscholar.org/paper/750228d26cb6a33bd7393372983edfde1e09733b)) | Huozhi Zhou, Jinglin Chen, L. Varshney, and A. Jagmohan | Trans. Mach. Learn. Res. |
| 20 | 2021 | 0.2 | Online Sub-Sampling for Reinforcement Learning with General Function Approximation ([link](https://www.semanticscholar.org/paper/668d882897d1dfd3cc4b59823388138c0764a46f)) | Dingwen Kong, R. Salakhutdinov, Ruosong Wang, and Lin Yang |  |
| 21 | 2023 | 3.4 | An Information-Theoretic Analysis of Nonstationary Bandit Learning ([link](https://doi.org/10.48550/arXiv.2302.04452)) | Seungki Min and Daniel Russo | International Conference on Machine Learning |
| 22 | 2020 | 23 | Information Theoretic Regret Bounds for Online Nonlinear Control ([link](https://www.semanticscholar.org/paper/458d4f3d398da068493c63687e285b691514dff5)) | S. Kakade, A. Krishnamurthy, Kendall Lowrey, Motoya Ohnishi, and Wen Sun | ArXiv |
| 23 | 2014 | 6.2 | Online Learning in Markov Decision Processes with Changing Cost Sequences ([link](https://doi.org/10.14288/1.0044649)) | Travis Dick, A. György, and Csaba Szepesvari | International Conference on Machine Learning |
| 24 | 2022 | 5.7 | Dynamic Regret of Online Markov Decision Processes ([link](https://doi.org/10.48550/arXiv.2208.12483)) | Peng Zhao, Longfei Li, and Zhi-Hua Zhou | International Conference on Machine Learning |
| 25 | 2025 | 1.0 | Online Regret Bounds for Satisficing in Markov Decision Processes ([link](https://doi.org/10.1287/moor.2023.0275)) | Hossein Hajiabolhassan and Ronald Ortner | Mathematics of Operations Research |
| 26 | 2020 | 0.6 | Counterfactual Programming for Optimal Control ([link](https://www.semanticscholar.org/paper/201b46b8f046329fecd316d182c4afe3714f8222)) | Luiz F. O. Chamon, Santiago Paternain, and Alejandro Ribeiro | Conference on Learning for Dynamics & Control |
| 27 | 2026 |  | DARLING: Detection Augmented Reinforcement Learning with Non-Stationary Guarantees ([link](https://www.semanticscholar.org/paper/60026b5b0f7605e6b6e1872171f87bf9c4fefbf8)) | A. Gerogiannis, Yu-Han Huang, and Venugopal V. Veeravalli |  |
| 28 | 2014 | 37 | Stochastic Multi-Armed-Bandit Problem with Non-stationary Rewards ([link](https://www.semanticscholar.org/paper/2295f2034c2a45a39fd1a08605d2a8e0588e7e4d)) | Y. Gur, A. Zeevi, and Omar Besbes | Neural Information Processing Systems |
| 29 | 2026 |  | On the Peril of (Even a Little) Nonstationarity in Satisficing Regret Minimization ([link](https://www.semanticscholar.org/paper/fb3f0e605737c837131d0e72bc841a82056fa106)) | Yixuan Zhang, Ruihao Zhu, and Qiaomin Xie |  |
| 30 | 2013 | 38 | Non-Stationary Stochastic Optimization ([link](https://doi.org/10.1287/opre.2015.1408)) | Omar Besbes, Y. Gur, and A. Zeevi | Oper. Res. |
| 31 | 2013 | 9.8 | Online Learning with Switching Costs and Other Adaptive Adversaries ([link](https://www.semanticscholar.org/paper/8a1d0d8f2cce1a180c6b41c733262fe81ce35a9c)) | N. Cesa-Bianchi, O. Dekel, and Ohad Shamir | ArXiv |
| 32 | 2012 | 2.5 | Deterministic MDPs with Adversarial Rewards and Bandit Feedback ([link](https://www.semanticscholar.org/paper/2b8ac3708075d0e35a0e4640807624376811fb79)) | R. Arora, O. Dekel, and Ambuj Tewari | Conference on Uncertainty in Artificial Intelligence |
| 33 | 2022 | 2.3 | Online Reinforcement Learning for Mixed Policy Scopes ([link](https://doi.org/10.52202/068431-0231)) | Junzhe Zhang and E. Bareinboim | Advances in Neural Information Processing Systems 35 |
| 34 | 2019 | 10 | Information-Theoretic Confidence Bounds for Reinforcement Learning ([link](https://www.semanticscholar.org/paper/3231ac937b2620cd3ea7c39fdacaf416a558d31c)) | Xiuyuan Lu and Benjamin Van Roy | Neural Information Processing Systems |
| 35 | 2021 | 10 | Non-stationary Online Learning with Memory and Non-stochastic Control ([link](https://www.semanticscholar.org/paper/d2ad60fe398784082cc2777208fe13a5fe163b55)) | Peng Zhao, Yu-Xiang Wang, and Zhi-Hua Zhou | International Conference on Artificial Intelligence and Statistics |
| 36 | 2020 | 13 | Designing Optimal Dynamic Treatment Regimes: A Causal Reinforcement Learning Approach ([link](https://www.semanticscholar.org/paper/162e03526f99e8a844022590ce1001d7f1987de1)) | Junzhe Zhang | International Conference on Machine Learning |
| 37 | 2019 | 2.9 | Online Learning for Markov Decision Processes in Nonstationary Environments: A Dynamic Regret Analysis ([link](https://doi.org/10.23919/ACC.2019.8815000)) | Yingying Li and Na Li | 2019 American Control Conference (ACC) |
| 38 | 2020 | 9.9 | Provably Efficient Causal Reinforcement Learning with Confounded Observational Data ([link](https://www.semanticscholar.org/paper/2d6937f8421d4d793ee0f03d3c60c6e794b25c36)) | Lingxiao Wang, Zhuoran Yang, and Zhaoran Wang | Neural Information Processing Systems |
| 39 | 2020 | 3.0 | A Duality Approach for Regret Minimization in Average-Award Ergodic Markov Decision Processes ([link](https://www.semanticscholar.org/paper/c758fb4bc55336c745a7bb6d13b2f99cc3a2b5e3)) | Hao Gong and Mengdi Wang | Conference on Learning for Dynamics & Control |
| 40 | 2019 | 14 | Hedging the Drift: Learning to Optimize under Non-Stationarity ([link](https://doi.org/10.1287/mnsc.2021.4024)) | Wang Chi Cheung, D. Simchi-Levi, and Ruihao Zhu | Manag. Sci. |
| 41 | 2021 | 2.3 | Causal Markov Decision Processes: Learning Good Interventions Efficiently ([link](https://www.semanticscholar.org/paper/7990ed58591c23dbef01bc8010220d20c13156d2)) | Yangyi Lu, A. Meisami, and Ambuj Tewari | ArXiv |
| 42 | 2022 | 6.3 | A New Look at Dynamic Regret for Non-Stationary Stochastic Bandits ([link](https://www.semanticscholar.org/paper/1d3ecde233d3a5e7ea64e63b2131bae8aae7180d)) | Yasin Abbasi-Yadkori, A. György, and N. Lazic | J. Mach. Learn. Res. |
| 43 | 2022 | 6.0 | Efficient Reinforcement Learning with Prior Causal Knowledge ([link](https://www.semanticscholar.org/paper/ec0b078243ad06fceda7cfd55ba8201953250cd5)) | Yangyi Lu and Ambuj Tewari | CLEaR |
| 44 | 2019 | 20 | A New Algorithm for Non-stationary Contextual Bandits: Efficient, Optimal, and Parameter-free ([link](https://www.semanticscholar.org/paper/0940c55435b3b12be85b90c140c703ec39fb0be7)) | Yifang Chen, Chung-Wei Lee, Haipeng Luo, and Chen-Yu Wei | ArXiv |
| 45 | 2016 | 5.1 | Markov Decision Processes with Unobserved Confounders : A Causal Approach ([link](https://www.semanticscholar.org/paper/69c68d804e7c052665d5b4049c0d9c9d8baa11c0)) | Junzhe Zhang and E. Bareinboim |  |
| 46 | 2018 | 21 | Learning to Optimize under Non-Stationarity ([link](https://doi.org/10.2139/SSRN.3261050)) | Wang Chi Cheung, D. Simchi-Levi, and Ruihao Zhu | ArXiv |
| 47 | 2023 | 4.1 | A Definition of Non-Stationary Bandits ([link](https://doi.org/10.48550/arXiv.2302.12202)) | Yueyang Liu, Benjamin Van Roy, and Kuang Xu | ArXiv |
| 48 | 2013 | 0.2 | Relax but stay in control: from value to algorithms for online Markov decision processes ([link](https://www.semanticscholar.org/paper/046f7cbdef9147411a5b3223325653fbcafd6caf)) | Peng Guan, M. Raginsky, and R. Willett | ArXiv |
| 49 | 2014 | 38 | An Information-Theoretic Analysis of Thompson Sampling ([link](https://www.semanticscholar.org/paper/de6c988f7a6962a09a1c11f41ded0b63a5418559)) | Daniel Russo and Benjamin Van Roy | J. Mach. Learn. Res. |
| 50 | 2014 | 26 | Learning to Optimize via Information-Directed Sampling ([link](https://doi.org/10.1287/opre.2017.1663)) | Daniel Russo and Benjamin Van Roy | Oper. Res. |
| 51 | 2020 | 8.0 | Mirror Descent and the Information Ratio ([link](https://www.semanticscholar.org/paper/ce4d6b23ee5bbd2f7acb8223ee73a23f715df887)) | Tor Lattimore and A. György | Annual Conference Computational Learning Theory |
| 52 | 2013 | 45 | (More) Efficient Reinforcement Learning via Posterior Sampling ([link](https://www.semanticscholar.org/paper/789783016fb708abbc061790612ebe91273c05d3)) | Ian Osband, Daniel Russo, and Benjamin Van Roy | Neural Information Processing Systems |
| 53 | 2021 | 7.1 | Revisiting Smoothed Online Learning ([link](https://www.semanticscholar.org/paper/034881a501ed487e769e50d70fe5a8c5b6a01096)) | Lijun Zhang, Wei Jiang, Shiyin Lu, and Tianbao Yang | ArXiv |
| 54 | 2021 | 9.6 | Causal Bandits with Unknown Graph Structure ([link](https://www.semanticscholar.org/paper/e463898622fbb117b04e25cf3e5d996821273a74)) | Yangyi Lu, A. Meisami, and Ambuj Tewari | Neural Information Processing Systems |
| 55 | 2022 | 7.2 | Model-based RL with Optimistic Posterior Sampling: Structural Conditions and Sample Complexity ([link](https://doi.org/10.48550/arXiv.2206.07659)) | Alekh Agarwal and T. Zhang | ArXiv |
| 56 | 2022 | 15 | A short note on an inequality between KL and TV ([link](https://www.semanticscholar.org/paper/4ae5b5db2bb394d30d2a19763f79309c31f7e5fe)) | C. Canonne |  |
| 57 | 2025 |  | When Should Reinforcement Learning Use Causal Reasoning? ([link](https://www.semanticscholar.org/paper/1ea0a4c86eaf441a6aafe6b52213985cd06304d9)) | Oliver Schulte and Pascal Poupart | Trans. Mach. Learn. Res. |
| 58 | 2022 | 8.6 | On the Complexity of Adversarial Decision Making ([link](https://doi.org/10.48550/arXiv.2206.13063)) | Dylan J. Foster, A. Rakhlin, Ayush Sekhari, and Karthik Sridharan | ArXiv |
| 59 | 2024 | 2.2 | Adaptive Smooth Non-Stationary Bandits ([link](https://doi.org/10.48550/arXiv.2407.08654)) | Joe Suk | ArXiv |
| 60 | 2024 | 1.6 | Causal Contextual Bandits with Adaptive Context ([link](https://doi.org/10.48550/arXiv.2405.18626)) | Rahul Madhavan, Aurghya Maiti, Gaurav Sinha, and Siddharth Barman | RLJ |
| 61 | 2019 | 2.2 | Understand Dynamic Regret with Switching Cost for Online Decision Making ([link](https://doi.org/10.1145/3375788)) | Yawei Zhao et al. | ACM Transactions on Intelligent Systems and Technology (TIST) |
| 62 | 2012 | 1.0 | Online Markov decision processes with Kullback-Leibler control cost ([link](https://doi.org/10.1109/ACC.2012.6314926)) | Peng Guan, M. Raginsky, and R. Willett | American Control Conference |
| 63 | 2014 | 11 | Optimal Exploration-Exploitation in a Multi-Armed-Bandit Problem with Non-stationary Rewards ([link](https://doi.org/10.2139/SSRN.2436629)) | Omar Besbes, Y. Gur, and A. Zeevi | ArXiv |

### Paper Details

1\. · 97% match · 2019 · 9.8 cit/yr\
**Variational Regret Bounds for Reinforcement Learning** ([link](https://www.semanticscholar.org/paper/16cdf928ac852257497f97c19f71eac802aab37d))\
Pratik Gajane, R. Ortner, and P. Auer\
*Conference on Uncertainty in Artificial Intelligence* · May 14, 2019 · 68 citations

> We consider undiscounted reinforcement learning in Markov decision processes (MDPs) where both the reward functions and the state-transition probabilities may vary (gradually or abruptly) over time. For this problem setting, we propose an algorithm and provide performance guarantees for the regret evaluated against the optimal non-stationary policy. The upper bound on the regret is given in terms of the total variation in the MDP. This is the first variational regret bound for the general reinforcement learning setting.

------------------------------------------------------------------------

2\. · 92% match · 2024 · 0.5 cit/yr\
**The Feasibility Theory of Constrained Reinforcement Learning: A Tutorial Study** ([link](https://doi.org/10.1108/FTSYS-03-2026-001))\
Yujie Yang, Zhilong Zheng, Masayoshi Tomizuka, Changliu Liu, and S. Li\
Apr 15, 2024 · 1 citations

> Satisfying safety constraints is a priority concern when solving optimal control problems (OCPs). Due to the existence of infeasibility phenomenon, where a constraint-satisfying solution cannot be found, it is necessary to identify a feasible region before implementing a policy. Existing feasibility theories built for model predictive control (MPC) only consider the feasibility of optimal policy. However, reinforcement learning (RL), as another important control method, solves the optimal policy in an iterative manner, which comes with a series of non-optimal intermediate policies. Feasibility analysis of these non-optimal policies is also necessary for iteratively improving constraint satisfaction; but that is not available under existing MPC feasibility theories. This paper proposes a feasibility theory that applies to both MPC and RL by filling in the missing part of feasibility analysis for an arbitrary policy. The basis of our theory is to decouple policy solving and implementation into two temporal domains: virtual-time domain and real-time domain. This allows us to separately define initial and endless, state and policy feasibility, and their corresponding feasible regions. Based on these definitions, we analyze the containment relationships between different feasible regions, which enables us to describe the feasible region of an arbitrary policy. We further provide virtual-time constraint design rules along with a practical design tool called feasibility function that helps to achieve the maximum feasible region. We review most of existing constraint formulations and point out that they are essentially applications of feasibility functions in different forms. We demonstrate our feasibility theory by visualizing different feasible regions under both MPC and RL policies in an emergency braking control task.

------------------------------------------------------------------------

3\. · 91% match · 2021 · 25 cit/yr\
**Non-stationary Reinforcement Learning without Prior Knowledge: An Optimal Black-box Approach** ([link](https://www.semanticscholar.org/paper/dc902589d77374364e1540885e6267939cc0d17c))\
Chen-Yu Wei and Haipeng Luo\
*ArXiv* · Feb 10, 2021 · 132 citations

> We propose a black-box reduction that turns a certain reinforcement learning algorithm with optimal regret in a (near-)stationary environment into another algorithm with optimal dynamic regret in a non-stationary environment, importantly without any prior knowledge on the degree of non-stationarity. By plugging different algorithms into our black-box, we provide a list of examples showing that our approach not only recovers recent results for (contextual) multi-armed bandits achieved by very specialized algorithms, but also significantly improves the state of the art for (generalized) linear bandits, episodic MDPs, and infinite-horizon MDPs in various ways. Specifically, in most cases our algorithm achieves the optimal dynamic regret $`\widetilde{\mathcal{O}}(\min\{\sqrt{LT}, \Delta^{1/3}T^{2/3}\})`$ where $`T`$ is the number of rounds and $`L`$ and $`\Delta`$ are the number and amount of changes of the world respectively, while previous works only obtain suboptimal bounds and/or require the knowledge of $`L`$ and $`\Delta`$.

------------------------------------------------------------------------

4\. · 86% match · 2020 · 2.8 cit/yr\
**Online learning with dynamics: A minimax perspective** ([link](https://www.semanticscholar.org/paper/59101599546a2e27deac59eefbd8627f2a653025))\
K. Bhatia and Karthik Sridharan\
*ArXiv* · Dec 3, 2020 · 15 citations

> We study the problem of online learning with dynamics, where a learner interacts with a stateful environment over multiple rounds. In each round of the interaction, the learner selects a policy to deploy and incurs a cost that depends on both the chosen policy and current state of the world. The state-evolution dynamics and the costs are allowed to be time-varying, in a possibly adversarial way. In this setting, we study the problem of minimizing policy regret and provide non-constructive upper bounds on the minimax rate for the problem. Our main results provide sufficient conditions for online learnability for this setup with corresponding rates. The rates are characterized by 1) a complexity term capturing the expressiveness of the underlying policy class under the dynamics of state change, and 2) a dynamics stability term measuring the deviation of the instantaneous loss from a certain counterfactual loss. Further, we provide matching lower bounds which show that both the complexity terms are indeed necessary. Our approach provides a unifying analysis that recovers regret bounds for several well studied problems including online learning with memory, online control of linear quadratic regulators, online Markov decision processes, and tracking adversarial targets. In addition, we show how our tools help obtain tight regret bounds for a new problems (with non-linear dynamics and non-convex losses) for which such bounds were not known prior to our work.

------------------------------------------------------------------------

5\. · 82% match · 2020 · 19 cit/yr\
**Reinforcement Learning for Non-Stationary Markov Decision Processes: The Blessing of (More) Optimism** ([link](https://www.semanticscholar.org/paper/5b3e68658c99ed9c461a909b16b862221946d6ad))\
Wang Chi Cheung, D. Simchi-Levi, and Ruihao Zhu\
*ArXiv* · Jun 24, 2020 · 114 citations

> We consider un-discounted reinforcement learning (RL) in Markov decision processes (MDPs) under drifting non-stationarity, i.e., both the reward and state transition distributions are allowed to evolve over time, as long as their respective total variations, quantified by suitable metrics, do not exceed certain variation budgets. We first develop the Sliding Window Upper-Confidence bound for Reinforcement Learning with Confidence Widening (SWUCRL2-CW) algorithm, and establish its dynamic regret bound when the variation budgets are known. In addition, we propose the Bandit-over-Reinforcement Learning (BORL) algorithm to adaptively tune the SWUCRL2-CW algorithm to achieve the same dynamic regret bound, but in a parameter-free manner, i.e., without knowing the variation budgets. Notably, learning non-stationary MDPs via the conventional optimistic exploration technique presents a unique challenge absent in existing (non-stationary) bandit learning settings. We overcome the challenge by a novel confidence widening technique that incorporates additional optimism.

------------------------------------------------------------------------

6\. · 81% match · 2023 · 6.7 cit/yr\
**Provably Efficient Model-Free Algorithms for Non-stationary CMDPs** ([link](https://doi.org/10.48550/arXiv.2303.05733))\
Honghao Wei, A. Ghosh, N. Shroff, Lei Ying, and Xingyu Zhou\
*International Conference on Artificial Intelligence and Statistics* · Mar 10, 2023 · 21 citations

> We study model-free reinforcement learning (RL) algorithms in episodic non-stationary constrained Markov Decision Processes (CMDPs), in which an agent aims to maximize the expected cumulative reward subject to a cumulative constraint on the expected utility (cost). In the non-stationary environment, reward, utility functions, and transition kernels can vary arbitrarily over time as long as the cumulative variations do not exceed certain variation budgets. We propose the first model-free, simulator-free RL algorithms with sublinear regret and zero constraint violation for non-stationary CMDPs in both tabular and linear function approximation settings with provable performance guarantees. Our results on regret bound and constraint violation for the tabular case match the corresponding best results for stationary CMDPs when the total budget is known. Additionally, we present a general framework for addressing the well-known challenges associated with analyzing non-stationary CMDPs, without requiring prior knowledge of the variation budget. We apply the approach for both tabular and linear approximation settings.

------------------------------------------------------------------------

7\. · 80% match · 2024 · 1.4 cit/yr\
**Dynamic Regret of Adversarial MDPs with Unknown Transition and Linear Function Approximation** ([link](https://doi.org/10.1609/aaai.v38i12.29261))\
Long-Fei Li, Peng Zhao, and Zhi-Hua Zhou\
*AAAI Conference on Artificial Intelligence* · Mar 24, 2024 · 3 citations

> We study reinforcement learning (RL) in episodic MDPs with adversarial full-information losses and the unknown transition. Instead of the classical static regret, we adopt dynamic regret as the performance measure which benchmarks the learner’s performance with changing policies, making it more suitable for non-stationary environments. The primary challenge is to handle the uncertainties of unknown transition and unknown non-stationarity of environments simultaneously. We propose a general framework to decouple the two sources of uncertainties and show the dynamic regret bound naturally decomposes into two terms, one due to constructing confidence sets to handle the unknown transition and the other due to choosing sub-optimal policies under the unknown non-stationarity. To this end, we first employ the two-layer online ensemble structure to handle the adaptation error due to the unknown non-stationarity, which is model-agnostic. Subsequently, we instantiate the framework to three fundamental MDP models, including tabular MDPs, linear MDPs and linear mixture MDPs, and present corresponding approaches to control the exploration error due to the unknown transition. We provide dynamic regret guarantees respectively and show they are optimal in terms of the number of episodes K and the non-stationarity P̄ᴋ by establishing matching lower bounds. To the best of our knowledge, this is the first work that achieves the dynamic regret exhibiting optimal dependence on K and P̄ᴋ without prior knowledge about the non-stationarity for adversarial MDPs with unknown transition.

------------------------------------------------------------------------

8\. · 80% match · 2023 · 11 cit/yr\
**Provably Efficient Primal-Dual Reinforcement Learning for CMDPs with Non-stationary Objectives and Constraints** ([link](https://doi.org/10.1609/aaai.v37i6.25900))\
Yuhao Ding and J. Lavaei\
*ArXiv* · Jun 26, 2023 · 31 citations

> We consider primal-dual-based reinforcement learning (RL) in episodic constrained Markov decision processes (CMDPs) with non-stationary objectives and constraints, which plays a central role in ensuring the safety of RL in time-varying environments. In this problem, the reward/utility functions and the state transition functions are both allowed to vary arbitrarily over time as long as their cumulative variations do not exceed certain known variation budgets. Designing safe RL algorithms in time-varying environments is particularly challenging because of the need to integrate the constraint violation reduction, safe exploration, and adaptation to the non-stationarity. To this end, we identify two alternative conditions on the time-varying constraints under which we can guarantee the safety in the long run. We also propose the Periodically Restarted Optimistic Primal-Dual Proximal Policy Optimization (PROPD-PPO) algorithm that can coordinate with both two conditions. Furthermore, a dynamic regret bound and a constraint violation bound are established for the proposed algorithm in both the linear kernel CMDP function approximation setting and the tabular CMDP setting under two alternative conditions. This paper provides the first provably efficient algorithm for non-stationary CMDPs with safe exploration.

------------------------------------------------------------------------

9\. · 79% match · 2022 · 2.8 cit/yr\
**Provably Efficient Primal-Dual Reinforcement Learning for CMDPs with Non-stationary Objectives and Constraints** ([link](https://www.semanticscholar.org/paper/1248dd2f6c2972e399ce11fc5768692e83c14a19))\
Yuhao Ding and J. Lavaei\
Jan 28, 2022 · 12 citations

> We consider primal-dual-based reinforcement learning (RL) in episodic constrained Markov decision processes (CMDPs) with non-stationary objectives and constraints, which plays a central role in ensuring the safety of RL in time-varying environments. In this problem, the reward/utility functions and the state transition functions are both allowed to vary arbitrarily over time as long as their cumulative variations do not exceed certain known variation budgets. Designing safe RL algorithms in time-varying environments is particularly challenging because of the need to integrate the constraint violation reduction, safe exploration, and adaptation to the non-stationarity. To this end, we identify two alternative conditions on the time-varying constraints under which we can guarantee the safety in the long run. We also propose the \underline{P}eriodically \underline{R}estarted \underline{O}ptimistic \underline{P}rimal-\underline{D}ual \underline{P}roximal \underline{P}olicy \underline{O}ptimization (PROPD-PPO) algorithm that can coordinate with both two conditions. Furthermore, a dynamic regret bound and a constraint violation bound are established for the proposed algorithm in both the linear kernel CMDP function approximation setting and the tabular CMDP setting under two alternative conditions. This paper provides the first provably efficient algorithm for non-stationary CMDPs with safe exploration.

------------------------------------------------------------------------

10\. · 78% match · 2020 · 12 cit/yr\
**Dynamic Regret of Policy Optimization in Non-stationary Environments** ([link](https://www.semanticscholar.org/paper/7c04e2a43c387b1fc9b4a1b603f6b9a19cd3cfd7))\
Yingjie Fei, Zhuoran Yang, Zhaoran Wang, and Qiaomin Xie\
*ArXiv* · Jun 30, 2020 · 68 citations

> We consider reinforcement learning (RL) in episodic MDPs with adversarial full-information reward feedback and unknown fixed transition kernels. We propose two model-free policy optimization algorithms, POWER and POWER++, and establish guarantees for their dynamic regret. Compared with the classical notion of static regret, dynamic regret is a stronger notion as it explicitly accounts for the non-stationarity of environments. The dynamic regret attained by the proposed algorithms interpolates between different regimes of non-stationarity, and moreover satisfies a notion of adaptive (near-)optimality, in the sense that it matches the (near-)optimal static regret under slow-changing environments. The dynamic regret bound features two components, one arising from exploration, which deals with the uncertainty of transition kernels, and the other arising from adaptation, which deals with non-stationary environments. Specifically, we show that POWER++ improves over POWER on the second component of the dynamic regret by actively adapting to non-stationarity through prediction. To the best of our knowledge, our work is the first dynamic regret analysis of model-free RL algorithms in non-stationary environments.

------------------------------------------------------------------------

11\. · 77% match · 2020 · 6.0 cit/yr\
**Efficient Learning in Non-Stationary Linear Markov Decision Processes** ([link](https://www.semanticscholar.org/paper/75d439faf8af5b9080a8955d85da9ca49929fc01))\
Ahmed Touati and Pascal Vincent\
*ArXiv* · Oct 24, 2020 · 33 citations

> We study episodic reinforcement learning in non-stationary linear (a.k.a. low-rank) Markov Decision Processes (MDPs), i.e, both the reward and transition kernel are linear with respect to a given feature map and are allowed to evolve either slowly or abruptly over time. For this problem setting, we propose OPT-WLSVI an optimistic model-free algorithm based on weighted least squares value iteration which uses exponential weights to smoothly forget data that are far in the past. We show that our algorithm, when competing against the best policy at each time, achieves a regret that is upped bounded by $`\widetilde{\mathcal{O}}(d^{7/6}H^2 \Delta^{1/3} K^{2/3})`$ where $`d`$ is the dimension of the feature space, $`H`$ is the planning horizon, $`K`$ is the number of episodes and $`\Delta`$ is a suitable measure of non-stationarity of the MDP. This is the first regret bound for non-stationary reinforcement learning with linear function approximation.

------------------------------------------------------------------------

12\. · 76% match · 2021 · 4.8 cit/yr\
**Optimistic Policy Optimization is Provably Efficient in Non-stationary MDPs** ([link](https://www.semanticscholar.org/paper/4a3ea0697e10b97a191d465fb465b1c1a035cd98))\
Han Zhong, Zhuoran Yang, Zhaoran Wang, and Csaba Szepesvari\
*ArXiv* · Oct 18, 2021 · 22 citations

> We study episodic reinforcement learning (RL) in non-stationary linear kernel Markov decision processes (MDPs). In this setting, both the reward function and the transition kernel are linear with respect to the given feature maps and are allowed to vary over time, as long as their respective parameter variations do not exceed certain variation budgets. We propose the \underline{p}eriodically \underline{r}estarted \underline{o}ptimistic \underline{p}olicy \underline{o}ptimization algorithm (PROPO), which is an optimistic policy optimization algorithm with linear function approximation. PROPO features two mechanisms: sliding-window-based policy evaluation and periodic-restart-based policy improvement, which are tailored for policy optimization in a non-stationary environment. In addition, only utilizing the technique of sliding window, we propose a value-iteration algorithm. We establish dynamic upper bounds for the proposed methods and a minimax lower bound which shows the (near-) optimality of the proposed methods. To our best knowledge, PROPO is the first provably efficient policy optimization algorithm that handles non-stationarity.

------------------------------------------------------------------------

13\. · 76% match · 2023 · 1.9 cit/yr\
**Tempo Adaptation in Non-stationary Reinforcement Learning** ([link](https://doi.org/10.52202/075280-0362))\
Hyunin Lee et al.\
*Advances in Neural Information Processing Systems 36* · Sep 26, 2023 · 5 citations

> We first raise and tackle a \`\`time synchronization’’ issue between the agent and the environment in non-stationary reinforcement learning (RL), a crucial factor hindering its real-world applications. In reality, environmental changes occur over wall-clock time ($`t`$) rather than episode progress ($`k`$), where wall-clock time signifies the actual elapsed time within the fixed duration $`t \in [0, T]`$. In existing works, at episode $`k`$, the agent rolls a trajectory and trains a policy before transitioning to episode $`k+1`$. In the context of the time-desynchronized environment, however, the agent at time $`t_{k}`$ allocates $`\Delta t`$ for trajectory generation and training, subsequently moves to the next episode at $`t_{k+1}=t_{k}+\Delta t`$. Despite a fixed total number of episodes ($`K`$), the agent accumulates different trajectories influenced by the choice of interaction times ($`t_1,t_2,...,t_K`$), significantly impacting the suboptimality gap of the policy. We propose a Proactively Synchronizing Tempo ($`\texttt{ProST}`$) framework that computes a suboptimal sequence {$`t_1,t_2,...,t_K`$} (= { $`t_{1:K}`$}) by minimizing an upper bound on its performance measure, i.e., the dynamic regret. Our main contribution is that we show that a suboptimal {$`t_{1:K}`$} trades-off between the policy training time (agent tempo) and how fast the environment changes (environment tempo). Theoretically, this work develops a suboptimal {$`t_{1:K}`$} as a function of the degree of the environment’s non-stationarity while also achieving a sublinear dynamic regret. Our experimental evaluation on various high-dimensional non-stationary environments shows that the $`\texttt{ProST}`$ framework achieves a higher online return at suboptimal {$`t_{1:K}`$} than the existing methods.

------------------------------------------------------------------------

14\. · 75% match · 2024 · 2.1 cit/yr\
**Pausing Policy Learning in Non-stationary Reinforcement Learning** ([link](https://doi.org/10.48550/arXiv.2405.16053))\
Hyunin Lee, Ming Jin, Javad Lavaei, and S. Sojoudi\
*International Conference on Machine Learning* · May 25, 2024 · 4 citations

> Real-time inference is a challenge of real-world reinforcement learning due to temporal differences in time-varying environments: the system collects data from the past, updates the decision model in the present, and deploys it in the future. We tackle a common belief that continually updating the decision is optimal to minimize the temporal gap. We propose forecasting an online reinforcement learning framework and show that strategically pausing decision updates yields better overall performance by effectively managing aleatoric uncertainty. Theoretically, we compute an optimal ratio between policy update and hold duration, and show that a non-zero policy hold duration provides a sharper upper bound on the dynamic regret. Our experimental evaluations on three different environments also reveal that a non-zero policy hold duration yields higher rewards compared to continuous decision updates.

------------------------------------------------------------------------

15\. · 74% match · 2021 · 9.2 cit/yr\
**Near-Optimal Model-Free Reinforcement Learning in Non-Stationary Episodic MDPs** ([link](https://www.semanticscholar.org/paper/190a29a8699a82d780f0de0bcd5bdb52626d3c72))\
Weichao Mao, K. Zhang, Ruihao Zhu, D. Simchi-Levi, and T. Başar\
*International Conference on Machine Learning* · 49 citations

> We consider model-free reinforcement learning (RL) in non-stationary Markov decision processes. Both the reward functions and the state transition functions are allowed to vary arbitrarily over time as long as their cumulative variations do not exceed certain variation budgets. We pro-pose Restarted Q-Learning with Upper Conﬁ-dence Bounds (RestartQ-UCB), the ﬁrst model-free algorithm for non-stationary RL, and show that it outperforms existing solutions in terms of dynamic regret. Speciﬁcally, RestartQ-UCB with Freedman-type bonus terms achieves a dynamic regret bound of (cid:101) O ( S 13 A 13 ∆ 13 HT 23 ) , where S and A are the numbers of states and actions, respectively, ∆ \> 0 is the variation budget, H is the number of time steps per episode, and T is the total number of time steps. We further show that our algorithm is nearly optimal by establishing an information-theoretical lower bound of Ω( S 13 A 13 ∆ 13 H 23 T 23 ) , the ﬁrst lower bound in non-stationary RL. Numerical experiments validate the advantages of RestartQ-UCB in terms of both cumulative rewards and computational efﬁ-ciency. We further demonstrate the power of our results in the context of multi-agent RL, where non-stationarity is a key challenge.

------------------------------------------------------------------------

16\. · 73% match · 2014 · 4.7 cit/yr\
**Online Markov Decision Processes With Kullback–Leibler Control Cost** ([link](https://doi.org/10.1109/TAC.2014.2301558))\
Peng Guan, M. Raginsky, and R. Willett\
*IEEE Transactions on Automatic Control* · Jan 14, 2014 · 58 citations

> This paper considers an online (real-time) control problem that involves an agent performing a discrete-time random walk over a finite state space. The agent’s action at each time step is to specify the probability distribution for the next state given the current state. Following the setup of Todorov, the state-action cost at each time step is a sum of a state cost and a control cost given by the Kullback-Leibler (KL) divergence between the agent’s next-state distribution and that determined by some fixed passive dynamics. The online aspect of the problem is due to the fact that the state cost functions are generated by a dynamic environment, and the agent learns the current state cost only after selecting an action. An explicit construction of a computationally efficient strategy with small regret (i.e., expected difference between its actual total cost and the smallest cost attainable using noncausal knowledge of the state costs) under mild regularity conditions is presented, along with a demonstration of the performance of the proposed strategy on a simulated target tracking problem. A number of new results on Markov decision processes with KL control cost are also obtained.

------------------------------------------------------------------------

17\. · 72% match · 2024 · 2.6 cit/yr\
**Learning Constrained Markov Decision Processes With Non-stationary Rewards and Constraints** ([link](https://doi.org/10.48550/arXiv.2405.14372))\
Francesco Emanuele Stradi, Anna Lunghi, Matteo Castiglioni, A. Marchesi, and Nicola Gatti\
*ArXiv* · May 23, 2024 · 5 citations

> In constrained Markov decision processes (CMDPs) with adversarial rewards and constraints, a well-known impossibility result prevents any algorithm from attaining both sublinear regret and sublinear constraint violation, when competing against a best-in-hindsight policy that satisfies constraints on average. In this paper, we show that this negative result can be eased in CMDPs with non-stationary rewards and constraints, by providing algorithms whose performances smoothly degrade as non-stationarity increases. Specifically, we propose algorithms attaining $`\tilde{\mathcal{O}} (\sqrt{T} + C)`$ regret and positive constraint violation under bandit feedback, where $`C`$ is a corruption value measuring the environment non-stationarity. This can be $`\Theta(T)`$ in the worst case, coherently with the impossibility result for adversarial CMDPs. First, we design an algorithm with the desired guarantees when $`C`$ is known. Then, in the case $`C`$ is unknown, we show how to obtain the same results by embedding such an algorithm in a general meta-procedure. This is of independent interest, as it can be applied to any non-stationary constrained online learning setting.

------------------------------------------------------------------------

18\. · 72% match · 2023\
**Tempo Adaption in Non-stationary Reinforcement Learning** ([link](https://doi.org/10.48550/arXiv.2309.14989))\
Hyunin Lee et al.\
*ArXiv* · 0 citations

------------------------------------------------------------------------

19\. · 71% match · 2020 · 5.9 cit/yr\
**Nonstationary Reinforcement Learning with Linear Function Approximation** ([link](https://www.semanticscholar.org/paper/750228d26cb6a33bd7393372983edfde1e09733b))\
Huozhi Zhou, Jinglin Chen, L. Varshney, and A. Jagmohan\
*Trans. Mach. Learn. Res.* · Oct 8, 2020 · 33 citations

> We consider reinforcement learning (RL) in episodic Markov decision processes (MDPs) with linear function approximation under drifting environment. Specifically, both the reward and state transition functions can evolve over time, as long as their respective total variations, quantified by suitable metrics, do not exceed certain \textit{variation budgets}. We first develop the $`\texttt{LSVI-UCB-Restart}`$ algorithm, an optimistic modification of least-squares value iteration combined with periodic restart, and establish its dynamic regret bound when variation budgets are known. We then propose a parameter-free algorithm, $`\texttt{Ada-LSVI-UCB-Restart}`$, that works without knowing the variation budgets, but with a slightly worse dynamic regret bound. We also derive the first minimax dynamic regret lower bound for nonstationary MDPs to show that our proposed algorithms are near-optimal. As a byproduct, we establish a minimax regret lower bound for linear MDPs, which is unsolved by \cite{jin2020provably}. In addition, we provide numerical experiments to demonstrate the effectiveness of our proposed algorithms. As far as we know, this is the first dynamic regret analysis in nonstationary reinforcement learning with function approximation.

------------------------------------------------------------------------

20\. · 70% match · 2021 · 0.2 cit/yr\
**Online Sub-Sampling for Reinforcement Learning with General Function Approximation** ([link](https://www.semanticscholar.org/paper/668d882897d1dfd3cc4b59823388138c0764a46f))\
Dingwen Kong, R. Salakhutdinov, Ruosong Wang, and Lin Yang\
Jun 14, 2021 · 1 citations

> Most of the existing works for reinforcement learning (RL) with general function approximation (FA) focus on understanding the statistical complexity or regret bounds. However, the computation complexity of such approaches is far from being understood – indeed, a simple optimization problem over the function class might be as well intractable. In this paper, we tackle this problem by establishing an efficient online sub-sampling framework that measures the information gain of data points collected by an RL algorithm and uses the measurement to guide exploration. For a value-based method with complexity-bounded function class, we show that the policy only needs to be updated for $`\propto\operatorname{poly}\log(K)`$ times for running the RL algorithm for $`K`$ episodes while still achieving a small near-optimal regret bound. In contrast to existing approaches that update the policy for at least $`\Omega(K)`$ times, our approach drastically reduces the number of optimization calls in solving for a policy. When applied to settings in \cite{wang2020reinforcement} or \cite{jin2021bellman}, we improve the overall time complexity by at least a factor of $`K`$. Finally, we show the generality of our online sub-sampling technique by applying it to the reward-free RL setting and multi-agent RL setting.

------------------------------------------------------------------------

21\. · 69% match · 2023 · 3.4 cit/yr\
**An Information-Theoretic Analysis of Nonstationary Bandit Learning** ([link](https://doi.org/10.48550/arXiv.2302.04452))\
Seungki Min and Daniel Russo\
*International Conference on Machine Learning* · Feb 9, 2023 · 11 citations

> In nonstationary bandit learning problems, the decision-maker must continually gather information and adapt their action selection as the latent state of the environment evolves. In each time period, some latent optimal action maximizes expected reward under the environment state. We view the optimal action sequence as a stochastic process, and take an information-theoretic approach to analyze attainable performance. We bound limiting per-period regret in terms of the entropy rate of the optimal action process. The bound applies to a wide array of problems studied in the literature and reflects the problem’s information structure through its information-ratio.

------------------------------------------------------------------------

22\. · 68% match · 2020 · 23 cit/yr\
**Information Theoretic Regret Bounds for Online Nonlinear Control** ([link](https://www.semanticscholar.org/paper/458d4f3d398da068493c63687e285b691514dff5))\
S. Kakade, A. Krishnamurthy, Kendall Lowrey, Motoya Ohnishi, and Wen Sun\
*ArXiv* · Jun 22, 2020 · 132 citations

> This work studies the problem of sequential control in an unknown, nonlinear dynamical system, where we model the underlying system dynamics as an unknown function in a known Reproducing Kernel Hilbert Space. This framework yields a general setting that permits discrete and continuous control inputs as well as non-smooth, non-differentiable dynamics. Our main result, the Lower Confidence-based Continuous Control ($`LC^3`$) algorithm, enjoys a near-optimal $`O(\sqrt{T})`$ regret bound against the optimal controller in episodic settings, where $`T`$ is the number of episodes. The bound has no explicit dependence on dimension of the system dynamics, which could be infinite, but instead only depends on information theoretic quantities. We empirically show its application to a number of nonlinear control tasks and demonstrate the benefit of exploration for learning model dynamics.

------------------------------------------------------------------------

23\. · 67% match · 2014 · 6.2 cit/yr\
**Online Learning in Markov Decision Processes with Changing Cost Sequences** ([link](https://doi.org/10.14288/1.0044649))\
Travis Dick, A. György, and Csaba Szepesvari\
*International Conference on Machine Learning* · Jun 21, 2014 · 73 citations

> In this paper we consider online learning in finite Markov decision processes (MDPs) with changing cost sequences under full and bandit-information. We propose to view this problem as an instance of online linear optimization. We propose two methods for this problem: MD2 (mirror descent with approximate projections) and the continuous exponential weights algorithm with Dikin walks. We provide a rigorous complexity analysis of these techniques, while providing near-optimal regret-bounds (in particular, we take into account the computational costs of performing approximate projections in MD2). In the case of full-information feedback, our results complement existing ones. In the case of bandit-information feedback we consider the online stochastic shortest path problem, a special case of the above MDP problems, and manage to improve the existing results by removing the previous restrictive assumption that the state-visitation probabilities are uniformly bounded away from zero under all policies.

------------------------------------------------------------------------

24\. · 66% match · 2022 · 5.7 cit/yr\
**Dynamic Regret of Online Markov Decision Processes** ([link](https://doi.org/10.48550/arXiv.2208.12483))\
Peng Zhao, Longfei Li, and Zhi-Hua Zhou\
*International Conference on Machine Learning* · Aug 26, 2022 · 21 citations

> We investigate online Markov Decision Processes (MDPs) with adversarially changing loss functions and known transitions. We choose dynamic regret as the performance measure, defined as the performance difference between the learner and any sequence of feasible changing policies. The measure is strictly stronger than the standard static regret that benchmarks the learner’s performance with a fixed compared policy. We consider three foundational models of online MDPs, including episodic loop-free Stochastic Shortest Path (SSP), episodic SSP, and infinite-horizon MDPs. For these three models, we propose novel online ensemble algorithms and establish their dynamic regret guarantees respectively, in which the results for episodic (loop-free) SSP are provably minimax optimal in terms of time horizon and certain non-stationarity measure. Furthermore, when the online environments encountered by the learner are predictable, we design improved algorithms and achieve better dynamic regret bounds for the episodic (loop-free) SSP; and moreover, we demonstrate impossibility results for the infinite-horizon MDPs.

------------------------------------------------------------------------

25\. · 66% match · 2025 · 1.0 cit/yr\
**Online Regret Bounds for Satisficing in Markov Decision Processes** ([link](https://doi.org/10.1287/moor.2023.0275))\
Hossein Hajiabolhassan and Ronald Ortner\
*Mathematics of Operations Research* · May 9, 2025 · 1 citations

> We consider general reinforcement learning under the average reward criterion in Markov decision processes (MDPs), when the learner’s goal is not to learn an optimal policy, but accepts any policy whose average reward is above a given satisfaction level \[Formula: see text\]. We show that with this more modest objective, it is possible to give algorithms that only have constant regret with respect to the level \[Formula: see text\], provided that there is a policy above this level. This is a generalization of known results from the bandit setting to MDPs. Further, we present a more general algorithm that achieves the best of both worlds: If the optimal policy has average reward above \[Formula: see text\], this algorithm has bounded regret with respect to \[Formula: see text\]. On the other hand, if all policies are below \[Formula: see text\], then the expected regret with respect to the optimal policy is bounded as for the UCRL2 algorithm. Funding: Financial support from the Austrian Science Fund (FWF) \[Grant TAI 590-N\] is gratefully acknowledged.

------------------------------------------------------------------------

26\. · 64% match · 2020 · 0.6 cit/yr\
**Counterfactual Programming for Optimal Control** ([link](https://www.semanticscholar.org/paper/201b46b8f046329fecd316d182c4afe3714f8222))\
Luiz F. O. Chamon, Santiago Paternain, and Alejandro Ribeiro\
*Conference on Learning for Dynamics & Control* · Jan 29, 2020 · 4 citations

> In recent years, considerable work has been done to tackle the issue of designing control laws based on observations to allow unknown dynamical systems to perform pre-specified tasks. At least as important for autonomy, however, is the issue of learning which tasks can be performed in the first place. This is particularly critical in situations where multiple (possibly conflicting) tasks and requirements are demanded from the agent, resulting in infeasible specifications. Such situations arise due to over-specification or dynamic operating conditions and are only aggravated when the dynamical system model is learned through simulations. Often, these issues are tackled using regularization and penalties tuned based on application-specific expert knowledge. Nevertheless, this solution becomes impractical for large-scale systems, unknown operating conditions, and/or in online settings where expert input would be needed during the system operation. Instead, this work enables agents to autonomously pose, tune, and solve optimal control problems by compromising between performance and specification costs. Leveraging duality theory, it puts forward a counterfactual optimization algorithm that directly determines the specification trade-off while solving the optimal control problem.

------------------------------------------------------------------------

27\. · 64% match · 2026\
**DARLING: Detection Augmented Reinforcement Learning with Non-Stationary Guarantees** ([link](https://www.semanticscholar.org/paper/60026b5b0f7605e6b6e1872171f87bf9c4fefbf8))\
A. Gerogiannis, Yu-Han Huang, and Venugopal V. Veeravalli\
Apr 17, 2026 · 0 citations

> We study model-free reinforcement learning (RL) in non-stationary finite-horizon episodic Markov decision processes (MDPs) without prior knowledge of the non-stationarity. We focus on the piecewise-stationary (PS) setting, where both the reward and transition dynamics can change an arbitrary number of times. We propose Detection Augmented Reinforcement Learning (DARLING), a modular wrapper for PS-RL that applies to both tabular and linear MDPs, without knowledge of the changes. Under certain change-point separation and reachability conditions, DARLING improves the best available dynamic regret bounds in both settings and yields strong empirical performance. We further establish the first minimax lower bounds for PS-RL in tabular and linear MDPs, showing that DARLING is the first nearly optimal algorithm. Experiments on standard benchmarks demonstrate that DARLING consistently surpasses the state-of-the-art methods across diverse non-stationary scenarios.

------------------------------------------------------------------------

28\. · 63% match · 2014 · 37 cit/yr\
**Stochastic Multi-Armed-Bandit Problem with Non-stationary Rewards** ([link](https://www.semanticscholar.org/paper/2295f2034c2a45a39fd1a08605d2a8e0588e7e4d))\
Y. Gur, A. Zeevi, and Omar Besbes\
*Neural Information Processing Systems* · Dec 8, 2014 · 417 citations

> In a multi-armed bandit (MAB) problem a gambler needs to choose at each round of play one of K arms, each characterized by an unknown reward distribution. Reward realizations are only observed when an arm is selected, and the gambler’s objective is to maximize his cumulative expected earnings over some given horizon of play T. To do this, the gambler needs to acquire information about arms (exploration) while simultaneously optimizing immediate rewards (exploitation); the price paid due to this trade off is often referred to as the regret, and the main question is how small can this price be as a function of the horizon length T. This problem has been studied extensively when the reward distributions do not change over time; an assumption that supports a sharp characterization of the regret, yet is often violated in practical settings. In this paper, we focus on a MAB formulation which allows for a broad range of temporal uncertainties in the rewards, while still maintaining mathematical tractability. We fully characterize the (regret) complexity of this class of MAB problems by establishing a direct link between the extent of allowable reward “variation” and the minimal achievable regret, and by establishing a connection between the adversarial and the stochastic MAB frameworks.

------------------------------------------------------------------------

29\. · 62% match · 2026\
**On the Peril of (Even a Little) Nonstationarity in Satisficing Regret Minimization** ([link](https://www.semanticscholar.org/paper/fb3f0e605737c837131d0e72bc841a82056fa106))\
Yixuan Zhang, Ruihao Zhu, and Qiaomin Xie\
Mar 19, 2026 · 0 citations

> Motivated by the principle of satisficing in decision-making, we study satisficing regret guarantees for nonstationary $`K`$-armed bandits. We show that in the general realizable, piecewise-stationary setting with $`L`$ stationary segments, the optimal regret is $`\Theta(L\log T)`$ as long as $`L\geq 2`$. This stands in sharp contrast to the case of $`L=1`$ (i.e., the stationary setting), where a $`T`$-independent $`\Theta(1)`$ satisficing regret is achievable under realizability. In other words, the optimal regret has to scale with $`T`$ even if just a little nonstationarity presents. A key ingredient in our analysis is a novel Fano-based framework tailored to nonstationary bandits via a \emph{post-interaction reference} construction. This framework strictly extends the classical Fano method for passive estimation as well as recent interactive Fano techniques for stationary bandits. As a complement, we also discuss a special regime in which constant satisficing regret is again possible.

------------------------------------------------------------------------

30\. · 61% match · 2013 · 38 cit/yr\
**Non-Stationary Stochastic Optimization** ([link](https://doi.org/10.1287/opre.2015.1408))\
Omar Besbes, Y. Gur, and A. Zeevi\
*Oper. Res.* · Jul 20, 2013 · 489 citations

> We consider a non-stationary variant of a sequential stochastic optimization problem, in which the underlying cost functions may change along the horizon. We propose a measure, termed variation budget , that controls the extent of said change, and study how restrictions on this budget impact achievable performance. We identify sharp conditions under which it is possible to achieve long-run average optimality and more refined performance measures such as rate optimality that fully characterize the complexity of such problems. In doing so, we also establish a strong connection between two rather disparate strands of literature: (1) adversarial online convex optimization and (2) the more traditional stochastic approximation paradigm (couched in a non-stationary setting). This connection is the key to deriving well-performing policies in the latter, by leveraging structure of optimal policies in the former. Finally, tight bounds on the minimax regret allow us to quantify the “price of non-stationarity,” which mathematically captures the added complexity embedded in a temporally changing environment versus a stationary one.

------------------------------------------------------------------------

31\. · 60% match · 2013 · 9.8 cit/yr\
**Online Learning with Switching Costs and Other Adaptive Adversaries** ([link](https://www.semanticscholar.org/paper/8a1d0d8f2cce1a180c6b41c733262fe81ce35a9c))\
N. Cesa-Bianchi, O. Dekel, and Ohad Shamir\
*ArXiv* · Feb 18, 2013 · 130 citations

> We study the power of different types of adaptive (nonoblivious) adversaries in the setting of prediction with expert advice, under both full-information and bandit feedback. We measure the player’s performance using a new notion of regret, also known as policy regret, which better captures the adversary’s adaptiveness to the player’s behavior. In a setting where losses are allowed to drift, we characterize —in a nearly complete manner— the power of adaptive adversaries with bounded memories and switching costs. In particular, we show that with switching costs, the attainable rate with bandit feedback is Θ(T2/3). Interestingly, this rate is significantly worse than the Θ(√T) rate attainable with switching costs in the full-information case. Via a novel reduction from experts to bandits, we also show that a bounded memory adversary can force \*\*\*\*Θ(T2/3) regret even in the full information case, proving that switching costs are easier to control than bounded memory adversaries. Our lower bounds rely on a new stochastic adversary strategy that generates loss processes with strong dependencies.

------------------------------------------------------------------------

32\. · 59% match · 2012 · 2.5 cit/yr\
**Deterministic MDPs with Adversarial Rewards and Bandit Feedback** ([link](https://www.semanticscholar.org/paper/2b8ac3708075d0e35a0e4640807624376811fb79))\
R. Arora, O. Dekel, and Ambuj Tewari\
*Conference on Uncertainty in Artificial Intelligence* · Aug 14, 2012 · 34 citations

> We consider a Markov decision process with deterministic state transition dynamics, adversarially generated rewards that change arbitrarily from round to round, and a bandit feedback model in which the decision maker only observes the rewards it receives. In this setting, we present a novel and efficient online decision making algorithm named MarcoPolo. Under mild assumptions on the structure of the transition dynamics, we prove that MarcoPolo enjoys a regret of O(T3/4 √log T) against the best deterministic policy in hindsight. Specifically, our analysis does not rely on the stringent unichain assumption, which dominates much of the previous work on this topic.

------------------------------------------------------------------------

33\. · 58% match · 2022 · 2.3 cit/yr\
**Online Reinforcement Learning for Mixed Policy Scopes** ([link](https://doi.org/10.52202/068431-0231))\
Junzhe Zhang and E. Bareinboim\
*Advances in Neural Information Processing Systems 35* · 10 citations

> Combination therapy refers to the use of multiple treatments – such as surgery, medication, and behavioral therapy - to cure a single disease, and has become a cornerstone for treating various conditions including cancer, HIV, and depression. All possible combinations of treatments lead to a collection of treatment regimens (i.e., policies) with mixed scopes, or what physicians could observe and which actions they should take depending on the context. In this paper, we investigate the online reinforcement learning setting for optimizing the policy space with mixed scopes. In particular, we develop novel online algorithms that achieve sublinear regret compared to an optimal agent deployed in the environment. The regret bound has a dependency on the maximal cardinality of the induced state-action space associated with mixed scopes. We further introduce a canonical representation for an arbitrary subset of interventional distributions given a causal diagram, which leads to a non-trivial, minimal representation of the model parameters.

------------------------------------------------------------------------

34\. · 58% match · 2019 · 10 cit/yr\
**Information-Theoretic Confidence Bounds for Reinforcement Learning** ([link](https://www.semanticscholar.org/paper/3231ac937b2620cd3ea7c39fdacaf416a558d31c))\
Xiuyuan Lu and Benjamin Van Roy\
*Neural Information Processing Systems* · Nov 21, 2019 · 66 citations

> We integrate information-theoretic concepts into the design and analysis of optimistic algorithms and Thompson sampling. By making a connection between information-theoretic quantities and confidence bounds, we obtain results that relate the per-period performance of the agent with its information gain about the environment, thus explicitly characterizing the exploration-exploitation tradeoff. The resulting cumulative regret bound depends on the agent’s uncertainty over the environment and quantifies the value of prior information. We show applicability of this approach to several environments, including linear bandits, tabular MDPs, and factored MDPs. These examples demonstrate the potential of a general information-theoretic approach for the design and analysis of reinforcement learning algorithms.

------------------------------------------------------------------------

35\. · 57% match · 2021 · 10 cit/yr\
**Non-stationary Online Learning with Memory and Non-stochastic Control** ([link](https://www.semanticscholar.org/paper/d2ad60fe398784082cc2777208fe13a5fe163b55))\
Peng Zhao, Yu-Xiang Wang, and Zhi-Hua Zhou\
*International Conference on Artificial Intelligence and Statistics* · Feb 7, 2021 · 54 citations

> We study the problem of Online Convex Optimization (OCO) with memory, which allows loss functions to depend on past decisions and thus captures temporal effects of learning problems. In this paper, we introduce dynamic policy regret as the performance measure to design algorithms robust to non-stationary environments, which competes algorithms’ decisions with a sequence of changing comparators. We propose a novel algorithm for OCO with memory that provably enjoys an optimal dynamic policy regret in terms of time horizon, non-stationarity measure, and memory length. The key technical challenge is how to control the switching cost, the cumulative movements of player’s decisions, which is neatly addressed by a novel switching-cost-aware online ensemble approach equipped with a new meta-base decomposition of dynamic policy regret and a careful design of meta-learner and base-learner that explicitly regularizes the switching cost. The results are further applied to tackle non-stationarity in online non-stochastic control (Agarwal et al., 2019), i.e., controlling a linear dynamical system with adversarial disturbance and convex cost functions. We derive a novel gradient-based controller with dynamic policy regret guarantees, which is the first controller provably competitive to a sequence of changing policies for online non-stochastic control.

------------------------------------------------------------------------

36\. · 56% match · 2020 · 13 cit/yr\
**Designing Optimal Dynamic Treatment Regimes: A Causal Reinforcement Learning Approach** ([link](https://www.semanticscholar.org/paper/162e03526f99e8a844022590ce1001d7f1987de1))\
Junzhe Zhang\
*International Conference on Machine Learning* · Jul 12, 2020 · 78 citations

> A dynamic treatment regime (DTR) consists of a sequence of decision rules, one per stage of intervention, that dictates how to determine the treat-ment assignment to patients based on evolving treatments and covariates’ history. These regimes are particularly effective for managing chronic disorders and is arguably one of the critical ingredients underlying more personalized decision-making systems. All reinforcement learning algorithms for ﬁnding the optimal DTR in online settings will suffer Ω( (cid:112) \| D X ∪ S \| T ) regret on some environments, where T is the number of experiments and D X ∪ S is the domains of the treatments X and covariates S . This implies that T = Ω( \| D X ∪ S \| ) trials will be required to generate an optimal DTR. In many applications, the domains of X and S could be enormous, which means that the time required to ensure appropriate learning may be unattainable. We show that, if the causal diagram of the underlying environment is provided, one could achieve regret that is exponentially smaller than D X ∪ S . In particular, we develop two online algorithms that satisfy such regret bounds by exploiting the causal structure underlying the DTR; one is the based on the principle of optimism in the face of uncertainty ( OFU-DTR ), and the other uses the posterior sampling learning ( PS-DTR ). Finally, we introduce efﬁcient methods to accelerate these online learning procedures by leveraging the abundant, yet biased observational (non-experimental) data.

------------------------------------------------------------------------

37\. · 56% match · 2019 · 2.9 cit/yr\
**Online Learning for Markov Decision Processes in Nonstationary Environments: A Dynamic Regret Analysis** ([link](https://doi.org/10.23919/ACC.2019.8815000))\
Yingying Li and Na Li\
*2019 American Control Conference (ACC)* · Jul 1, 2019 · 20 citations

> In an online Markov decision process (MDP) with time-varying reward functions, a decision maker has to take an action before knowing the current reward function at each time step. This problem has received many research interests because of its wide range of applications. The literature usually focuses on static regret analysis by comparing the total reward of the optimal offline stationary policy and that of the online policies. This paper studies a different measure, dynamic regret, which is the reward difference between the optimal offline (possibly nonstationary) policies and the online policies. The measure suits better the time-varying environment. To obtain a meaningful regret analysis, we introduce a notion of total variation for the time-varying reward functions and bound the dynamic regret using the total variation. We propose an online algorithm, Follow the Weighted Leader (FWL), and prove that its dynamic regret can be upper bounded by the total variation. We also prove a lower bound of dynamic regrets for any online algorithm. The lower bound matches the upper bound of FWL, demonstrating the optimality of the algorithm. Finally, we show via simulation that our algorithm FWL significantly outperforms the existing algorithms in literature.

------------------------------------------------------------------------

38\. · 55% match · 2020 · 9.9 cit/yr\
**Provably Efficient Causal Reinforcement Learning with Confounded Observational Data** ([link](https://www.semanticscholar.org/paper/2d6937f8421d4d793ee0f03d3c60c6e794b25c36))\
Lingxiao Wang, Zhuoran Yang, and Zhaoran Wang\
*Neural Information Processing Systems* · Jun 22, 2020 · 58 citations

> Empowered by expressive function approximators such as neural networks, deep reinforcement learning (DRL) achieves tremendous empirical successes. However, learning expressive function approximators requires collecting a large dataset (interventional data) by interacting with the environment. Such a lack of sample efficiency prohibits the application of DRL to critical scenarios, e.g., autonomous driving and personalized medicine, since trial and error in the online setting is often unsafe and even unethical. In this paper, we study how to incorporate the dataset (observational data) collected offline, which is often abundantly available in practice, to improve the sample efficiency in the online setting. To incorporate the possibly confounded observational data, we propose the deconfounded optimistic value iteration (DOVI) algorithm, which incorporates the confounded observational data in a provably efficient manner. More specifically, DOVI explicitly adjusts for the confounding bias in the observational data, where the confounders are partially observed or unobserved. In both cases, such adjustments allow us to construct the bonus based on a notion of information gain, which takes into account the amount of information acquired from the offline setting. In particular, we prove that the regret of DOVI is smaller than the optimal regret achievable in the pure online setting by a multiplicative factor, which decreases towards zero when the confounded observational data are more informative upon the adjustments. Our algorithm and analysis serve as a step towards causal reinforcement learning.

------------------------------------------------------------------------

39\. · 55% match · 2020 · 3.0 cit/yr\
**A Duality Approach for Regret Minimization in Average-Award Ergodic Markov Decision Processes** ([link](https://www.semanticscholar.org/paper/c758fb4bc55336c745a7bb6d13b2f99cc3a2b5e3))\
Hao Gong and Mengdi Wang\
*Conference on Learning for Dynamics & Control* · 19 citations

> In light of the Bellman duality, we propose a novel value-policy gradient algorithm to explore and act in inﬁnite-horizon Average-reward Markov Decision Process (AMDP) and show that it has sublinear regret. The algorithm is motivated by the Bellman saddle point formulation. It learns the optimal state-action distribution, which encodes a randomized policy, by interacting with the environment along a single trajectory and making primal-dual updates. The key to the analysis is to establish a connection between the min-max duality gap of Bellman saddle point and the cumulative regret of the learning agent. We show that, for ergodic AMDPs with ﬁnite state space S and action space A and uniformly bounded mixing times, the algorithm’s T -time step regret is

------------------------------------------------------------------------

40\. · 54% match · 2019 · 14 cit/yr\
**Hedging the Drift: Learning to Optimize under Non-Stationarity** ([link](https://doi.org/10.1287/mnsc.2021.4024))\
Wang Chi Cheung, D. Simchi-Levi, and Ruihao Zhu\
*Manag. Sci.* · Mar 4, 2019 · 101 citations

> We introduce data-driven decision-making algorithms that achieve state-of-the-art dynamic regret bounds for a collection of nonstationary stochastic bandit settings. These settings capture applications such as advertisement allocation, dynamic pricing, and traffic network routing in changing environments. We show how the difficulty posed by the (unknown a priori and possibly adversarial) nonstationarity can be overcome by an unconventional marriage between stochastic and adversarial bandit learning algorithms. Beginning with the linear bandit setting, we design and analyze a sliding window-upper confidence bound algorithm that achieves the optimal dynamic regret bound when the underlying variation budget is known. This budget quantifies the total amount of temporal variation of the latent environments. Boosted by the novel bandit-over-bandit framework that adapts to the latent changes, our algorithm can further enjoy nearly optimal dynamic regret bounds in a (surprisingly) parameter-free manner. We extend our results to other related bandit problems, namely the multiarmed bandit, generalized linear bandit, and combinatorial semibandit settings, which model a variety of operations research applications. In addition to the classical exploration-exploitation trade-off, our algorithms leverage the power of the “forgetting principle” in the learning processes, which is vital in changing environments. Extensive numerical experiments with synthetic datasets and a dataset of an online auto-loan company during the severe acute respiratory syndrome (SARS) epidemic period demonstrate that our proposed algorithms achieve superior performance compared with existing algorithms. This paper was accepted by George J. Shanthikumar for the Management Science Special Issue on Data-Driven Prescriptive Analytics.

------------------------------------------------------------------------

41\. · 53% match · 2021 · 2.3 cit/yr\
**Causal Markov Decision Processes: Learning Good Interventions Efficiently** ([link](https://www.semanticscholar.org/paper/7990ed58591c23dbef01bc8010220d20c13156d2))\
Yangyi Lu, A. Meisami, and Ambuj Tewari\
*ArXiv* · Feb 15, 2021 · 12 citations

> We introduce causal Markov Decision Processes (C-MDPs), a new formalism for sequential decision making which combines the standard MDP formulation with causal structures over state transition and reward functions. Many contemporary and emerging application areas such as digital healthcare and digital marketing can benefit from modeling with C-MDPs due to the causal mechanisms underlying the relationship between interventions and states/rewards. We propose the causal upper confidence bound value iteration (C-UCBVI) algorithm that exploits the causal structure in C-MDPs and improves the performance of standard reinforcement learning algorithms that do not take causal knowledge into account. We prove that C-UCBVI satisfies an Õ(HS √ ZT ) regret bound, where T is the the total time steps, H is the episodic horizon, and S is the cardinality of the state space. Notably, our regret bound does not scale with the size of actions/interventions (A), but only scales with a causal graph dependent quantity Z which can be exponentially smaller than A. By extending C-UCBVI to the factored MDP setting, we propose the causal factored UCBVI (CF-UCBVI) algorithm, which further reduces the regret exponentially in terms of S. Furthermore, we show that RL algorithms for linear MDP problems can also be incorporated in C-MDPs. We empirically show the benefit of our causal approaches in various settings to validate our algorithms and theoretical results.

------------------------------------------------------------------------

42\. · 53% match · 2022 · 6.3 cit/yr\
**A New Look at Dynamic Regret for Non-Stationary Stochastic Bandits** ([link](https://www.semanticscholar.org/paper/1d3ecde233d3a5e7ea64e63b2131bae8aae7180d))\
Yasin Abbasi-Yadkori, A. György, and N. Lazic\
*J. Mach. Learn. Res.* · Jan 17, 2022 · 27 citations

> We study the non-stationary stochastic multi-armed bandit problem, where the reward statistics of each arm may change several times during the course of learning. The performance of a learning algorithm is evaluated in terms of their dynamic regret, which is defined as the difference between the expected cumulative reward of an agent choosing the optimal arm in every time step and the cumulative reward of the learning algorithm. One way to measure the hardness of such environments is to consider how many times the identity of the optimal arm can change. We propose a method that achieves, in $`K`$-armed bandit problems, a near-optimal $`\widetilde O(\sqrt{K N(S+1)})`$ dynamic regret, where $`N`$ is the time horizon of the problem and $`S`$ is the number of times the identity of the optimal arm changes, without prior knowledge of $`S`$. Previous works for this problem obtain regret bounds that scale with the number of changes (or the amount of change) in the reward functions, which can be much larger, or assume prior knowledge of $`S`$ to achieve similar bounds.

------------------------------------------------------------------------

43\. · 52% match · 2022 · 6.0 cit/yr\
**Efficient Reinforcement Learning with Prior Causal Knowledge** ([link](https://www.semanticscholar.org/paper/ec0b078243ad06fceda7cfd55ba8201953250cd5))\
Yangyi Lu and Ambuj Tewari\
*CLEaR* · 26 citations

> We introduce causal Markov Decision Processes (C-MDPs), a new formalism for sequential decision making which combines the standard MDP formulation with causal structures over state transition and reward functions. Many contemporary and emerging application areas such as digital healthcare and digital marketing can beneﬁt from modeling with C-MDPs due to the causal mechanisms underlying the relationship between interventions and states/rewards. We propose the causal upper conﬁdence bound value iteration (C-UCBVI) algorithm that exploits the causal structure in C-MDPs and improves the performance of standard reinforcement learning algorithms that do not take causal knowledge into account. We prove that C-UCBVI satisﬁes an ˜ O ( HS √ ZT ) regret bound, where T is the the total time steps, H is the episodic horizon, and S is the cardinality of the state space. Notably, our regret bound does not scale with the size of actions/interventions ( A ), but only scales with a causal graph dependent quantity Z which can be exponentially smaller than A . By extending C-UCBVI to the factored MDP setting, we propose the causal factored UCBVI (CF-UCBVI) algorithm, which further reduces the regret exponentially in terms of S . Furthermore, we show that RL algorithms for linear MDP problems can also be incorporated in C-MDPs. We empirically show the beneﬁt of our causal approaches in various settings to validate our algorithms and theoretical results.

------------------------------------------------------------------------

44\. · 51% match · 2019 · 20 cit/yr\
**A New Algorithm for Non-stationary Contextual Bandits: Efficient, Optimal, and Parameter-free** ([link](https://www.semanticscholar.org/paper/0940c55435b3b12be85b90c140c703ec39fb0be7))\
Yifang Chen, Chung-Wei Lee, Haipeng Luo, and Chen-Yu Wei\
*ArXiv* · Feb 3, 2019 · 144 citations

> We propose the first contextual bandit algorithm that is parameter-free, efficient, and optimal in terms of dynamic regret. Specifically, our algorithm achieves dynamic regret $`\mathcal{O}(\min\{\sqrt{ST}, \Delta^{\frac{1}{3}}T^{\frac{2}{3}}\})`$ for a contextual bandit problem with $`T`$ rounds, $`S`$ switches and $`\Delta`$ total variation in data distributions. Importantly, our algorithm is adaptive and does not need to know $`S`$ or $`\Delta`$ ahead of time, and can be implemented efficiently assuming access to an ERM oracle. Our results strictly improve the $`\mathcal{O}(\min \{S^{\frac{1}{4}}T^{\frac{3}{4}}, \Delta^{\frac{1}{5}}T^{\frac{4}{5}}\})`$ bound of (Luo et al., 2018), and greatly generalize and improve the $`\mathcal{O}(\sqrt{ST})`$ result of (Auer et al, 2018) that holds only for the two-armed bandit problem without contextual information. The key novelty of our algorithm is to introduce replay phases, in which the algorithm acts according to its previous decisions for a certain amount of time in order to detect non-stationarity while maintaining a good balance between exploration and exploitation.

------------------------------------------------------------------------

45\. · 50% match · 2016 · 5.1 cit/yr\
**Markov Decision Processes with Unobserved Confounders : A Causal Approach** ([link](https://www.semanticscholar.org/paper/69c68d804e7c052665d5b4049c0d9c9d8baa11c0))\
Junzhe Zhang and E. Bareinboim\
53 citations

> Markov decision processes (MDPs) constitute one of the most general frameworks for modeling decision-making under uncertainty, being used in multiple fields, including economics, medicine, and engineering. The goal of the agent in an MDP setting is to learn more about the environment so as to optimize a certain criterion. This task is pursued through the exploration of the environment by actively performing interventions (i.e., through the randomization of its actions), which contrasts with the agent passively observing the environment and not exerting any control over it (i.e., through random sampling). The existence of unobserved confounders, namely, unmeasured variables affecting both the action and the outcome or both the action and the state variables, implies that these two datacollection modes (passive and active) will in general not coincide. It is clear that by performing interventions, any potential inclination (intuition) of the agent will be ignored, which will imply a loss of information and failure to achieve an optimal behavior. In this paper, we formalize this observation and study its conceptual and algorithmic implications. We first demonstrate that standard algorithms may act sub-optimally when unobserved confounders are present. We then propose a systematic method to enhance these algorithms using causal inference theory and leveraging observational data. We formally and empirically show that this new approach produces superior results than current state-of-the-art MDP algorithms.

------------------------------------------------------------------------

46\. · 49% match · 2018 · 21 cit/yr\
**Learning to Optimize under Non-Stationarity** ([link](https://doi.org/10.2139/SSRN.3261050))\
Wang Chi Cheung, D. Simchi-Levi, and Ruihao Zhu\
*ArXiv* · Oct 5, 2018 · 159 citations

> We introduce algorithms that achieve state-of-the-art \emph{dynamic regret} bounds for non-stationary linear stochastic bandit setting. It captures natural applications such as dynamic pricing and ads allocation in a changing environment. We show how the difficulty posed by the non-stationarity can be overcome by a novel marriage between stochastic and adversarial bandits learning algorithms. Defining $`d,B_T,`$ and $`T`$ as the problem dimension, the \emph{variation budget}, and the total time horizon, respectively, our main contributions are the tuned Sliding Window UCB (\texttt{SW-UCB}) algorithm with optimal $`\widetilde{O}(d^{2/3}(B_T+1)^{1/3}T^{2/3})`$ dynamic regret, and the tuning free bandit-over-bandit (\texttt{BOB}) framework built on top of the \texttt{SW-UCB} algorithm with best $`\widetilde{O}(d^{2/3}(B_T+1)^{1/4}T^{3/4})`$ dynamic regret.

------------------------------------------------------------------------

47\. · 48% match · 2023 · 4.1 cit/yr\
**A Definition of Non-Stationary Bandits** ([link](https://doi.org/10.48550/arXiv.2302.12202))\
Yueyang Liu, Benjamin Van Roy, and Kuang Xu\
*ArXiv* · Feb 23, 2023 · 13 citations

> Despite the subject of non-stationary bandit learning having attracted much recent attention, we have yet to identify a formal definition of non-stationarity that can consistently distinguish non-stationary bandits from stationary ones. Prior work has characterized non-stationary bandits as bandits for which the reward distribution changes over time. We demonstrate that this definition can ambiguously classify the same bandit as both stationary and non-stationary; this ambiguity arises in the existing definition’s dependence on the latent sequence of reward distributions. Moreover, the definition has given rise to two widely used notions of regret: the dynamic regret and the weak regret. These notions are not indicative of qualitative agent performance in some bandits. Additionally, this definition of non-stationary bandits has led to the design of agents that explore excessively. We introduce a formal definition of non-stationary bandits that resolves these issues. Our new definition provides a unified approach, applicable seamlessly to both Bayesian and frequentist formulations of bandits. Furthermore, our definition ensures consistent classification of two bandits offering agents indistinguishable experiences, categorizing them as either both stationary or both non-stationary. This advancement provides a more robust framework for non-stationary bandit learning.

------------------------------------------------------------------------

48\. · 47% match · 2013 · 0.2 cit/yr\
**Relax but stay in control: from value to algorithms for online Markov decision processes** ([link](https://www.semanticscholar.org/paper/046f7cbdef9147411a5b3223325653fbcafd6caf))\
Peng Guan, M. Raginsky, and R. Willett\
*ArXiv* · Oct 27, 2013 · 2 citations

> Online learning algorithms are designed to perform in non-stationary environments, but generally there is no notion of a dynamic state to model constraints on current and future actions as a function of past actions. State-based models are common in stochastic control settings, but commonly used frameworks such as Markov Decision Processes (MDPs) assume a known stationary environment. In recent years, there has been a growing interest in combining the above two frameworks and considering an MDP setting in which the cost function is allowed to change arbitrarily after each time step. However, most of the work in this area has been algorithmic: given a problem, one would develop an algorithm almost from scratch. Moreover, the presence of the state and the assumption of an arbitrarily varying environment complicate both the theoretical analysis and the development of computationally efficient methods. This paper describes a broad extension of the ideas proposed by Rakhlin et al. to give a general framework for deriving algorithms in an MDP setting with arbitrarily changing costs. This framework leads to a unifying view of existing methods and provides a general procedure for constructing new ones. Several new methods are presented, and one of them is shown to have important advantages over a similar method developed from scratch via an online version of approximate dynamic programming.

------------------------------------------------------------------------

49\. · 46% match · 2014 · 38 cit/yr\
**An Information-Theoretic Analysis of Thompson Sampling** ([link](https://www.semanticscholar.org/paper/de6c988f7a6962a09a1c11f41ded0b63a5418559))\
Daniel Russo and Benjamin Van Roy\
*J. Mach. Learn. Res.* · Mar 20, 2014 · 458 citations

> We provide an information-theoretic analysis of Thompson sampling that applies across a broad range of online optimization problems in which a decision-maker must learn from partial feedback. This analysis inherits the simplicity and elegance of information theory and leads to regret bounds that scale with the entropy of the optimal-action distribution. This strengthens preexisting results and yields new insight into how information improves performance.

------------------------------------------------------------------------

50\. · 46% match · 2014 · 26 cit/yr\
**Learning to Optimize via Information-Directed Sampling** ([link](https://doi.org/10.1287/opre.2017.1663))\
Daniel Russo and Benjamin Van Roy\
*Oper. Res.* · Mar 20, 2014 · 315 citations

> We propose information-directed sampling - a new algorithm for online optimization problems in which a decision-maker must balance between exploration and exploitation while learning from partial feedback. Each action is sampled in a manner that minimizes the ratio between the square of expected single-period regret and a measure of information gain: the mutual information between the optimal action and the next observation.
>
> We establish an expected regret bound for information-directed sampling that applies across a very general class of models and scales with the entropy of the optimal action distribution. For the widely studied Bernoulli and linear bandit models, we demonstrate simulation performance surpassing popular approaches, including upper confidence bound algorithms, Thompson sampling, and knowledge gradient. Further, we present simple analytic examples illustrating that information-directed sampling can dramatically outperform upper confidence bound algorithms and Thompson sampling due to the way it measures information gain.

*Showing top 50 of 63 papers. Full details available via CSV or BibTeX export.*
