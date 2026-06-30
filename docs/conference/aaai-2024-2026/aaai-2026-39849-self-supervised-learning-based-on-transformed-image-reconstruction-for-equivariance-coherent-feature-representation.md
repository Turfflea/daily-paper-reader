---
title: Self-Supervised Learning Based on Transformed Image Reconstruction for Equivariance-Coherent Feature Representation
title_zh: 基于变换图像重建的自监督学习用于等变相干特征表示
authors: "Qin Wang, Alessio Quercia, Benjamin Bruns, Abigail Morrison, Hanno Scharr, Kai Krajsek"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39849/43810"
tags: ["query:mt"]
score: 9.0
evidence: 基于变换图像重建的自监督学习用于等变表示
tldr: 针对现有自监督学习丢弃变换信息导致无法用于需要等变特征的任务的问题，提出一种基于中间变换重建的自监督辅助任务，学习等变相干表示，在图像分类和变换相关任务上取得更好效果，其等变学习思想可启发性地用于管理科学中的数据表征。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39849/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1674, \"height\": 615, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39849/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 758, \"height\": 383, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39849/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 786, \"height\": 630, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39849/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 747, \"height\": 594, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39849/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 754, \"height\": 595, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39849/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1639, \"height\": 504, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39849/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 833, \"height\": 326, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39849/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 786, \"height\": 286, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39849/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 804, \"height\": 396, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39849/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1642, \"height\": 210, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39849/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1755, \"height\": 250, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39849/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 874, \"height\": 244, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39849/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 822, \"height\": 393, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39849/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 877, \"height\": 213, \"label\": \"Table\"}]"
motivation: 现有自监督方法学习不变特征，丢弃了变换信息，限制了在需要等变特征任务上的应用。
method: 提出新的自监督辅助任务，通过学习重建中间变换来获得等变相干表示。
result: 方法在保持分类精度的同时，增强了变换信息的保留。
conclusion: 等变表示学习为自监督学习提供了新视角，可在管理科学的时间序列预测等任务中借鉴。
---

## Abstract
Self-supervised learning (SSL) methods have achieved remarkable success in learning image representations allowing invariances in them — but therefore discarding transformation information that some computer vision tasks actually require. While recent approaches attempt to address this limitation by learning equivariant features using linear operators in feature space, they impose restrictive assumptions that constrain flexibility and generalization.
We introduce a weaker definition for the transformation relation between image and feature space denoted as equivariance-coherence. We propose a novel SSL auxillary task that learns equivariance-coherent representations through intermediate transformation reconstruction, which can be integrated with existing joint embedding SSL methods. Our key idea is to reconstruct images at intermediate points along transformation paths, e.g. when training on 30° rotations, we reconstruct the 10° and 20° rotation states. Reconstructing intermediate states requires the transformation information used in augmentations, rather than suppressing it, and therefore fosters features containing the augmented transformation information. Our method decomposes feature vectors into invariant and equivariant parts, training them with standard SSL losses and reconstruction losses, respectively. We demonstrate substantial improvements on synthetic equivariance benchmarks while maintaining competitive performance on downstream tasks requiring invariant representations. The approach seamlessly integrates with existing SSL methods (iBOT, DINOv2) and consistently enhances performance across diverse tasks, including segmentation, detection, depth estimation, and video dense prediction. Our framework provides a practical way for augmenting SSL methods with equivariant capabilities while preserving invariant performance.

---

## 论文详细总结（自动生成）

好的，我将以资深学术论文分析助手的身份，使用中文、以Markdown格式，对您提供的论文进行结构化、深入、客观的总结。

### **论文核心问题与整体含义**

*   **研究动机与背景**:
    *   当前主流的自监督学习（SSL）方法，特别是基于联合嵌入的方法（如 iBOT， DINOv2），其核心任务是学习对输入变换保持不变的图像特征。这虽然对分类任务有益，但却丢弃了变换信息。
    *   许多计算机视觉任务（如物体检测、分割、姿态估计）实际上需要**等变特征**，即特征应随输入变换而发生可预测的变化，从而保留空间和几何信息。
    *   现有学习等变特征的方法（如 SIE）通常在潜在空间中对变换进行线性建模，这施加了严格的架构约束，并假设等变映射是线性的，限制了灵活性和泛化能力。
*   **核心问题**: 如何在保持SSL方法学习不变特征能力的同时，有效学习并保留变换信息，即获得**等变相干表征**，且无需对特征空间的变换关系做出严格的线性假设。
*   **整体含义**: 本文提出了一种更通用、更实用的方法来增强自监督学习，使其能够同时学习不变和等变特征，从而在更广泛的、需要不同特征类型的下游任务上实现更优性能，弥合了现有方法在不变性与等变性需求之间的鸿沟。

### **论文提出的方法论**

*   **核心思想**:
    *   引入一种新的辅助任务：**中间变换图像重建**。
    *   不直接学习特征空间的等变映射，而是通过要求模型重建变换路径上中间状态的图像，强制特征编码器保留变换过程中的关键信息，从而隐式地学习等变相干特征。
*   **关键技术细节**:
    *   **特征分解**: 将编码器输出的特征向量 `z` 沿特征维度拆分为两部分：
        *   **不变特征** (`z_inv`): 使用标准的SSL损失（如 iBOT、VICReg损失）进行监督，以学习对数据增强不变的特征。
        *   **等变特征** (`z_equi`): 用于重建中间变换图像，以学习对变换敏感的特征。
    *   **中间变换生成**: 对一个输入视图，除了应用基础数据增强外，还应用一系列等距参数的连续变换（如旋转 30°），生成变换路径。路径的终点作为第二个输入视图（`v2`），而路径上的中间状态图像（如旋转 10° 和 20°）作为重建目标 `u_k`。
    *   **重建过程**:
        1.  将两个输入视图 `v1` 和 `v2` 的等变特征 `z_equi_1` 和 `z_equi_2` 拼接起来。
        2.  使用 K 个独立的、轻量级的解码器 `D_k`，每个解码器负责从拼接后的等变特征中重建一个特定的中间状态图像 `û_k`。
    *   **损失函数**:
        *   `L_total = L_SSL(z_inv_1, z_inv_2) + λ * L_recon`
        *   其中 `L_SSL` 是基线方法（如 VICReg, iBOT）的不变损失，`L_recon = (1/K) * Σ ||û_k - u_k||²₂` 是重建损失，为均方误差（MSE）。
*   **算法流程**:
    1.  对输入图像 `I` 应用两组基础增强，得到 `v1` 和 `u`。
    2.  对 `u` 应用一系列变换（`g_θ1`, `g_θ2`, ..., `g_θ`），得到中间目标 `u_k` 和第二视图 `v2`。
    3.  `v1` 和 `v2` 通过共享权重或指数移动平均（EMA）的编码器 `f`，得到特征 `z1`, `z2`。
    4.  拆分特征得到 `z_inv` 和 `z_equi`。
    5.  使用 `z_inv` 计算 `L_SSL`。
    6.  拼接 `z_equi_1`, `z_equi_2`，输入各解码器 `D_k` 得到重建图像 `û_k`。
    7.  计算重建损失 `L_recon`，并与 `L_recon` 结合进行反向传播。

### **实验设计**

*   **合成等变基准测试 (Synthetic Equivariance Benchmarks)**:
    *   **任务**: 评估模型特征对特定变换（旋转、颜色抖动、模糊、平移）参数的可预测性。
    *   **设计**: 对原始和变换后图像提取特征，再用一个轻量级多层感知机（MLP）回归其变换参数。
    *   **指标**: 决定系数 R²，衡量预测参数与真实参数的对齐程度。
    *   **对比方法**: SIE（分不变-等变框架）及其变体，基于交叉注意力的重建方法。
*   **真实图像下游任务 (Natural Image Tasks)**:
    *   **不变性相关任务**:
        *   **线性分类**: 在 CIFAR10/100， Aircraft， Food 等 7 个分类数据集上，固定预训练骨干网络，微调线性头。
        *   **指标**: Top-1 准确率。
    *   **等变性相关任务（密集预测）**:
        *   **语义分割**: ADE20K 数据集，UPerNet 解码器，指标为平均交并比（mIoU）。
        *   **目标检测与实例分割**: COCO 数据集，Mask R-CNN 模型，指标为 APᵇ， APᵐ 等。
        *   **关键点检测**: MPII 数据集，指标为 PCKh。
        *   **单应性估计**: S-COCO 数据集，指标为平均角点误差（MCE）。
        *   **单目深度估计**: NYU 和 KITTI 数据集，指标为均方根误差（RMSE）和绝对相对误差（AbsRel）。
        *   **视频任务**: DAVIS 2017（视频目标分割）和 VIP（语义部位传播）数据集。
*   **对比方法**:
    *   **主要基线**: iBOT， DINOv2， SIE（使用 VICReg 损失）。
    *   **无/少增强方法比较**: MAE， I-JEPA。
    *   **本文方法变体**: `Ours(base_SSL_method, augmentation_pipeline, reconstruction_target)`，如 `Ours(iBOT, all, SE(2))`。

### **资源与算力**

*   **硬件**: 实验在 JUWELS Booster 超级计算机上进行。
*   **辛烷值**: 对于基于 VICReg 的方法，使用 4 个节点共 **16 块 A100 GPU**。对于基于 iBOT 和 DINOv2 的方法，使用 16 个节点共 **64 块 A100 GPU**。
*   **训练效率**: 提出的辅助任务带来了轻微的计算开销。
    *   在 ViT-S/16 的 VICReg 训练上，每轮时间增加 **14.7%**。
    *   在 ViT-L/16 的 iBOT 训练上，每轮时间增加 **8.3%**。
    *   在 ViT-L/16 的 DINOv2 训练上，每轮时间增加 **7.2%**。

### **实验数量与充分性**

*   **实验数量**: 实验数量庞大且覆盖全面。
    *   **合成基准测试**: 包含与 SIE 等多组配置（旋转、颜色、模糊、平移、SE(2)和所有组合）的详细对比（表1），以及对 iBOT， DINOv2， SimCLR 增强的测试（表4）。
    *   **真实图像任务**: 在 **7 个分类数据集**（表5）和 **8 个密集预测基准**（表6，表7）上进行了评估。
    *   **对比分析**: 与主流 SSL 范式（联合嵌入、掩码重建）进行了系统对比（图 3，表 8）。
    *   **消融实验**: 对关键超参数 `λ`（损失权重）和 `d_equi`（等变特征维度）进行了网格搜索（图 4，图 5），对中间重建图像数量 `K` 进行了分析（表9）。
*   **充分性**: **非常充分**。实验设计不仅验证了方法在目标等变任务上的优势，还严格检验了其在标准不变性任务上性能的保持，并通过全面的消融实验提供了参数选择指导。
*   **公平性**: 实验设置 **公平客观**。与基线方法使用相同的骨干网络（ViT-S/16， ViT-L/16）、预训练数据集（ImageNet-1K）和标准评估协议。对于 DINOv2 等基线，为保证公平，部分模型是从头预训练的，而非直接使用官方权重。

### **主要结论与发现**

*   提出的中间变换重建任务能够高效地学习**等变相干特征**，其在合成等变基准测试上的性能全面且显著超越了基于线性假设的 SIE 方法及交叉注意力重建方法。
*   该方法能**无缝集成**到现有的 SSL 框架（如 iBOT， DINOv2）中，并**一致地提升**它们在需要等变特征的下游任务（如单应性估计、深度估计、视频任务）上的性能。
*   在增强等变能力的同时，该方法**保持了甚至略微提升了**在需要不变特征的标准分类任务上的性能，实现了“鱼与熊掌兼得”的效果，优于在不变性上表现不佳的纯掩码重建方法（如 MAE）。
*   该方法通过消除对特征空间内线性等变映射的假设，提供了**更强的灵活性和泛化能力**，尤其适用于复杂或非刚性变换。

### **优点**

*   **概念新颖简洁**: “中间变换重建”的想法直观且有效，避免了复杂的特征空间变换建模。
*   **弱假设与强泛化**: 只要求“等变相干”，不要求变换在特征空间满足线性或群结构，因此更通用。
*   **即插即用**: 方法作为一个轻量级辅助任务，可以很容易地与多种现有 SSL 框架结合，实用性强。
*   **性能均衡**: 是少数能在不变性和等变性任务上同时取得优异性能的方法之一，解决了 SSL 领域的一个关键权衡问题。
*   **实验扎实**: 实验设计极为全面，覆盖合成、分类、密集预测（2D、3D、视频）等多种任务，并包含充分的消融研究和公平对比。

### **不足与局限**

*   **变换依赖的先验知识**: 方法需要预先定义要学习的变换类型（如旋转、平移）。虽然对未见过的变换有泛化能力，但训练时的变换集构成了一个重要的先验。
*   **超参数敏感性**: 等变特征维度 `d_equi` 和损失权重 `λ` 是重要的超参数，其最优值取决于下游任务（图4，5所示），实际应用中可能需要调整。
*   **大模型与大数据上的探索有限**: 论文主要基于 ViT-S/L 和 ImageNet-1K 进行实验，虽然与 ViT-H/G 或 LVD-142M 蒸馏的方法进行了对比，但未直接展示该方法在更大规模数据和模型上（如从头预训练 ViT-G）的表现潜力。
*   **重建解码器简单**: 使用了轻量级解码器，其保真度有限，可能限制了对更细致变换信息的学习。

（完）
