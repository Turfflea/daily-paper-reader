---
title: "ICE-T: Interactions-aware Cross-column Contrastive Embedding for Heterogeneous Tabular Datasets"
title_zh: ICE-T：异质表格数据集的交互感知跨列对比嵌入
authors: "Tomas Tokar, Scott Sanner"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/35385/37540"
tags: ["query:mt"]
score: 8.0
evidence: 表格数据的自监督对比表示学习
tldr: 针对异质表格数据的高质量表征学习难题，提出ICE-T方法，将每个表格列视为独立模态，通过跨列对比学习捕获交互，自监督地学习数据嵌入。实验表明，该方法在下游任务上优于现有表格表征方法，为无标签表格数据的表示学习提供了新框架，可广泛应用于管理科学中的结构化数据分析。ICE-T利用多模态对比学习框架，通过最大化列间互信息学习列交互，生成鲁棒的表格数据嵌入，在多个公开数据集上验证了其有效性。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-35385/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 839, \"height\": 896, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-35385/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 782, \"height\": 574, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-35385/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1827, \"height\": 933, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-35385/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1814, \"height\": 517, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-35385/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1647, \"height\": 592, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-35385/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 883, \"height\": 776, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-35385/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 512, \"height\": 375, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-35385/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 818, \"height\": 2024, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-35385/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 717, \"height\": 280, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-35385/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 870, \"height\": 353, \"label\": \"Table\"}]"
motivation: 异质表格数据普遍存在于各领域，但现有方法难以有效捕获列间交互信息，制约了下游任务性能。
method: 提出ICE-T，将列视为模态，通过跨列对比损失最大化互信息，自监督学习列感知的表格数据嵌入。
result: 在多个下游任务上，ICE-T超越了现有表格表征方法，证明了跨列对比学习在异质表格数据上的有效性。
conclusion: ICE-T为异质表格数据提供了强大的自监督表征学习框架，可迁移至管理科学中的结构化数据分析任务。
---

## Abstract
Finding high-quality representations of heterogeneous tabular datasets is crucial for their effective use in downstream machine learning tasks. Contrastive representation learning (CRL) methods have been previously shown to provide a straightforward way to learn such representations across various data domains. Current tabular CRL methods learn joint embeddings of data instances (tabular rows) by minimizing a contrastive loss between the original instance and its perturbations. Unlike existing tabular CRL methods, we propose leveraging frameworks established in multimodal representation learning, treating each tabular column as a distinct modality. A naive approach that applies a contrastive loss pairwise to tabular columns is not only prohibitively expensive as the number of columns increases, but as we demonstrate, it also fails to capture interactions between variables. Instead, we propose a novel method called ICE-T that learns each columnar embedding by contrasting it with aggregate embeddings of the complementary part of the table, thus capturing interactions and scaling linearly with the number of columns. Unlike existing tabular CRL methods, ICE-T allows for column-specific embeddings to be obtained independently of the rest of the table, enabling the inference of missing values and translation between columnar variables. We provide a comprehensive evaluation of ICE-T across diverse datasets, demonstrating that it generally surpasses the performance of the state-of-the-art alternatives.

---

## 论文详细总结（自动生成）

# 论文详细分析与总结

## 1. 论文核心问题与研究动机
- **问题背景**：异质表格数据（含数值、类别、文本、图像等列）在各应用领域普遍存在，但深度神经网络在该类数据上的表示学习仍具挑战。
- **现有方法局限**：
  - 当前表格数据对比表示学习（CRL）方法（如Scarf、SubTab）以行为单位，通过对原始行进行扰动来学习联合嵌入，难以捕获列间交互。
  - 将多模态CRL直接应用于表格列（每列视为一模态）存在两大难题：①两两列间对比损失导致计算复杂度随列数二次增长；②简单的成对对比无法捕捉变量间的**交互作用**（如XOR问题）。
- **研究动机**：借鉴多模态表示学习框架，设计一种既支持跨列交互建模，又具有线性计算复杂度的表格表示学习方法，以提升下游任务的性能，同时实现缺失值推断、模态翻译等附加能力。

## 2. 方法论：ICE-T核心思想与关键技术
### 2.1 整体框架
- 将每个表格列视为独立“模态”，为每列配备专属编码器 \( f^{(m)} \)，将原始输入映射为中间隐藏向量 \( h^{(m)} \)。
- 对任一目标列 \( m \)，其余列的中间表示取**算术平均**，得到“锚”向量 \( \mu^{(\backslash m)} = \frac{1}{M-1} \sum_{n \neq m} h^{(n)} \)。
- \( h^{(m)} \) 与 \( \mu^{(\backslash m)} \) 通过共享投影网络 \( g \) 映射到最终嵌入空间，得到 \( z^{(m)} \) 与 \( z^{(\backslash m)} \)。
- 使用**余弦相似度 + InfoNCE损失**最大化匹配对（同一样本的列嵌入与对应锚）的相似度，最小化不匹配对的相似度。

### 2.2 损失函数
对于第 \( i \) 个实例的第 \( m \) 个变量，其损失为：
\[
l_i^{(m)} = -\log \frac{\exp\big(s^{(m)}(i,i)/\tau\big)}{\sum_j \exp\big(s^{(m)}(i,j)/\tau)}
\]
其中 \( s^{(m)}(i,j) \) 表示 \( z_i^{(m)} \) 与 \( z_j^{(\backslash m)} \) 的余弦相似度，\( \tau \) 为可训练温度参数。总损失为所有实例、所有变量的加和。

### 2.3 关键特性
- **线性扩展**：只需一次线性遍历计算所有列嵌入的总和，第二次线性遍历即可逐个计算 leave-one-out 锚并计算损失，复杂度 \( O(M) \)。
- **交互捕获**：锚聚合了其余列信息，单列与锚的对比隐式建模了列间交互，解决了成对对比的XOR失败问题。
- **嵌入通用性**：训练后可获得列特定嵌入、完整实例联合嵌入（所有列平均）以及任意列子集的联合嵌入。
- **模态翻译**：通过最大化 \( s(z^{(m)}, z^{(A)}) \) 可从证据列推断缺失列的值，支持缺失值插补。
- **数据不可知**：列编码器可根据数据类型灵活选择（MLP、CNN、Transformer等），适应图像、文本列。

## 3. 实验设计概览
### 3.1 数据集
- 总计 **48个真实世界数据集**：44个来自表格基准（Grinsztajn et al. 2022）的纯表格数据；2个图像-表格数据；2个文本-表格数据。
- 涵盖分类与回归任务，变量数从个位数到数十个，包含数值、类别、文本、图像等多种类型。

### 3.2 对比方法
- **表格CRL方法**：Scarf（随机特征损坏）、SubTab（子集分割，对比+距离损失）。
- **多模态CRL方法**：CLIP（成对模态对比）、GMC（模态特定嵌入与联合嵌入对比）、MCN（带聚类质心的对比）。
- 对于变量数超过25的数据集，因计算量排除CLIP和MCN。

### 3.3 评估任务与指标
- **插补（模态翻译）**：从其余列估计目标列的值，数值列用MSE，类别列用平衡准确率，统一为插补得分。
- **聚类**：对嵌入做k-means，用轮廓系数评估。
- **监督学习**：用KNN预测目标，分类用平衡准确率，回归用MSE。
- **迁移学习**：固定预训练编码器，加单隐层MLP预测头微调评估；对比纯MLP和XGBoost。
- 各数据集结果归一化后计算**平均相对分数**。

### 3.4 合成数据验证
- 构造含三个变量的高斯XOR数据，验证ICE-T能否捕获“任一变量由另两变量交互决定”的模式，对比CLIP、GMC、MCN。

## 4. 资源与算力
- **论文未明确披露**使用的GPU型号、显存、训练时长或总浮点运算量。
- 仅提及因计算限制排除了一个最大数据集（delays zurich airport），并对变量 >25 的数据集排除了成对对比方法以**避免过长运行时间**，但未给出具体资源配置。

## 5. 实验数量与充分性分析
- **实验组次**：48个数据集 × 多个方法 × 4个任务。每个纯表格数据集重复5次，图像/文本表格重复3次，以均值汇报。
- **超参数优化**：每个方法在验证集上选择最佳超参数配置，确保公平比较。
- **基准全面**：既有浅层方法（原始数据k-means、KNN、XGBoost），也有针对表格和多模态的SOTA对比学习方法；同时涵盖聚类、分类/回归、迁移和插补任务。
- **充分客观**：实验覆盖多种数据大小和列类型，使用标准评估协议和归一化排名，结果以胜率矩阵和箱线图多角度呈现，避免单一指标偏误；合成实验进一步验证核心技术假设。

## 6. 主要结论与发现
- **插补任务**：ICE-T在所有对比方法中表现最优，证明其通过锚对比能有效建模列间依赖，实现高质量的模态翻译。
- **监督学习**：ICE-T平均相对分数最高，随后依次为MCN、CLIP等。
- **迁移学习**：ICE-T预训练编码器 + MLP预测头显著超越XGBoost及纯MLP，展示强大的表示泛化能力；之前工作（Scarf）仅60%数据集超越XGBoost，本工作结果一致且更优。
- **聚类**：Scarf略优于ICE-T，可能因Scarf以整行扰动学习全局联合嵌入，更利于全局结构保持，但ICE-T仍具竞争力。
- **扩展性**：ICE-T线性复杂度相比成对对比方法在列数多时具有明显计算优势，且避免了因交互缺失的嵌入退化。

## 7. 优点总结
- **方法创新**：首次将多模态CRL思想系统性地适配到表格数据，并以均值锚而非成对比较建模交互，简单而有效。
- **计算效率**：实现线性时间复杂度，不受列数平方增长的制约。
- **通用性与灵活性**：支持列特定嵌入、任意子集联合嵌入，天然支持缺失值推断和跨模态翻译，编码器可根据列类型灵活替换。
- **实验全面性**：在48个多种模态数据集上，覆盖4种下游任务，与5种前沿方法全面对比，证据充分。
- **可复现性**：提供补充材料与代码，方法描述清晰，有批量计算算法伪代码。

## 8. 不足与局限
- **未明确算力消耗**：缺乏GPU型号、训练时间等报告，实际应用时的资源需求难以评估。
- **聚类性能非最优**：在捕获实例全局结构方面不如以整行对比的Scarf，可能因锚平均损失了一定程度的 instance-level 对齐信息。
- **实验局限**：
  - 最大数据集被排除，ICE-T在超大数据规模上的表现未知。
  - 虽支持图像/文本列，但仅4个此类数据集，多模态复杂表格的扩展性有待进一步验证。
  - 对极高维度列（如包含大量类别的特征）的编码器选择和锚平均的适用性未深入探讨。
- **理论分析**：交互捕获的保证仍基于实验和直观解释，缺乏严格的理论证明，例如对比损失与互信息下界的关系未充分推导。
- **可调温度参数**：温度τ对性能可能较敏感，超参数搜调成本未讨论。

（完）
