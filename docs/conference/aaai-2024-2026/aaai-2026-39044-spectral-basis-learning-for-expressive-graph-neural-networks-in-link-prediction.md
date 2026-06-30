---
title: Spectral Basis Learning for Expressive Graph Neural Networks in Link Prediction
title_zh: 基于谱基础学习的表达性图神经网络用于链接预测
authors: "Niloofar Azizi, Nils M. Kriege, Nicholas J. A. Harvey, Horst Bischof"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39044/43006"
tags: ["query:mt"]
score: 7.0
evidence: 通过谱图嵌入提升GNN链接预测的表达能力
tldr: 针对GNN在链接预测中表现力有限的问题，提出基于图拉普拉斯特征基的谱基础学习方法。利用可学习Lanczos算法将诱导子图嵌入特征空间，超越一维WL测试的限制。实验表明该方法显著提升链接预测性能，为关系数据建模提供了更强的GNN框架。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39044/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1854, \"height\": 604, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39044/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 875, \"height\": 277, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39044/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 984, \"height\": 208, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39044/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 755, \"height\": 209, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39044/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1745, \"height\": 674, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39044/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 875, \"height\": 480, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39044/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 887, \"height\": 442, \"label\": \"Table\"}]"
motivation: GNN在链接预测中受消息传递限制，表达能力不超过一维WL测试。
method: 引入可学习Lanczos算法，将顶点删除子图或施加诺伊曼特征值约束的子图嵌入到拉普拉斯特征基。
result: 链接预测性能超越传统方法和标准GNN，在同构图区分上能力增强。
conclusion: 提出的谱基础学习框架能有效提升GNN的表示能力，适用于关系推理任务。
---

## Abstract
Graph Neural Networks (GNNs) excel in handling graph-structured data but often underperform in link prediction tasks compared to classical methods, mainly due to the limitations of the commonly used message-passing principle. Notably, their ability to distinguish non-isomorphic graphs is limited by the 1-dimensional Weisfeiler-Lehman test (WL). Our study presents a novel method to enhance the expressivity of GNNs by embedding induced subgraphs into the eigenbasis of the graph Laplacian. We introduce a Learnable Lanczos algorithm with Linear Constraints (LLwLC), proposing two novel subgraph extraction strategies: encoding vertex-deleted subgraphs and applying Neumann eigenvalue constraints. For the former, we demonstrate the ability to distinguish graphs that are indistinguishable by 2-WL, while maintaining efficiency. The latter focuses on link representations enabling differentiation between k-regular graphs and node automorphism, a vital aspect for link prediction tasks. Our approach results in a lightweight architecture, reducing the need for extensive training datasets. Empirically, our method improves performance in challenging link prediction tasks across benchmark datasets, establishing its practical utility and supporting our theoretical findings. Notably, LLwLC achieves 20x and 10x speedups by requiring only 5% and 10% of the data from the PubMed and OGBL-Vessel datasets, while comparing to the state-of-the-art.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **研究动机**：图神经网络（GNN）虽然在图结构数据处理上表现出色，但在链接预测任务中往往不如传统启发式方法。其根本原因在于主流的消息传递范式（MPNNs）的表达能力被严格限制在一维 Weisfeiler‑Leman (1‑WL) 测试水平，无法区分某些重要结构（如 k-正则图），并导致节点自同构问题——两个结构相同的节点获得完全相同的嵌入，从而在链接预测中给出无差别的预测。
- **整体含义**：本文旨在突破 1-WL 的表达瓶颈，提出一种基于谱图理论和图信号处理的新方法。通过将图的诱导子图信息作为线性约束嵌入图拉普拉斯矩阵的特征基中，构建既高效又极具表达力的 GNN 架构，特别适用于链接预测场景。

### 2. 方法论
- **核心思想**：利用带线性约束的 Lanczos 算法（Learnable Lanczos with Linear Constraints, LLwLC）求解一个受约束的特征值问题，从而获得满足特定子图约束的特征基。在这些基上建立可学习的谱滤波器，使 GNN 能够捕获比 MPNNs 更丰富的结构信息。
- **关键技术细节**：
  - **约束特征值问题**：求解 \(\min_{C^\top f=0, f\neq 0} \frac{f^\top L f}{f^\top f}\)，其中 \(L\) 为图拉普拉斯矩阵，\(C\) 为约束矩阵。
  - **LLwLC 算法**（Algorithm 1）：外循环执行标准 Lanczos 迭代，内循环通过 QR 分解或最小二乘求解将当前 Lanczos 向量投影到约束矩阵的零空间，得到正交矩阵 \(Q\) 和三对角矩阵 \(T\)。对 \(T\) 进行特征分解得到 Ritz 值 \(R\) 和特征向量 \(B\)，最终得到满足约束的特征基 \(V = QB\)。
  - **两种子图约束策略**：
    - **诺伊曼（Neumann）特征值约束**：将链接两跳邻域内的边界条件编码入 \(C\)，强制边界上相邻节点的特征差之和为零，从而解决节点自同构问题并增强对 k-正则图的区分能力。
    - **顶点删除子图约束**：每列对应一个顶点删除子图，节点度数填入相应位置，其余为 0。这类约束使模型能够区分 2‑WL 无法分辨的图（如 4×4 车图与 Shirkhande 图）。
  - **LLwLCNet 网络（Algorithm 2）**：多层块堆叠，每块计算 \(X_{i+1} = \sigma(V f_i(R) V^\top X_i W)\)，其中 \(f_i\) 是可学习的 MLP 谱滤波器，\(W\) 为权重矩阵。最后通过全局排序池化和全连接层输出链接概率。
- **理论保证**：证明了在有限精度 QR 分解下，扰动后的 Lanczos 过程对应某个矩阵的精确 Lanczos 过程，并给出了低秩近似的上界，保证了收敛性。

### 3. 实验设计
- **数据集与场景**：在 6 个公开链接预测基准上进行评估：
  - Planetoid 系列：Cora、Citeseer、PubMed
  - OGB 大规模数据集：OGBL‑Collab、OGBL‑Vessel、OGBL‑PPA
- **对比方法**：
  - 启发式方法：CN（共同邻居）、AA（Adamic‑Adar）、RA（资源分配）
  - 标准 GNNs：GCN、GraphSAGE
  - 基于子图或标记的方法：SEAL、NBFNet
  - 成对特征增强方法：Neo‑GNN、BUDDY、NCNC
- **评价指标**：HR@100、HR@50 和 ROC‑AUC。

### 4. 资源与算力
- **硬件环境**：所有实验在单张 **NVIDIA GeForce GT1030 GPU** 上完成，配合 CUDA 11.6 和 PyTorch 1.13。
- **训练配置**：模型训练 20 个 epoch，学习率 0.001，优化器为 Adam。LLwLCNet 层内包含 2 个 32 通道的 MLP，dropout 0.1。特征对数量上限为 10（不足补零）。
- **训练时长**：论文给出了每 epoch 的训练时间（秒），例如在 PubMed 上 SEAL 需 81 s/epoch，BUDDY 需 166 s/epoch，而 LLwLC 在 10%、5% 数据下仅为 8 s 和 7 s/epoch；在 OGBL‑Vessel 上 SEAL 需 12600 s，LLwLC 仅需 1200 s（10%）或 700 s（5%）。整体训练耗时极短。

### 5. 实验数量与充分性
- **实验组数**：
  - 主实验覆盖 6 个数据集，每个方法重复多次并报告均值与标准差（如 Cora 上 LLwLCNet HR@100 为 91.44±0.50）。
  - 消融实验：有无诺伊曼约束对 AUC 的影响（Cora、Citeseer、PubMed）；不同约束策略（纯 LanczosNet、仅诺伊曼约束、10 个顶点删除子图约束）对 HR 的影响（Collab、Cora、PubMed）。
  - 效率与数据量消融：调整训练数据比例（100%、10%、5%、1%）对比 SEAL、BUDDY、GCN 的性能与训练时间。
- **充分性与公平性**：实验设计全面，涵盖小型引文网络与大规模 OGB 数据集，并与主流及 SOTA 方法比较。所有对比基线的结果均取自原论文或标准公开排行榜，评价指标一致，公正性较高。消融实验清晰展示了各新组件的贡献。

### 6. 主要结论与发现
- LLwLC 通过谱基嵌入子图约束，使 GNN 的表达能力超越 1‑WL，能够区分 2‑WL 难以分辨的图，并解决节点自同构问题。
- 提出两种新型子图提取策略（诺伊曼约束与顶点删除子图），二者都能显著提升链接预测性能，且可随机稀疏使用少量约束，降低开销。
- 模型参数量极少（0.018 M–0.038 M），在多数基准上达到最优或次优，且仅需极少训练数据（如 PubMed 上仅用 5% 数据即可超越 SOTA，并实现 20 倍加速）。

### 7. 优点
- **轻量化与高效**：无需昂贵的预处理步骤，单张低端 GPU 即可完成训练，推理复杂度接近线性（\(O(\kappa E + k^2 n + k^3)\)），大幅减少训练时间和数据需求。
- **表达力强**：理论上可超越 2‑WL，实验中在多个数据集上取得了有竞争力的结果，尤其对节点自同构问题有本质改善。
- **方法新颖**：首次将线性约束引入 Lanczos 谱分解以编码子图信息，结合图信号处理与可学习滤波，提供了一种普适的增强 GNN 表达性的范式。
- **实验扎实**：覆盖小规模和大规模数据集，与多种基线对比充分，消融研究清晰，且有理论分析支撑。

### 8. 不足与局限
- **子图约束选择依赖启发式**：诺伊曼约束需定义两跳边界，顶点删除子图的随机选择可能影响稳定性，尚缺乏系统性的子图重要性评估策略。
- **实验场景局限**：仅在链接预测任务上验证，未拓展到节点分类、图分类等任务，方法的普适性有待进一步检验。
- **大规模图上的约束数量限制**：尽管实验表明少量约束即可生效，但对于极度稀疏或超大图，构建约束矩阵及 QR 分解仍可能成为瓶颈，文中未探讨极端规模的扩展方案。
- **硬件对比基础较弱**：实验中使用的 GT1030 为低端显卡，未与现代高端 GPU 或分布式环境下的训练时间进行对比，可能会低估部分基线方法的实际优化潜力。
- **对比方法不够完全**：未包含近期基于 Transformer 的链接预测模型（如 LPFormer）作为对比，仅与 NCNC、BUDDY 等比较。

（完）
