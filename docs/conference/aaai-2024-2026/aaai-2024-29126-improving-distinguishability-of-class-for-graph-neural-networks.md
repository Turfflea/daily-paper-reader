---
title: Improving Distinguishability of Class for Graph Neural Networks
title_zh: 提升图神经网络中类可区分性的方法
authors: "Dongxiao He, Shuwei Liu, Meng Ge, Zhizhi Yu, Guangquan Xu, Zhiyong Feng"
date: 2024-03-25
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/29126/30130"
tags: ["query:mt"]
score: 8.0
evidence: 引入类可区分性指导GNN训练，缓解深层网络性能退化
tldr: 针对GNN层数增加导致性能下降的问题，提出Disc-GNN，通过监控节点表示的类间可区分性，自适应调整层间信息传播，有效缓解过平滑；在深层GNN上取得稳定提升，为构建更深层GNN提供了有效思路。
source: AAAI-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29126/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1834, \"height\": 405, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29126/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1815, \"height\": 669, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29126/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 873, \"height\": 445, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29126/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 852, \"height\": 490, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29126/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 883, \"height\": 360, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29126/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1842, \"height\": 501, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29126/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1843, \"height\": 909, \"label\": \"Table\"}]"
motivation: GNN层数增加常导致节点表示可区分性下降，模型性能退化。
method: 定义节点表示的类可区分性度量，设计Disc-GNN通过该度量指导层间聚合和正则化。
result: 在多个节点分类数据集上，深层GNN的性能显著提升，超越了普通堆叠方法。
conclusion: 类可区分性引导的设计为构建更深层GNN提供了有效思路。
---

## Abstract
Graph Neural Networks (GNNs) have received widespread attention and applications due to their excellent performance in graph representation learning. Most existing GNNs can only aggregate 1-hop neighbors in a GNN layer, so they usually stack multiple GNN layers to obtain more information from larger neighborhoods. However, many studies have shown that model performance experiences a significant degradation with the increase of GNN layers. In this paper, we first introduce the concept of distinguishability of class to indirectly evaluate the learned node representations, and verify the positive correlation between distinguishability of class and model performance. Then, we propose a Graph Neural Network guided by Distinguishability of class (Disc-GNN) to monitor the representation learning, so as to learn better node representations and improve model performance. Specifically, we first perform inter-layer filtering and initial compensation based on Local Distinguishability of Class (LDC) in each layer, so that the learned node representations have the ability to distinguish different classes. Furthermore, we add a regularization term based on Global Distinguishability of Class (GDC) to achieve global optimization of model performance. Extensive experiments on six real-world datasets have shown that the competitive performance of Disc-GNN to the state-of-the-art methods on node classification and node clustering tasks.

---

## 论文详细总结（自动生成）

好的，作为一名资深学术论文分析助手，下面我将以 Markdown 形式，对论文《Improving Distinguishability of Class for Graph Neural Networks》进行结构化、深入、客观的总结。

### 1. 论文的核心问题与整体含义

*   **研究背景**：图神经网络（GNN）在图表示学习任务中表现出色，但它们通常只能聚合1跳邻居信息。为了捕获更大邻域信息，需要堆叠多个GNN层。
*   **核心问题**：随着GNN层数加深，模型性能会出现显著退化（性能下降）。现有研究多将此归因于过平滑问题，但本文认为这不足以完全解释该现象。
*   **研究动机**：论文观察到深层GNN的节点表示逐渐丧失对不同类别的区分能力。为此，作者提出了“类可区分性”（Distinguishability of Class）这一新概念，旨在从节点能否被清晰分类的角度来间接评估和监控表示学习的质量，并验证了其与模型性能的正相关性。
*   **整体含义**：本文旨在通过确保节点在深层网络中仍具备高“类可区分性”，来缓解性能退化问题，进而学习到更优的节点表示。

### 2. 论文提出的方法论

*   **核心思想**：提出一种由“类可区分性”引导的图神经网络（Disc-GNN），通过局部和全局两种可区分性度量来监控和优化节点表示的学习过程，使其在不同层都能有效区分不同类别。
*   **关键技术细节与组件**：
    *   **类可区分性度量**：
        *   **局部类可区分性 (LDC)**：为每个节点定义，计算其分类概率向量中最大概率与最小概率之差。LDC越接近1，表示该节点类别归属越明确；越接近0，则表示特征混淆严重。公式为：`LDC_i^{(l)} = max(z_i^{(l)}) - min(z_i^{(l)})`。
        *   **全局类可区分性 (GDC)**：计算所有节点LDC的平均值，衡量整个图在当前层的平均类别区分能力。公式为：`GDC^{(l)} = \frac{1}{N} \sum_{v_i \in V} LDC_i^{(l)}`。
    *   **Disc-GNN模型架构（三个核心组件）**：
        1.  **基于LDC的层间过滤 (Inter-layer Filtering)**：设计了一个门控机制。对于第`l`层的节点 `v_i`，如果其新学习到的表示 `gLDC_i^{(l)}` 相较于上一层的 `LDC_i^{(l-1)}` 有所提升，则保留该表示；否则，将其过滤（置零），以防止低可区分性表示传播到后续层。公式化为 `H^{(l)} = σ((I - Φ^{(l)}) \tilde H^{(l)} + Φ^{(l)} \tilde H^{(l)} W^{(l)})`，其中 `Φ^{(l)}` 是过滤矩阵。
        2.  **基于LDC的初始补偿 (Initial Compensation)**：为解决因层数加深导致的初始特征损失，该机制将当前层表示与初始特征 `H^{(0)}` 进行加权组合。权重 `Λ^{(l)}` 通过比较当前层LDC与初始层LDC自适应决定：若当前层LDC相对较低，则更多地补偿初始特征。公式为：`H^{(l)} = (I - Λ^{(l)}) H^{(0)} + Λ^{(l)} \hat H^{(l)}`。
        3.  **基于GDC的全局优化 (Global Optimization)**：在训练的目标函数中加入一个基于最终层GDC的正则化项，以鼓励模型在全局层面上最大化所有节点表示的类可区分性。总的损失函数为：`argmin_Θ L = argmin_Θ (L_{task} - η GDC^{(L)})`。

### 3. 实验设计

*   **数据集与场景**：在六个真实世界数据集上进行了节点分类和节点聚类两类下游任务的实验。
    *   **同配图 (Homophilic)**： Cora, Citeseer, Pubmed（引文网络）。
    *   **异配图 (Heterophilic)**： Wisconsin, Texas, Cornell（网页网络）。
*   **基准方法与对比对象**：实验对比了三大类共九个先进的GNN模型：
    *   **经典模型**： GCN, GAT, SGC。
    *   **缓解过平滑模型**： JK-Net, APPNP, DAGNN。
    *   **同时缓解过平滑与异配性模型**： DeCorr, FAGCN, GCNII。
*   **评估指标**：对于节点分类任务采用准确率（ACC），对于节点聚类任务采用归一化互信息（NMI）和调整兰德指数（ARI）。

### 4. 资源与算力

*   论文未明确提及实验所使用的GPU型号、数量、具体训练时长或浮点运算次数等算力资源信息。
*   仅说明所有方法均基于PyTorch框架和Adam优化器实现。

### 5. 实验数量与充分性

*   **实验数量**：实验设计较为全面，覆盖了多个维度的评估。
    *   **主实验**： 包括在6个数据集上的节点分类任务（与9种方法对比）和节点聚类任务（与9种方法对比），总计进行了超过 `(6 datasets * 1 task) + (6 datasets * 1 task) = 12` 组主要对比，每组又包含多种方法。
    *   **深度分析**： 对模型性能随层数（2, 5, 10, 15, 20层）变化进行了分析。
    *   **消融实验**： 设计了三个模型变体（去除层间过滤、去除初始补偿、去除全局优化），以验证各组件的贡献。
*   **充分性与公平性**：
    *   **充分性**： 实验设计充分，不仅覆盖了同配和异配两种性质的图，还从分类、聚类任务、不同深度影响、自身组件有效性等多个角度进行了验证。
    *   **公平性**： 论文指出所有对比方法均遵循其原论文的默认超参数设置，并多次运行取平均值与标准差，保证了对比的客观性和统计稳定性。

### 6. 论文的主要结论与发现

*   **新视角有效性**： 类可区分性（GDC）与模型性能（ACC）之间存在显著正相关（皮尔逊相关系数达0.9968），证明了从类可区分性角度分析节点表示变化是有效的。
*   **模型性能优越**： 提出的Disc-GNN在绝大多数数据集（尤其是异配图数据集）上，无论是节点分类还是节点聚类任务，性能都优于对比的九种先进模型。
*   **缓解性能退化**： Disc-GNN能有效防止模型在深层（如20层）时出现性能急剧下降，证明其能够从多跳同配邻居中学习到更好的表示。
*   **组件关键作用**： 消融实验证实，初始补偿机制对缓解深层信息损失至关重要，而层间过滤机制在异配图上作用更显著，能有效应对维度间频繁交互所带来的负面影响。

### 7. 优点

*   **创新的研究视角**： 首次提出“类可区分性”（LDC和GDC）的概念，为诊断和理解深层GNN的性能退化提供了超越“过平滑”的补充性视角。
*   **方法论简洁有效**： Disc-GNN模型设计直观，三个组件（过滤、补偿、正则化）直接针对问题设计，通过自适应门控机制而非复杂架构实现了性能提升。
*   **显著的性能提升**： 尤其在异配图数据集上，相较于现有方法取得了大幅度的性能提升（例如在Texas数据集上，节点分类ACC最高提升超过30%），证明了方法的优越性。
*   **实验扎实全面**： 在多个性质不同的数据集和下游任务上进行验证，并通过深度分析和消融实验充分展示了模型的特性与各组件的价值。

### 8. 不足与局限

*   **计算效率与可扩展性未讨论**： 模型中的LDC计算和过滤矩阵涉及对每个节点进行额外运算，论文未讨论其在大规模图上的计算开销和可扩展性问题。
*   **超参数敏感性**： 尽管消融实验验证了组件有效性，但未对各组件涉及的多个超参数（如松弛因子 `ε`、正则化系数 `η`、常量 `μ`）的敏感性进行深入分析，这对其在不同场景下的调参便捷性有影响。
*   **缺少理论分析**： 论文主要基于实验观察和直觉提出LDC/GDC概念，缺乏关于为何这些度量能有效指导学习、以及它们与过平滑、过相关等已知问题之间内在联系的理论分析。
*   **单任务扩展性**： 论文聚焦于节点级任务，该方法对图层级或边层级任务的适用性尚待研究。

（完）
