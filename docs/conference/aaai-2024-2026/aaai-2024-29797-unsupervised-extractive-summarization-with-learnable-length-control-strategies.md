---
title: Unsupervised Extractive Summarization with Learnable Length Control Strategies
title_zh: 具有可学习长度控制策略的无监督抽取式摘要
authors: "Renlong Jie, Xiaojun Meng, Xin Jiang, Qun Liu"
date: 2024-03-25
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/29797/31378"
tags: ["query:mt"]
score: 8.0
evidence: 具有可学习长度控制的无监督抽取式摘要
tldr: 现有无监督摘要方法依赖图排序且无法端到端训练，同时缺乏有效的长度可控提取器。本文提出一种新颖的无监督抽取式摘要框架，引入可学习的长度控制策略，结合句子中心性图排序进行端到端训练。实验表明，该方法在多个数据集上达到有竞争力的摘要质量，且能灵活控制摘要长度，为无监督摘要研究提供了新范式。
source: AAAI-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29797/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1482, \"height\": 770, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29797/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 698, \"height\": 698, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29797/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 880, \"height\": 447, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29797/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1578, \"height\": 696, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29797/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 884, \"height\": 354, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29797/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 872, \"height\": 676, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29797/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 871, \"height\": 678, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29797/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 687, \"height\": 212, \"label\": \"Table\"}]"
motivation: 现有无监督摘要方法依赖图排序且无法端到端训练，同时缺乏有效的长度可控提取器。
method: 引入可学习的长度控制策略，结合句子中心性图排序进行端到端训练。
result: 在多个数据集上达到有竞争力的摘要质量，且能灵活控制摘要长度。
conclusion: 提出了一种可端到端训练的无监督摘要框架，兼具高质量与长度可控性。
---

## Abstract
Unsupervised extractive summarization is an important technique in information extraction and retrieval. Compared with supervised method, it does not require high-quality human-labelled summaries for training and thus can be easily applied for documents with different types, domains or languages. Most of existing unsupervised methods including TextRank and PACSUM rely on graph-based ranking on sentence centrality. However, this scorer can not be directly applied in end-to-end training, and the positional-related prior assumption is often needed for achieving good summaries. In addition, less attention is paid to length-controllable extractor, where users can decide to summarize texts under particular length constraint. This paper introduces an unsupervised extractive summarization model based on a siamese network, for which we develop a trainable bidirectional prediction objective between the selected summary and the original document. Different from the centrality-based ranking methods, our extractive scorer can be trained in an end-to-end manner, with no other requirement of positional assumption. In addition, we introduce a differentiable length control module by approximating 0-1 knapsack solver for end-to-end length-controllable extracting. Experiments show that our unsupervised method largely outperforms the centrality-based baseline using a same sentence encoder. In terms of length control ability, via our trainable knapsack module, the performance consistently outperforms the strong baseline without utilizing end-to-end training. Human evaluation further evidences that our method performs the best among baselines in terms of relevance and consistency.

---

## 论文详细总结（自动生成）

好的，这是对给定论文的结构化深度总结。

### 1. 论文的核心问题与整体含义

*   **核心问题**：现有的无监督抽取式文档摘要方法面临两大挑战。第一，主流方法（如 TextRank, PACSUM）依赖基于图排序的句子中心度（Centrality），这类方法通常**不可微分**，无法进行端到端的优化训练，且往往需要引入位置相关的先验假设才能获得良好性能。第二，学术界对**长度可控的抽取式摘要**问题关注不足，如何在严格的长度约束下优化句子选择，是一个未被充分解决的组合优化难题。
*   **整体含义**：本文旨在提出一种全新的无监督抽取式摘要框架。该框架的核心思想是用一个**可端到端训练的双向预测目标**替代传统的、不可微分的中心度计算，从而实现无监督学习。同时，将长度可控的摘要问题建模为经典的 0-1 背包问题，并引入一个**可微分的神经网络模块（背包Transformer）**来逼近其求解器，使得整个长度控制策略也能进行端到端学习。

### 2. 论文提出的方法论

*   **核心架构：基于 SimSiam 的孪生网络**
    模型整体采用类似 SimSiam 的架构，包含一个句子抽取模块和一个对比学习模块。
    *   **句子抽取模块**：由一个预训练的句子编码器（如 SimCSE）和一个可训练的 Transformer 评分器（Scorer）组成。编码器负责生成句子的初级表示，评分器则为其分配重要性分数。
    *   **对比学习模块**：包含另一个 Transformer 编码器（预测器），用于生成句子的次级表示，并服务于下文所述的双向预测任务。

*   **关键技术：双向预测学习目标**
    替代传统的中心度求和，论文提出两个新的训练目标，旨在通过提高预测准确性来学习如何抽取重要句子。
    1.  **Rest-Ext 目标**：提升从文档的剩余句子（`H_S'`，去掉一个已选句）预测**被选中的摘要句子**（`h_Si`）的能力。损失函数旨在最大化两者表示间的余弦相似度，同时最小化对一个未选中句子（负样本）的预测相似度。
        *   损失函数：`L_{Rest-Ext} = -cos(ĥ_{Si}, h_{Si}) + |cos(ĥ_{Si'}, h_{Si})|`
    2.  **Ext-Doc 目标**：提升从所有**被选中的摘要句子集合**（`H_S''`）预测**全文表示**（`¯h_S`）的能力。同样地，要最小化从非摘要句子集合预测全文表示的相似度。
        *   损失函数：`L_{Ext-Doc} = -cos(ĥ_D, ¯h_S) + |cos(ĥ'_D, ¯h_S)|`
    *   最终损失是两者的加权和：`L = λ_1 * L_{Rest-Ext} + λ_2 * L_{Ext-Doc}`。

*   **关键技术：可微分的长度控制策略（背包Transformer）**
    *   **问题建模**：将带长度约束的抽取摘要建模为 **0-1 背包问题**。其中，句子分数（`vi`）类比为“价值（profit）”，句子长度（`li`）类比为“重量（size）”，长度预算（`C`）是背包容量。目标是选出总长度不超过 `C` 且总价值最大的句子集合。
    *   **可微求解器**：设计并预训练一个**背包 Transformer**，来逼近动态规划（DP）求解器的行为。
        *   **结构**：模型输入是形状为 `(2, n)` 的矩阵（价值向量和重量向量堆叠），通过线性层映射后送入标准 Transformer 编码器，最终输出一个 Sigmoid 激活后的分数，表示每个句子被选中的概率。
        *   **预训练**：通过模拟大量价值/重量数据，并以 DP 求解器的最优解作为标签（0/1向量），使用二元交叉熵损失进行训练。
    *   **端到端训练**：在联合模型中，使用 **Gumbel-Softmax** 和**直通估计器**技巧，使基于分数选择句子的离散过程变得可微分，从而梯度可以回传至句子评分器进行端到端优化。

### 3. 实验设计

*   **数据集与场景**：在三个公开的新闻摘要数据集上进行实验，覆盖中英文。
    *   **CNNDM**: 英文新闻摘要。
    *   **NYT**: 英文新闻摘要。
    *   **CNewSum**: 中文新闻摘要。
*   **评估基准（Benchmark）与对比方法**：
    *   **无监督摘要质量（无长度控制）**：与 Oracle、Lead-3、TextRank (tf-idf, BERT, SimCSE)、Adv-RF、TED、PACSUM、STAS、FAR、PMI 等方法对比。
    *   **长度可控摘要性能**：在指定多个目标长度下，对比不同评分方法（TextRank, Ours）结合动态规划（DP）求解的效果。
    *   **人工评估**：与有监督方法 BertSum 以及不同配置的自有方法对比。
    *   **背包网络自身性能**：与 FPTAS-NN、Neural Knapsack、Reinforcement Learning 等神经背包方法对比。
*   **评价指标**：
    *   **自动评估**：ROUGE-1 (R1), ROUGE-2 (R2), ROUGE-L (RL) F1 分数。
    *   **长度控制**：生成摘要的平均长度与标准差的接近程度。
    *   **神经背包**：错误率和匹配率。
    *   **人工评估**：相关性、连贯性和事实一致性（1-5分）。

### 4. 资源与算力

*   论文**未明确提及**具体的 GPU 型号、数量或总训练时长。
*   文中仅提供了模型的超参数配置和优化器设置，例如：评分器和预测器的 Transformer 层数（2层/4层）、隐藏层维度（768）、注意力头数（4/8），以及学习率（`3e-6` 或 `1e-6`）和批次大小（64）。

### 5. 实验数量与充分性

*   **实验组数较多**，设计较为充分：
    1.  **主实验**：在 3 个数据集上对比了 12+ 种基线方法。
    2.  **长度控制实验**：在 2 个数据集上，分别针对 3 种不同目标长度，对比了多种方法组合的性能。
    3.  **消融实验**：在 3 个数据集上，分别验证了两个训练目标、预测器、句子编码器、负采样策略等 5 个组件的作用。
    4.  **神经背包网络自身评估**：在一个数据集分布上对比了 4 种不同的方法。
    5.  **人工评估**：在 1 个数据集上，邀请 3 位评审对 3 种模型生成的输出进行评估。
*   **公平性与客观性**：对比了充分的 SoTA 基线，统一使用了 ROUGE 等标准指标，并进行了人工评估和统计显著性检验，总体客观公平。

### 6. 论文的主要结论与发现

1.  **无监督摘要性能卓越**：提出的方法在三个数据集上均取得了极具竞争力的 ROUGE 分数，不仅大幅超越基于中心度的 TextRank 基线，也超越了 PACSUM、STAS 等当前最优的无监督方法，接近 Oracle 上限。
2.  **长度控制策略有效且更优**：通过引入可训练的背包Transformer进行端到端学习，模型的长度控制能力超越了仅使用 DP 后处理的方案，在不同长度预算下均取得了更高的 ROUGE 分数，且实际摘要长度与目标值更接近。
3.  **人类评估表现最佳**：在人工评估中，本方法的相关性和一致性得分甚至超越了有监督的 BertSum 模型，证明了其生成的摘要在事实性和信息覆盖上更胜一筹。
4.  **双向预测目标设计合理**：消融实验证明，两个预测目标（尤其是 Rest-Ext）以及负采样、预测器等组件对最终性能至关重要。

### 7. 优点

*   **新颖的无监督范式**：用可学习的双向预测替代了不可微的中心度计算，实现了无监督抽取式摘要的端到端训练，摆脱了对位置先验的依赖。
*   **巧妙的问题转化**：将长度控制问题建模为 0-1 背包问题，并创新性地提出用神经网络（背包Transformer）来逼近不可微的 DP 求解器，使其能够嵌入深度学习框架进行联合优化。
*   **方法通用性强**：无需人工标注，可灵活应用于不同领域、类型和语言的文档，具备良好的实际应用潜力。
*   **实验扎实全面**：在多个数据集上进行了丰富的自动和人工评估，并配有详尽的消融实验，论证充分。

### 8. 不足与局限

*   **计算开销未量化**：论文未提供任何关于训练时间、GPU 消耗等资源开销的数据。端到端训练背包Transformer会引入额外的计算成本，这一点的缺失会影响对方法实际可用性的判断。
*   **背包模块的依赖**：背包Transformer本身需要预先用模拟数据进行单独训练。模拟数据的分布可能与真实文本的长度、分数分布存在偏差，这种偏差对最终模型泛化能力的影响未被讨论。
*   **中文数据集的性能**：虽然在 CNNDM 和 NYT 上表现出色，但在中文数据集 CNewSum 上的 ROUGE 分数提升相对不够显著，可能需要进一步分析。
*   **超参数敏感性**：方法引入了额外的超参数，如损失权重 λ1 和 λ2，文中仅给出了最终的设定值，其敏感性和调参难度未作说明。

（完）
