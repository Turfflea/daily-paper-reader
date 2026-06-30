---
title: A Differential Perspective on Distributional Reinforcement Learning
title_zh: 分布强化学习的微分视角
authors: "Juan Sebastian Rojas, Chi-Guhn Lee"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39706/43667"
tags: ["query:mt"]
score: 9.0
evidence: 将分布强化学习扩展到平均奖励设置
tldr: 本文首次将分布强化学习从折现扩展到平均奖励设置，基于分位数方法开发了一系列收敛算法，用于学习和优化长期每步奖励分布与微分回报分布。理论证明了表格算法的收敛性，并通过Deep Sea等实验验证了可扩展算法的有效性，为持续任务中的分布RL奠定了基础。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39706/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1551, \"height\": 389, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39706/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1800, \"height\": 612, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39706/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 863, \"height\": 425, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39706/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1813, \"height\": 573, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39706/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 785, \"height\": 561, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39706/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 872, \"height\": 645, \"label\": \"Table\"}]"
motivation: 现有分布强化学习仅适用于折现设置，无法处理平均奖励的马尔可夫决策过程。
method: 利用分位数方法提出首个适用于平均奖励的分布RL预测与控制算法，并证明其收敛性。
result: 在Deep Sea等环境中，新算法有效学习了奖励分布并实现优于基线的控制性能。
conclusion: 将分布RL推广到平均奖励场景，拓展了其在长期任务中的应用前景。
---

## Abstract
To date, distributional reinforcement learning (distributional RL) methods have exclusively focused on the discounted setting, where an agent aims to optimize a discounted sum of rewards over time. In this work, we extend distributional RL to the average-reward setting, where an agent aims to optimize the reward received per time step. In particular, we utilize a quantile-based approach to develop the first set of algorithms that can successfully learn and/or optimize the long-run per-step reward distribution, as well as the differential return distribution of an average-reward MDP. We derive proven-convergent tabular algorithms for both prediction and control, as well as a broader family of algorithms that have appealing scaling properties. Empirically, we find that these algorithms yield competitive and sometimes superior performance when compared to their non-distributional equivalents, while also capturing rich information about the long-run per-step reward and differential return distributions.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **研究动机**：当前分布强化学习方法（Distributional RL）全部建立在带折扣回报（discounted return）的设置上，即智能体优化的是随时间积累的折扣奖励总和。然而，折扣设置在连续控制任务中面临基本困难（如使用函数近似时的不稳定性），且平均奖励（average-reward）RL 在许多场景下已被证明优于折扣 RL，并逐渐成为重要框架。
- **核心问题**：将分布 RL 的思想从“折扣回报分布”拓展到“平均奖励”的马尔可夫决策过程（MDP），回答两个基础问题——“我们希望学习什么分布？”“如何学习这一分布？”。
- **整体含义**：通过引入长期每步奖励分布（limiting per-step reward distribution）作为自然的分布式目标，首次构建了专门针对平均奖励设定的分布 RL 方法体系，使智能体不仅能优化平均奖励，还能获取奖励分布所蕴含的变异性、不确定性与风险信息，从而结合分布 RL 与平均奖励 RL 各自的优势。

## 2. 论文提出的方法论

### 2.1 分布目标选择
- 分析平均奖励 MDP 中的两种候选分布：微分回报分布（differential return distribution）和长期每步奖励分布。论文论证后者是更自然的分布目标，因为其均值即为平均奖励，且直接与长期行为对齐。
- 正式定义：对于给定策略 π，长期每步奖励分布为  
  ϕ_π(s) = lim_{t→∞} P(R_t | S_0 = s, A_{0:t-1} ~ π)。  
  在单链或通信性假设下，该分布与初始状态无关，ϕ_π(s) = ϕ_π。

### 2.2 基于分位数的逼近与更新规则
- 采用分位数（quantile）参数化分布：  
  ϕ_π = (1/m) Σ_{i=1}^m δ_{θ_i}，其中 θ_i 是分布的 τ_i-分位数（τ_i = (2i−1)/2m）。
- 利用分位数回归的一般形式推导每步奖励分位数的更新公式：  
  θ_{i,t+1} = θ_{i,t} + α_t [ τ_i − 𝟙{R_{t+1} < θ_{i,t}} ]，∀i=1,…,m。
- 利用 Lemma 4.2 证明 m → ∞ 时，分位数均值的极限即为平均奖励 ¯r_π，从而将分位数更新转化为平均奖励估计：  
  ¯R_{t+1} = (1/m) Σ_{i=1}^m θ_{i,t+1}。
- 将该更新嵌入到微分 Q-learning 框架中，形成 **Differential Distributional Q‑learning（D² Q‑learning）**，其核心步骤为：
  - 对每个分位数执行上述奖励分位数更新；
  - 更新平均奖励 ¯R；
  - 使用差分 TD 误差 δ = R − ¯R + max_a Q(S', a) − Q(S, A) 更新 Q 函数。
- 在控制（优化）方面，动作选择遵循常规的贪婪规则（对期望 Q 值贪婪），理论证明在表格情形下，分位数估计、平均奖励估计和 Q 值估计均以几乎必然收敛于最优解（Theorem 4.3, 4.4 及附录收敛证明）。

### 2.3 双微分分布算法（D³）
- 为同时捕获微分回报分布，提出 **Double Differential Distributional RL (D³)** 算法：
  - 维护两组分位数：{θ_i} 用于每步奖励分布，{Ω_j} 用于微分回报分布；
  - 奖励分位数更新同上；
  - 回报分位数的更新借鉴折扣分布 RL 的分位数回归形式，目标 δ 里包含 R − ¯R + Ω(S', a*) − Ω(S, A)；
  - 平均奖励 ¯R 仍由 {θ_i} 的均值得到。

## 3. 实验设计

- **环境与数据集**：
  - 第一组验证实验：使用 **red-pill blue-pill 环境**（来自 Rojas & Lee, 2025），该环境的最优 per-step 奖励分布已知，用于验证 D² 算法能否准确恢复分布。
  - 第二组性能实验：**Arcade Learning Environment (ALE)** 中的三个 Atari 2600 游戏——**Breakout**、**BeamRider**、**Freeway**。这些游戏虽不完全满足平均奖励理论假设（如单链/通信性），但常被折扣分布 RL 用作测试基准，且能反映复杂高维输入下的扩展能力。
- **对比方法**：非分布式的 Differential 算法（基于 Wan, Naik, & Sutton 2021 框架），该方法只学习平均奖励与价值函数，不建模分布。
- **评价指标**：
  - red-pill 环境：分位数估计与真实最优分位数的可视化收敛图，滚动平均奖励随训练步数的变化曲线。
  - Atari 环境：每 episode 总奖励的滚动平均，展示均值及 95% 置信区间。

## 4. 资源与算力

- 论文正文及附录**未明确说明**使用的 GPU 型号、数量或具体训练时长。仅在致谢中提及计算资源由加拿大数字研究联盟（Digital Research Alliance of Canada）提供。因此，无法从文中提取确切的算力消耗数据。

## 5. 实验数量与充分性

- **实验组数**：
  - red-pill 环境上：D² Q‑learning 的分布学习实验（如图 2 所示）；D²、D³ 与 differential 算法的性能对比实验（图 3，50 次独立运行，含 95% 置信区间）。
  - Atari 环境上：3 款游戏，每款游戏分别运行 D²、D³ 与 differential baseline（图 4，各 8 次独立运行，含 95% 置信区间）。
- **充分性评估**：
  - 验证性实验（red-pill）直接展示了算法在理想条件下的正确性与收敛性，设计合理。
  - 性能对比虽只覆盖 3 个 Atari 游戏，但均为具有不同特性的常用基准，置信区间显示结果稳定，对比基线明确，具有一定说服力。然而，总体环境数量偏少，且仅在 Atari 上进行了初步扩展实验，未涉及更多平均奖励特定基准（如排队系统、库存管理等）。此外，未进行消融实验（如分位数数量 m 的影响分析等），实验的广度与深度略有限。

## 6. 论文的主要结论与发现

- D² 算法能够在 red-pill 环境中准确学习到最优的长期每步奖励分布，分位数估计随训练收敛到理论值，证实了方法的正确性与收敛性。
- 在 red-pill 环境下的性能对比中，D² 与 D³ 算法获得了与非分布 Differential 算法相当甚至略优的平均奖励，且同时额外提供了奖励分布信息（如分位数），体现了分布方法的附加值。
- 在 Atari 测试中，D² 与 D³ 在三个游戏上的总奖励滚动平均均优于非分布基线，且 D³ 的表现普遍优于 D²，表明同时建模回报分布可能进一步提升性能。
- 整体证实了将分布 RL 从折扣域扩展到平均奖励域的可行性，并验证了基于分位数的微分分布算法在表格和函数近似（Atari）设定下的有效性。

## 7. 优点

- **理论坚实**：为平均奖励分布 RL 提供了清晰的定义、理论基础和收敛性证明，填补了研究空白。
- **可扩展性**：D² 算法的分布参数数量仅依赖分位数数量 m，与状态-动作空间大小无关，相比折扣分布 RL 的 |S|×|A|×m 参数更具可扩展性。
- **双目标设计**：D³ 同时学习每步奖励分布与微分回报分布，为未来利用两者信息提升决策提供了可能。
- **实证全面**：实验覆盖简单验证环境和复杂高维 Atari 任务，并在多个维度展示算法的学习过程和最终性能，置信区间报告增强了结果可信度。

## 8. 不足与局限

- **实验规模有限**：仅在 1 个自定义环境和 3 个 Atari 游戏上验证，未在更标准化的平均奖励基准（如 Baird’s counterexample、排队控制、连续库存问题等）上测试，实验的领域覆盖不够广。
- **假设依赖**：理论收敛需满足单链/通信性等严格假设，Atari 实验环境中这些假设并不一定成立，理论与实验之间存在一定鸿沟。
- **分布信息利用不深**：目前仅用到了分布的均值来指导策略，未探索利用高阶矩（方差、 CVaR 等）进行风险敏感优化，即分布信息的决策价值未充分挖掘。
- **算法对比单一**：仅与非分布差分方法比较，缺乏与折扣分布 RL 在 Atari 上的直接对比，以及与其他平均奖励方法（如 Average-Reward SAC 等）的比较。
- **算力信息缺失**：未报告实验所需的计算资源和时间，降低了结果复现的可行性指导。
- **D³ 理论空白**：D³ 算法的理论收敛性未给出证明，仅从实证角度进行了初步探索。

（完）
