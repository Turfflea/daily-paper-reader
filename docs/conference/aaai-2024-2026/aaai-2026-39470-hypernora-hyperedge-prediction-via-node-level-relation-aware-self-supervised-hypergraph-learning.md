---
title: "HyperNoRA: Hyperedge Prediction via Node-Level Relation-Aware Self-Supervised Hypergraph Learning"
title_zh: HyperNoRA：基于节点级关系感知的自监督超图学习用于超边预测
authors: "Ming Li, Zhanle Zhu, Xinyi Li, Lu Bai, Lixin Cui, Feilong Cao, Ke Lv"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39470/43431"
tags: ["query:mt"]
score: 9.0
evidence: 基于对比学习的自监督超图学习用于超边预测
tldr: 针对现有超边预测方法忽视全局结构依赖的问题，提出HyperNoRA自监督框架，通过构建全局节点关系图指导结构感知聚合器，并结合对比学习增强节点表示，在超边预测任务上取得显著提升，其自监督表示学习思想可迁移至管理科学中的关系预测。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39470/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 709, \"height\": 270, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39470/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1613, \"height\": 777, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39470/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1794, \"height\": 635, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39470/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1573, \"height\": 866, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39470/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1572, \"height\": 744, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39470/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 814, \"height\": 230, \"label\": \"Table\"}]"
motivation: 现有超边预测方法仅依赖局部聚合，忽略节点间全局结构依赖。
method: 提出HyperNoRA，构建全局节点关系图引导聚合器，并使用对比学习进行自监督训练。
result: 增强了节点和超边的表示能力，提高了超边预测准确性。
conclusion: 自监督超图学习为复杂关系建模提供了新工具，可用于管理科学中的团队合作等超图分析。
---

## Abstract
Hyperedge prediction plays a critical role in high-order relational modeling with hypergraphs, yet most existing methods primarily focus on sampling strategies or local aggregation within candidate hyperedges. These approaches often overlook global structural dependencies that are essential for learning expressive node and hyperedge representations. In this paper, we propose HyperNoRA, a novel self-supervised hypergraph learning framework that integrates global node-level relation awareness with contrastive learning. Specifically, we construct a global node relation graph that captures both direct and indirect structural correlations, which guides a structure-aware aggregator to enhance node representations with informative global context. To prevent over-smoothing and maintain discriminability, a contrastive learning module is introduced to align representations across graph augmentations while separating semantically dissimilar nodes. Extensive experiments on several benchmark datasets demonstrate that HyperNoRA consistently outperforms state-of-the-art baselines, and ablation studies verify the effectiveness of its key components.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义

- **研究背景与动机**  
  超图能够自然地表示高阶关系（如多个作者共同撰写一篇论文、多种药物组分作为一个处方），因此在药物-靶点交互、合著网络、群组推荐等应用中十分重要。  
  超边预测任务的目标是推断是否应将一个候选超边（一组节点）视为真实存在的高阶关系，它是补全和发现高阶关联的关键环节。

- **现存方法的局限**  
  目前大多数超边预测方法依赖于负采样策略或候选超边内部的局部聚合，忽略超图全局结构依赖。  
  即使像 CASH 等方法引入上下文聚合，仍主要关注局部重要性，缺少对节点间长距离、间接结构关系的建模，导致表示缺乏表达力，尤其在稀疏或噪声超图中表现不佳。

- **本文核心思想**  
  提出 HyperNoRA，一个自监督超图学习框架，通过**构建全局节点关系图**捕获节点间的直接与间接结构关联，指导结构感知聚合器增强节点表示；同时引入**对比学习模块**避免过平滑，保持表示区分性，最终实现更准确、更鲁棒的超边预测。


### 2. 论文提出的方法论

- **整体框架**  
  HyperNoRA 包含三个主要部分：全局节点关系图构建、超图编码与超边预测、自监督对比学习。整体流程为“图构建—编码—预测与对比联合优化”。

- **全局节点关系图构建**
  - 从原始超图 \( \mathcal{H} \) 构建有向加权图 \( \mathcal{G} \)，节点共现于同一超边则连边，权重为共现频次。
  - 对边权重进行去噪过滤（阈值 \( \omega \)），去除弱连接。
  - 使用最短路径算法将边权转换为距离，再转换为相似度分数，得到关系矩阵 \( R \)，保留各节点最重要的 \( k \) 个邻居，形成稀疏全局节点关系图 \( \mathcal{G}_k \)。

- **超图编码**
  - 节点特征先线性投影到 \( d \) 维隐空间。
  - 采用多层的“节点→超边→节点”消息传递（类似 HGNN），使用 PReLU 激活。
  - 最终得到节点嵌入 \( \mathbf{N}^{(l)} \)。

- **超边预测（双通道聚合）**
  - **结构感知聚合器**：对于候选超边 \( e' \) 中的每个节点 \( v_i \)，从全局关系图 \( \mathcal{G} \) 中取 top-\( k \) 相关邻居的平均作为结构增强嵌入，再用注意力机制聚合超边内部节点，经最大池化得到超边表示 \( \mathbf{h}_{e'}^{\text{sp}} \)。
  - **语义感知聚合器**：仅基于超边内节点原始嵌入执行注意力聚合和最大池化，得到 \( \mathbf{h}_{e'}^{\text{se}} \)。
  - **自适应门控融合**：两个表示通过门控机制加权融合，得到最终超边嵌入。
  - 用线性分类器和 Sigmoid 输出有效的预测概率。

- **自监督对比学习**
  - 对原超图执行两次随机节点掩码和特征掩码，得到两个增强视图 \( \mathcal{H}_1, \mathcal{H}_2 \)，分别编码获得节点嵌入 \( \mathbf{Z}_1, \mathbf{Z}_2 \)。
  - 对比损失鼓励同一节点在两个视图中的表示一致，同时将其在全局关系图中的 top-\( k \) 相关邻居视为负例，提升嵌入辨别力。

- **联合训练目标**  
  \( \mathcal{L} = \mathcal{L}_p + \alpha \mathcal{L}_c \)，其中 \( \mathcal{L}_p \) 为正负样本 1:1 采样的二分类交叉熵损失，\( \alpha \) 平衡监督与自监督项。

- **复杂度分析**  
  总体复杂度近似 \( O\left((|\mathcal{H}| + |\mathcal{V}| + d) \cdot d\right) \)，与节点数、超边数及特征维度线性相关；全局关系图构建为一次性预处理。


### 3. 实验设计

- **数据集**  
  四个常用真实超图数据集：  
  - Cora 和 Citeseer（论文共引网络，节点=论文，超边=共引关系）  
  - DBLP（学者合著网络，节点=学者，超边=合著组）  
  - NDC class（药物-物质网络，节点=化学组分，超边=药物配方）  
  节点特征分别为词袋表示（Cora, Citeseer, DBLP）和独热编码（NDC class）。

- **评估设置与指标**  
  - 采用五折独立划分（训练 60% / 验证 20% / 测试 20%）。  
  - 为全面评估，构造四种不同难度的测试集：SNS（基于尺寸的负采样）、MNS（基于模体的负采样）、CNS（基于团的负采样）以及 MIX（前三者混合）。  
  - 主要指标为 AUROC 和 Average Precision（AP）。

- **对比方法**  
  与四个代表性基线对比：HyperSAGNN（自注意力 GNN）、NHP（超边感知池化）、AHP（对抗负采样）、CASH（上下文自监督模型）。


### 4. 资源与算力

- 论文未明确提及训练所使用的 GPU 型号、数量、显存或精确的单次训练时长。  
- 仅给出部分运行时间数据（表 3）：最短路径预处理（秒级）与单轮训练平均耗时（如 DBLP 约 332.6 秒/epoch），并提到在 DBLP 上使用 C++ 重新实现最短路径计算以降低内存（从 1866.2 MB 降至 5.4 MB）和时间（10.7 秒）开销。  
- 总体算力需求约为常规图神经网络级别，未涉及大型集群或特殊硬件。


### 5. 实验数量与充分性

- **实验数量**  
  - 4 个数据集 × 4 种测试集（SNS/MNS/CNS/MIX）× 2 个指标，对 5 种方法（4 基线 + 本文）进行对比，构成约 160 组主要结果。  
  - 消融实验比较了 4 个模型变体（去除结构感知聚合器、去除对比学习、两者均去除、完整模型）在四个数据集上的表现。  
  - 参数敏感性分析对 3 个关键超参数 \( \omega \)、\( s_k \)、\( c_k \) 在两个数据集上测试多个取值。  
  - 计算成本分析提供了预处理与训练时间统计。

- **充分性评估**  
  实验覆盖多种场景、多个数据集和难度层次，与最新基线严格对比，并辅以消融和参数分析，较为全面。所有方法使用相同的公开数据划分和评估协议，保证了客观公平性。


### 6. 论文的主要结论与发现

- HyperNoRA 在所有数据集的平均指标和最具挑战性的混合测试集（MIX）上均取得最优 AUROC 和 AP，整体优于现有方法。  
- 结构感知聚合器对挖掘全局结构依赖至关重要，尤其在 DBLP 和 NDC class 等数据上提升明显。  
- 对比学习模块有效防止表示坍缩，增强嵌入鲁棒性和区分性，移除后性能普遍下降。  
- 参数选择（如关系图过滤阈值、邻居数）显著影响模型表现，适当过滤噪声和选择合理邻居数可带来增益。  
- 该方法在资源受限环境下亦具可行性，C++ 优化可大幅降低预处理内存与时间开销。


### 7. 优点

- **概念创新**：首次在超边预测中通过显式全局节点关系图编码高阶、长距离结构依赖，弥补了现有方法的不足。  
- **双通道设计**：结构感知与语义感知聚合器互补，通过门控自适应融合，兼顾外部结构信号与内部语义交互。  
- **自监督增强**：引入基于全局关系图的对比学习，将相关但非同一节点的邻居作为负例，增强了模型的表示辨别力。  
- **实验全面**：在多个数据集和不同难度的测试条件下评估，验证了方法的鲁棒性；消融与参数分析充分证明了各模块的必要性。  
- **工程可行性**：提供了一步式预处理与线性复杂度分析，并给出 C++ 优化策略以应对大规模数据。


### 8. 不足与局限

- **方法复杂度依赖图构建**：全局关系图构建依赖最短路径计算，尽管可一次性预处理，在大规模超图上仍可能成为瓶颈；论文仅在 DBLP（约数千节点）上测试了效率，无超大规模（百万节点）验证。  
- **超参数敏感性**：性能高度依赖于过滤阈值、邻居数等超参数，需要针对不同数据集调节，缺乏自适应机制。  
- **对比学习负例选择较简单**：仅基于全局关系图的 top-\( k \) 邻居作为负例，可能遗漏其他难分负样本，对比学习策略仍有提升空间。  
- **节点特征依赖**：对节点初始特征质量有一定要求（如使用词袋或独热编码），在特征缺失或极稀疏时效果未给出系统分析。  
- **未探索更丰富的超图类型**：实验仅涉及静态、无向、无权超图，未讨论动态、异质或带属性的超图场景，适用范围有待拓宽。

（完）
