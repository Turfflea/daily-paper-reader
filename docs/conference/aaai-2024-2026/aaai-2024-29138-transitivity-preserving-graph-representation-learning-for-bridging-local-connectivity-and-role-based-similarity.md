---
title: Transitivity-Preserving Graph Representation Learning for Bridging Local Connectivity and Role-Based Similarity
title_zh: 保持传递性的图表示学习：连接局部连通性与角色相似性
authors: "Van Thuy Hoang, O-Joun Lee"
date: 2024-03-25
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/29138/30153"
tags: ["query:mt"]
score: 9.0
evidence: 统一图Transformer网络融合局部与全局结构进行关系数据建模
tldr: 现有图表示学习方法多只关注局部连接而忽视长程连接和节点角色。为此提出统一图Transformer网络UGT，先通过k跳邻域聚合局部结构，再构建基于角色相似性的虚拟边并应用Transformer捕获全局信息，同时保持传递性，从而得到富含局部和全局结构的节点表示，在节点分类和链接预测任务上取得领先性能，为关系数据建模提供了更全面的图表征方案。
source: AAAI-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29138/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 570, \"height\": 327, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29138/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 839, \"height\": 391, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29138/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1716, \"height\": 766, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29138/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 840, \"height\": 769, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29138/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 862, \"height\": 518, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29138/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 811, \"height\": 765, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29138/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1806, \"height\": 721, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29138/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 833, \"height\": 741, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29138/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1820, \"height\": 740, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29138/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 845, \"height\": 706, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29138/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1371, \"height\": 556, \"label\": \"Table\"}]"
motivation: 现有图学习方法忽略长程连接和节点角色，导致表示能力不足。
method: 设计统一图Transformer，融合局部子结构聚合与全局虚拟边及Transformer编码。
result: 在多个基准图上，UGT在节点分类和链接预测任务上均达最优，消融实验验证各模块贡献。
conclusion: 整合局部与全局结构并保持传递性的图Transformer显著提升了图表征的质量。
---

## Abstract
Graph representation learning (GRL) methods, such as graph neural networks and graph transformer models, have been successfully used to analyze graph-structured data, mainly focusing on node classification and link prediction tasks. However, the existing studies mostly only consider local connectivity while ignoring long-range connectivity and the roles of nodes. In this paper, we propose Unified Graph Transformer Networks (UGT) that effectively integrate local and global structural information into fixed-length vector representations. First, UGT learns local structure by identifying the local sub-structures and aggregating features of the k-hop neighborhoods of each node. Second, we construct virtual edges, bridging distant nodes with structural similarity to capture the long-range dependencies. Third, UGT learns unified representations through self-attention, encoding structural distance and p-step transition probability between node pairs. Furthermore, we propose a self-supervised learning task that effectively learns transition probability to fuse local and global structural features, which could then be transferred to other downstream tasks. Experimental results on real-world benchmark datasets over various downstream tasks showed that UGT significantly outperformed baselines that consist of state-of-the-art models. In addition, UGT reaches the third-order Weisfeiler-Lehman power to distinguish non-isomorphic graph pairs.

---

## 论文详细总结（自动生成）

# 论文总结：保持传递性的图表示学习——连接局部连通性与角色相似性

## 1. 研究动机与整体含义
- 现有图表示学习方法（图神经网络、图Transformer）主要关注**局部k‑跳连通性**，普遍忽视**长程依赖**与**节点的角色相似性**。
- 由此产生三个核心局限：
  - **局部结构相似性未被捕捉**：常用的位置编码（如拉普拉斯特征向量）可为子结构赋予可区分的表示，但无法反映子结构之间的相似性，甚至给结构相似节点带来误导信号。
  - **全局结构信息缺失**：基于邻域聚合的GNN和图Transformer缺乏捕获全局角色相似性（如社交网络中的关键影响节点）的能力。
  - **局部与全局特征难以融合**：角色相似性不一定与k‑跳距离一致，两种视角之间存在概念鸿沟，难以统一为定长向量。
- 本工作提出**统一图Transformer网络（UGT）**，旨在克服上述局限，将局部连通性和基于角色的全局相似性融合到统一表示中，并保持节点间的**传递性**（transition probability），从而提升各类下游任务性能。

## 2. 方法论

### 2.1 整体框架
UGT以Transformer为基础，通过**上下文节点采样**、**结构距离与转移概率编码**以及**自监督预训练**来融合局部与全局结构信息。

### 2.2 上下文节点采样与虚拟边
对每个目标节点 \(v_i\)，上下文节点集 \(\mathcal{N}_i\) 由两部分组成：
- **k‑跳邻域** \(\mathcal{N}^{(k)}_i\)：提供局部结构信息。
- **结构相似节点** \(\mathcal{N}^{(st)}_i\)：若节点 \(v_j\) 与 \(v_i\) 在有序度序列（1至k跳）上高度相似，则构建**虚拟边**，捕获长程依赖。  
相似度通过动态时间规整（DTW）度量，并引入对度较高节点的增强项：
\[
S(v_i, v_j) = \exp(-\sqrt{s_k}) + \exp\!\left(-\frac{1}{\sqrt{d_{v_i} + d_{v_j}}}\right)
\]
其中 \(s_k\) 为度序列的DTW距离，\(d_{v_i}\) 为节点度。

### 2.3 输入表示与位置编码
- 节点原始特征经线性映射得到初始隐藏表示 \(\hat{x}^0_i\)。
- 使用图拉普拉斯矩阵的**特征向量**作为位置编码 \(\lambda_i\)，经线性变换后加入节点特征，并在训练中随机翻转符号以学习符号不变性。

### 2.4 全局结构自注意力
自注意力分数中额外注入两项可学习偏差：
- **结构距离偏差** \(D^{k,l}_{ij}\)：由节点结构身份 \(I_i\)（包含度及k跳邻域的度统计量，如最小值、最大值、均值、标准差）计算结构距离 \(d_{ij}\)，线性编码后加入注意力。
- **转移概率偏差** \(M^{k,l}_{ij}\)：利用多步（p步）邻接矩阵的转移概率 \(A^p_{ij}\) 编码节点间的连接性，线性变换后注入。  
注意力公式：
\[
\alpha^{k,l}_{ij} = \frac{(Q^{k,l}h^l_i) \cdot (K^{k,l}h^l_j)}{\sqrt{d_k}} + D^{k,l}_{ij} + M^{k,l}_{ij}
\]

### 2.5 图Transformer层与结构身份增强
- 多头注意力输出经拼接和线性变换后，与残差连接、层归一化、前馈网络堆叠。
- 为进一步提升表达能力，每层**额外加入结构身份** \(I^l_i\)，相当于在注意力聚合结果上叠加角色特征，以增强对非同构子结构的分辨力。
\[
h^{l+1}_i = f(I^l_i) + O^l_h \Bigg\|_{k=1}^{H} \Bigg(\sum\nolimits_{v_j} \tilde{\alpha}^{k,l}_{ij} V^{k,l} h^l_j\Bigg)
\]

### 2.6 自监督预训练任务
预训练阶段使用两个损失：
- **p步转移概率保持损失** \(\mathcal{L}^p_1\)：基于噪声对比估计（NCE）使得相关节点的嵌入内积恢复对数尺度转移概率矩阵 \(A'^{(p)}\)，保留多尺度连接信息。
- **节点特征重建损失** \(\mathcal{L}_2\)：重建原始特征 \(\|x_i - \hat{x}_i\|^2\)。  
总损失：\(\mathcal{L} = \alpha\sum_p \mathcal{L}^p_1 + \beta\mathcal{L}_2\)。
- 预训练所得表示可直接用于下游任务的微调（节点聚类、节点分类、图分类）。

### 2.7 复杂度
- 结构特征（I, d）预计算一次。度序列匹配用快速DTW（线性于序列长度）。最坏情况邻接矩阵和距离矩阵为 \(O(N^2)\)。  
- Transformer层全注意力计算也为 \(O(N^2)\)，与其他图Transformer一致。

## 3. 实验设计

### 3.1 数据集与任务
- **节点级任务**（11个数据集）：
  - 航空网络：Brazil、Europe、USA
  - 网页网络：Chameleon、Squirrel、Actor、Cornell、Texas、Wisconsin
  - 引文网络：Cora、Citeseer
- **图级分类**（5个数据集）：
  - TUDataset：Enzymes、Proteins、NCI1、NCI109
  - OGBG‑MolHIV（大规模）
- **同构测试**：Graph8c（1‑WL图对）及5个强正则图数据集（3‑WL等价对）

### 3.2 Benchmark 与对比方法
- **GNN变体**：GCN、GCNII、GIN、GAT、GATv2、GraphSAGE、WRGAT、WRGCN、DeeperGCN、Geom‑GCN
- **图Transformer**：GT、SAN、SAT、GPS、ANS‑GT、Graphormer（GP）、GD‑WL
- **同构测试**额外对比ChebNet、PPGN、GNNML3等强表达力模型。

### 3.3 评估指标
- 节点聚类：conductance (C↓) 与 modularity (Q↑)
- 节点分类：准确率 (ACC)
- 图分类：ACC 与 AUC (MolHIV)
- 同构测试：未能区分的图对数

### 3.4 实验设置与消融
- 每个实验随机划分80%/10%/10%，重复10次，报告测试集均值±标准差。
- 超参数（层数、隐维）在验证集上调优。
- 消融分析：UGT(PT) 即无预训练版本；UGT(K) 将预训练嵌入直接用于K‑means；UGT w/o λ（无拉普拉斯位置编码）、UGT w/o I（无结构身份）。
- 敏感性分析：采样k跳范围、虚拟边与实际边比例的影响。

## 4. 资源与算力
- 实验使用**两台服务器**，每台配备**4块 NVIDIA RTX A5000 GPU（24 GB显存）**。
- 论文未明确报告单次训练时长及总训练时间。

## 5. 实验数量与充分性
- **主要实验组数**：
  - 节点聚类：11个数据集 × 十几种方法 × 10次重复 ≈ 1500+ 组结果
  - 节点分类：同样11个数据集，多方法，报告标准差
  - 图分类：5个数据集多方法
  - 同构测试：Graph8c + 4个SRG数据集，多种模型
  - 消融：多版本UGT
  - 敏感性：k‑hop范围与虚拟边比例
- 实验覆盖广泛，超参搜索确保公平，重复实验提供统计显著性，整体实验**充分且客观**。

## 6. 主要结论与发现
- UGT在节点聚类、节点分类、图分类任务上**全面优于或可比**当前最优基线模型。
- 即使直接使用预训练嵌入进行K‑means聚类，UGT也表现出显著优势，说明其学习到的表示能够反映图的稠密关系。
- UGT能够完美区分**Graph8c和SRGs中的所有非同构图对**，表达力达到3‑WL级，超越多数GNN和图Transformer。
- 结构身份和拉普拉斯位置编码共同作用，赋予UGT识别同构/非同构子结构的能力，而常见随机游走位置编码在稠密图中可能失效。
- 虚拟边的引入对同质和异质图均有帮助，采样k‑hop范围对异质图收益更大。

## 7. 优点与亮点
- **统一局部与全局结构**：通过同时覆盖k‑跳邻域和角色相似虚拟边，捕获互补的图结构信息。
- **传递性保持**：利用多步转移概率将局部连接和全局关联融合进统一向量空间。
- **结构身份机制**：不仅区分非同构子结构，还为自注意力提供层次化角色信息，表达能力超越1‑WL。
- **自监督预训练**：使学习到的表示可在无标签时有效编码图结构，并可直接迁移至不同下游任务。
- **实验设计与可复现性**：大量基准数据集对比、消融、敏感性分析，并开源代码。

## 8. 不足与局限
- **计算复杂度**：虚拟边构造和全图结构距离计算在最坏情况下为 \(O(N^2)\)，对超大规模图不友好。作者计划通过图粗化和社区内邻近度来缓解。
- **仅考虑结构相似性**：采样虚拟边时未融合节点特征，可能在属性信息重要的图上遗漏某些长程关联。
- **缺乏与更近期方法的比较**：对比基线截至2023年，可能有更新的架构未被包含。
- **训练时长未报告**：读者无法评估实际训练成本。
- **大规模图测试有限**：仅MolHIV一个较大规模数据集，在其他大型图（如OGBN‑Products）上的有效性待验证。

（完）
