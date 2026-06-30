---
title: Learning to Reweight for Generalizable Graph Neural Network
title_zh: 面向泛化图神经网络的学习重加权方法
authors: "Zhengyu Chen, Teng  Xiao, Kun Kuang, Zheqi Lv, Min Zhang, Jinluan Yang, Chengqiang Lu, Hongxia Yang, Fei Wu"
date: 2024-03-25
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/28673/29307"
tags: ["query:mt"]
score: 9.0
evidence: 通过样本重加权提升图神经网络在分布外场景的泛化能力
tldr: 针对图神经网络在分布偏移下泛化退化的问题，提出L2R-GNN框架，通过学习样本重加权消除虚假相关性，在OOD场景中显著提升GNN的预测性能，为可靠图学习提供了新途径。
source: AAAI-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-28673/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1268, \"height\": 753, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-28673/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 842, \"height\": 280, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-28673/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 853, \"height\": 318, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-28673/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1466, \"height\": 247, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-28673/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 848, \"height\": 428, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-28673/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 840, \"height\": 368, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-28673/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 837, \"height\": 481, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-28673/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1443, \"height\": 562, \"label\": \"Table\"}]"
motivation: 现有GNN在训练与测试图数据分布不一致时泛化能力严重下降。
method: 提出L2R-GNN，通过学习为训练样本分配权重来抑制虚假统计相关性。
result: 在多种OOD图任务上，L2R-GNN相比于基线方法取得了显著的性能提升。
conclusion: 该方法有效增强了GNN的泛化鲁棒性，可用于更广泛的现实场景。
---

## Abstract
Graph Neural Networks (GNNs) show promising results for graph tasks. However, existing GNNs' generalization ability will degrade when there exist distribution shifts between testing and training graph data.
The fundamental reason for the severe degeneration is that most GNNs are designed based on the I.I.D hypothesis. In such a setting, GNNs tend to exploit subtle statistical correlations existing in the training set for predictions, even though it is a spurious correlation.
In this paper, we study the problem of the generalization ability of GNNs on Out-Of-Distribution (OOD) settings.
To solve this problem, we propose the Learning to Reweight for Generalizable Graph Neural Network (L2R-GNN) to enhance the generalization ability for achieving satisfactory performance on unseen testing graphs that have different distributions with training graphs.
We propose a novel nonlinear graph decorrelation method, which can substantially improve the out-of-distribution generalization ability and compares favorably to previous methods in restraining the over-reduced sample size.
The variables of graph representation are clustered based on the stability of their correlations, and graph decorrelation method learns weights to remove correlations between the variables of different clusters rather than any two variables.
Besides, we introduce an effective stochastic algorithm based on bi-level optimization for the L2R-GNN framework, which enables simultaneously learning the optimal weights and GNN parameters, and avoids the over-fitting issue.
Experiments show that L2R-GNN greatly outperforms baselines on various graph prediction benchmarks under distribution shifts.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **研究动机**：图神经网络（GNN）在训练与测试图数据服从独立同分布（I.I.D）假设时表现出色，然而真实场景中经常存在分布偏移（Out‑Of‑Distribution, OOD），训练集中的虚假统计相关性（spurious correlation）会被 GNN 利用，导致在未知测试数据上性能大幅退化。
- **核心目标**：面向图级任务（图分类、图属性预测）的 OOD 泛化问题，提出**学习重加权（Learning to Reweight）** 方法，通过样本权重调整消除图表示中的虚假相关性，使模型专注真实因果特征，从而提升在分布偏移下的泛化能力。

### 2. 方法论
#### 2.1 核心思想
- **变量聚类与跨簇去相关**：认为不是所有图表示变量间的相关性都需要消除。依据变量间相关性的稳定性，将图表示变量聚类；同一簇内变量属于整体不变特征，只需消除**不同簇之间**的虚假统计依赖，从而避免过度缩减有效样本量。
- **双层优化（Bi‑level Optimization）**：将样本权重学习与 GNN 参数训练耦合为双层优化问题，外层在验证集上优化权重以解耦虚假相关性，内层在训练集上优化模型参数，有效缓解过拟合。

#### 2.2 关键技术细节
- **非线性图去相关方法**：
  - 使用随机傅里叶特征（RFF）和 Hilbert‑Schmidt 独立性准则（HSIC）的近似——偏交叉协方差矩阵的 Frobenius 范数来度量变量间依赖性。
  - 引入可学习的图权重 \(W=\{w_n\}_{n=1}^N\)，加权后的偏交叉协方差矩阵为：
    \[
    \hat{\Sigma}_{Z_{:,i},Z_{:,j}}^W = \frac{1}{N-1}\sum_{n=1}^N \left[ w_n u(Z_{ni}) - \frac{1}{N}\sum_{m=1}^N w_m u(Z_{mi}) \right]^\top \left[ w_n v(Z_{nj}) - \frac{1}{N}\sum_{m=1}^N w_m v(Z_{mj}) \right]
    \]
  - 目标为最小化不同簇变量间该范数的加权和：
    \[
    W^* = \arg\min_W \sum_{1\le i < j \le d} \mathbf{I}_{(i,j)} \|\hat{\Sigma}_{Z_{:,i},Z_{:,j}}^W\|_F^2
    \]
    其中 \(\mathbf{I}_{(i,j)}\) 指示变量是否属于不同簇（不同簇时为 1）。
- **变量聚类**：定义变量间的差异度 \(Dis(Z_{:,i},Z_{:,j})\) 为每个图中两变量 Pearson 相关系数与平均相关系数之差的均方根，通过最小化簇内差异度进行聚类。
- **动量图权重估计器**：为降低全量数据计算代价，维护图表示队列和权重队列，用动量系数更新队列，计算时只需使用 `(K+1)×batch_size` 的样本，将复杂度从 \(O(N)\) 降至 \(O(kB)\)。

#### 2.3 算法流程（文字说明）
- 外层循环固定权重 \(W\)，在训练集上更新 GNN 参数 \(\theta\)：
  \[
  \theta^{(i)} = \theta^{(i-1)} - \eta_\theta \nabla_\theta \mathcal{L}_{\text{train}}(\theta^{(i-1)}, W^{(i-1)})
  \]
- 内层循环利用更新后的 \(\theta^{(i)}\) 作为 \(\theta^*(W)\) 的近似，在验证集上更新权重 \(W\)：
  \[
  W^{(i)} = W^{(i-1)} - \eta_W \nabla_W \mathcal{L}_{\text{val}}(\theta^{(i)}, W^{(i-1)})
  \]
- 采用一阶近似简化梯度计算，避免高阶求导开销。

### 3. 实验设计
#### 3.1 数据集与场景
- **合成数据集**：构造带有“车轮” motif 的图分类任务，通过控制正样本中添加虚假“星形” motif 的比例（\(\mu \in \{0.6,0.7,0.8,0.9\}\)）模拟不同程度的虚假相关和分布偏移。
- **真实数据集——图大小泛化**：COLLAB、PROTEINS、D&D，按照节点数划分训练（小图）和测试（大图），考察模型在大小分布偏移下的泛化能力。
- **真实数据集——分子图属性预测（OGB）**：TOX21、BACE、BBBP、CLINTOX、HIV、ESOL，使用脚手架划分（scaffold split）造成结构分布偏移。

#### 3.2 Benchmark 与对比方法
- **Baseline 方法**：GCN、GIN、SGC、JKNet、FactorGCN、PNA、TopKPool、SAGPool 等经典 GNN，以及专门针对 OOD 泛化的 OOD‑GNN 和 StableGNN。
- **评估指标**：图分类准确率（COLLAB、PROTEINS、D&D），ROC‑AUC（OGB 分类数据集）和 RMSE（ESOL 回归）。

### 4. 资源与算力
- 论文**未明确说明**所使用的 GPU 型号、数量或训练时长等计算资源细节。

### 5. 实验数量与充分性
- **大规模对比实验**：在 6 个 OGB 数据集、3 个大小泛化数据集及 4 种偏移程度的合成数据集上，与超过 10 种基线方法进行全面比较。
- **稳健性分析**：
  - 不同相关度设置下的性能测试（合成数据）。
  - 消融实验：移除双层优化（L2R‑GNN w/o Bi）和移除图去相关模块（L2R‑GNN w/o GD），验证各组件的独立贡献。
  - 权重分布可视化：在无偏和有偏合成数据以及两个真实数据集上展示学习到的图权重分布。
  - 案例研究：利用 GNNExplainer 可视化 GCN 与 L2R‑GNN 关注的关键子图。
  - 过拟合分析：比较训练损失与验证损失，验证双层优化的过拟合缓解效果。
- 实验设计**客观、公平**，所有方法在同一数据划分和评价标准下对比，消融与可视化多角度支撑主要结论。

### 6. 主要结论与发现
- L2R‑GNN 在所有测试的 OOD 图分类和图属性预测任务上均显著优于现有基线方法，尤其在大型真实图（如 OGB 数据集）上优势明显。
- 所提出的非线性图去相关方法通过仅消除不同变量簇间的相关性，有效避免了样本量过度缩减的问题，同时提升了 OOD 泛化能力。
- 双层优化策略能够同步学习最优样本权重和 GNN 参数，成功缓解了传统重加权方法中的过拟合问题。
- 学习到的权重能够反映样本中虚假相关性的强弱，权重分布可视化表明 L2R‑GNN 可识别并抑制噪声样本。

### 7. 优点
- **新颖的“簇间去相关”策略**：突破了之前方法强制所有变量相互独立的激进做法，既保留内在稳定关联，又去除有害虚假相关，设计更符合图数据特点。
- **双层优化与动量估计的算法设计**：用双层优化自然整合权重学习和模型训练，动量队列估算极大降低了计算开销，使方法可规模化用于大型数据集。
- **实验全面且具有说服力**：涵盖合成、真实的多类分布偏移（虚假相关偏移、大小偏移、脚手架结构偏移），多角度消融和可视化验证了各模块的有效性。

### 8. 不足与局限
- **算力信息缺失**：论文未报告任何计算资源与训练时间，复现时的效率评估受限。
- **节点级任务的泛化能力未验证**：方法聚焦于图级任务，对于同样存在分布偏移的节点分类或链路预测任务未做探索。
- **聚类超参数未讨论**：变量聚类数目 \(K\) 的选择方式、对性能的敏感性未在文中解释，实际应用时需要额外调优。
- **极端分布偏移下的鲁棒性**：虽然合成实验涵盖了不同偏移比例，但真实世界中可能存在更复杂的非独立同分布情形，方法的下限和失败模式未深入分析。

（完）
