---
title: Non-parametric Representation Learning with Kernels
title_zh: 基于核方法的非参数表示学习
authors: "Pascal Esser, Maximilian Fleissner, Debarghya Ghoshdastidar"
date: 2024-03-25
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/29077/30039"
tags: ["query:mt"]
score: 10.0
evidence: 基于核方法的自监督与无监督表示学习
tldr: 针对表示学习领域核方法研究不足的问题，提出基于核的自监督学习和自动编码器模型，并建立了相应的表示定理，证明核方法同样能有效学习无监督表征，扩展了表示学习的工具集。
source: AAAI-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29077/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 817, \"height\": 577, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29077/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 807, \"height\": 240, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29077/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 554, \"height\": 220, \"label\": \"Figure\"}]"
motivation: 现有表示学习大多基于神经网络，核方法在此领域尚未被充分探索。
method: 定义了两个核对比自监督模型和一个核自动编码器，并推导了表示学习下的新表示定理。
result: 实验证明核表示学习模型能有效提取特征，与神经网络方法形成互补。
conclusion: 为无监督表示学习提供了灵活的非参数化替代方案，具有理论和实践价值。
---

## Abstract
Unsupervised and self-supervised representation learning has become popular in recent years for learning useful features from unlabelled data. Representation learning has been mostly developed in the neural network literature, and other models for representation learning are surprisingly unexplored. In this work, we introduce and analyze several kernel-based representation learning approaches: Firstly, we define two kernel Self-Supervised Learning (SSL) models using contrastive loss functions and secondly, a Kernel Autoencoder (AE) model based on the idea of embedding and reconstructing data. We argue that the classical representer theorems for supervised kernel machines are not always applicable for (self-supervised) representation learning, and present new representer theorems, which  show that the representations learned by our kernel models can be expressed in terms of kernel matrices. We further derive generalisation error bounds for representation learning with kernel SSL and AE, and empirically evaluate the performance of these methods in both small data regimes as well as in comparison with neural network based models.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **研究背景**：无监督与自监督表示学习（Representation Learning）主要通过神经网络实现，核方法（Kernel Methods）在此领域几乎未被探索。
- **核心问题**：能否利用核方法构建非参数化的表示学习模型，填补核方法在表示学习中的空白？
- **整体含义**：本文提出并分析多种基于核的表示学习方法（对比自监督学习和自编码器），证明核方法同样可用于学习高质量的无监督表示，并将理论工具（表示定理、泛化界）推广至该范式，为小数据场景和理论分析提供新工具。

### 2. 论文提出的方法论
- **核心思想**：将神经网络中常见的表示学习范式（对比损失、重建损失）移植到核方法框架，利用正定核隐式地进行非线性映射，并借助再生核希尔伯特空间（RKHS）的范数或正交约束正则化。
- **关键技术细节**：
  - **新的表示定理**：针对表示学习中常见的正交约束（\(W^*W = I_h\)），给出了一个必要且充分的条件，确保优化问题的最优解仍落在数据张成的有限维子空间内（定理2），从而将无限维问题转化为有限维核矩阵优化。
  - **核自监督学习（对比损失）**：
    - **简单对比损失（Definition 1）**：目标为使锚点与正样本的对齐度高于与负样本的对齐度，并施加正交约束。该问题可转化为一个核矩阵的广义特征值问题，最终表示为闭式解（定理3）。
    - **谱对比损失（Definition 2）**：采用 HaoChen 等人提出的损失，加入 RKHS 范数惩罚。通过引入嵌入矩阵 \(Z \in \mathbb{R}^{h \times 3n}\)，可将优化问题表述为仅依赖核矩阵及其梯度的形式（定理4），从而能用梯度下降求解。
  - **核自编码器（Kernel AE）**（Definition 3）：
    - 编码器：\(W_1^T \phi_1(x)\) 将数据映射到 h 维瓶颈层。
    - 解码器：\(W_2^T \phi_2(z)\) 将瓶颈层重构回原始空间。
    - 损失函数同时包含重构误差、编码器和解码器的 RKHS 范数惩罚，以及瓶颈层归一化约束（\(\|W_1^T \phi(x_i)\|^2 = 1\)），以防止平凡解。
    - 对通用核，优化可转化为对嵌入矩阵 \(Z\) 和重构的核矩阵形式优化（定理5）。
  - **泛化误差界**：为核对比学习和核自编码器分别推导了基于 Rademacher 复杂度的泛化界（定理6、7），表明模型性能随未标注数据量增加而提升；同时还将无监督损失与下游监督任务误差相联结。
- **公式/算法流程**（文字化）：对于每种核方法，首先根据定理1或定理2将问题限制到数据张成的有限维空间，然后通过核矩阵运算得到闭式解或可微损失，再利用核矩阵分解或梯度下降求解最优嵌入；对新样本，利用核函数与训练核矩阵进行插值得到表示。

### 3. 实验设计
- **数据集与场景**：
  - **小数据分类**：concentric circles、cubes、Iris、Ionosphere（实验设定为 50% 未标注、5% 标注、45% 测试；嵌入维度 h=2）。
  - **图像分类**：MNIST（前两类，n=500）、CIFAR-10。
  - **图像去噪**：MNIST（前五类，n=225）、CIFAR-10、SVHN，噪声为 \(\mathcal{N}(0,0.1)\) 添加到输入。
- **基准与对比方法**：
  - 核方法内部对比：线性核、1层ReLU核、高斯核、拉普拉斯核；
  - 与核主成分分析（Kernel PCA）对比；
  - 与基于单隐藏层神经网络（NN）的表示学习对比（对应损失函数相同，激活函数为 ReLU 等）；
  - 下游任务：固定使用 3-最近邻（k-nn）分类器，在嵌入上评估。
- **评价指标**：分类准确率（多次随机分割的均值和标准差）；去噪效果用测试集均方误差（MSE）。

### 4. 资源与算力
- 论文未明确提及 GPU 型号、数量或训练时长。由于核方法主要涉及矩阵分解与梯度下降，且实验数据量较小（如最大 n=500），所需算力应不大，但文中未给出具体描述。

### 5. 实验数量与充分性
- **实验组数**：
  - 四组小数据分类实验（图1），四种数据集，每种对比了原始特征、KPCA、三种核 SSL 方法，并测试四种核函数；
  - 两组图像分类对比实验（图2），MNIST 与 CIFAR，对比核 SSL 与神经网络 SSL；
  - 三组图像去噪实验（图3），MNIST、CIFAR、SVHN，对比核 AE 与神经网络 AE。
  - 每种实验均涉及超参数（如带宽）的网格搜索和交叉验证。
- **充分性评估**：
  - 实验覆盖了小数据分类、图像分类和去噪，多类数据集，多类核函数，并与常见的神经网络和 KPCA 进行了比较，可初步验证方法的有效性。
  - 但实验规模较小（样本数在 150–500 范围），未见大规模数据集（如完整 MNIST、ImageNet）的对比，且未深入分析不同超参数（如正则化系数 \(\lambda\)）对性能的影响，缺少消融实验以证明各组件（如归一化约束、正交约束）的作用。

### 6. 论文的主要结论与发现
- 核方法可以成功实现自监督对比学习和无监督自编码器表示学习，填补了该领域的空白。
- 新的表示定理确保了核表示学习问题可归结为有限维核矩阵优化，与经典监督核方法类似。
- 理论上给出了泛化误差界，表明增加未标注数据能提升模型性能。
- 实验表明，在小数据场景下，核表示学习方法能优于或持平原始特征的 k-nn 分类、Kernel PCA，并且与神经网络表示学习相比具有竞争力；在图像去噪上，核自编码器的重构误差甚至低于神经网络自编码器。

### 7. 优点
- **理论创新**：将表示定理推广至带正交约束和范数惩罚的表示学习场景，为核表示学习奠定了理论基础。
- **方法多样性**：覆盖了两种主流的对比损失和一个自编码器模型，体现了核框架的灵活性。
- **闭式解与可解释性**：部分模型有闭式解，所有模型均可归结为核矩阵运算，利于理论分析和解释。
- **非参数优势**：无需预设网络结构深度或宽度，核方法天然适合小数据、高可解释性需求场景。
- **实验对比全面**：与神经网络直接对比，凸显核方法在小样本情况下的竞争力。

### 8. 不足与局限
- **可扩展性**：核矩阵计算复杂度为 \(O(n^2)\) 或 \(O(n^3)\)，未讨论随机傅里叶特征等近似方法，难以直接应用于大规模数据。
- **实验局限性**：
  - 仅在极小型数据集上验证（最大 n=500），缺乏在中等规模或标准基准上的测试，泛化能力存疑。
  - 缺少消融实验以验证正则项、归一化约束等设计的必要性。
  - 未与更先进的深度表示学习方法（如 SimCLR, BYOL 等）进行对比，仅与简单的单隐藏层网络比较。
- **理论假设**：需要核为通用核才能将损失转化为嵌入矩阵优化，限制了核类型的选择；部分定理依赖于“损失在正交补上消失”等条件。
- **应用限制**：核自编码器的重构需要预定义良好的核才能得到语义上合理的输出，实际应用中核的选择和调参可能比设计神经网络更依赖经验。
- **与深度学习极限联系的未明确性**：虽指出核 SSL 可能与无限宽网络对应，但未给出证明，且核 AE 在瓶颈层存在时与 NTK 的对应关系尚不明确。

（完）
