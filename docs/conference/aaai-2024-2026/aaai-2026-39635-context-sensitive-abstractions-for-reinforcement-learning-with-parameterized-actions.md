---
title: Context-Sensitive Abstractions for Reinforcement Learning with Parameterized Actions
title_zh: 参数化动作强化学习中的上下文敏感抽象
authors: "Rashmeet Kaur Nayyar, Naman Shah, Siddharth Srivastava"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39635/43596"
tags: ["query:mt"]
score: 9.0
evidence: 参数化动作的强化学习顺序决策
tldr: 针对真实顺序决策中参数化动作空间带来的挑战，提出上下文敏感抽象方法，使强化学习能处理长时程稀疏奖励环境。该方法无需手工模型，通过抽象学习提升决策效率，在多个基准任务上验证了有效性，为复杂决策优化提供了通用框架。实验证明，该方法在多个复杂决策环境中显著优于现有方法，能够自动发现有效的抽象结构，为参数化动作空间的强化学习提供了新的解决思路，并可迁移至管理科学中的资源分配与调度优化问题。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39635/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 659, \"height\": 650, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39635/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 866, \"height\": 704, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39635/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 834, \"height\": 424, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39635/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 868, \"height\": 710, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39635/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1776, \"height\": 1008, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39635/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 873, \"height\": 525, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39635/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1777, \"height\": 559, \"label\": \"Figure\"}]"
motivation: 真实顺序决策常面临参数化动作空间，传统RL方法难以处理离散与连续动作的组合，限制应用。
method: 提出上下文敏感抽象学习机制，自动从环境中提取有效抽象，使RL在参数化动作空间中高效决策。
result: 在多个复杂决策任务中显著优于现有方法，能自动发现抽象结构，提升长时程稀疏奖励下的性能。
conclusion: 上下文敏感抽象为参数化动作空间的强化学习提供了通用框架，可迁移至管理中的资源优化问题。
---

## Abstract
Real-world sequential decision-making often involves parameterized action spaces that require both, decisions regarding discrete actions and decisions about continuous action parameters governing how an action is executed. Existing approaches exhibit severe limitations in this setting---planning methods demand hand-crafted action models, and standard reinforcement learning (RL) algorithms are designed for either discrete or continuous actions but not both, and the few RL methods that handle parameterized actions typically rely on domain-specific engineering and fail to exploit the latent structure of these spaces. This paper extends the scope of RL algorithms to long-horizon, sparse-reward settings with parameterized actions by enabling agents to autonomously learn both state and action abstractions online. We introduce algorithms that progressively refine these abstractions during learning, increasing  fine-grained detail in the critical regions of the state–action space where greater resolution improves performance. Across several continuous-state, parameterized-action domains, our abstraction-driven approach enables TD(λ)  to achieve markedly higher sample efficiency than state-of-the-art baselines.

---

## 论文详细总结（自动生成）

### 论文核心问题与整体含义
- **研究背景**：现实世界的序贯决策任务（如导航、机器人操作）常涉及**参数化动作空间**，即智能体需同时选择离散动作类型及其连续参数，但现有强化学习（RL）方法难以有效处理这种混合动作空间。
- **现有局限**：
  - 经典规划方法依赖手工构建的动作模型，可扩展性差。
  - 标准RL算法通常仅针对纯离散或纯连续动作空间设计。
  - 少数支持参数化动作的RL方法往往依赖领域特定的工程技巧，且未能挖掘动作空间本身的隐式结构。
- **核心问题**：如何在长时程、稀疏奖励、参数化动作的未知环境中，让智能体自主发现并利用状态-动作空间的潜在结构，实现高效、可扩展的策略学习。
- **整体含义**：本文提出一种上下文敏感的状态与动作抽象学习框架，使基础RL算法（TD(λ)）能在此类挑战性任务中取得远优于现有前沿方法的样本效率。

### 方法论
- **核心思想**：智能体在RL学习过程中，在线构建并渐进式细化一个融合状态与动作抽象的统一层次化结构，称为**状态与参数化动作条件抽象树（SPA-CAT）**。
  - **状态抽象**：将原始连续状态空间划分为若干抽象状态区域。
  - **动作抽象**：为每个抽象状态，为每个动作的参数空间构建一棵**动作参数树（APT）**，其叶子节点代表参数区间的划分，从而实现不同状态下对动作参数精度的差异化需求。
- **关键技术细节**：
  1.  **抽象表示（SPA-CAT）**：
      - SPA-CAT 是一个有向层次树，根节点覆盖整个状态空间，每个节点对应一个状态子集。
      - 每个节点与所有动作的 APT 关联。叶子节点（fringe）定义了当前的抽象状态空间和每个抽象状态下的抽象动作参数区间。
  2.  **异质性驱动的渐进式细化**：
      - 设计了一种混合异质性度量 `H(s, a)`，用于发现当前抽象结构中“过度合并”的区域，即那些将具有不同未来趋势的真实状态错误归为一类的抽象状态-动作对。
      - **公式要点**：`H(s, a) = β * SD[TD-error] + (1-β) * SD[状态价值估计]`
        - `SD[TD-error]`：抽象状态-动作对中TD误差的标准差，在早期学习阶段提供更强的行为不一致信号。
        - `SD[状态价值估计]`：抽象状态内部各具体状态价值估计的标准差，在后期策略稳定时作为更可靠的异质性指标。
        - `β`：一个随训练进程从1退火到0的权重参数，实现从依赖TD误差到依赖价值估计的平滑过渡。
  3.  **两种细化策略**：
      - **均匀划分**：沿状态变量维度进行正交二分，形成轴对齐的超矩形抽象状态。
      - **灵活划分**：通过凝聚聚类和SVM分类器，学习不规则的、能更好贴合环境几何与决策边界的抽象状态边界。
- **算法流程（PEARL算法）**：
  - 交替执行**学习阶段**与**细化阶段**。
  - **学习阶段**：基于当前抽象的SPA-CAT结构，使用表格型TD(λ)学习抽象策略。执行动作时从抽象参数区间均匀采样得到具体参数。
  - **细化阶段**：每经历固定`nrefine`个回合后，根据收集的轨迹数据计算异质性，选取异质性最高的前k个抽象状态和动作进行细化，更新SPA-CAT结构，然后清空数据缓冲区继续学习。

### 实验设计
- **测试环境**：四个具有连续状态变量、参数化动作、随机环境动力学和稀疏奖励的挑战性领域：
  1.  **Office World**：导航至不同地点取送咖啡和邮件。
  2.  **Pinball**：控制小球避开复杂障碍物进入目标洞。
  3.  **Multi-City Transport**：在城市间取送包裹，需利用机场中转。
  4.  **Robot Soccer Goal**：控制球员绕过守门员将球踢进球门。
- **基线方法**：与少数能处理参数化动作的前沿Deep RL方法对比。
  - **MP-DQN**：通过多通道网络结构解决P-DQN的过参数化问题。
  - **HyAR**：使用变分自编码器学习混合动作的隐式表征。
- **评价指标**：
  - 训练过程中的累积平均回报。
  - 最终贪婪策略的成功率（通过评估回合测量）。

### 资源与算力
- 论文正文及提供的元数据中**未明确提及**实验所用的具体硬件配置（如GPU型号、数量）或绝对训练时长。
- 论文指出PEARL因使用表格型抽象和TD(λ)而非深度神经网络，其运行时间显著低于基线方法。

### 实验数量与充分性
- **实验数量**：在4个不同领域上，将2种PEARL变体（灵活版、均匀版）与2种基线方法进行对比，**共计至少4×4组核心实验**。
- **消融与辅助实验**：
  - 对比了`PEARL-灵活版`的激进与保守细化策略（通过控制最大抽象状态数），分析抽象粒度对性能与简洁性的影响。
  - 对关键超参数`β`（异质性估计退火）进行了消融实验，对比了仅用TD误差、仅用价值函数和混合使用三种设置。
- **公平性**：所有基线均移除了原代码中特定于环境的、可能提供先验偏好的初始化设置，改为零初始化或随机初始化，确保了比较的客观性与公平性。所有结果均在**50次独立运行**上统计均值和标准差。

### 主要结论与发现
1.  **性能显著提升**：PEARL的两个变体在所有四个领域上，从零开始学习，其样本效率和最终策略成功率均一致且显著地优于MP-DQN和HyAR。
2.  **灵活抽象的优势**：`PEARL-灵活版`通过学习非均匀、更贴合环境结构的抽象，取得了总体最优的性能。
3.  **粒度可控的权衡**：`PEARL-灵活版`能通过调整参数控制抽象粒度，实现策略性能与抽象简洁性之间的平衡。
4.  **混合异质性信号的关键作用**：结合TD误差与状态价值估计的混合异质性度量，在学习全程中均优于仅使用单一信号的方案。
5.  **方法有效性假设得到验证**：证明了在长时程、稀疏奖励的参数化动作任务中，自主学习和利用状态与动作空间的结构化抽象，是提升基础TD(λ)算法能力并超越复杂深度RL方法的有效路径。

### 优点
- **方法创新性**：首次提出统一的状态和参数化动作条件抽象框架（SPA-CAT），形式化定义清晰。根据上下文所需精度动态调整动作参数区间的思想具有启发性。
- **技术深度**：基于TD-Error与价值估计的混合异质性度量，以及从均匀细分到灵活聚类的抽象学习策略，设计精巧，理论依据明确。
- **显著的实验效果**：在不依赖深度神经网络和领域知识初始化的情况下，以简单的表格型TD(λ)大幅超越现有深度强化学习基线，结果极具说服力。
- **实用性与可解释性**：学习到的抽象结构（如图1所示的状态分区和动作参数区间）具备良好的可视化效果和可解释性，有助于理解智能体决策逻辑。

### 不足与局限
- **状态空间维度限制**：SPA-CAT本质上是一种分区机制，虽比均匀划分更灵活，但在面对更高维状态空间时可能遭遇维度灾难，其可扩展性有待验证。
- **动态环境的适应性**：所学习的抽象结构是针对当前环境学得的，文中未讨论当环境动力学或目标变化时，该方法能否快速适应或迁移。
- **聚类方法的局限**：灵活抽象依赖于基于距离的凝聚聚类和SVM分离超平面，可能无法完美捕捉需要非线性复杂边界的情况。
- **计算开销的详细分析缺失**：虽定性指出运行时间低于深度RL基线，但缺乏对抽象学习过程本身计算复杂度（如聚类、SVM训练）随问题规模增长的定量分析。
- **基准方法的选择**：对比的基线仅限于两种参数化动作RL方法，未与其他可能的方案（如层次化RL结合基础离散-连续控制器）进行对比。

（完）
