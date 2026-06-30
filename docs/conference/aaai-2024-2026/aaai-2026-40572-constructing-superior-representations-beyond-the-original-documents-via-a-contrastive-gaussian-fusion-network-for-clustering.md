---
title: Constructing Superior Representations Beyond the Original Documents via a Contrastive Gaussian Fusion Network for Clustering
title_zh: 通过对比高斯融合网络构建超越原始文档的优质表示用于聚类
authors: "Ao Shen, Ruizhang Huang, Jingjing Xue, Ruina Bai"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40572/44533"
tags: ["query:mt"]
score: 8.0
evidence: 用于文本挖掘中文档聚类的对比高斯融合网络
tldr: 针对现有文档聚类方法忽视数据集级特征、无法构建优质表示的问题，本文提出对比高斯融合网络（CGFN）。CGFN在潜在空间中融合邻居衍生信息的分布与文档内在文本特征的高斯分布，并引入对比学习以学习高质量表示，同时抑制噪声。实验表明，该方法在多个文档聚类基准上显著优于传统方法，有效提升了文本挖掘任务中表示学习的质量与鲁棒性。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40572/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 879, \"height\": 489, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40572/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1473, \"height\": 828, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40572/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1726, \"height\": 1037, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40572/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 717, \"height\": 227, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40572/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1681, \"height\": 228, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40572/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1685, \"height\": 620, \"label\": \"Table\"}]"
motivation: 现有文档聚类方法主要关注文档内部特征，忽略了数据集级特征，导致表示质量不高。
method: 提出CGFN，在潜在空间融合邻居信息和内在文本特征的高斯分布，并运用对比学习优化表示。
result: 实验结果显示，CGFN在文档聚类任务上显著优于现有方法，学习到更高质量和抗噪的表示。
conclusion: CGFN通过融合多源信息有效提升了文档聚类性能，为文本挖掘提供了强大的表示学习工具。
---

## Abstract
Document clustering plays an important role in text mining and information retrieval. Existing methods primarily focus on document-intrinsic features, overlooking dataset-level features and consequently failing to construct superior representations. We propose a Contrastive Gaussian Fusion Network (CGFN) that can construct superior representations beyond the original documents.
Specifically, CGFN fuses the Gaussian distributions of neighbor-derived information and intrinsic textual features in the latent space. By incorporating contrastive learning into the fusion process, our proposed method is able to learn high-quality representations while simultaneously mitigating noise and minimizing information loss. Experiments on four real-world datasets demonstrate that CGFN outperforms state-of-the-art methods, achieving superior clustering by robustly capturing holistic distributions and neighbor patterns.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
*   **核心问题**：现有基于变分自编码器（VAE）的文档聚类方法主要关注文档内在特征，忽略了数据集层面的结构信息（如邻居文档模式）。这在高维稀疏的文本空间中，容易导致潜在分布估计不准确（后验坍塌问题）和信息不完整，从而无法构造出能捕捉完整语义和结构信息的**优质表示**。
*   **研究动机**：
    *   **后验坍塌**：VAE在学习复杂约束（如邻居信息）时，其近似后验分布容易坍缩到先验分布，无法学习到有意义的输入特征。
    *   **融合噪声与信息丢失**：简单地拼接或融合内在文本特征与邻居信息，会引入噪声或丢失关键信息，无法有效建模跨模态关系。
*   **整体含义**：本文旨在通过巧妙融合文档内在特征与邻居衍生信息的高斯分布，构建超越原始文档的表示，以显著提升文档聚类的精度和鲁棒性。

### 2. 论文提出的方法论
论文提出了一种名为**对比高斯融合网络（Contrastive Gaussian Fusion Network, CGFN）** 的新框架。其核心思想是在潜在空间中，将文档的原始文本特征和通过K近邻（KNN）得到的邻居信息，分别编码为高斯分布，然后通过巧妙的方式进行融合和优化。
*   **核心技术细节**：
    *   **双通道高斯推断模块**：模型使用两个VAE编码器，分别将原始文档 `x` 和邻居平均文档 `x'` 推断为潜在空间中的高斯分布 `N(μ, σ²)` 和 `N(μ', σ'²)`。
    *   **对比高斯融合模块 (CGFM)**：这是模型的核心。
        *   **高斯融合子模块**：通过计算两个高斯分布的乘积，得到一个新的融合分布 `N(μz, σz²)`。公式为：μz = (μ'σ² + μσ'²) / (σ² + σ'²)，σz² = (σ²σ'²) / (σ² + σ'²)。这种参数层面的融合旨在保留核心语义并减少信息丢失。
        *   **对比学习子模块**：通过一个投影头（MLP）将一方分布的参数（如μ, σ）转换后，与另一方的分布计算KL散度，并构造一个对称损失函数 `L_cfm`。其目的是提取双方的共享信息，同时过滤邻居带来的噪声。
    *   **一致性增强模块 (CEM)**：通过修改证据下界（ELBO），引入一个小于1的系数 `β` 来弱化KL散度正则项（`-β D_KL`），允许后验分布偏离先验分布，从而保护文本数据的弱信号，有效缓解后验坍塌问题。
    *   **聚类模块**：使用学生t分布计算样本与聚类中心的软分配概率 `q_ij`，并通过自蒸馏的方式生成目标分布 `p_ij`，最后通过最小化两者之间的KL散度来迭代优化聚类结果。
*   **训练流程**：
    1. 预训练CGFN模型。
    2. 计算融合表示 `z`，并初始化聚类中心。
    3. 联合优化由CEM和对比学习模块组成的表示学习损失 `L_cgfm`，以及聚类损失 `L_kl`。

### 3. 实验设计
*   **数据集与场景**：在四个真实世界的文档数据集上进行评估。
    *   **BBC**: 2225篇新闻文章，5个类别。
    *   **Abstract**: 4306篇论文摘要，3个类别。
    *   **BBCSports**: 737篇体育新闻，5个类别。
    *   **Reuters-10k**: 10000篇新闻，4个类别。
*   **评估基准（Metrics）**：聚类准确度（ACC）、归一化互信息（NMI）、调整兰德指数（ARI）。
*   **对比方法**：比较了三类共13种模型。
    *   **传统方法**：K-means, LDA。
    *   **深度聚类方法**：AE, IDEC, DCEC, VAE, VaDE。
    *   **结构语义融合方法**：VGAE, SDCN, SDCMS, DSEDC, SEDCN。

### 4. 资源与算力
*   文中仅提及使用Adam优化器，以及在BBC和Reuters-10k数据集上设置学习率为0.0005，在Abstract和BBCSports数据集上为0.001；预训练50轮，最大迭代3000次，批次大小为256。
*   **未明确提及使用了何种GPU、数量及具体训练时长**。

### 5. 实验数量与充分性
*   **实验数量**：
    *   **主实验**：在4个数据集上，对比了12种基准方法，包含3项评估指标。
    *   **消融实验**：设计了3个CGFN的变体（CGFN-f, CGFN-r, CGFN-c），在所有4个数据集上验证各模块（高斯融合、一致性增强、聚类模块）的有效性。
    *   **可视化实验**：在4个数据集上，将CGFN和标准VAE的聚类结果进行t-SNE可视化对比。
*   **充分性与公平性评估**：
    *   **充分性**：实验设计较为全面，主流对比实验、多层次的消融实验和定性可视化分析三者结合，有力地支撑了模型的贡献声明。所有实验重复10次取平均值，避免极端情况，增强了结果的可靠性。
    *   **公平性**：对比基准涵盖了传统、深度和结构融合等多种方法，较为全面。消融实验清晰地剥离了各模块，证明了每个组件的价值。

### 6. 论文的主要结论与发现
*   CGFN在所有四个数据集的关键指标NMI上均显著优于所有对比模型，证明了通过分布融合增强表示的有效性。
*   一致性增强模块（CEM）通过降低KL散度权重，有效缓解了VAE的后验坍塌问题，并能保留更多文本信息。
*   对比高斯融合模块（CGFM）比传统方法（如参数拼接）能更有效地融合内在特征和邻居信息，在丰富语义的同时抑制了噪声和减少了信息丢失。
*   可视化结果直观显示，CGFN能学习到更紧凑、分离度更高的簇结构。

### 7. 优点：方法或实验设计上的亮点
*   **创新的融合方式**：在潜在空间对两个高斯分布进行“乘积”操作和参数级融合，数学形式优雅，且有明确的概率意义，为多源信息融合提供了新思路。
*   **巧妙的约束设计**：通过β系数动态调节KL散度正则项，针对文本数据的特性专门设计解决后验坍塌的方案，具有很强的针对性。
*   **噪声过滤机制**：引入对比学习来对齐原生和邻居分布，巧妙地实现了在融合邻居信息的同时，提取共享特征并过滤邻居噪声，提升了融合的鲁棒性。
*   **实验论证扎实**：消融实验设计清晰，逐步剥离各模块，充分验证了每个模块的独立贡献，结论可信度高。

### 8. 不足与局限
*   **算力成本不明确**：未报告模型训练所用算力（GPU型号、数量及时间），无法评估其计算效率和复现所需资源门槛。
*   **超参数敏感性与细节**：邻居数量k（固定为50）、关键平衡系数α和β（固定为0.1）等重要超参数的敏感性分析缺失。针对不同数据集，这些值是否鲁棒尚不清楚，影响实际调优指导。
*   **融合方法局限性**：作者在结论中指出，当邻居信息与文本信息高度近似时，高斯乘积融合的有效性可能会被削弱，这是方法本身的一个风险点。
*   **数据集规模与领域限制**：所用数据集均为英文新闻或论文摘要，规模中等偏小（最大10k），模型在更大规模、更多样化领域（如社交媒体、长文本）的中文或其他语言数据集上的泛化能力有待考证。

（完）
