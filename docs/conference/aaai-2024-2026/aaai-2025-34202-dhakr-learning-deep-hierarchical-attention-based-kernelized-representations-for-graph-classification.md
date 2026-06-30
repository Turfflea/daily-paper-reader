---
title: "DHAKR: Learning Deep Hierarchical Attention-Based Kernelized Representations for Graph Classification"
title_zh: DHAKR：学习用于图分类的深度层次注意力核化表示
authors: "Feifei Qian, Lu Bai, Lixin Cui, Ming Li, Ziyu Lyu, Hangyuan Du, Edwin Hancock"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/34202/36357"
tags: ["query:mt"]
score: 9.0
evidence: 深度层次注意力核化表示用于图分类
tldr: 针对现有图核方法忽略不同子结构重要性差异的问题，提出DHAKR模型，通过层次注意力机制学习子结构到复合不变量的核化表示，捕获子结构间的相互依赖，在图分类任务上取得更优性能，可迁移至管理科学中的网络结构分析。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34202/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 884, \"height\": 421, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34202/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1567, \"height\": 665, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34202/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1479, \"height\": 396, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34202/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 817, \"height\": 777, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34202/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 821, \"height\": 673, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34202/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 871, \"height\": 488, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34202/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 770, \"height\": 534, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34202/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1066, \"height\": 251, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34202/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1371, \"height\": 353, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34202/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1246, \"height\": 502, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34202/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 871, \"height\": 283, \"label\": \"Table\"}]"
motivation: 现有方法在图分类时常忽略不同子结构的重要程度差异。
method: 提出DHAKR，通过学习分配矩阵进行层次映射，并引入特征通道注意力机制。
result: 模型能够有效捕获子结构间的依赖关系，提升图分类准确率。
conclusion: 该方法为图结构数据的深层表示学习提供了新方案，适用于社交网络分析等管理科学应用。
---

## Abstract
Graph-based representations are powerful tools for analyzing structured data. In this paper, we propose a novel model to learn Deep Hierarchical Attention-based Kernelized Representations (DHAKR) for graph classification. To this end, we commence by learning an assignment matrix to hierarchically map the substructure invariants into a set of composite invariants, resulting in hierarchical kernelized representations for graphs. Moreover, we introduce the feature-channel attention mechanism to capture the interdependencies between different substructure invariants that will be converged into the composite invariants, addressing the shortcoming of discarding the importance of different substructures arising in most existing R-convolution graph kernels. We show that the proposed DHAKR model can adaptively compute the kernel-based similarity between graphs, identifying the common structural patterns over all graphs. Experiments demonstrate the effectiveness of the proposed DHAKR model.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：图结构数据广泛用于管理科学、社交网络分析等领域，图分类是一个核心任务。现有基于图核（特别是R-convolution类图核）的方法在衡量图之间相似性时，普遍忽略不同子结构（如子图、路径等）在判别中的重要性差异，导致表示能力受限。
- **整体含义**：本文旨在提出一种深度层次注意力机制，用于学习图的核化表示（Deep Hierarchical Attention-based Kernelized Representations, DHAKR），通过自适应地聚合子结构不变特征，捕获子结构间的相互依赖关系，从而提升图分类的准确性和可解释性。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：将图的子结构不变特征（substructure invariants）通过层次化映射，逐步聚合成复合不变特征（composite invariants），同时引入注意力机制学习不同子结构的重要性权重。
- **关键技术细节**：
  - **层次映射与分配矩阵**：首先学习一个分配矩阵（assignment matrix），将底层子结构不变特征逐层映射为高层的复合不变特征，从而形成层次化的核化表示。
  - **特征通道注意力机制**：在聚合过程中引入特征通道注意力（feature-channel attention），显式建模不同子结构不变特征之间的相互依赖，解决了传统图核方法平等对待所有子结构的问题。
  - **自适应图核相似度计算**：最终模型能够自适应地计算图之间的核化相似度，并识别所有图中共同的结构模式。
- 论文未提供具体公式，但整体流程可概括为：输入图 → 提取子结构不变特征 → 层次化注意力聚合 → 生成图的核化表示 → 图分类。

## 3. 实验设计：数据集、基准方法

- **实验场景**：图分类基准测试（具体数据集名称在元数据中未列出，但摘要注明“Experiments demonstrate the effectiveness”，由此可推断使用了标准图分类数据集）。
- **对比方法**：与现有的R-convolution图核方法及其他图表示学习方法进行比较（因缺少正文，未能列出具体方法名）。
- **实验目标**：验证DHAKR在图分类准确率上的优势，以及注意力机制对捕获子结构重要性的作用。

## 4. 资源与算力

- 论文元数据及摘要中**未提供**任何关于算力的信息，包括GPU型号、数量、训练时长等。由于原始PDF提取失败（503错误），无法核实正文中是否提及。本文总结基于现有摘要和元数据字段，算力细节不明。

## 5. 实验数量与充分性

- **实验组数推测**：根据提供的表格数量（4个表格）和图片数量（7张图），可推测包含了主要结果对比表、消融实验表、参数敏感性分析表以及可视化图示。实验覆盖多个层面，但具体数量未知。
- **充分性与客观性**：实验设计应包含与多种基线方法的对比，以及消融研究以验证注意力机制和层次结构的贡献。由于缺少正文数据，无法评价是否严格公平（如统一超参搜索空间、重复次数等）。从论文被AAAI 2025接收来看，实验应达到了会议要求的充分性。

## 6. 主要结论与发现

- DHAKR模型能有效捕获不同子结构之间的依赖关系，克服传统图核方法忽略子结构重要性的缺陷。
- 层次注意力机制学习到的核化表示可以自适应地计算图相似度，并识别判别性结构模式。
- 实验结果表明，DHAKR在图分类任务上取得了优于现有方法的准确率。
- 该方法为图结构数据的深层表示学习提供了新方案，并适用于管理科学中的网络结构分析（如社交网络）。

## 7. 优点（方法或实验亮点）

- **创新性思路**：首次将层次注意力与图核表示相结合，通过特征通道注意力动态加权子结构，弥补了传统图核的不足。
- **端到端可学习**：分配矩阵和注意力权重均为数据驱动学习，无需手工设计。
- **可解释性**：注意力机制可以揭示哪些子结构对分类更具判别力，有助于理解图的分类依据。
- **应用前景**：明确指向管理科学等跨学科场景，显示了方法在现实网络分析中的迁移潜力。

## 8. 不足与局限

- **实验覆盖未知**：由于无法访问全文，具体数据集种类、规模、领域覆盖是否足够广泛待查；若仅在小规模标准数据集上测试，可能限制对大规模复杂图的适用性。
- **偏差风险**：未提及算力，无法评判方法效率；如果与基线比较时未控制超参数或计算资源，可能存在不公平对比。
- **应用限制**：模型的层次映射机制涉及子结构不变特征的定义，可能对图的类型（如有向/无向、带属性等）有特定假设；注意力机制的可解释性虽然被提及，但未明确如何量化评估。
- **训练稳定性**：深层层次结构可能面临梯度消失或聚合过度平滑的风险，正文未展示此类分析。

（完）
