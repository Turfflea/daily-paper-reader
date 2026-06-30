---
title: Towards Global-Topology Relation Graph for Inductive Knowledge Graph Completion
title_zh: 面向全局拓扑关系图的归纳式知识图谱补全
authors: "Ling Ding, Lei Huang, Zhizhi Yu, Di Jin, Dongxiao He"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/33260/35415"
tags: ["query:mt"]
score: 9.0
evidence: 构建全局关系图进行归纳式知识图谱补全
tldr: 现有归纳式知识图谱补全方法侧重新实体而随机初始化关系，未充分利用关系结构的全局不变性。本文提出TARGI，首先从全局视角为每种拓扑构建关系图，然后利用该图聚合丰富的嵌入信息，实现简单有效的归纳式补全。实验表明，该方法在多个基准数据集上取得了领先性能，显著提升了归纳式知识图谱补全的准确性。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33260/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 883, \"height\": 460, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33260/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 879, \"height\": 416, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33260/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1822, \"height\": 760, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33260/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 868, \"height\": 318, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33260/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1825, \"height\": 709, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33260/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 878, \"height\": 711, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33260/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 877, \"height\": 531, \"label\": \"Table\"}]"
motivation: 现有归纳式知识图谱补全方法主要关注新实体，关系表示随机初始化，未充分利用关系结构的全局不变性。
method: 提出TARGI，从全局视角为每种拓扑构建关系图，并利用该图聚合丰富的嵌入信息。
result: 在多个基准数据集上取得了领先性能，验证了方法的有效性和简单性。
conclusion: 通过利用关系结构的全局不变性，显著提升了归纳式知识图谱补全的准确性。
---

## Abstract
Knowledge Graphs (KGs) are structured data presented as directed graphs. Due to the common issues of incompleteness and inaccuracy encountered during construction and maintenance, completing KGs becomes a critical task. Inductive Knowledge Graph Completion (KGC) excels at inferring patterns or models from seen data to be applied to unseen data. However, existing methods mainly focus on new entities, while relations are usually randomly initialized. To this end, we propose TARGI, a simple yet effective inductive method for KGC. Specifically, we first construct a global relation graph for each topology from a global graph perspective, thus leveraging the in-variance of relation structures. We then utilize this graph to aggregate the rich embeddings of new relations and new entities, thereby performing KGC robustly in inductive scenarios. This successfully addresses the excessive reliance on the degree of relations and resolves the high complexity and limited scope of enclosing subgraph sampling in existing fully inductive algorithms. We conduct KGC experiments on six inductive datasets using inference data where entities are entirely new and new relations at 100 percent, 50 percent, and 0 percent radios. Extensive results demonstrate that our model accurately learns the topological structures and embeddings of new relations, and guides the embedding learning of new entities. Notably, our model outperforms 15 SOTA methods, especially in two fully inductive datasets.

---

## 论文详细总结（自动生成）

好的，作为一名资深学术论文分析助手，我将使用中文、以 Markdown 形式，对这篇论文《Towards Global-Topology Relation Graph for Inductive Knowledge Graph Completion》进行结构化、深入且客观的总结。

### 1. 论文的核心问题与整体含义

*   **研究背景与动机**：
    *   知识图谱补全旨在预测知识图谱中缺失的事实（三元组），以解决其固有的不完整性。
    *   传统方法多为直推式学习，无法处理推理阶段出现的新实体或新关系。
    *   现有的归纳式知识图谱补全方法主要聚焦于处理**新实体**，但通常假设所有**关系**在训练时已被观测到，并将关系视为直推式学习（例如，随机初始化或粗略处理），忽略了关系之间的语义关联。
    *   现实应用场景中，新关系会不断涌现，因此需要能同时处理**新实体和新关系**的完全归纳式学习能力。现有的一些方法（如RMPI使用子图采样，InGram依赖关系度）存在复杂性高、泛化能力受限或对高频关系有偏等问题。

*   **核心问题**：
    如何在一个知识图谱中，有效学习关系的**全局拓扑结构不变性**，从而在训练和推理阶段关系集合不同（即出现全新关系）的归纳场景下，仍能准确生成新实体和新关系的嵌入表示，以完成知识图谱补全任务。

*   **整体含义**：
    本文提出，关系间的语义相关性与其在整个图谱中的拓扑结构高度相关，且这种拓扑模式具有跨不同实体集的**不变性**。通过从全局视角构建并学习这种关系间拓扑图，可以为归纳式场景下未知关系的嵌入生成提供有效且鲁棒的引导。

### 2. 论文提出的方法论

*   **核心思想**：
    提出了一种基于图神经网络的归纳式知识图谱补全模型 **TARGI**。其核心是从整个知识图谱的全局视角出发，为每一种关系间的拓扑模式（如头对尾、并列等）分别构建一个关系图。这些关系图不依赖于具体实体，因此其结构是不变的，可以被迁移到包含新实体和新关系的推理图中，指导嵌入学习。

*   **关键技术细节与算法流程**：
    TARGI 模型包含三个主要模块，流程如下：
    1.  **全局拓扑关系图构建**：
        *   基于TACT的工作，定义关系对之间的6种拓扑模式：“头-尾”，“尾-尾”，“头-头”，“尾-头”，“并行”和“循环”。
        *   遍历整个训练知识图谱，为每种模式分别构建一个关系图。在关系图中，节点是关系，边表示这两个关系之间存在对应的拓扑模式连接。这形成了一个形状为 `[6, |R|, |R|]` 的三维邻接张量 `A`。
        *   由于图结构基于关系间的拓扑，而非具体实体，当推理阶段出现新关系时，该全局关系图的结构不变，可以捕捉新关系的交互。

    2.  **关系嵌入学习**：
        *   对关系节点进行初始化，并通过一个可学习的投影矩阵 `H` 映射到隐藏空间。
        *   在每一种拓扑模式的关系图上，使用一个改进的图注意力网络来处理静态注意力问题，聚合邻居关系的信息，更新关系节点的嵌入 `s_rel^(l+1)`。
        *   将6种关系图上学习到的嵌入进行拼接，并通过一个**自适应加权聚合策略**（β参数）形成最终的综合关系嵌入 `s^(l+1)`。这使得关系嵌入能捕获多样且复杂的结构特征。

    3.  **实体嵌入学习**：
        *   利用学习到的最终关系嵌入 `s^(L)` 来指导实体层级的聚合。
        *   在实体聚合时，不仅考虑邻居实体 `v_j` 的嵌入 `h_j`，还显式地将连接它们的关系嵌入 `s_k` 以及一个关系感知的自环信息 `s_i^(L)`（通过频率计算）整合到注意力分数的计算中。
        *   采用类似GATv2的动态注意力机制，计算实体 `v_i` 和 `v_j` 及其关系 `r_k` 的注意力权重，然后聚合邻居信息更新实体嵌入 `h_i^(l+1)`。

    4.  **优化目标**：
        *   将链接预测任务统一转化为尾实体预测问题`(v_i, r_k, ?)`。
        *   采用负采样技术，通过替换头实体或尾实体生成负三元组。
        *   使用基于间隔的排序损失函数进行模型优化：`L = max(0, γ - f(T_tr) + f(T_neg))`。

### 3. 实验设计

*   **数据集与场景**：
    *   **数据集**：使用两个公开数据集 **FB15K-237** 和 **NELL995**，并依据InGram的处理方式，将其重新划分为6个更具挑战性的归纳式数据集。
    *   **归纳式场景分类**：
        *   **全归纳场景 (Fully Inductive)**：推理图（`G_inf`）中的关系100%是全新的（**FB-100, NL-100**）。实体也全是新的。
        *   **半归纳场景 (Semi-Inductive)**：推理图中的关系50%是新关系，50%是已知关系（**FB-50, NL-50**）。实体全是新的。
        *   **传统归纳场景 (Traditional Inductive)**：关系完全已知（0%新关系），但实体是全新的（**NELL995-v1, NL-0**）。

*   **Benchmark与对比方法**：
    *   TARGI与**15种当前最先进的方法**进行了全面对比，涵盖了基于子图、基于嵌入、基于规则、基于GNN等多种类型，包括：GraIL， CoMPILE， RMPI， CompGCN， NodePiece， NeuralLP， DRUM， BLP， QBLP， NBFNet， RED-GNN， RAILD， SE-GNN， GreenKGC， 和 InGram。

*   **评估指标**：
    *   使用知识图谱补全领域的标准指标，包括 Mean Reciprocal Rank (MRR)， Hits@10， Hits@1， 和 Mean Rank (MR)。

### 4. 资源与算力

*   论文中**未明确提及**所使用的具体GPU型号、数量，也未说明模型的具体训练时长或总计算开销。
*   论文仅提及使用默认的PyTorch设置和Adam优化器，以及所有方法的嵌入维度统一设置为 `d=32` 以保证公平比较。

### 5. 实验数量与充分性

*   **实验数量**：
    *   论文在**6个不同归纳场景**的数据集上分别进行了实验。
    *   包含与**15种基线模型**的性能对比。
    *   进行了详细的**消融研究**，测试了模型中各个模块的贡献，具体包括：替换加权聚合为均值或求和聚合、替换关系嵌入层为普通GAT、单独应用6种拓扑关系图中的任一种、以及仅使用普通拓扑（排除LOOP和PARA）。
    *   从实验组数上看，总计约有 6个主要场景 × 16个模型 + 多个消融变体 = 超过100组的实验设置，实验数量充分。

*   **实验的充分性、客观性与公平性**：
    *   **充分性**：实验覆盖了从传统归纳到半归纳再到完全归纳的多种场景，并结合了详尽的消融研究，能够系统地验证模型各个组件的有效性。
    *   **客观性与公平性**：为保证公平，所有模型（包括基线）的嵌入维度都统一设为32，并重复运行5次取平均值。作者也指出了部分基线方法在特定场景下无法获取结果的原因（如计算不可扩展、无法处理新关系等），体现了报告的诚实性。

### 6. 论文的主要结论与发现

*   **性能优越性**：TARGI在6个数据集上的表现总体上超越了所有15种基线模型，尤其是在最具挑战性的两个全归纳数据集（FB-100, NL-100）上取得了显著领先优势。
*   **模型有效性**：
    *   拓扑结构的重要性被验证：通过构建全局关系图学习关系间的拓扑模式，可以有效地泛化到包含全新关系的场景中。
    *   “关系加权聚合器”和“专有的关系嵌入学习层”是模型的核心贡献。消融实验表明，替换这些模块会导致性能大幅下降（如MRR最高下降57%）。
    *   不同类型的拓扑模式对不同数据集的贡献不同，证明了多维度拓扑分析的必要性。
*   **能力验证**：TARGI能够精准学习新关系的拓扑结构和嵌入，并有效指导新实体的嵌入学习，解决了现有方法对“关系度”的过度依赖以及子图采样方法的局限性。

### 7. 优点与亮点

*   **方法设计**：
    *   **视角新颖**：从全局拓扑角度出发，利用关系结构的不变性来解决归纳学习问题，思路巧妙，直击要害。
    *   **架构简洁而有效**：“分别构建拓扑图，再进行融合”的策略简洁美观，避免了复杂的子图采样，降低了计算复杂度。
    *   **关系表示增强**：显式融合了6种拓扑模式，并采用了改进的GAT和自适应加权机制，能更精细地捕捉关系的语义和结构特征。
    *   **实体学习改进**：在实体聚合中同时引入了“关系感知”和“自环”信息，增强了实体嵌入的表达能力。

*   **实验设计**：
    *   **场景全面**：实验覆盖了全归纳、半归纳到传统归纳的多种场景，评估非常详尽。
    *   **对比充分**：与大量（15种）不同类型的SOTA基线进行了对比，说服力强。
    *   **消融彻底**：通过多种消融实验，清晰地验证了各个模块和不同拓扑图的价值，分析深入。

### 8. 不足与局限

*   **实验覆盖**：
    *   实验仅在FB15K-237和NELL995两个数据集上进行，虽然构造了6种场景，但数据集本身的多样性有限，未来需要在更多样化、规模更大的知识图谱上验证模型泛化能力。
*   **计算与资源报告**：
    *   论文完全没有提及模型的计算资源消耗（如GPU型号、训练时长、显存占用），这不利于其他研究者评估其成本与可行性，是一个明显的报告缺陷。
*   **偏差风险**：
    *   模型的构建严格依赖于预定义的6种拓扑模式。如果某些知识图谱的语义关系无法被这6种模式很好地捕捉，模型性能可能会受限。
    *   方法的基础是“关系结构的不变性”，这一假设在极端动态变化或噪声严重的图谱中是否依然成立，值得探讨。
*   **应用限制**：
    *   虽然解决了新关系问题，但TARGI是`Transductive`地在关系图上学习，意味着如果出现一个与训练阶段拓扑模式完全不同的全新关系簇，模型可能仍难以处理。
    *   模型是纯粹基于图结构的，没有利用任何关于实体或关系的文本、属性等多模态信息，信息来源单一。

（完）
