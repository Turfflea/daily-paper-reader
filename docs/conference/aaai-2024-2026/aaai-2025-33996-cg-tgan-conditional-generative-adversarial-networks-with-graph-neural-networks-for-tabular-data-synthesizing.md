---
title: "CG-TGAN: Conditional Generative Adversarial Networks with Graph Neural Networks for Tabular Data Synthesizing"
title_zh: "CG-TGAN: 条件生成对抗网络与图神经网络结合的表格数据合成方法"
authors: "Seungcheol Lee, Moohong Min"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/33996/36151"
tags: ["query:mt"]
score: 8.0
evidence: 使用图神经网络建模列关系进行表格数据合成
tldr: 为合成反映列间关系的表格数据，提出CG-TGAN，结合条件GAN与图神经网络建模列依赖；解决了多模态、不平衡列建模难题，生成的数据在保持隐私的同时具有高可用性，为敏感表格数据共享提供了关系感知的合成方案。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33996/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1825, \"height\": 695, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33996/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 717, \"height\": 444, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33996/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1634, \"height\": 841, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33996/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 887, \"height\": 1691, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33996/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1338, \"height\": 268, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33996/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1807, \"height\": 265, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33996/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 878, \"height\": 203, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33996/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1400, \"height\": 318, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33996/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 851, \"height\": 115, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33996/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 879, \"height\": 266, \"label\": \"Table\"}]"
motivation: 传统表格数据合成模型难以捕捉列间复杂关系，且面临多模态、不平衡数据挑战。
method: 设计条件生成对抗网络，利用图神经网络显式建模表格列之间的关系结构。
result: 生成数据在保持隐私的同时，在机器学习效用测试上接近真实数据。
conclusion: CG-TGAN为敏感表格数据共享提供了关系感知的合成方案，可迁移至管理数据分析。
---

## Abstract
Data sharing is necessary for AI to be widely used, but sharing sensitive data with others with privacy is risky.
To solve these problems, it is necessary to synthesize realistic tabular data.
In many cases, tabular data contains a mixture of continuous, mixed, categorical columns.
Moreover, columns of the same type may have multimodal distribution or be highly imbalanced.
These issues make it challenging to synthesize tabular data.
The synthesized tabular data should reflect the relational meaning between columns of tabular data, so modeling the probability distribution of tabular data is a non-trivial task.
Traditional tabular data synthesizing models are based on GAN or diffusion models and are built using fully connected or convolutional layers.
However, fully connected layers have the disadvantage of low inductive bias, and convolutional layers are not invariant to the column order of tabular data.
Therefore, we assume that converting tabular data into graph-structured data and using a graph neural network would produce better synthetic data than using fully connected layers or convolutional layers.
Our study aims to show that GANs constructed with graph neural networks can outperform existing GAN models using fully connected layers or convolutional layers.
We propose CG-TGAN, a conditional GAN built using graph neural networks. To learn how to synthesize realistic data, the graph neural networks in the discriminator and generator learn graph-level tasks and node-level tasks together.
The discriminator of CG-TGAN learns a graph-level task to distinguish between real and synthetic data and node-level tasks to predict the value of the target node.
CG-TGAN’s generator learns a graph-level task to synthesize an overall graph similar to real data and node-level tasks to learn how to synthesize a fake graph with the proper relation between nodes.
In this paper, we show that CG-TGAN outperforms GAN-based models and is comparable to diffusion-based models.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
*   **核心问题**：研究旨在解决敏感表格数据共享中的隐私风险问题，通过生成高质量的合成表格数据来替代真实数据。
*   **研究动机**：
    *   传统表格数据合成模型（如基于全连接层或卷积层的GAN/扩散模型）存在固有限制。
    *   全连接层**归纳偏置较弱**，难以捕捉列间复杂关系。
    *   卷积层对**列的顺序不具有不变性**，而表格数据的列顺序通常无实际意义，这导致结果不稳定。
    *   表格数据本身具有挑战性，包含**连续、混合和类别列的混合**，且常呈现**多模态分布**或高度**不平衡**。
*   **核心假设**：将表格数据转换为图结构数据，并使用图神经网络（GNN）进行建模，能够更好地捕捉列间关系，从而生成比传统方法更优的合成数据。

### 2. 论文提出的方法论
*   **核心思想：CG-TGAN**
    *   提出一种基于**图神经网络（GNN）的条件生成对抗网络（CGAN）**，用于表格数据合成。模型名为 **CG-TGAN**。
*   **关键技术细节**:
    *   **表格数据到图的转换**：
        *   **节点**：表格的每一列被转换为图中的一个节点。
        *   **节点特征**：根据列类型使用不同编码器（连续列用模式归一化，混合列用混合编码器，类别列用独热编码），然后通过投影层映射到相同维度。
        *   **边与邻接矩阵**：所有节点之间全连接。为避免缺乏先验的列间关系信息，CG-TGAN 引入**可学习的参数矩阵 $A_P$**，并分别派生出生成器的邻接矩阵 $A_G$ 和判别器的邻接矩阵 $A_D$ ($A_D = A_P^{-1}$)，以模拟生成与判别过程的互逆性。
    *   **条件节点**：为处理条件信息，CG-TGAN 在图中引入一个与所有原始列节点全连接的“**条件节点**”，其节点特征为条件向量，以此将条件信息传递给整个图。
    *   **双重任务学习**：
        *   **判别器**：
            *   **图级任务**：判断整个输入图是真实数据还是生成数据（使用WGAN-GP损失）。
            *   **节点级任务**：预测图中一个特定“目标节点”的值（连续/混合列用MSE损失，类别列用交叉熵损失），以显式学习列间关系。
        *   **生成器**：
            *   **图级任务**：从噪声和条件向量生成宏观上接近真实图的假图。
            *   **节点级任务**：生成具有正确列间关系的假图。
    *   **训练流程**：每个训练迭代中，依次执行判别器的图级任务更新、判别器的节点级任务更新、生成器的图级任务更新、生成器的节点级任务更新。GNN 架构主要使用 3 层带残差连接的**图卷积网络（GCN）**。

### 3. 实验设计
*   **数据集**：在 **7个广泛使用的数据集** 上进行了评估。
    *   **回归任务**：Abalone（鲍鱼年龄）、King（房价）、Insurance（医疗费用）。
    *   **二分类任务**：Adult（收入预测）、Diabetes（糖尿病预测）、Wilt（植被识别）。
    *   **多分类任务**：Gesture（手势识别）。
*   **基准方法（对比模型）**：
    *   **GAN-based**: DPGAN, Pate-GAN, CTGAN, CTAB-GAN, CTAB-GAN+。
    *   **Diffusion-based**: CoDi, TabDDPM。
*   **评估指标**：
    *   **机器学习效用**：比较在合成数据与在真实数据上训练的机器学习模型的性能差异。分类任务使用准确率、AUC、F1分数；回归任务使用MAPE、MAE、R²。
    *   **隐私保护**：使用最近邻距离比率（NNDR）和到最近记录的距离（DCR）来衡量合成数据点与真实数据点的距离，评估隐私泄露风险。

### 4. 资源与算力
*   论文**未明确提及**使用的 GPU 型号、数量或具体训练时长等计算资源信息。

### 5. 实验数量与充分性
*   **实验数量充足**：论文涵盖了**7个多样化数据集**上的效用和隐私对比实验，与7种基线模型进行比较。
*   **实验充分且公平**：
    *   评估维度全面，同时考虑了**机器学习效用和隐私保护能力**。
    *   除了与外部模型的横向对比，还进行了详细的**消融实验**，验证了GCN层数、残差连接、条件节点、邻接矩阵初始化方式等关键设计对性能的影响。
    *   实验设计客观，使用了学术界的标准数据集和评价指标。

### 6. 论文的主要结论与发现
*   **优于GAN类模型**：CG-TGAN 在所有实验数据集上的机器学习效用和生成数据分布质量上，**一致地优于**传统的基于GAN的表格数据合成模型（如CTGAN、CTAB-GAN+）。
*   **可比肩扩散模型**：CG-TGAN 在机器学习效用上与先进的扩散模型（TabDDPM）**性能相当**，并在部分小型数据集（如Insurance, Diabetes）上表现更佳。
*   **隐私保护更优**：相较于TabDDPM，CG-TGAN 生成的合成数据具有更高的NNDR和DCR值，显示出**更强的隐私保护能力**，尤其是在保持高效用的同时。
*   **假设验证**：实验结果证明了将表格数据图结构化并使用GNN进行建模这一核心假设的有效性。

### 7. 优点
*   **方法创新性**：首次将图神经网络系统地引入CGAN框架进行表格数据合成，通过图结构天然地解决了列顺序非不变性问题。
*   **显式关系建模**：通过可学习的邻接矩阵和节点级预测任务，模型能有效学习并利用列间的内在关系，增强了合成数据的逻辑一致性。
*   **综合性能出色**：CG-TGAN 在**效用与隐私的权衡上表现突出**，在生成高质量数据的同时提供了比同类优秀模型更好的隐私保护。
*   **训练机制精妙**：提出的图级与节点级双重任务训练策略，使判别器和生成器不仅能完成真假判断，还能深入理解数据结构。

### 8. 不足与局限
*   **算力与时间成本未明**：论文未报告模型的训练时间复杂度、计算资源消耗等信息，使得其实用性和扩展性难以评估。
*   **未完全超越扩散模型**：虽然效用可比，但在多数数据集上并未在效用指标上显著超越最优的扩散模型（TabDDPM），主要优势体现在隐私保护方面。
*   **图构建的单一性**：节点初始特征依赖于传统编码，且图结构为全连接图，未来可探索引入更复杂的图结构学习或基于因果关系的建图方式。
*   **实验覆盖有限**：实验虽覆盖了7个数据集，但主要在中小规模数据集上测试，其在大规模高维表格数据上的表现和效率有待验证。

（完）
