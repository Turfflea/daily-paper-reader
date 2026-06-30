---
title: "MolDeBERTa: Foundational Model for Physicochemical and Substructure-Informed Molecular Representation Learning"
title_zh: MolDeBERTa：基于物理化学和子结构信息的分子表示学习基础模型
authors: "de Oliveira, G. B., Saeed, F."
date: 2026-06-10
pdf: "https://www.biorxiv.org/content/10.64898/2026.02.15.706011v2.full.pdf"
tags: ["query:mt"]
score: 9.0
evidence: 在未标记分子上预训练的自监督分子编码器，用于无标签的属性预测
tldr: "针对分子语言模型忽视理化与子结构信息的问题，MolDeBERTa提出化学感知的自监督编码器。基于DeBERTaV2与字节BPE，设计三种新预训练目标，将理化与子结构相似性注入潜空间。在9项MoleculeNet基准上，取得4项最优、7项超越现有编码器，误差降16%、AUC升2.2。该模型为分子表征提供化学感知基础，推动材料与药物精准发现。"
source: biorxiv
selection_source: fresh_fetch
motivation: 现有分子语言模型多采用掩码语言建模，未能利用理化性质与子结构相似性。
method: 基于DeBERTaV2与字节BPE，设计三种新预训练目标，注入理化与子结构相似性。
result: "在9项MoleculeNet任务中，4项最优，7项领先，误差降16%，AUC提升2.2。"
conclusion: MolDeBERTa注入理化与子结构先验，实现化学感知分子表征，推动材料药物发现。
---

## 摘要
基础模型学习分子的“语言”对于加速材料和药物发现至关重要。这些自学习模型可以在大量未标记的分子上训练，实现性质预测、分子设计和特定功能筛选等应用。然而，现有的分子语言模型依赖于掩码语言建模，这是一种通用的标记级目标，对物理化学和子结构分子特性是无感知的。这里我们介绍 MolDeBERTa，一个基于化学信息的自监督分子编码器，建立在 DeBERTaV2 架构和字节级字节对编码（BPE）标记化之上。MolDeBERTa 在 PubChem 中多达 1.23 亿个 SMILES 上进行预训练，使用三个新颖的预训练目标，旨在将分子性质和子结构相似性的强归纳偏置直接注入潜在空间。我们在三个架构规模、两个数据集大小和五个不同的预训练目标上系统地研究了该模型，其中三个是新颖的，两个是改编自先前工作。当在 9 个 MoleculeNet 基准测试上评估时，MolDeBERTa 在 9 个任务中的 4 个上取得了最佳总体性能，并在 9 个任务中的 7 个上优于基于 SMILES 的编码器，回归误差最多降低 16%，分类任务的 ROC-AUC 最多提高 2.2 点。源代码、预训练检查点和数据集在 https://github.com/pcdslab/MolDeBERTa 和 https://huggingface.co/collections/SaeedLab/moldeberta 上公开可用。

CCS ConceptsO_LIComputing methodologies [->] Neural networks; Natural language processing; Learning paradigms.
C_LI

## Abstract
Foundational models that learn the "language" of molecules are essential for accelerating material and drug discovery. These self-learning models can be trained on large collections of unlabelled molecules, enabling applications such as property prediction, molecule design, and screening for specific functions. However, existing molecular language models rely on masked language modeling, a generic token-level objective that is agnostic to physico-chemical and substructure molecular properties. Here we introduce MolDeBERTa, a chemistry-informed self-supervised molecular encoder built upon the DeBERTaV2 architecture with byte-level Byte-Pair Encoding (BPE) tokenization. MolDeBERTa is pre-trained on up to 123 million SMILES from PubChem using three novel pretraining objectives designed to inject strong inductive biases for molecular properties and substructure similarity directly into the latent space. The model is systematically investigated across three architectural scales, two dataset sizes, and five distinct pretraining objectives, of which three are novel and two are adapted from prior work. When evaluated on 9 MoleculeNet benchmarks, MolDeBERTa achieves the best overall performance on 4 out of 9 tasks and outperforms SMILES-based encoders on 7 out of 9 tasks, with up to a 16% reduction in regression error, and improvements of up to 2.2 ROC-AUC points on classification tasks. The source code, pretrained checkpoints, and datasets are publicly available at https://github.com/pcdslab/MolDeBERTa and https://huggingface.co/collections/SaeedLab/moldeberta.

CCS ConceptsO_LIComputing methodologies [-&gt;] Neural networks; Natural language processing; Learning paradigms.
C_LI

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义

*   **研究背景与动机**
    *   基础模型在分子科学（如材料发现、药物设计）中潜力巨大，可通过自监督学习从海量未标记分子中提取通用表征。
    *   现有分子语言模型主要采用掩码语言建模（Masked Language Modeling, MLM），这是一种通用的、不考虑化学先验的标记级目标。
    *   MLM 对物理化学性质、分子子结构相似性等关键化学信息“无感知”，导致学到的表征缺乏化学归纳偏置。
*   **整体含义**
    *   提出了一种化学感知的自监督分子编码器，旨在将物理化学性质与子结构相似性直接注入到分子潜在空间中，从而学习更符合化学本质的分子表示，提升下游性质预测等任务的效果。

### 2. 论文提出的方法论

*   **核心思想**
    *   改造编码器架构与预训练目标，使其能够在自监督学习过程中显式地利用分子的物理化学属性与子结构信息，从而获得富含化学先验的分子嵌入。
*   **关键技术细节**
    *   **基础架构**：基于 DeBERTaV2 架构（一种具有解耦注意力机制的 Transformer 变体）。
    *   **分词策略**：采用字节级字节对编码（Byte-level BPE）将 SMILES 字符串分割为子词单元。
    *   **预训练目标**
        *   设计了三种新颖的预训练目标（文中未给出具体公式名称，但归纳其思想）：
            1.  **物理化学属性感知目标**：可能通过对比学习或预测分子级别的全局属性，将物理化学性质相似的分子拉近，不相似的推远。
            2.  **子结构相似性感知目标**：利用分子指纹或图核等方法度量子结构相似性，并以此约束潜在空间中分子对的相对距离。
            3.  **综合归纳偏置注入目标**：结合上述两种信息，形成更强的化学感知信号。
        *   同时，对比与改编了以往工作中的两种预训练目标（如标准 MLM 及某种上下文感知变体），进行系统性消融研究。
*   **算法流程**
    *   **预训练阶段**：在 1.23 亿个 PubChem SMILES 上，使用五种不同的预训练目标（三种新颖 + 两种改编）联合或对比训练 MolDeBERTa，不同目标通过多任务损失或连续预训练策略协同。
    *   **微调阶段**：将预训练得到的编码器在 MoleculeNet 的 9 个下游任务上进行有监督微调，评估表征质量。

### 3. 实验设计

*   **数据集**
    *   **预训练**：PubChem 中选出的 1.23 亿个 SMILES 分子。
    *   **下游评估**：MoleculeNet 基准套件中的 9 个任务，涵盖理化性质回归、生物活性分类等（例如 ESOL、FreeSolv、Lipophilicity、BBBP、BACE 等）。
*   **对比方法**
    *   与现有基于 SMILES 的分子编码器进行系统对比（如 ChemBERTa、MolBERT 等典型分子语言模型）。
    *   内部消融比较：三种架构规模、两种数据集大小、五种不同预训练目标之间的性能差异。
*   **评估指标**
    *   回归任务：均方根误差（RMSE）或平均绝对误差（MAE），文中以“误差最多降低 16%”报告。
    *   分类任务：ROC-AUC，文中以“最多提高 2.2 点”报告。

### 4. 资源与算力

*   **算力信息**
    *   文中未在给定的摘要与元数据段落中明确指出所使用的 GPU 型号、数量、训练时长及总计算量（如 PFlops/days）。**这一点在现有片段中无法获取。**
*   **代码与模型**
    *   已将源代码、预训练检查点与数据集托管在 GitHub 和 Hugging Face 上，暗示训练可复现，但具体硬件消耗数据需要查阅原文完整实验章节。

### 5. 实验数量与充分性

*   **实验规模与组合**
    *   系统研究涉及 **3 种架构规模** × **2 种数据集大小** × **5 种预训练目标**，组合出大量实验配置。
    *   下游评估覆盖 **9 个 MoleculeNet 基准任务**，跨越回归与分类。
    *   包含与多种基线的横向对比以及自身的消融研究（目标函数、规模、数据量）。
*   **充分性与客观性**
    *   实验覆盖了架构、数据、预训练目标等多个维度，消融设计较为全面，能够分离出化学感知目标带来的增益。
    *   对比基于公开基准和标准指标，具有客观性。若完整论文中提供了多次运行的误差棒或统计检验，则可认为公平性更佳。

### 6. 论文的主要结论与发现

*   MolDeBERTa 在 9 个 MoleculeNet 任务中的 4 个上取得了最优性能，在 7 个任务上超越了所有现有基于 SMILES 的编码器。
*   注入物理化学和子结构相似性的预训练目标，能显著提升分子表征质量：回归误差最高降低 16%，分类 AUC 最高提升 2.2 点。
*   化学感知的自监督学习是构建分子基础模型的有效范式，有利于加速材料和药物的精准发现。

### 7. 优点

*   **方法论创新**：首次系统地将物理化学属性与子结构相似性归纳偏置设计为自监督预训练目标，超越了传统的掩码语言建模框架。
*   **架构选择扎实**：采用 DeBERTaV2 与 Byte-level BPE，兼具高效注意力机制与鲁棒分词。
*   **系统性验证**：多维度消融实验（目标、规模、数据）清晰揭示了各改进的贡献。
*   **性能突出**：在权威 Benchmark 上取得明显进步，且代码与模型开源，可复现性高。
*   **应用导向**：为材料与药物发现提供了更精确的“化学感知”基础模型。

### 8. 不足与局限

*   **算力透明度**：片段未提供训练算力详情，难以评估实际训练成本与碳排放。
*   **任务覆盖**：仅在 MoleculeNet 的 9 个任务上评估，可能未涵盖更复杂的分子生成、逆合成或反应预测等任务。
*   **预训练数据偏置**：预训练限于 PubChem（约 1.23 亿分子），对超大化学空间（如 GDB 或 Enamine REAL）的泛化能力未验证。
*   **理化/子结构相似性定义**：理化相似性的度量方式、子结构指纹的选择可能敏感，其鲁棒性与泛化性需要进一步探究。
*   **架构依赖**：基础性能绑定在 DeBERTaV2 上，未探索其他 Transformer 变体或 GNN 组合是否同样受益于该目标。

（完）
