---
title: Safe Multi-Agent Reinforcement Learning via Distributional Safety Critic and Maximum Entropy Optimization
title_zh: 基于分布安全评审和最大熵优化的安全多智能体强化学习
authors: "Qiwei Liu, Ye Yuan, Lingyue Zhang, Kaitian Chen, Yunkai Lv, Sheng Gao, Huaicheng Yan"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40198/44159"
tags: ["query:mt"]
score: 8.0
evidence: 提出基于分布安全评论家和最大熵的 safe 多智能体强化学习方法用于序列决策
tldr: 多智能体强化学习应用于安全关键系统时面临探索不足和约束保证弱的问题。本文提出基于条件风险价值的最大熵安全框架，设计最坏情况多智能体软演员-评论家算法，实现全流程安全，为多主体序贯决策优化提供了可靠方案，对管理中的自动化调度与风险控制有借鉴意义。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40198/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1773, \"height\": 908, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40198/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 878, \"height\": 933, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40198/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 878, \"height\": 932, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40198/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 878, \"height\": 932, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40198/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 882, \"height\": 866, \"label\": \"Figure\"}]"
motivation: 多智能体强化学习在安全关键领域面临探索效率低和期望约束无法保证全程安全的问题。
method: 提出基于CVaR的联合安全度量，构建最大熵安全MARL框架和最坏情况多智能体软演员-评论家算法。
result: 实验表明，该方法在满足安全约束的同时能学到较优的联合策略。
conclusion: 引入分布安全评论家可有效提升多智能体强化学习的安全性，适用于管理决策的分布式优化。
---

## Abstract
Deploying multi-agent reinforcement learning (MARL) in safety-critical systems faces significant challenges due to insufficient agent exploration and inadequate safety constraint guarantees. Current approaches are constrained by two fundamental limitations: inefficient exploration leading to suboptimal policies, and expected-cost-based constraint frameworks failing to ensure full-process safety. To address these challenges, this paper proposes a novel safety-aware maximum entropy MARL framework using Conditional Value-at-Risk (CVaR) as a joint safety metric, which quantifies constraint satisfaction under worst-case scenarios for multi-agent systems. Moreover, we develop the Worst-Case Multi-Agent Soft Actor-Critic (WCMASAC) algorithm, incorporating sequential update mechanisms and maximum entropy optimization for heterogeneous agents, enhanced with distributed safety critics. Theoretically, we establish the monotonic improvement property, guaranteed constraint satisfaction, and convergence to a generalized Nash equilibrium for WCMASAC. Extensive experiments on Safety-Gymnasium based benchmarks demonstrate that WCMASAC outperforms state-of-the-art baselines in both task reward acquisition and safety constraint violation reduction, while exhibiting superior exploration efficiency and risk-aware control capabilities.

---

## 论文详细总结（自动生成）

## 详细中文总结

### 1. 论文的核心问题与整体含义
- **研究背景**：多智能体强化学习（MARL）在安全关键系统中面临两大挑战：代理探索不充分导致策略次优，以及基于期望代价的约束框架无法保证全过程安全（无法抑制瞬时高危行为）。
- **研究动机**：现有安全 MARL 方法（如 MACPO、MAPPO‑Lagrangian）仅约束期望代价，缺乏对代价分布重尾风险的建模，且保守的安全约束限制了探索范围，容易陷入局部安全最优。最大熵 RL 可以增强探索，但在多智能体系统中融入安全约束尚未被充分研究。
- **整体含义**：本文提出一种融入分布安全评论的**最大熵安全 MARL 框架**，以条件风险价值（CVaR）作为联合安全度量，既保证全过程约束满足，又提升探索效率，实现风险感知的协同决策。

### 2. 论文提出的方法论
- **核心思想**：将最大熵强化学习（最大化回报+策略熵）与基于 CVaR 的分布安全约束相结合，使多智能体系统在最坏情况风险可控的前提下学习随机策略，最终收敛到量子响应均衡（QRE）。
- **关键技术细节**：
  - **安全度量**：采用 CVaR 替代期望代价作为约束指标，关注代价分布的尾部风险（`ϕ_{1−ω}(C_{ij}) ≤ d_{ij}`）。
  - **安全分解假设**：引入“风险敏感个体‑全局‑最大”（RIGM）条件，使得联合代价分布可分解为各代理代价的加权和。
  - **理论与算法**：基于优势分解引理和代价分布分解引理，证明序贯局部策略更新等价于联合策略更新，并推导出 WCMASAC 算法。
  - **WCMASAC 实现**：
    - **奖励评论家**：集中式软 Q 函数，最小化 Bellman 残差（含熵项）。
    - **代价评论家**：基于分位数回归和多头注意力的分布安全评论家，最小化 Huber 损失，学习 CVaR 风险度量。
    - **演员更新**：每个代理按序贯更新策略，最小化 KL 散度，其中目标包含 Q 值、CVaR 约束惩罚和熵奖励，即 `J_loss(θ_a) = E[ Q_θr − λ ϕ_{1−ω}[C_θc] − α log π ]`。
  - **理论保证**：证明了策略的单调改善、安全约束满足以及收敛到 QRE（广义纳什均衡）。

### 3. 实验设计
- **数据集/场景**：基于 **Safety‑Gymnasium** 基准的 **Safe Multi‑MuJoCo** 多智能体速度约束任务，具体包括：
  - `2x4Ant`、`4x2Ant`、`2x3HalfCheetah`、`6x1HalfCheetah`、`3x1Hopper`、`2x3Walker2D`。
- **对比方法**：
  - 有安全约束的 SOTA：**MACPO**、**MAPPO‑Lagrangian**。
  - 无约束的 SOTA MARL 基线：**HASAC**。
  - 公平对比的变体：**Constrained‑HASAC**（直接约束联合期望，而非 CVaR）。
- **评估指标**：平均幕长奖励（任务性能）与平均幕长代价（安全约束违反程度），测试阶段统计。

### 4. 资源与算力
- 文中未明确提及所使用的 GPU 型号、数量、训练时长等具体算力资源。仅说明所有算法在统一的标准框架下实现，网络架构、超参数、优化器等细节提供在附录中。**算力信息缺失**。

### 5. 实验数量与充分性
- **实验组数**：
  - 6 个不同任务下与 4 种算法（MACPO、MAPPO‑Lag、HASAC、Constrained‑HASAC）的奖励与代价对比，总计至少 24 组主要实验。
  - 额外对 **风险水平参数 ω** 进行敏感性分析（1−ω = 0.01, 0.5, 0.99），展示奖励与代价变化趋势，相当于再增加约 6 组实验。
- **充分性与公平性**：
  - 覆盖多种机器人形态与数量组合，环境多样。
  - 直接与 SOTA 安全方法和未约束方法对比，并设计了 Constrained‑HASAC 进行消融/公平对比，突出 CVaR 约束的收益。
  - 统计分析展示学习曲线和最终性能，实验设定客观。但未报告多个随机种子的标准差或统计显著性检验，略显不足。

### 6. 论文的主要结论与发现
- WCMASAC 在所有测试环境中均能**满足安全约束（全程不违反）**，而 MACPO 在多环境下违反约束，MAPPO‑Lag 偶有违反。
- 相比安全基线，WCMASAC 在**奖励获取和收敛速度**上显著更优，同时保持更低或可比的代价水平；其探索效率优于 MACPO 等保守方法。
- 相比无约束的 HASAC，WCMASAC 大幅降低了安全代价，同时奖励仍处高水平，证明了最大熵安全框架的有效性。
- CVaR 约束比期望约束更有效：WCMASAC 比 Constrained‑HASAC 在多数环境中代价更低，尾部风险控制能力更强。
- 风险水平 ω 可调，1−ω 越高策略越保守，可实现奖励‑安全间的灵活折中。

### 7. 优点
- **理论创新**：首次将分布 CVaR 约束与最大熵 MARL 结合，给出了单调改进、约束满意和 QRE 收敛的理论证明。
- **算法设计**：序贯更新机制+分布安全评论家实现风险感知的异构代理策略学习，无需强假设的价值分解。
- **安全表现**：在所有实验环境中均实现全程零违反，体现了分布安全评论家对高风险动作的抑制能力。
- **探索效率**：最大熵鼓励探索，使奖励收敛速度快于现有安全方法。
- **灵活性**：可通过调整风险水平参数 ω 适应不同安全需求。

### 8. 不足与局限
- **可扩展性未验证**：实验仅限中小规模机器人任务，未在大规模智能体网络或高维状态/动作空间中测试，可扩展性需进一步研究。
- **假设依赖**：使用 RIGM 条件（联合代价可分解为个体代价加权和），该假设在部分非完全协同或不满足因子分解的安全场景下可能不成立。
- **算力与复现细节缺失**：未说明 GPU 资源与训练时间，可能影响复现和实用部署评估。
- **统计稳定性**：未见多次运行的标准差或显著性检验，难以评估算法在不同随机种子下的稳定性。
- **应用限制**：仅在仿真环境中测试，未考虑现实世界的不确定性、通信延迟等问题。

（完）
