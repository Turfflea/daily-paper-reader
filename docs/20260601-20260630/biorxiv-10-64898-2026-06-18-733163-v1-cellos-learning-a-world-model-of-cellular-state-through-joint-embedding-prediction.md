---
title: "CellOS: Learning a World Model of Cellular State through Joint Embedding Prediction"
title_zh: CellOS：通过联合嵌入预测学习细胞状态的世界模型
authors: "Zhou, Q., Le, Y., Qi, X., Chang, S., Lu, H., Wu, Y., Wang, H., Ran, R., li, x."
date: 2026-06-23
pdf: "https://www.biorxiv.org/content/10.64898/2026.06.18.733163v1.full.pdf"
tags: ["query:mt"]
score: 9.0
evidence: 多视图基础模型从配对视图无标签学习细胞状态表示。
tldr: 现有单细胞基础模型多从单一基因表达视图学习，无法显式融合互补的细胞状态信息。CellOS提出多视图联合嵌入预测框架，通过三阶段训练（因果细胞句子建模、密集到MoE扩展、基于LLM-JEPA的隐空间对齐）学习细胞表征。在12B参数、3.9亿单细胞转录组上训练的CellOS，在细胞状态注释、批次整合和扰动响应预测等基准上全面超越前沿模型。这项工作表明，互补细胞视图间的预测对齐是实现表征中心细胞世界模型和可迁移AI虚拟细胞的可扩展路径。
source: biorxiv
selection_source: fresh_fetch
motivation: 现有单细胞基础模型多从基因表达单视图学习，无法显式融合互补视图以全面表征细胞状态。
method: CellOS通过三阶段训练（因果细胞句子建模、MoE扩展和LLM-JEPA隐空间对齐）实现多视图联合嵌入预测。
result: 12B参数模型在3.9亿单细胞转录组上训练，在细胞注释、批次整合及扰动预测任务上全面超越前沿模型。
conclusion: 互补视图间的预测对齐为构建表征中心细胞世界模型和可迁移AI虚拟细胞提供了可扩展路径。
---

## 摘要
从单细胞转录组学习的基础模型是实现能够表示、查询和预测细胞状态的AI虚拟细胞的核心。然而，当前大多数单细胞基础模型仅从基因表达的单一视图学习，并主要通过重建或下一个标记预测进行优化。因此，它们捕获了表达丰度，但无法明确调和细胞状态的互补视图。在此，我们提出了CellOS，一种多视图基础模型，它从配对的表达和感知视图中学习细胞表示。CellOS通过一个可扩展的三阶段训练策略整合互补视图，该策略结合了因果细胞-句子语言建模、保函数的密集到专家混合扩展以及通过LLM-JEPA目标进行的潜在空间对齐。使用这一框架，我们在3.905亿个单细胞转录组上训练了一个120亿参数模型。在涵盖细胞状态注释、批次整合和扰动响应预测的多种基准测试中，CellOS一致优于最先进的单细胞基础模型。总之，这些结果表明，互补细胞视图之间的预测对齐为以表示为中心的细胞世界模型和可迁移的AI虚拟细胞提供了一条可扩展的路径。

## Abstract
Foundation models learned from single-cell transcriptomes are central to the prospect of AI virtual cell that can represent, query and predict cellular state. However, most current single-cell foundation models learn from a single view of gene expression and are optimized primarily through reconstruction or next-token prediction. As a result, they capture expression abundance but cannot explicitly reconcile complementary views of cellular state. Here we present CellOS, a multi-view foundation model that learns cellular representations from paired expression and perception views. CellOS integrates complementary views through a scalable three-stage training strategy that combines causal cell-sentence language modelling, function-preserving dense-to-mixture-of-experts expansion and latent-space alignment via an LLM-JEPA objective. Using this framework, we trained a 12-billion-parameter model on 390.5 million single-cell transcriptomes. Across diverse benchmarks spanning cell-state annotation, batch integration and perturbation-response prediction, CellOS consistently outperformed state-of-the-art single-cell foundation models. Together, these results suggest that predictive alignment between complementary cellular views provides a scalable path toward representation-centric cellular world models and transferable AI virtual cells.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **研究背景**：现代单细胞组学数据（特别是转录组）急剧增长，催生了以“AI虚拟细胞”为目标的基础模型，期望能表示、查询和预测细胞状态。
- **现有局限**：当前多数单细胞基础模型仅从“基因表达丰度”这一单一视图学习，优化目标多为重构或下一个词元预测，无法显式调和细胞状态中互补且经常隐晦的视图（如“感知”视图）。
- **核心问题**：如何构建一个能同时从配对表达视图与感知视图（或其他互补视图）中学习统一细胞表示的世界模型，以突破单视图的表示瓶颈。
- **整体含义**：CellOS 提出以“互补视图间的预测对齐”为学习目标的框架，旨在实现可扩展的、以表示为中心的细胞世界模型，为可迁移的 AI 虚拟细胞铺平道路。

### 2. 论文提出的方法论
- **核心思想**：采用**多视图联合嵌入预测**，从配对的“表达”视图和“感知”视图中学习细胞的隐空间表示，迫使模型在不同视图之间进行可预测的对齐，从而捕捉更本质的细胞状态。
- **三阶段可扩展训练策略**：
    1. **因果细胞‑句子语言建模**：将单细胞表达谱转化为“细胞句子”，进行因果（自回归）语言建模，使模型初步理解基因间的顺序依赖关系。
    2. **保函数的密集到专家混合扩展**：在无需破坏已学表示的前提下，将密集的前馈网络扩张为条件计算的**混合专家**结构，大幅增加模型容量而又控制每次激活的计算量。
    3. **基于 LLM‑JEPA 的隐空间对齐**：引入**联合嵌入预测架构**思想，通过对比式或预测式目标，将表达视图与感知视图的隐表示对齐。其核心是**LLM‑JEPA 目标**，即在潜在空间中预测某个视图的表示，而无需在原始像素/标记空间重构，从而引导模型学习跨视图共有的、与细胞状态高度相关的抽象编码。
- **模型规模与数据**：最终训得一个 **120 亿参数**的模型，训练数据规模为 **3.905 亿个单细胞转录组**。

### 3. 实验设计
- **数据集/场景**：
    - 训练集：3.905 亿个单细胞转录组，来源未详述，但规模极大。
    - 下游评测涵盖三大类任务：
        - **细胞状态注释**
        - **批次整合**
        - **扰动响应预测**
- **对比方法**：与“最先进的单细胞基础模型”全面对比，具体哪些模型摘要未列出，推断应包含 scGPT、Geneformer、scBERT、scFoundation 等代表性工作。
- **评测基准与指标**：摘要未给出具体指标名称，但强调“一致优于”，说明在多个独立基准上进行了系统性比较。

### 4. 资源与算力
- 论文**未明确说明**使用的 GPU 型号、数量、训练时长等算力细节。
- 仅提到最终训练的模型为 120 亿参数，处理了 3.9 亿级别样本，间接暗示需要大规模并行训练集群。但确切资源信息缺失，无法量化。

### 5. 实验数量与充分性
- **实验数量**：摘要提及“多种基准测试”（diverse benchmarks），涵盖三个性质迥异的下游任务；且明确进行了多轮对比（“consistently outperformed”），推断至少包含 **细胞注释、批次整合、扰动预测**三大类数十项细分实验。
- **充分性与公平性**：
    - 充分性较高：所选任务代表了单细胞基础模型的核心应用维度，且统一使用预训练模型，评估维度较全面。
    - 公平性：与现有最优模型对比，但摘要未描述是否网格调参、特征提取协议是否一致等细节。
- **缺乏细节**：没有消融实验具体提及，无法判断三阶段各自的贡献是否被量化，因此实验完整性有待确认。

### 6. 论文的主要结论与发现
- CellOS 通过学习互补视图间的预测对齐，成功构建了 120 亿参数的细胞世界模型。
- 在细胞注释、批次整合及扰动预测等关键基准上，CellOS 的表现**全面超越当时的最优单细胞基础模型**。
- 核心发现：**互补细胞视图之间的预测对齐**是一条可扩展的道路，通向以表示为中心的细胞世界模型和可迁移的 AI 虚拟细胞。

### 7. 优点
- **创新性多视图框架**：将联合嵌入预测架构引入单细胞领域，不依赖于朴素的表达重构，而是对齐互补视图的潜在表示，更具生物物理直觉。
- **可扩展训练方案**：三阶段设计（因果语言建模 → MoE 扩展 → JEPA 对齐）巧妙解耦了容量提升与表示学习，适合从数亿细胞数据中训练超大模型。
- **表现优异且通用**：在三种性质差异巨大的下游任务上均取得 SOTA，验证了方法的鲁棒性和泛化能力。
- **宏大的愿景**：明确提出“世界模型”和“AI 虚拟细胞”概念，具有明确的长远科学目标。

### 8. 不足与局限
- **算力及实验细节缺失**：摘要未给出任何 GPU 资源、训练时间等关键信息，复现性和成本评估困难。
- **消融实验不明确**：三阶段各自的重要性未在摘要中量化，无法判断因果语言建模或 MoE 扩张是否为必须。
- **视图定义模糊**：“感知”视图的具体内涵（如蛋白质丰度、形态学图像？）未在摘要中解释，可能造成理解上的歧义。
- **数据偏差风险**：3.9 亿细胞的数据来源、物种、组织分布未知，可能存在批次效应、平台偏差或物种单一性。
- **应用限制**：超大参数量可能不利于资源有限的实验室微调；作为世界模型，仅有转录组和未知感知视图，与真正的全方位细胞模拟仍有距离。
- **验证不足**：摘要仅覆盖三项任务，未提 zero‑shot 扰动预测、基因‑疾病关联推断等实用场景的验证。

（完）
