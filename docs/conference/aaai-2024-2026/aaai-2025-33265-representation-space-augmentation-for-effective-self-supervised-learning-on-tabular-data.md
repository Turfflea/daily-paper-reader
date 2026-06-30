---
title: Representation Space Augmentation for Effective Self-Supervised Learning on Tabular Data
title_zh: 面向表格数据有效自监督学习的表示空间增强
authors: "Moonjung Eo, Kyungeun Lee, Hye-Seung Cho, Dongmin Kim, Ye Seul Sim, Woohyung Lim"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/33265/35420"
tags: ["query:mt"]
score: 9.0
evidence: 通过表示空间增强实现表格数据的自监督学习
tldr: 针对表格数据缺乏显式结构导致传统输入级增强效果不佳的问题，本文提出RaTab方法，将增强从输入级转移到表示级。通过矩阵分解在表示空间中进行增强，有效平衡信息保留与多样性，实现了表格数据上深度神经网络的自监督预训练。实验显示RaTab在下游任务上显著优于现有方法，为表格数据的自监督学习开辟了新路径，尤其适用于工业界广泛存在的结构化数据场景。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33265/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1826, \"height\": 605, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33265/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 831, \"height\": 297, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33265/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 635, \"height\": 475, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33265/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 658, \"height\": 520, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33265/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 663, \"height\": 364, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33265/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 745, \"height\": 399, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33265/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1758, \"height\": 289, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33265/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1743, \"height\": 717, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33265/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1470, \"height\": 206, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33265/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 652, \"height\": 285, \"label\": \"Table\"}]"
motivation: 表格数据在深度学习中研究不足，自监督学习潜力受限于合适增强方法的设计，因其缺乏显式空间或语义结构。
method: 提出RaTab，利用矩阵分解在表示空间进行增强，替代传统输入级增强，适用于表格数据的自监督预训练。
result: 实验表明，RaTab在下游任务上取得优于现有方法的性能，有效平衡了信息保留与多样性。
conclusion: RaTab为表格数据的自监督学习提供了新颖有效的增强策略，具有广泛的工业应用前景。
---

## Abstract
Tabular data, widely used across industries, remains underexplored in deep learning. Self-supervised learning (SSL) shows promise for pre-training deep neural networks (DNNs) on tabular data, but its potential is hindered by challenges in designing suitable augmentations. Unlike image and text data, where SSL leverages inherent spatial or semantic structures, tabular data lacks such explicit structure. This makes traditional input-level augmentations, like modifying or removing features, less effective due to difficulties in balancing critical information preservation with variability. To address these challenges, we propose RaTab, a novel method that shifts augmentation from input-level to representation-level using matrix factorization, specifically truncated SVD. This approach preserves essential data structures while generating diverse representations by applying dropout at various stages of the representation, thereby significantly enhancing SSL performance for tabular data.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **核心问题：** 表格数据（Tabular Data）在深度学习领域的自监督学习（SSL）研究中应用不足，主要瓶颈在于缺乏有效的数据增强手段。
- **研究背景与动机：**
  - 图像和文本数据具有天然的空间或语义结构，容易设计出合理的输入级增强（如裁剪、旋转、同义词替换）。
  - 表格数据则异构性强、列序任意且特征关系复杂，直接修改或删除特征的传统输入级增强容易破坏关键模式。
  - 自监督学习（特别是对比学习）对表格数据潜力巨大，但缺乏既保留关键信息又产生足够多样性的增强策略。
- **整体含义：** 提出一种创新的表示空间增强方法RaTab，将增强从输入端转向潜在的表示空间，利用矩阵分解技术（截断SVD）来挖掘并保留表格数据中的核心结构，旨在解决表格SSL的根本性难题，并实现性能的显著跃升。

### 2. 论文提出的方法论
- **核心思想：将增强从输入空间转移到表示空间**，认为编码器生成的表示（Representation）比原始输入更加结构化、更易于进行有效增强。
- **关键技术细节：**
    - **截断SVD增强：**
        1. 获取编码器（Encoder）最后一层的权重矩阵 \(W\) 。
        2. 对 \(W\) 进行奇异值分解 \(W = U \Sigma V^T\)。
        3. 保留前k个最大的奇异值及对应向量，得到截断权重矩阵 \(W_k = U_k \Sigma_k V_k^T\)。
        4. 用 \(W_k\) 更新编码器权重，得到增强后的编码器 \(\hat{f}_{en}\)，并以此生成增强视图 \(z_{aug} = \hat{f}_{en}(x)\)。
        5. 通过控制保留秩的比例 \(p\)（实验取50%、60%、70%）来平衡信息保留与变化程度。
    - **结合Dropout增加多样性：** 采用原始编码器中的Dropout机制与截断SVD叠加，为增强表示引入更多的随机性。
    - **双目标损失函数：**
        - **重构损失 \(L_{recon}\)：** 均方误差（MSE），促使解码器还原原始样本，以建模样本内特征关系。
        - **对比损失 \(L_{InfoNCE}\)：** 使用InfoNCE损失，最大化原始表示 \(z\) 与增强表示 \(z_{aug}\) 的相似度，以学习样本间判别性特征。
        - **总损失：** \(L_{overall} = \lambda L_{recon} + (1 - \lambda) L_{InfoNCE}\)，其中 \(\lambda\) 默认为0.5。
- **算法流程概览：**
    输入一个批次数据 -> 经原始编码器得表示 \(z\) -> 提取编码器末层权重进行截断SVD -> 更新编码器得到增强表示 \(z_{aug}\) -> \(z\) 送入解码器计算重构损失 -> \(z\) 与 \(z_{aug}\) 经投影头计算对比损失 -> 反向传播更新模型。

### 3. 实验设计
- **数据集：** 使用来自OpenML的13个公开表格数据集，覆盖二分类、多分类和回归任务。样本量范围从1,000到98,050不等，特征数5至93个，包含分类和连续变量。
- **对比基准：**
    - **传统树模型：** XGBoost、CatBoost、LightGBM。
    - **深度神经网络：** MLP、FT-Transformer、ResNet、NODE、TabNet、DCNv2、T2G-former、SAINT、SCARF。
    - **多种增强策略对比：** 在统一MLP骨干网上，与四种增强方法（输入级补全Masking、输入级置换Shuffling、输入级裁剪混合CutMix、潜在空间混合Contrastive-mixup）进行了直接比较。
- **应用场景：** 自监督预训练 + 微调的标准范式。预训练时使用全部无标签数据，微调时冻结编码器并附加分类头进行监督训练。

### 4. 资源与算力
- **硬件条件：** 所有实验均在**单个NVIDIA GeForce RTX 3090**上完成。
- **时间与效率：** 论文未明确给出总训练时长，但提及通过随机化SVD可以显著降低矩阵分解的计算复杂度，解决了标准SVD在大权重矩阵上的效率问题，这表明方法对算力要求相对适中，可在消费级高端显卡上复现。

### 5. 实验数量与充分性
- **实验组数概览：**
    - **主表对比：** 在13个数据集上，RaTab与10余种主流模型进行了性能比较。
    - **增强策略对比：** 与4种不同的增强方法在全部13个数据集上进行了对比。
    - **嵌入分析：** 通过t-SNE可视化和对齐/均匀性指标，对嵌入空间进行了定性及定量分析。
    - **鲁棒性测试：** 设计了向数据集注入大量非信息性特征的实验。
    - **消融研究：**
        1.  损失函数消融（仅重构、仅对比、组合）。
        2.  超参数λ和保留秩比例P的敏感度分析。
        3.  逐层应用RaTab（不同层 vs. 多层）的消融。
- **充分性与客观性评估：**
    - 实验设计**相当充分且严谨**。覆盖了从分类到回归的多种任务，对比了神经网络、树模型及多种前沿SSL增强策略。
    - 通过控制变量法（统一MLP骨干网）对比了各类增强技术，并通过多组消融实验验证了方法中各组件（截断SVD、损失函数组合、施加层数）的贡献，结论具有说服力。

### 6. 论文的主要结论与发现
- **性能领先：** RaTab在所有13个数据集上，无论使用何种骨干网络（MLP、FT-Transformer、T2G-former），均一致地优于对比的树模型和深度神经网络基线，尤其是在BM、CS、DIA等数据集上优势显著。
- **增强策略优越性：** 在MLP上，RaTab全面超越了输入空间增强和其他潜在空间增强方法，证明了基于截断SVD的表示空间增强是其性能的核心驱动力。
- **平衡信息与多样性：** 对齐/均匀性分析显示，RaTab在降低对齐得分（更好保留关键信息）的同时维持了有竞争力的均匀性（增加多样性），实现了二者的有效平衡。
- **鲁棒性强：** RaTab对添加的噪声/非信息性特征表现出显著的鲁棒性，性能下降极小，优于其他模型。
- **损失协同增益：** 混合使用重构和对比损失比单独使用任一损失效果更好，证明了双目标优化的有效性。

### 7. 优点
- **方法论创新性强：** 首次将矩阵分解（截断SVD）直接应用于编码器的权重矩阵以实现表示空间的数据增强，提供了全新的设计思路。
- **理论基础扎实：** 方法出发点明确（表格数据缺结构、输入级增强困难），通过截断奇异值来保留核心信息、引入Dropout增加多样性的设计有理论依据。
- **实验评估全面扎实：** 在丰富的基准数据集和任务类型上，与主流方法和多种增强策略进行了细致对比，并通过多种可视化及消融实验深入分析了方法的性质。
- **泛用性好且即插即用：** RaTab仅修改编码器末层，与MLP、Transformer等多种骨干网络架构兼容，易于集成到现有流程中。

### 8. 不足与局限
- **计算开销与大规模数据处理：** 尽管提出了随机化SVD来加速，但对非常大的网络权重矩阵执行SVD仍有一定计算成本，论文未给出极端大规模数据和模型下的具体训练时间与效率对比。
- **超参数搜索范围：** 截断秩 \(k\) 的搜索范围仅为50%-70%，在部分数据集上可能存在更优的值，搜索空间可以更为细致或自适应。
- **领域限制：** 方法针对表格数据设计，论文也明确指出，其在图像、文本等其他非表格领域的有效性有待未来验证。
- **与最新基准的比较：** 虽然对比了众多模型，但表格深度学习领域发展迅速，汇报的性能可能未包含一些实验同期或稍晚出现的更先进方法。

（完）
