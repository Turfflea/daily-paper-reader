---
title: "TabGLM: Tabular Graph Language Model for Learning Transferable Representations Through Multi-Modal Consistency Minimization"
title_zh: TabGLM：通过多模态一致性最小化学习可迁移表示的表格图语言模型
authors: "Anay Majee, Maria Xenochristou, Wei-Peng Chen"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/34134/36289"
tags: ["query:mt"]
score: 8.0
evidence: 通过多模态一致性最小化在表格数据上进行自监督表示学习
tldr: 针对表格数据中特征异质性挑战，本文提出TabGLM，将表格转换为图和语言两种模态，通过多模态一致性最小化联合学习结构和语义信息，实现可迁移的表示。实验显示该方法优于传统基线，为深度表格学习提供了新的范式。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34134/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 858, \"height\": 491, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34134/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1820, \"height\": 591, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34134/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 793, \"height\": 941, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34134/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1511, \"height\": 1130, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34134/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 769, \"height\": 548, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34134/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 861, \"height\": 334, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34134/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 660, \"height\": 467, \"label\": \"Table\"}]"
motivation: 现有深度模型处理异质表格数据时表现不佳，单一模态转换无法有效捕获特征关系。
method: 提出多模态架构TabGLM，结合图与语言模型，通过一致性最小化进行自监督学习。
result: 在异质表格数据集上，TabGLM取得了优于线性模型和树模型的性能。
conclusion: 多模态一致学习能有效处理表格异质性，所得表示可迁移至下游任务，为表格深度学习提供新思路。
---

## Abstract
Handling heterogeneous data in tabular datasets poses a significant challenge for deep learning models. While attention-based architectures and self-supervised learning have achieved notable success, their application to tabular data remains less effective over linear and tree based models. Although several breakthroughs have been achieved by models which transform tables into uni-modal transformations like image, language and graph, these models often underperform in the presence of feature heterogeneity. To address this gap, we introduce TabGLM (Tabular Graph Language Model), a novel multi-modal architecture designed to model both structural and semantic information from a table. TabGLM transforms each row of a table into a fully connected graph and serialized text, which are then encoded using a graph neural network (GNN) and a text encoder, respectively. By aligning these representations through a joint, multi-modal, self-supervised learning objective, TabGLM leverages complementary information from both modalities, thereby enhancing feature learning. TabGLM's flexible graph-text pipeline efficiently processes heterogeneous datasets with significantly fewer parameters over existing Deep Learning approaches. Evaluations across 25 benchmark datasets demonstrate substantial performance gains, with TabGLM achieving an average AUC-ROC improvement of up to 5.56% over State-of-the-Art (SoTA) tabular learning methods.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体意义
- **核心问题**：真实表格数据常混合数值、类别、文本等多种特征类型（异质性）。现有深度学习方法（如注意力机制、自监督预训练）在这种异质表格上常不如传统的线性模型和树模型；而将表格转化为单一模态（图像、文本或图）的方法，往往只能捕捉结构或语义中的一类信息，导致性能下降。
- **整体含义**：论文旨在通过同时建模表格的**结构关系**（特征列之间的关联）和**语义信息**（单元格值及列名的自然语言含义），提升深度学习在异质表格数据上的表示能力与下游分类性能，缩小与树模型的差距并取得新突破。

### 2. 方法论：TabGLM 架构与训练策略
- **核心思想**：对表格的每一行，同时构造**全连接图**和**序列化文本**两种模态，分别送入图神经网络（GNN）和预训练文本编码器，再通过多模态一致性学习目标对齐两种表示，最后用于下游分类。
- **关键技术细节**：
  - **图管道**：将一行转为图 \(G_i(v,e)\)，节点为各列的值（类别列先编码为数值），边为列间关系。使用消息传递 GNN 学习节点特征，经读出层和图投影层得到图嵌入 \(\mathbf{g}^{graph}_i\)。图编码器从头训练。
  - **文本管道**：将一行按模板序列化为自然语言文本（如“列名是值”），经分词后送入冻结的预训练文本编码器（如 TAPAS/BERT-based），得到文本嵌入 \(\mathbf{g}^{text}_i\)。
  - **训练目标 – MUCOSA**：联合损失函数 \( \mathcal{L} = (1-\lambda) \mathcal{L}_{supervised} + \lambda \mathcal{L}_{consistency} \)。
    - 监督损失 \(\mathcal{L}_{supervised}\)：仅用图嵌入经分类头预测标签，与真实标签计算交叉熵。
    - 一致性损失 \(\mathcal{L}_{consistency}\)：无标签的双向对比损失，对齐同一行的图嵌入与文本嵌入（式3），类似于 CLIP 的对称 InfoNCE 损失，温度 \(\tau=0.1\)。
    - \(\lambda\) 控制一致性正则化的强度（默认 0.2）。
  - **推理阶段**：仅使用图编码器，省略文本管道，提升推理速度。

### 3. 实验设计
- **数据集**：25 个分类数据集（二分类与多分类），来自 TabLLM、TabPFN、OpenML。涵盖：
  - 异构数据集（兼顾数值与类别列）：Bank, Creditg, Heart, Income 等。
  - 纯数值数据集：blood, calhousing, coil2000 等。
  - 纯类别数据集：car 等。
  数据量从约 500 行到 4.5 万余行不等。
- **对比方法**：
  - 传统机器学习：CatBoost, XGBoost, Gradient Boosting, Random Forest, Logistic Regression。
  - 表格深度学习模型：FT-Transformer, TabTransformer, NODE。
  - 单模态转换方法：IGNNet（表→图），TabLLM（表→文本），TabPFN。
- **评价指标**：AUC-ROC（5 个随机种子平均值）。

### 4. 资源与算力
- **硬件**：4 块 NVIDIA V100 GPU。
- **训练配置**：学习率 \(1e-4\)，批大小 256，一致性损失权重 \(\lambda=0.2\)，固定超参。
- **参数量对比**：TabGLM（TAPAS-base 为文本编码器）约 129M，若计入图编码器约 336M，相比 TabLLM 的 2.9B 减少了超 80%。
- **未明确说明**：单次训练时长、总训练周期数等资源消耗细节。

### 5. 实验数量与充分性
- **主要实验**：在全部 25 个数据集上对比了 10 余种基线，给出平均 AUC-ROC 与逐数据集结果。
- **消融实验**：
  - 多模态 vs. 单模态：在 pc3, bank, creditg 上分别测试仅图管道、仅文本管道和完整多模态，证明融合模态的优势。
  - 文本编码器选择：对比 TAPAS（BERT-base）、TAPEX、TabLLM 三种预训练模型，显示 TAPAS 小而高效。
  - 参数效率分析。
- **充分性评价**：实验覆盖面广，数据集多样，基线方法既包括经典 ML 也涵盖最新深度学习与单模态转换方法；消融实验分析了关键设计选择，比较公平。

### 6. 主要结论与发现
- TabGLM 通过多模态图-语言联合建模，显著优于传统线性/树模型（平均相对提升 2–5% AUC-ROC）及现有表格深度学习方法。
- 在 TabLLM 基准的 9 个异构数据集上，TabGLM 超越 TabLLM （+1.35%）和 IGNNet （+7.96%）。
- 多模态一致性损失不仅促进信息融合，还起到正则化作用，缓解过拟合。
- 参数效率极高，推理时只需图编码器，兼具高性能与轻量化。

### 7. 优点
- **创新性**：首次将表格记录同时建模为图和文本，并通过多模态对比对齐实现表示学习。
- **处理异质数据能力强**：图捕捉结构，文本保留语义，互补性强。
- **高效性**：冻结大文本模型，训练仅更新图编码器和分类头，参数量远低于同类方法；推理无需文本编码器，速度快。
- **可扩展与通用**：在 25 个多类型数据集上均表现出鲁棒提升。

### 8. 不足与局限
- **文本编码器限制**：受限于 TAPAS 的 512 token 输入长度，对超宽表或极长单元格不友好；且冻结的预训练模型可能不适应领域特定词汇。
- **图构造简化**：全连接图权重单一，且类别特征须强制数值化，可能导致语义损失；未探索更复杂的图结构（如异构图、超图）。
- **纯类别数据表现**：对仅有类别列的数据集，TabGLM 退化为仅用文本管道，未能发挥多模态优势。
- **实验细节缺失**：未提供训练时间、超参数敏感性分析，λ 固定为 0.2 缺乏调优讨论。
- **与树模型仍存差距**：在某些简单或特征数少的数据集上，CatBoost 等仍保持领先，说明多模态方法未必在所有情况下都是最优。
- **可能过拟合风险**：文中虽指出一致性损失有助于正则化，但仅有 3 个数据集的消融，无法全面评估在不同规模/维度下的过拟合抑制效果。

（完）
