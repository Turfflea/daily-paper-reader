---
title: Graph Neural Networks with Soft Association between Topology and Attribute
title_zh: 拓扑与属性软关联的图神经网络
authors: "Yachao Yang, Yanfeng Sun, Shaofan Wang, Jipeng Guo, Junbin Gao, Fujiao Ju, Baocai  Yin"
date: 2024-03-25
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/28778/29491"
tags: ["query:mt"]
score: 8.0
evidence: 提出拓扑与属性软关联的图神经网络，提升节点表示学习
tldr: 针对图神经网络中拓扑与属性干扰导致节点表示失真的问题，本文提出GNN-SATA模型。该模型使用不同嵌入分别捕获属性和结构信息，并通过软关联机制建立两者的联系，从而有效处理异配图。实验表明，GNN-SATA在节点分类任务上显著优于现有方法，为图结构数据建模提供了新思路，其方法可迁移至管理科学中的关系预测场景。
source: AAAI-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-28778/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1805, \"height\": 542, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-28778/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 827, \"height\": 369, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-28778/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1714, \"height\": 657, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-28778/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 840, \"height\": 283, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-28778/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 851, \"height\": 286, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-28778/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1627, \"height\": 749, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-28778/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1564, \"height\": 314, \"label\": \"Table\"}]"
motivation: 现有GNN在异配图上因拓扑与属性干扰导致节点表示失真，本文旨在解决此问题。
method: 提出GNN-SATA，利用不同嵌入分别捕获属性和结构信息，并通过软关联建立两者间联系。
result: 在异配图节点分类等任务上取得优于主流GNN模型的性能。
conclusion: GNN-SATA有效缓解拓扑与属性干扰，提升图表示学习质量，适用于复杂关系建模。
---

## Abstract
Graph Neural Networks (GNNs) have shown great performance in learning representations for graph-structured data. However, recent studies have found that the interference between topology and attribute can lead to distorted node representations. Most GNNs are designed based on homophily assumptions, thus they cannot be applied to graphs with heterophily. This research critically analyzes the propagation principles of various GNNs and the corresponding challenges from an optimization perspective. A novel GNN called Graph Neural Networks with Soft Association between Topology and Attribute (GNN-SATA) is proposed. Different embeddings are utilized to gain insights into attributes and structures while establishing their interconnections through soft association. Further as integral components of the soft association, a Graph Pruning Module (GPM) and Graph Augmentation Module (GAM) are developed. These modules dynamically remove or add edges to the adjacency relationships to make the model better fit with graphs with homophily or heterophily. Experimental results on homophilic and heterophilic graph datasets convincingly demonstrate that the proposed GNN-SATA effectively captures more accurate adjacency relationships and outperforms state-of-the-art approaches. Especially on the heterophilic graph dataset Squirrel, GNN-SATA achieves a 2.81% improvement in accuracy, utilizing merely 27.19% of the original number of adjacency relationships. Our code is released at https://github.com/wwwfadecom/GNN-SATA.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义

- **研究动机**：图神经网络（GNN）在图结构数据表示学习中表现出色，但现有研究发现**拓扑结构与节点属性之间的干扰**会导致节点表示失真。
- **核心问题**：大多数 GNN 基于**同配性假设**（相邻节点具有相同标签），难以适用于**异配图**（相连节点可能类别不同）。拓扑平滑性约束与属性拟合之间存在冲突，要么导致过平滑、丢失属性信息，要么因不准确的拓扑关系而产生错误表示。
- **整体含义**：论文旨在提出一种新型 GNN 模型，在**拓扑和属性之间建立“软关联”**，减轻相互干扰，使模型既能处理同配图，也能处理异配图，并自适应地调整图结构以提高节点分类性能。

### 2. 方法论

- **核心思想**：分别学习**属性表示** \( U \) 和**拓扑表示** \( Z \)，并通过一个软关联项将二者联系起来，避免直接相互干扰。
- **优化目标**（简化）：
  \[
  \min_{Z,U,E_P,E_A} \|U - X\|_F^2 + \alpha_1\, \text{tr}(Z^\top \tilde{L}_A Z) + \alpha_2 \big\| UU^\top - (A \odot E_P + (\mathbf{1}-A) \odot E_A) \big\|_F^2
  \]
  - 第一项：属性表示 \( U \) 接近原始特征 \( X \)。
  - 第二项：拓扑表示 \( Z \) 满足图平滑性约束（基于自适应拉普拉斯矩阵 \( \tilde{L}_A \)）。
  - 第三项：利用 \( UU^\top \) 构建特征相似矩阵，并使其逼近一个**可学习的自适应邻接矩阵** \( \bar{A} = A \odot E_P + (\mathbf{1}-A) \odot E_A \)，实现软关联。
- **关键模块**：
  - **图剪枝模块 (GPM)**：通过可学习矩阵 \( E_P \) 对原始邻接矩阵 \( A \) 进行哈达玛积，动态移除不可靠的边。
  - **图增强模块 (GAM)**：通过可学习矩阵 \( E_A \) 对互补邻接矩阵 \( \mathbf{1}-A \) 作用，动态添加可能缺失的边。
- **迭代更新公式**（交替优化）：
  - \( U^{k+1} = X - \alpha_2\,\sigma\big[(2U^k U^{k\top} - \bar{A})U^k W_U\big] \)
  - \( Z^{k+1} = \bar{A} Z^k \)
  - \( E_P^{k+1} = (2\alpha_2 A \odot \hat{U}_P^k - V_P^k) / (2\alpha_2 A \odot A) \)
  - \( E_A^{k+1} = (2\alpha_2 A' \odot \hat{U}_A^k - V_A^k) / (2\alpha_2 A' \odot A') \)
- **分类预测**：拼接最终的 \( U^K \) 和 \( Z^K \)，经过线性变换和 softmax 得到节点分类概率，损失函数为标准交叉熵。

### 3. 实验设计

- **数据集**：
  - 同配图：Cora, Citeseer, Photo, Computer。
  - 异配图：Squirrel, Chameleon。
- **训练/验证/测试划分**：每个类别 60% 训练，20% 验证，20% 测试。
- **评估指标**：准确率 (ACC)。
- **对比方法**（13 个基线）：
  - 经典 GCN：GCN, GAT, GraphSAGE。
  - 缓解过平滑：GCNII, APPNP, JKNet。
  - 处理异配图：Geom-GCN, GPRGNN, FAGCN, H2GCN, Ordered GNN, GNN-BC。
  - 基础 MLP 也作为基准。

### 4. 资源与算力

- 论文提及使用 **RTX 3090 Ti GPU** 进行实验，训练迭代 **200 个 epoch**。
- 未明确说明 GPU 数量（可能为单卡）、总训练时长或显存占用等细节。

### 5. 实验数量与充分性

- **主要实验**：在 6 个数据集上与 13 个基线方法对比，共产生约 6×13 = 78 组对比结果。
- **消融实验**：设计了四种变体（SATA‑S, SATA‑P, SATA‑A, SATA‑AP）以验证软关联、GPM、GAM 的贡献，5 种变体 × 6 数据集 = 30 组实验。
- **自适应邻接矩阵分析**：统计了学习到的邻接矩阵的同配率、边数量比例等，6 个数据集。
- **参数敏感性分析**：对超参数 \( \alpha_1 \) (5 个值) 和 \( \alpha_2 \) (4 个值) 组合搜索，展示了在两个数据集上的热力图。
- **可视化**：使用 t‑SNE 展示了 6 个数据集上原始特征与 GNN‑SATA 表示的分布。
- 实验设计比较全面，涵盖主流benchmark，对比方法多样，消融和参数分析充分，能较客观地反映方法性能。

### 6. 主要结论与发现

- GNN‑SATA 在所有六个数据集上均取得**最优或次优**性能，平均优于最佳基线 1.82%，尤其在异配数据集 Squirrel 上提升 2.81%，且仅使用原始边数量的 27.19%。
- 软关联、GPM 和 GAM 均对性能有正向贡献，且 GPM 的作用更大。
- 模型能够**显著提升异配图的同配率**（\( H(\bar{A}) / H(A) > 1 \)），对于低同配率的数据改善尤为明显。
- 超参数 \( \alpha_2 \) 需要谨慎调节，过大则退化为纯拓扑平滑，导致性能下降。

### 7. 优点

- **思路新颖**：从优化角度解释 GNN 中的拓扑‑属性干扰，并提出“软关联”概念，构建可学习的自适应邻接矩阵。
- **结构设计巧妙**：将图结构学习分解为剪枝和增强两个模块，既能删除噪声边，也能补充缺失边，增强对异配图的适应能力。
- **方法简洁有效**：无需复杂的高阶邻居编码或多模态集成，直接拼接属性和拓扑表示即可获得优异性能。
- **实验充分且可复现**：代码公开，对比方法覆盖全面，消融和超参数分析严谨，验证了各模块的有效性。

### 8. 不足与局限

- **计算复杂度**：交替优化和两个可学习边矩阵可能增加训练负担，论文未分析时间复杂度或在大规模图上的可扩展性。
- **超参数敏感**：\( \alpha_2 \) 对性能影响较大，需要针对不同数据集仔细调参，实际应用中可能存在额外开销。
- **应用场景局限**：仅在节点分类任务上验证，缺少在边预测、图分类等其他图学习任务上的测试。
- **理论分析不足**：对软关联约束如何保证收敛性、解的唯一性等缺乏深入理论阐述。
- **对比方法覆盖**：未纳入某些最新异配图方法（如 ACM 等），可能遗漏更强的基线。

（完）
