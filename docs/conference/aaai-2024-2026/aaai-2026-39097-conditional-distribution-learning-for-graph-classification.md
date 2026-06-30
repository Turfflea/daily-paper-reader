---
title: Conditional Distribution Learning for Graph Classification
title_zh: 面向图分类的条件分布学习
authors: "Jie Chen, Hua Mao, Chuanbin Liu, Zhu Wang, Xi Peng"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39097/43059"
tags: ["query:mt"]
score: 8.0
evidence: 通过条件分布学习解决图分类中消息传递与对比学习冲突
tldr: 为解决图神经网络中消息传递导致节点嵌入趋同与对比学习要求增大差异之间的冲突，提出条件分布学习方法，在保持语义的同时学习判别性图表征；在多个图分类数据集上取得先进性能，为图表示学习提供了一种协调消息传递与对比学习的新视角。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39097/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1289, \"height\": 514, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39097/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1637, \"height\": 380, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39097/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1733, \"height\": 996, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39097/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1778, \"height\": 458, \"label\": \"Table\"}]"
motivation: GNN层间嵌入相似性增加与对比学习期望负样本间高差异性之间存在根本冲突。
method: 提出端到端的条件分布学习框架，通过条件分布对齐不同的图增强视图的表示，兼顾语义一致性与判别性。
result: 在多个基准图分类任务中，分类准确率优于现有图对比学习方法。
conclusion: CDL为图表示学习提供了一种协调消息传递与对比学习的新视角。
---

## Abstract
Leveraging the diversity and quantity of data provided by various graph-structured data augmentations while preserving intrinsic semantic information is challenging. Additionally, successive layers in graph neural network (GNN) tend to produce more similar node embeddings, while graph contrastive learning aims to increase the dissimilarity between negative pairs of node embeddings. This inevitably results in a conflict between the message-passing mechanism (MPM) of GNNs and the contrastive learning (CL) of negative pairs via intraviews. In this paper, we propose a conditional distribution learning (CDL) method that learns graph representations from graph-structured data for semisupervised graph classification. Specifically, we present an end-to-end graph representation learning model to align the conditional distributions of weakly and strongly augmented features over the original features. This alignment enables the CDL model to effectively preserve intrinsic semantic information when both weak and strong augmentations are applied to graph-structured data. To avoid the conflict between the MPM and the CL of negative pairs, positive pairs of node representations are retained for measuring the similarity between the original features and the corresponding weakly augmented features. Extensive experiments with several benchmark graph datasets demonstrate the effectiveness of the proposed CDL method.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
*   **研究动机**：在半监督图分类任务中，图数据增强（如边扰动、属性掩码）能提升模型泛化能力，但不同强度的增强容易破坏图本身的固有语义信息。同时，图神经网络（GNN）的消息传递机制会使深层节点嵌入趋于相似，而图对比学习（GCL）却要求增大负样本对之间的差异，二者存在根本性冲突。
*   **整体含义**：该论文提出条件分布学习（CDL）方法，旨在解决上述两大挑战：通过对齐弱增强与强增强视图在原始特征上的条件分布，在保留语义信息的同时利用数据增强的多样性；并通过仅使用正样本对进行相似性度量来规避消息传递与负样本对比学习的冲突，从而提升半监督图分类的性能。

### 2. 论文提出的方法论
*   **核心思想**：构建一个端到端的图表征学习模型，强制弱增强和强增强的节点嵌入在给定原始节点嵌入时的条件分布保持一致。弱增强分布作为监督信号，引导强增强分布的学习，减少强增强对语义信息的破坏。同时，抛弃传统的负样本对比，仅用原始视图与弱增强视图之间的正样本对来计算相似性损失。
*   **关键技术细节与公式**：
    *   **图增强**：定义弱增强为轻微扰动（如低比例属性掩码），强增强为显著扰动（如高比例掩码）。
    *   **模型架构**：包含共享GNN编码器（GCN + 全局池化 + MLP）、投影头模块（二层MLP）和条件分布构建模块。从原始图、弱增强图、强增强图中分别得到图级表示H、Hw、Hs，以及投影表示P、Pw。
    *   **条件分布构建**：对于节点i，其弱增强嵌入hwi在给定原始嵌入hi时的条件分布p(hwi|hi)定义为：
        p(hwi|hi) = exp( sim(hwi, hi)/τ ) / [ exp( sim(hwi, hi)/τ ) + Σ_{k=1}^{K} exp( sim(hwk, hi)/τ ) ]，其中K为负采样数，sim为余弦相似度。强增强分布p(hsi|hi)同理定义。
    *   **损失函数与训练流程**：
        1.  **预训练阶段**（仅用无标签图）：最小化相似性损失Ls，仅涉及原始图投影P与弱增强投影Pw之间的正样本对，公式为负对数似然，分母为当前正样本对与所有其他图的投影对得分总和之比。这本质是最大化正样本互信息的下界。
        2.  **微调阶段**（用少量标签图）：总体损失L = Lc（交叉熵分类损失） + αLs + βLd。其中Ld为分布散度损失，用于对齐两个条件分布：Ld = -(1/ng) Σ p(hwi|hi) log(p(hsi|hi))。α, β为超参数。
    *   **理论支撑**：证明了Ls是互信息的下界；定理1保证当负样本数K足够大时，分布散度Ld存在下界，优化更具稳定性。

### 3. 实验设计
*   **数据集**：使用来自TUDataset的8个标准图分类数据集：MUTAG、PROTEINS、IMDB-B、NCI1、RDT-B、RDT-M5K、COLLAB、GITHUB。
*   **对比方法**：与多种前沿方法对比，包括基础GNN（GCN、GAT）和图对比学习/增强方法（GCL、GLIA、G-Mixup、GCMAE、GRDL）。
*   **评估设置**：采用10折交叉验证，分别在30%、50%、70%的标签比例下测试半监督分类准确率，以平均准确率和标准差为指标。

### 4. 资源与算力
*   论文正文及提供的材料中，**未明确说明**所使用的GPU型号、数量、训练时长或任何具体的算力消耗信息。

### 5. 实验数量与充分性
*   **主要实验**：在8个数据集 × 3种标签比例 = 24个不同场景下，与7种方法（含本方法）进行了全面对比，结果如表1所示。
*   **消融实验**：对CDL的三个核心损失组件（Lc、Ls、Ld）进行消融，设立“仅分类损失(Lc)”、“分类+正样本对比(Lc+Ls)”、“完整CDL”三种变体，在全部数据集上进行了验证，证明了预训练和条件分布损失的必要性。
*   **参数敏感性分析**：对弱增强的节点掩码比例（0.1, 0.2, 0.3, 0.35）进行了实验分析，考察其对分类性能的影响。
*   **实验充分性与公平性**：实验覆盖了不同规模、不同类型的数据集，对比方法均为同期先进工作，实验设置遵循同类研究惯例，整体较为充分、客观且公平。

### 6. 论文的主要结论与发现
*   CDL方法在所有8个数据集和3种标签比例下，分类准确率均一致地优于所有对比方法，尤其是在标签比例较低时优势更加明显。
*   通过条件分布对齐弱/强增强视图，有效保留了图的内在语义信息，使强增强能够安全地用于提升模型鲁棒性和泛化能力。
*   提出的仅正样本对比损失成功规避了GNN消息传递与GCL负样本排斥之间的潜在冲突，验证了该设计思路的有效性。
*   整体预训练+微调的半监督学习方案具有良好的协同作用，预训练阶段保留的语义信息为微调阶段的分布对齐提供了可靠监督。

### 7. 优点
*   **问题洞察深刻**：准确指出了消息传递平滑性与对比学习区分性之间的根本冲突，并提出针对性的解决方案。
*   **方法论创新**：将条件分布对齐引入图增强学习，为协调不同强度的数据增强提供了新视角，理论上有下界保证。
*   **设计精巧**：仅使用正样本对进行对比学习，避免了复杂的负样本选择问题，同时解决冲突。
*   **实验扎实**：在多个标准数据集上进行了充分的定性和定量分析，消融实验和参数分析完善，结果有说服力。

### 8. 不足与局限
*   **增强策略单一**：实验仅采用属性掩码作为弱/强增强，未探索其他类型的图数据增强（如边扰动、子图采样等）在框架下的表现。
*   **超参数敏感性**：损失函数含有α、β、温度系数τ以及掩码比例等超参数，论文虽分析了掩码比，但对其他参数的敏感性讨论不足，实际调优可能较复杂。
*   **计算开销未讨论**：训练过程需处理原始、弱增强、强增强三份数据，并计算条件分布，其额外的内存和计算代价未与对比方法进行对比分析。
*   **适用范围限制**：论文聚焦于半监督图分类，该方法在节点分类或全监督图分类等任务上的扩展性和有效性有待进一步验证。
*   **大规模图应用**：部分对比方法（GCMAE、GRDL）出现了因内存不足无法运行的情况，CDL自身在大规模图数据集下的可扩展性也未得到体现。

（完）
