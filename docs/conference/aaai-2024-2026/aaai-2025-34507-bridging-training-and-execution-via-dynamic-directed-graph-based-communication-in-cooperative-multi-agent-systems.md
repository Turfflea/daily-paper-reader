---
title: Bridging Training and Execution via Dynamic Directed Graph-Based Communication in Cooperative Multi-Agent Systems
title_zh: 基于动态有向图通信的协作多智能体系统训练与执行桥接
authors: "Zhuohui Zhang, Bin He, Bin Cheng, Gang Li"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/34507/36662"
tags: ["query:mt"]
score: 8.0
evidence: 多智能体强化学习的图通信机制
tldr: 针对多智能体系统在部分可观测任务中缺乏动态通信机制的问题，提出TGCNet算法，学习动态有向图通信拓扑，并利用图粗化网络近似全局状态，桥接训练与执行。实验表明，TGCNet在协作任务中优于现有方法，提升了通信效率和协作性能，为管理科学中的资源联合调度、供应链协作等提供借鉴。TGCNet结合Transformer解码器进行特征聚合，实现了高效的动态通信，在多智能体基准测试中验证了其先进性能。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34507/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 710, \"height\": 458, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34507/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1399, \"height\": 716, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34507/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1359, \"height\": 378, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34507/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1835, \"height\": 404, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34507/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1750, \"height\": 894, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34507/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1746, \"height\": 452, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34507/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1331, \"height\": 402, \"label\": \"Figure\"}]"
motivation: 多智能体协作通常需要通信，但现有方法缺乏动态有向通信机制，且依赖全局状态，限制了应用。
method: 提出TGCNet，学习动态有向图通信拓扑，并通过图粗化网络近似全局状态，桥接训练与执行。
result: TGCNet在协作多智能体任务中优于现有MARL方法，提高了通信效率和团队协作性能。
conclusion: TGCNet为多智能体系统提供了高效的动态通信机制，其图粗化桥接训练与执行的方法可应用于供应链协调等管理场景。
---

## Abstract
Multi-agent systems must learn to communicate and understand interactions between agents to achieve cooperative goals in partially observed tasks. However, existing approaches lack a dynamic directed communication mechanism and rely on global states, thus diminishing the role of communication in centralized training. Thus, we propose the Transformer-based graph coarsening network (TGCNet), a novel multi-agent reinforcement learning (MARL) algorithm. TGCNet learns the topological structure of a dynamic directed graph to represent the communication policy and integrates graph coarsening networks to approximate the representation of global state during training. It also utilizes the Transformer decoder for feature extraction during execution. Experiments on multiple cooperative MARL benchmarks demonstrate state-of-the-art performance compared to popular MARL algorithms. Further ablation studies validate the effectiveness of our dynamic directed graph communication mechanism and graph coarsening networks.

---

## 论文详细总结（自动生成）

### 论文核心问题与整体含义
- **研究动机**：多智能体系统在部分可观测的协作任务中必须通过通信来弥补局部视野的局限。然而，现有方法普遍存在两大缺陷：一是通信机制通常为静态广播或简单注意力，缺乏动态有向的针对性交互；二是在集中训练阶段严重依赖全局状态，而在分散执行时无法获取，导致训练与执行脱节，通信的作用未能在两个阶段一致发挥。
- **核心问题**：论文旨在设计一种统一范式，同时解决通信的五个关键问题——**与谁通信、何时通信、通信什么信息、如何融合接收信息、如何避免依赖全局状态**，从而桥接训练与执行，提升多智能体协作性能。

### 方法论
- **整体框架**：提出 **TGCNet**，一种基于动态有向图与 Transformer 的值分解多智能体强化学习算法。它学习一个动态有向图的拓扑结构作为通信策略，训练时利用 **图粗化网络** 近似全局状态，执行时用 **Transformer 解码器** 提取特征，两者共享同一动态有向图，实现训练与执行的无缝衔接。
- **关键技术细节**：
  - **动态有向图与邻接轨迹矩阵**：将智能体间通信建模为一个沿时间变化的动态有向图，用布尔邻接轨迹矩阵 \(A\) 表示时刻 \(t\) 从 \(i\) 到 \(j\) 是否通信。消息 \(\tilde{m}_i^t = A_i^t \odot m_i^t\)。
  - **多键门控通信机制（学习 \(A\)）**：采用类似编码器的结构，用 **Gumbel-Softmax** 为每条可能的边输出 0/1 决策；多键（multi-key）设计允许同一时间步产生多次定向通信，通过取多次键值的最大值并向上取整，得到最终的邻接矩阵 \(A_i^t\)。该机制直接决定通信的对象与时机。
  - **Transformer 解码器（融合信息）**：接收通信后的消息 \(\tilde{m}_i^t\)，通过两层 Transformer 块（自注意力、残差连接等）进行特征聚合与交互，最终输出个体 Q 值与下一时刻的隐藏状态。
  - **图粗化网络（近似全局状态）**：在训练阶段，基于局部观测和邻接矩阵，通过多层图卷积（GCN）与自注意力池化（Self-AttPooling）对图进行粗化，最后用全局读出操作（拼接平均池化和最大池化）生成全局状态的近似 \(s_t\)，替代真实全局状态输入到混合网络 \(Q_{tot}\) 中。该网络的设计基于理论分析，将全局状态分解为聚合与去重叠两个部分。
  - **训练目标**：端到端最小化 TD 误差，混合网络结构与 QMIX 类似，但**不再需要显式的全局状态**作为输入。

### 实验设计
- **测试场景与基准**：
  - **Hallway**：一个需要高频通信才能完成的走廊导航任务（4个智能体）。
  - **Level‑Based Foraging (LBF)**：11×11 网格的稀疏奖励觅食任务（6个智能体收集4个食物）。
  - **StarCraft Multi-Agent Challenge (SMAC)**：8张不同难度的地图，包括 `1o2r_vs_4r`、`1o10b_vs_1r` 等需要通信发现敌人的场景，以及6个硬/超硬场景（如 `5m_vs_6m`、`27m_vs_30m`）。
- **对比方法**：5个代表性基线 —— QMIX（无通信）、IC3Net、DGN、G2ANet 和 GraphMix。所有算法均使用相同的 EPyMARL 框架实现。
- **评估方式**：5 个随机种子，结果以均值与 95% 置信区间呈现测试胜率或回报。

### 资源与算力
- 论文正文及附录中**未提及**所使用的 GPU 型号、数量或具体训练时长。因此无法评估其算力开销与效率。

### 实验数量与充分性
- **总体实验量**：覆盖 3 大类环境（Hallway, LBF, SMAC），至少 10 个不同难度的场景（含 8 个 SMAC 地图），并在每个场景上与 5 种基线进行了对比；所有实验均重复 5 次随机种子。
- **消融实验**：在 3 个硬/超硬场景上，对比了 TGCNet、去除通信机制的 TGCNet‑QMIX（用真实全局状态替换图粗化网络）和全通信版本 TGCNet‑FC，验证了图粗化网络与多键门控各自的作用。
- **可解释性分析**：对训练后的通信策略进行可视化，展示了图结构随任务状态（是否发现敌人）的动态变化。
- **公平性与充分性**：统一代码框架、多随机种子、包含消融研究，实验设计较为系统、公平。但未报告超参数搜索细节，且缺少在连续控制或更大规模环境下的验证。

### 主要结论与发现
- TGCNet 在所有任务上均取得了**最先进的收敛性能**，尤其在必须通信的 Hallway 任务中，无通信方法完全失败，TGCNet 显著优于其他通信方法。
- **图粗化网络能够有效拟合全局状态**：替换为真实全局状态的 TGCNet‑QMIX 性能与 TGCNet 相当，证明它成功避免了训练时对全局状态的依赖。
- **多键门控通信机制在减少冗余的同时不牺牲性能**：全通信版本 TGCNet‑FC 收敛更慢但最终性能相近，表明多键网络优化了通信结构，降低了通信开销。
- 可视化显示学习到的动态有向图具有**可解释性**：当敌方进入视野时通信增强，无新信息时通信自然减少。

### 优点
- **范式创新**：首次将动态有向图同时应用于训练和执行，用图粗化网络替代全局状态，实现了训练与执行的真实桥接。
- **问题求解全面**：一体式解决通信的五个核心问题，设计精巧，思路连贯。
- **技术融合**：将 Gumbel‑Softmax、Transformer 解码器与图池化有机结合，在通信生成与信息融合上具有先进性。
- **实验扎实**：在多种离散动作环境上进行详尽对比，消融分析清晰，并提供了通信可解释性证据。

### 不足与局限
- **算力信息缺失**：未报告计算成本，难以评估实际部署效率与资源消耗。
- **环境局限性**：所有实验均为网格世界或星际争霸微操场景，未涉及连续动作空间或更复杂的机器人仿真环境，泛化性有待进一步验证。
- **通信成本量化不足**：虽定性表明减少了通信，但未提供通信量（如带宽、消息数量）的定量对比。
- **可扩展性验证薄弱**：宣称可应对智能体数量变化的场景，但实验未包含动态增减智能体的任务。
- **方法限制**：仅与基于价值分解（QMIX 系）的算法结合，尚未扩展到策略梯度方法（如 MAPPO），且 Transformer 解码器和图卷积可能带来额外计算开销，对大规模智能体系统的适用性未知。

（完）
