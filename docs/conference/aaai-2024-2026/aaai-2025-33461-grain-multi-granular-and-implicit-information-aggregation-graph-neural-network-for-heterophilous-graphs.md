---
title: "GRAIN: Multi-Granular and Implicit Information Aggregation Graph Neural Network for Heterophilous Graphs"
title_zh: GRAIN：面向异配图的多粒度和隐式信息聚合图神经网络
authors: "Songwei Zhao, Yuan Jiang, Zijing Zhang, Yang Yu, Hechang Chen"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/33461/35616"
tags: ["query:mt"]
score: 9.0
evidence: 面向异配图的多粒度和隐式信息聚合图神经网络
tldr: 针对图神经网络在异配图上性能下降的问题，本文提出GRAIN模型。该模型通过在多个粒度级别聚合多视图信息，并利用隐式关系捕获远距离节点间的联系，增强了节点嵌入的表达能力。实验表明，GRAIN在异配图节点分类等任务上显著优于MLP和现有GNN方法，有效突破了传统GNN的同配性假设限制，为复杂关系数据建模提供了更灵活的解决方案。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33461/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1539, \"height\": 839, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33461/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 834, \"height\": 503, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33461/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1519, \"height\": 758, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33461/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1829, \"height\": 600, \"label\": \"Table\"}]"
motivation: 现有GNN在异配图任务中常被简单MLP超越，因其忽略了信息粒度的重要性且较少考虑远距离节点间的隐式关系。
method: 提出GRAIN，通过多粒度级别聚合多视图信息，并捕获远距离节点的隐式关系，增强节点表示。
result: 实验证明，GRAIN在异配图节点分类等任务上优于MLP和现有GNN方法，具有显著性能提升。
conclusion: GRAIN有效解决了异配图上的信息聚合问题，为图神经网络在非标准关系数据上的应用开辟了新途径。
---

## Abstract
Graph neural networks (GNNs) have shown significant success in learning graph representations. However, recent studies reveal that GNNs often fail to outperform simple MLPs on heterophilous graph tasks, where connected nodes may differ in features or labels, challenging the homophily assumption. Existing methods addressing this issue often overlook the importance of information granularity and rarely consider implicit relationships between distant nodes. To overcome these limitations, we propose the Granular and Implicit Graph Network (GRAIN), a novel GNN model specifically designed for heterophilous graphs. GRAIN enhances node embeddings by aggregating multi-view information at various granularity levels and incorporating implicit data from distant, non-neighboring nodes. This approach effectively integrates local and global information, resulting in smoother, more accurate node representations. We also introduce an adaptive graph information aggregator that efficiently combines multi-granularity and implicit data, significantly improving node representation quality, as shown by experiments on 13 datasets covering varying homophily and heterophily. GRAIN consistently outperforms 12 state-of-the-art models, excelling on both homophilous and heterophilous graphs.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **核心问题**：传统图神经网络（GNN）依赖于同配性假设（homophily），认为相连节点具有相似特征或标签。然而，在异配图（heterophilous graphs）中，相连节点在特征或标签上可能存在显著差异，导致 GNN 的信息聚合方式失效，甚至表现不如简单的多层感知机（MLP）。
- **研究动机**：现有解决异配问题的方法常常只针对异配图设计，在同配图上泛化不足；且普遍忽略了信息粒度的选择（粗粒度 vs 细粒度），以及远距离非邻居节点之间的**隐式关系**。因此，需要一种能自适应地融合多粒度信息、捕获远距离隐式关联，并在同配与异配图上均表现优越的方法。
- **整体含义**：论文提出 **GRAIN（Granular and Implicit Graph Network）** 模型，通过多粒度信息聚合与隐式关系建模，生成更平滑、更准确的节点表示，从而提升在异配图上的节点分类性能，同时保持在同配图上的竞争力。

## 2. 论文提出的方法论

### 2.1 整体框架
GRAIN 包含两大核心模块：
1. **智能粒度感知器（Intelligent Granularity Perceiver）**：将图中每个节点的信息聚合跳数选择建模为一个**马尔可夫决策过程（MDP）**，利用强化学习（RL）动态调整聚合范围。
2. **多视图聚合器（Multi-view Aggregator）**：根据感知器输出的策略，聚合不同粒度的邻域信息及隐式信息，生成最终节点嵌入。

### 2.2 智能粒度感知器
- **MDP 建模**：
  - **状态（State）**：当前节点的表示。
  - **动作（Action）**：当前节点聚合信息的跳数（连续值）。
  - **奖励（Reward）**：最近若干时间步内验证集分类准确率的滑动平均奖励。
- **RL 算法**：采用 **Actor-Critic** 框架，具体使用 **TD3（Twin Delayed Deep Deterministic Policy Gradient）** 算法处理连续动作空间。Critic 双网络取最小值防止 Q 值过估计，并利用该奖励信号更新 Actor。
- **核心作用**：使每个节点能够根据不同上下文自适应决定聚合多少跳邻居信息，同时通过连续动作的设计捕捉远距离节点间的**隐式联系**（如潜在语义相似性）。

### 2.3 多视图聚合器
聚合函数整合三种信息分量：
- **粗/细粒度信息**：整数跳内的邻居特征取平均，即 \(\frac{1}{\langle a_t \rangle} \sum_{k=1}^{\langle a_t \rangle} \hat{\mathbf{A}}^k \mathbf{H}^{(l)}\)。
- **隐式信息**：通过取动作值的下取整与上取整之间的差值，加权插入更远一跳的信息，即 \((a_t - \lfloor a_t \rfloor) \hat{\mathbf{A}}^{\langle a_t \rangle} \mathbf{H}^{(l)} + (\lceil a_t \rceil - a_t) \hat{\mathbf{A}}^{\langle a_t \rangle + 1} \mathbf{H}^{(l)}\)。
- 最终节点表示 = 上述分量之和，再经过激活函数与 Dropout 后接 log softmax。

### 2.4 效率优化技术
- **参数共享**：不同聚合层共享权重矩阵，将参数量从 \(O(N \cdot \langle a_t \rangle^2)\) 降至 \(O(N \cdot \langle a_t \rangle)\)。
- **缓冲机制**：缓存探索过程中收集的 (状态, 动作, 奖励) 数据，达到 batch size 后再批量训练 GNN，减少重复计算。

这些设计使 GRAIN 能够在动态调整聚合范围的同时保持较高的训练效率。

## 3. 实验设计

### 3.1 数据集
- **同配数据集**：Cora, Citeseer, Pubmed（引文网络），Computers, Photo（Amazon 共购网络），Coauthor CS（合著网络）。
- **异配数据集**：Cornell, Wisconsin, Texas, Film, Actor, Squirrel, Chameleon。
- 共 **13 个**数据集，覆盖不同同配率（edge homophily ratio）。

### 3.2 对比基准方法
- **传统方法**：MLP（两层）。
- **基础 GNN**：GCN, GAT, GraphSAGE。
- **同配导向方法**：MixHop, APPNP, GCNII。
- **异配导向方法**：GPR‑GNN, GGCN, ACM‑GCN, FE‑GNN, GNN‑SATA。
- 共 **12 种**基准模型。

### 3.3 评估指标与设置
- 任务：节点分类。
- 评价指标：分类准确率（average test accuracy），并使用 **Drop（↓）** 表示基线相对于 GRAIN 的性能下降幅度。
- 实验设置：GNN 模块使用 ReLU 激活，batch size 128；超参数 α（平衡聚合信息与自身特征）在 0–1 之间调节，文中给出灵敏度分析。

## 4. 资源与算力
- 论文正文及附录均**未明确说明**所使用 GPU 型号、数量、训练时长等算力资源信息。

## 5. 实验数量与充分性
- **主实验**：8 个数据集（Table 1），另加 5 个数据集在附录（C.2）中报告，覆盖 13 个数据集上 12 种方法的全比较。
- **消融实验**：
  - 不同 RL 方法对比（C.3，附录），比较值函数型、策略梯度型与 Actor‑Critic 型 RL 的效果。
  - 超参数 α 的灵敏度分析（Figure 2）。
  - 参数共享与缓冲机制的效率分析（文中提及，但未给出单独详细表格，可能隐含在训练描述中）。
- **可视化分析**：t‑SNE 降维可视化（Figure 3），直观展示嵌入可分离性。
- **公平性**：所有基准方法均采用原论文推荐设置或默认配置，实验结果在统一的数据划分下进行，比较客观。实验数量充足，覆盖广泛，消融与分析有力支撑结论。

## 6. 论文的主要结论与发现
- **GRAIN 在主实验中全面领先**：在绝大多数数据集上取得最佳分类准确率，总体平均提升 2.36%（与最佳基线相比），且在同配与异配数据集上均表现优秀。
- **多粒度信息至关重要**：灵敏度分析表明，适当增大粗/细粒度信息的比例（α = 0.2 附近）有助于学习更平滑的表示，提升性能。
- **隐式关系建模有效**：连续动作空间和 Actor‑Critic RL 让模型能够捕获远距离节点的潜在关联，进一步强化节点嵌入。
- **效率优化可行**：参数共享与缓冲机制使模型在保持高性能的同时控制训练开销。

## 7. 优点
- **创新理念**：首次将信息粒度选择与隐式关系建模统一到 GNN 框架中，并利用 RL 动态决策聚合跳数。
- **通用性强**：在同配与异配图上均大幅超越或持平 SOTA，展现出良好的泛化能力。
- **设计灵活**：Aggregator 可集成到简单的 GCN 架构中，验证方法对基本 GNN 的提升效果，解耦清晰。
- **实验扎实**：13 个数据集、12 个强基线，细致的超参数分析与可视化，提供了充分的证据。

## 8. 不足与局限
- **计算开销未被量化**：虽引入了参数共享与缓冲，但 RL 迭代训练与频繁的 GNN 前传在大规模图上的耗时、显存占用未详细报告，文中没有给出 GPU 资源或训练时间。
- **下一状态采样随机性**：MDP 中下一节点随机采样邻居，未考虑高回报路径，可能限制策略最优性。
- **缺乏超大规模图实验**：实验集中在中等规模图（如 Cora、Actor 等），未在百万级节点图或工业级场景下验证。
- **超参数敏感性**：α 等超参数需根据数据集调优，且 RL 奖励尺度 φ 等也可能影响收敛，但文中未全面分析。
- **理论分析较浅**：对隐式信息的定义和学习性质缺乏严格的理论保证。
- 总而言之，GRAIN 在中小型异配图上表现出色，但扩展到更大网络时的资源要求和最优性保障仍需进一步研究。

（完）
