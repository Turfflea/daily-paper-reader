---
title: Boosting Short Text Classification with Multi-Source Information Exploration and Dual-Level Contrastive Learning
title_zh: 基于多源信息探索与双层级对比学习的短文本分类增强方法
authors: "Yonghao Liu, Mengyu Li, Wei Pang, Fausto Giunchiglia, Lan Huang, Xiaoyue Feng, Renchu Guan"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/34650/36805"
tags: ["query:mt"]
score: 9.0
evidence: 双层级对比学习用于短文本上的自监督表征增强
tldr: 短文本分类面临语义稀疏和标注不足难题。提出MI-DELIGHT模型，首先整合统计、语言和事实等多源信息缓解稀疏性，然后通过图学习方法对短文本进行表征，并引入实例级和簇级双层级对比学习作为自监督辅助任务，在不增加标签需求的情况下有效捕获判别性特征，显著提升了短文本分类的性能，为文本挖掘中的小样本学习提供了借鉴。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34650/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1836, \"height\": 672, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34650/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1417, \"height\": 268, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34650/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1485, \"height\": 1118, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34650/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1649, \"height\": 554, \"label\": \"Table\"}]"
motivation: 短文本语义稀疏且标注样本不足，导致分类困难。
method: 利用多源信息丰富表示，构建图学习模型，并设计双层级对比学习自监督优化判别特征。
result: 在多个短文本分类数据集上，MI-DELIGHT优于现有强基线，尤其在小样本下优势明显。
conclusion: 多源信息与自监督对比学习结合可有效克服短文本数据稀缺和语义稀疏问题。
---

## Abstract
Short text classification, as a research subtopic in natural language processing, is more challenging due to its semantic sparsity and insufficient labeled samples in practical scenarios. We propose a novel model named MI-DELIGHT for short text classification in this work. Specifically, it first performs multi-source information (i.e., statistical information, linguistic information, and factual information) exploration to alleviate the sparsity issues. Then, the graph learning approach is adopted to learn the representation of short texts, which are presented in graph forms. Moreover, we introduce a dual-level (i.e., instance-level and cluster-level) contrastive learning auxiliary task to effectively capture different-grained contrastive information within massive unlabeled data. Meanwhile, previous models merely perform the main task and auxiliary tasks in parallel, without considering the relationship among tasks. Therefore, we introduce a hierarchical architecture to explicitly model the correlations between tasks. We conduct extensive experiments across various benchmark datasets, demonstrating that MI-DELIGHT significantly surpasses previous competitive models. It even outperforms popular large language models on several datasets.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：短文本分类在实际应用中面临两大挑战：语义稀疏（文本极短，缺乏上下文）和标注样本不足（尤其在垂直领域）。
- **研究背景**：传统方法依赖大规模标注数据，在小样本下容易过拟合。现有工作或引入外部知识丰富语义，或采用图学习进行半监督分类，但在有效利用大量未标注数据和多粒度语义方面仍有不足。
- **整体含义**：论文旨在通过多源信息补全和双层级对比学习相结合，在不增加标注成本的情况下，充分挖掘未标注数据中的细粒度与粗粒度对比信号，从而大幅提升短文本分类性能。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **多源信息探索**：
  - **统计信息**：基于词共现构造词图 \(G^w\)，使用 PMI 计算边权，节点特征由 GloVe 初始化，经 GCN 聚合得到富含统计信息的特征 \(H^w\)。
  - **语言信息**：根据词性标注构建词性图 \(G^p\)，同样使用 PMI 建边，节点特征为 one-hot，经 GCN 得到 \(H^p\)。
  - **事实信息**：利用知识图谱抽取实体，构建实体图 \(G^e\)，实体嵌入由 TransE 初始化，边权由余弦相似度计算，经 GCN 得到 \(H^e\)。
  - **文本表征融合**：通过 TF-IDF 或二值关联将图中节点特征聚合为文本表示 \(Z^{\pi}\)，并拼接得到最终文本嵌入 \(Z\)。
- **数据增强**：通过 WordNet 同义词替换生成正样本对，构建增强语料 \(D^{\text{aug}}\)。
- **双层级对比学习与层次结构**：
  - **实例级对比学习**：直接使用原始特征 \(Z\) 进行归一化，最大化同一样本的不同视图间一致性，损失为经典的 InfoNCE 形式 \( \mathcal{L}_{\text{ICL}} \)。
  - **簇级对比学习**：在实例级特征基础上，利用余弦相似度和连通分量标记算法生成伪簇标签，并在投影头映射后的特征空间上交换监督信号进行对比，损失为 \( \mathcal{L}_{\text{CCL}} \)。
  - **层次结构**：任务复杂度由低到高依次进行 ICL → CCL → 分类，每一级特征为下一级提供更好的初始化和语义抽象，显式建模任务间因果关系。
- **模型优化**：总损失为分类交叉熵损失与两项对比损失加权和 \( \mathcal{L} = \mathcal{L}_{\text{CE}} + \eta \mathcal{L}_{\text{ICL}} + \zeta \mathcal{L}_{\text{CCL}} \)。

### 3. 实验设计
- **数据集**：Twitter、MR、Snippets、Ohsumed、TagMyNews 五个公开短文本数据集，涵盖二分类和多分类场景，标注样本比例极低（每类仅 20 条训练数据）。
- **对比方法**：包含四大类基线：
  - 传统模型（TF-IDF+SVM、PTE）；
  - 深度学习模型（CNN、LSTM、BERT-avg、BERT-cls）；
  - 图神经网络模型（TLGNN、HyperGAT、TextING、DADGNN、TextGCN）；
  - 深度短文本模型（STCKA、HGAT、STGCN、SHINE、NC-HGAT、GIFT）；
  - 大语言模型（GPT-3.5、Bloom-7.1B、Llama2-7B、Llama3-8B）。
- **评价指标**：准确率（ACC）和宏观 F1 分数。

### 4. 资源与算力
- 论文并未明确给出 GPU 型号、数量或具体训练时长。仅在介绍大语言模型实验时提及“due to computational resource constraints, we only fine-tune approximately 7B LLMs through some GPU reduction techniques”，暗示使用可支持 70 亿参数模型推理的 GPU 资源，但无进一步细节。

### 5. 实验数量与充分性
- **主实验**：在 5 个数据集上与 15 个以上基线方法对比，重复 10 次取平均值，使用 t-test 验证显著性。
- **消融实验**：设计了 7 种变体（移除词图、词性图、实体图、双层级 CL、仅去除 CCL、仅去除 ICL、并行结构）验证各模块贡献。
- **数据增强变体实验**：对比删除、上下文替换和 WordNet 同义词替换三种增强策略。
- **其他分析未详细列出但可能涉及**：论文提到在实验中探讨不同数据增强的影响。
- **充分性与公平性**：实验覆盖多种类型方法，包括最新 LLM，对比公正；消融实验全面，能充分说明各组件有效性；评估方式标准，结论可信。

### 6. 论文的主要结论与发现
- MI-DELIGHT 在所有基准数据集上显著超越已有强基线方法，甚至在部分数据集上优于 GPT-3.5、Llama 等大语言模型。
- 多源信息（尤其词图）和双层级对比学习对缓解语义稀疏与标注稀缺问题至关重要。
- 层次结构通过挖掘任务间关联，进一步提升了模型性能。
- 无监督数据量越大，模型提取自监督信号的能力越强，性能提升越明显。

### 7. 优点：方法或实验设计上的亮点
- **多源信息融合**：从统计、语言、外部事实三个维度丰富短文本表示，有效缓解语义稀疏。
- **双层级对比学习**：结合实例级和簇级对比，同时捕捉细粒度与粗粒度判别信息，优于单一层级。
- **层次化任务建模**：主动设计任务间的递进依赖关系，而非简单并行，符合人类认知由浅入深的过程，提升特征抽象能力。
- **实验全面**：对比基线丰富，包含传统、深度学习、GNN、专业短文本模型及 LLM，结论具有说服力。
- **开源代码**：提供了 GitHub 链接，方便复现。

### 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等
- **计算资源描述缺失**：未报告训练时间、GPU 等具体资源需求，难以评估实际部署效率。
- **领域泛化性**：仅在 5 个英文基准数据集上测试，对低资源语言或极端稀疏场景的效果未知。
- **图谱构建依赖外部工具**：实体链接和质量受 TAGME 和知识图谱覆盖度影响，迁移到新领域可能需要额外适配。
- **伪标签质量依赖对比学习效果**：簇级对比生成的伪簇在初期可能含有噪声，若引入自适应过滤机制可能更稳定。
- **数据增强敏感**：实验结果依赖 WordNet 同义词替换，其他增强方式可能效果波动。

（完）
