---
title: "HINPool: A Unified Heterogeneous Graph Pooling Framework for Accurate Molecular and Protein Property Prediction"
title_zh: HINPool：面向精确分子与蛋白质性质预测的统一异构图池化框架
authors: "Ming-Yi Hong, You-Chen Teng, Shao-En Lin, Chih-Yu Wang, Che Lin"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39326/43287"
tags: ["query:mt"]
score: 9.0
evidence: 异构图神经网络用于分子性质预测
tldr: 针对分子与蛋白质性质预测中传统图池化方法忽略异质性的问题，提出HINPool框架，将分子图建模为异质信息网络，利用类型感知选择器进行图池化，以更精确地捕获复杂交互。实验表明，HINPool在多个分子和蛋白质基准数据集上实现了最先进的预测性能，为生物分子性质预测提供了统一高效的图池化方案，其异质图建模思想可迁移至管理科学中的异质关系网络分析。HINPool通过类型感知选择器保留关键节点，有效聚合异质信息，在多个分子性质预测任务上刷新了性能纪录。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39326/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1735, \"height\": 737, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39326/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1447, \"height\": 804, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39326/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 829, \"height\": 573, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39326/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 747, \"height\": 267, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39326/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1730, \"height\": 391, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39326/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 581, \"height\": 429, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39326/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1801, \"height\": 308, \"label\": \"Table\"}]"
motivation: 分子性质预测中图池化通常忽视节点和边类型的异质性，限制了信息聚合能力，影响预测精度。
method: 提出HINPool，将分子图构建为异质信息网络，设计类型感知选择器进行异质图池化。
result: HINPool在多个分子与蛋白质数据集上达到最先进预测性能，超越同质图池化方法。
conclusion: HINPool统一了异质图池化框架，适用于生物分子性质预测，其异质建模思路可推广至社交网络、供应链等管理领域。
---

## Abstract
Graph pooling has gained significant progress in recent years as an effective solution for graph-level property classification tasks. With the emergence of research on Heterogeneous Information Networks (HINs), this paper argues that graph-level datasets for graph classification should be treated as HINs rather than homogeneous graphs to enhance information aggregation. We propose HINPool, a novel and general graph pooling framework for graph-level property classification with HINs. First, we devise a systematic HIN construction procedure from the original data to capture complex interactions. Next, we introduce a type-aware heterogeneous graph pooling method featuring a Type-Aware Selector (TAS) to select essential nodes and a Readout Aggregator (RA) to fuse critical information into a graph-level representation. Finally, a cross-layer fusion function is applied to combine the output embeddings from each graph pooling layer, creating a final graph representation for downstream classification tasks. 
Our approach achieves near state-of-the-art performance on widely used graph classification benchmark datasets, demonstrating significant improvements in four out of five datasets. This work redefines the strategy for graph-level property classification with HGNNs and heterogeneous graph pooling to model intricate relationships, enhancing performance without requiring extensive domain-specific knowledge.

---

## 论文详细总结（自动生成）

## 论文核心问题与整体含义（研究动机和背景）

- 图池化（graph pooling）是图级性质分类的关键技术，但现有方法几乎都基于 **同质图（homogeneous graph）** 假设，忽视节点与边的类型差异。
- 分子和蛋白质结构数据天然包含多种**原子类型、化学键类型或氨基酸二级结构（SSE）**等异质信息，若将其视为同质图会导致信息损失，降低分类精度。
- 异质信息网络（HIN）能更好地建模这类复杂关系，但**异构图池化在图级分类中的应用几乎空白**，现有异构图神经网络（HGNN）多聚焦于节点分类和链接预测。
- 本文提出 **HINPool**，一个统一的异构图池化框架，旨在将分子与蛋白质图作为 HIN 处理，利用**类型感知的池化策略**更有效地聚合异质信息，以提升图级性质预测性能。

## 论文提出的方法论

### 总体思路
- 将原始同质图数据**系统性地构建为 HIN**（例如原子类型作为节点类型、键类型作为边类型，或蛋白质二级结构作为节点类型）。
- 设计 **类型感知异构图池化（Type-aware Heterogeneous Graph Pooling, THeGP）** 层，其中包含 **HGNN 编码器**、**类型感知选择器（Type-Aware Selector, TAS）** 和 **读出聚合器（Readout Aggregator, RA）**。
- 叠加多个 THeGP 层后，通过**跨层融合** 整合各层输出的图层级表示，得到最终图嵌入用于分类。

### 关键技术细节与流程

1. **HIN 构建**
   - 根据节点属性（如原子类型、氨基酸二级结构）赋予节点类型，边类型由连接关系确定。
   - 节点特征保留原有理化信息，边特征可为空或可学习的类型嵌入。

2. **THeGP 层**
   - **HGNN 编码器**（以 SimpleHGN 为例）：
     - 输入上一层的节点表示，产生当前层节点嵌入 \(\mathbf{Z}_t^n\)。
     - 利用可学习的边类型嵌入增强消息传递。
   - **类型感知选择器（TAS）**（图 2）：
     - **得分投影**：对每类节点 \(t\) 的嵌入 \(\mathbf{Z}_t^n\) 用**类型专属的 MLP** 计算得分 \(\mathbf{S}_t^n\)。
     - **Top‑K 选择**：在每个节点类型内分别选出得分最高的 \(K\) 个节点（\(K\) 由池化比率控制），得到保留节点嵌入 \(\mathbf{Z}_t^{n'}\) 和对应得分 \(\mathbf{S}_t^{n'}\)。
     - **嵌入注意力**：将保留节点的嵌入乘以自身得分，输出加权的节点表示 \(\mathbf{H}_t^n = \mathbf{Z}_t^{n'} \cdot \mathbf{S}_t^{n'}\)，以强化高分节点的作用。
   - **读出聚合器（RA）**（图 3）：
     - **类型感知读出融合**：对每种节点类型的保留嵌入先取均值，再通过融合函数 \(\phi_{TR}\)（如拼接、平均、最大化或注意力机制）合并，得到 \(\mathbf{h}_{Fused}^n\)。
     - **混合类型读出**：对所有节点（不分类型）取均值，得到全局视图 \(\mathbf{h}_{Mixed}^n\)。
     - **图层级表示**：拼接上述两部分得到当前层图嵌入 \(\mathbf{h}_{Layer}^n = \mathbf{h}_{Fused}^n \| \mathbf{h}_{Mixed}^n\)。

3. **跨层融合**
   - 将所有 THeGP 层的 \(\mathbf{h}_{Layer}^n\) 通过跨层融合函数 \(\phi_{CL}\)（如注意力机制）整合为最终图嵌入 \(\mathbf{h}_G\)。
   - 将 \(\mathbf{h}_G\) 送入线性分类器，使用交叉熵损失训练。

## 实验设计

### 数据集
- **分子数据集**：MUTAG（致突变性）、NCI1 和 NCI109（抗癌活性）。
- **蛋白质数据集**：PROTEINS（酶与非酶分类）、ENZYMES（酶类别多分类，6 类）。
- 数据集规模：图数量 188～4127，平均节点数 18～39，平均边数 20～73。

### 评估协议
- **指标**：主要采用 AUROC（阈值无关），辅助用准确率（Acc）。对多分类 ENZYMES 使用一对多策略计算 AUROC。
- **数据划分**：固定划分为训练/验证/测试 = 80%/10%/10%，重复 5 次不同随机划分，报告测试集平均结果及标准差。训练采用验证集 AUROC 早停（100 轮）。

### 对比方法
- 同质图池化基线：DiffPool、SAGPool、HGP‑SL、MVPool、Graph U‑Nets、GIUNet。
- 所有基线按原文或官方代码默认设置复现。

## 资源与算力

- 文中**未明确说明**使用的 GPU 型号、数量或训练总时长。
- 仅提供复杂度分析（总复杂度 \(O(n(\log n + e))\)）和 PROTEINS 测试集的推理时间（0.31 秒/图），但未给出训练阶段的硬件细节。

## 实验数量与充分性

- **主实验**：在 5 个数据集上对比 7 种方法（含 HINPool），每个数据集 5 次独立运行，共约 **5×7×5 = 175 次**完整训练。
- **消融实验**：在 MUTAG、PROTEINS、ENZYMES 三个数据集上，分别移除跨层融合、整个 TAS、类型感知 Top‑K（改为混合 Top‑K）、类型感知读出等 4 个关键模块，分析各模块贡献。
- **超参数分析**：在 PROTEINS 上测试不同池化比率（0.6～0.9）及不同层数（2～4）对性能的影响。
- **融合函数比较**：对比了拼接、平均、最大化、共享注意力和分离注意力等多种读出融合函数。
- 实验从数据集覆盖、对比方法数量、消融维度和统计显著性检验（p 值）看，设计较为充分，具备一定的客观性和公平性。

## 论文的主要结论与发现

- HINPool 在 **4/5 个数据集**上取得最优或次优性能，尤其在 ENZYMES 上 AUROC **显著超越所有基线**（提升约 7%）。
- 将分子和蛋白质数据建模为 HIN、并使用类型感知的池化机制，能有效捕捉结构和语义的异质性，提升分类精度与稳定性。
- **类型感知读出** 和 **跨层融合** 是性能的关键贡献者，移除任一模块均会导致大幅下降（如 MUTAG 上移除跨层融合致 AUROC 下降 39%）。
- 池化比率不宜过小（如 0.6 已导致性能下降），合适的层数（3 层）能平衡表示能力与过平滑问题。

## 优点

- **统一且通用的框架**：首次提出面向 HIN 的通用图池化方法，可灵活替换 HGNN 编码器和融合函数，扩展性强。
- **类型感知设计**：通过类型专属的得分投影和 Top‑K 选择，保留不同节点类型的结构重要性，避免类型混同导致的信息损失。
- **多层级信息整合**：跨层融合机制汇总不同池化阶段的语义，形成更全面的图表示。
- **无需深度领域知识**：HIN 构建仅依赖节点和边的固有属性（如原子类型、二级结构），不需要人工设计元路径或功能基团。
- **实验严谨**：采用固定划分和多次重复，报告 AUROC 及统计显著性，消融实验全面验证各组件有效性。

## 不足与局限

- **部分数据集提升有限**：NCI109 上性能略低于 GIUNet，对某些分子图而言，类型异质性的建模增益不明显。
- **节点选择策略仍有改进空间**：消融实验显示，类型感知 Top‑K 相比混合 Top‑K 在某些数据集上降幅较小，说明 TAS 的区分度可进一步增强。
- **应用域局限**：仅在分子和蛋白质图上验证，尚未在社交网络、引文网络等其他典型 HIN 分类任务中测试。
- **算力与资源未明**：缺少 GPU 配置、训练耗时等报告，难以定量评估实际计算开销。
- **未探索动态或边特征丰富的 HIN**：HIN 构建假定节点类型由属性天然决定，若面对更模糊的类型或大规模异构图时，方法有效性未知。

（完）
