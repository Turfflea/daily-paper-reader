---
title: Microbial Named Entity Recognition and Normalisation for AI-assisted Literature Review and Meta-Analysis
title_zh: 用于人工智能辅助文献综述与元分析的微生物命名实体识别与标准化
authors: "Patel, D., Lain, A. D., Vijayaraghavan, A., Faghih Mirzaei, N., Mweetwa, M. N., Wang, M., Beck, T., Posma, J. M."
date: 2026-06-09
pdf: "https://www.biorxiv.org/content/10.1101/2025.08.29.671515v2.full.pdf"
tags: ["query:mt"]
score: 9.0
evidence: 深度学习算法用于微生物组文献的命名实体识别与标准化
tldr: "面向微生物组文献高效分析的需求，构建了首个微生物专有文本语料库，并训练了基于Transformer的命名实体识别和标准化模型。微调BioBERT的实体识别F1值达96%，标准化准确率91%，单篇全文仅需7秒，大幅超越传统规则方法。该工具已用于全文献荟萃分析，识别出14个领域的显著关联微生物类群，并可集成到文献综述工作流，显著提升综述速度与准确性，为微生物组文本挖掘提供强大支持，加速科学发现。"
source: biorxiv
selection_source: fresh_fetch
motivation: 微生物组文献快速增长，手动标注低效且通用大语言模型缺乏领域知识，亟需专用命名实体识别与标准化工具。
method: 构建包含1410篇全文的微生物语料库，微调BioBERT等模型，在288篇金标准测试集上评估NER与标准化性能。
result: "微调BioBERT的NER F1达96%，标准化准确率91%，显著优于规则方法(69%)，处理单篇全文仅需7秒，性能远超传统管道。"
conclusion: 该模型实现了高精度、高速的微生物命名实体识别与标准化，可无缝集成文献综述流程，极大提升荟萃分析效率。
---

## 摘要
动机 生物医学文献的人工整理缓慢且易出错，尽管在通用文本上训练的大语言模型（LLMs）已被证明对文本摘要很有用，但这些方法缺乏准确执行此任务所需的领域专业知识。在此，我们描述了首个微生物组特定文本语料库的创建，使用它训练用于命名实体识别（NER）和标准化（NEN）的深度学习算法，并展示其在微生物组文献元分析中的应用。

方法 我们开发了一个自动化流程，注释了1,410篇全文微生物组文章中所有细菌、古菌和真菌的提及。我们手动注释（金标准）了一个包含288份文档的独立测试集。我们训练了不同的基于Transformer的语言模型，用于微生物组识别和标准化到分类标识符，并使用测试集上的精确率、召回率、F1值和准确性评估其性能。最佳模型用于自动注释所有可获取的开放获取全文微生物组文章（n=6,927），并识别在14个领域中显著过度代表的分类群。

结果 训练和验证集总共包含90,150个注释（包括长格式和缩写）。使用金标准测试集，标注者间一致率NER为99.52%，NEN为88.31%，对训练好的模型进行评估，我们微调的BioBERT模型实现了NER的96% F1值，超越了基于规则和字典的注释流程（94%）。对于NEN，深度学习模型获得的准确性大大优于流程（91%对69%）。在整个可用文献中评估，我们的模型仅需7秒即可注释完整全文文档。

结论 我们的算法具有近乎完美的精确度，大大加快了全文文章中微生物的注释过程。我们通过分析整个可用文献展示了这些方法的能力，并在元分析中描述了与每个领域相关的分类群，举例说明了如何将这些方法集成到文献综述工作流程中，从而提高结果的速度和准确性。

可用性 用于自动注释、模型训练和生成可视化数据的分类树的所有代码和数据将在同行评审后提供，并附有如何在新文本上部署模型的说明，从https://github.com/omicsNLP/microbELP获取。

## Abstract
MotivationManual curation of biomedical literature is slow and error-prone and while large language models (LLMs) trained on general texts have shown to be useful for text summarisation, these methods lack the domain-specific expertise required to perform this task accurately. Here we describe the creation of the first microbiome-specific text corpus, use this to train deep learning algorithms for named-entity recognition (NER) and normalisation (NEN), and demonstrate their use to meta-analyse microbiome literature.

MethodsWe developed an automated pipeline to annotate all mentions of bacteria, archaea, and fungi in 1,410 full-text microbiome articles. We manually annotated (gold-standard) a separate test set of 288 documents. We trained different transformer-based language models for microbiome recognition and normalisation to taxonomic identifiers and evaluate their performance using the precision, recall, F1-score, and accuracy on the test set. The best models were used to automatically annotate all available Open Access, full-text microbiome articles (n=6,927) and identify taxa that are significantly overrepresented across 14 domains.

ResultsThe training and validation set contained a total of 90,150 annotations (both long form and abbreviations). Using the gold-standard test set, with an inter-annotator agreement rate of 99.52% for NER and 88.31% for NEN, the trained models were evaluated and our fine-tuned BioBERT model achieved an F1-score of 96% for NER surpassing a rule- and dictionary-based annotation pipeline (94%). For NEN the accuracy obtained by the deep learning models greatly surpassed that of the pipeline (91% vs 69%). Evaluated across the entire available literature, our models annotate an entire full-text document in only 7 seconds.

ConclusionOur algorithms have near perfect precision and greatly speed up the process of annotating microbes in full-text articles. We demonstrated the capabilities of these methods by analysing the entire available literature and describe the taxa associated with each of the domains in our meta-analysis, and exemplify how these methods can be integrated into literature review workflows improving both the speed and accuracy of results.

AvailabilityAll codes and data for automatic annotation, model training, and generation of taxonomic trees visualising the data will be made available following peer review with instructions on how to deploy the model on new texts from https://github.com/omicsNLP/microbELP.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义

*   **研究动机**
    *   微生物组相关文献呈指数增长，依靠人工从海量文献中提取微生物实体（细菌、古菌、真菌）并进行分类标准化，效率极低且容易出错。
    *   通用大语言模型在文本摘要等下游任务中表现亮眼，但缺乏微生物领域的专有知识，无法准确完成命名实体识别与标准化这类需要强先验约束的任务。
*   **整体含义**
    *   本文构建了第一个微生物组领域的专有文本语料库，并基于此训练了高精度的命名实体识别与标准化模型。
    *   该工具链可将整篇文献的微生物注释时间压缩至秒级，并直接服务于全文献规模的荟萃分析，为加速微生物组领域的科学发现提供了可嵌入文献综述工作流的实用工具。

### 2. 论文提出的方法论

*   **核心思想**
    *   先以自动管道生成带噪声的大规模弱标签数据，再结合小规模人工金标准数据，微调领域适配的Transformer模型，以同时解决实体检测和分类ID映射两个子任务。
*   **关键流程**
    *   **语料库构建**：开发自动化注释管道，对1,410篇微生物组全文中的菌名提及（长格式与缩写）进行初步标注。独立构建包含288篇文献的手工金标准测试集。
    *   **命名实体识别**：将微生物识别视作序列标注任务，微调BioBERT等基于Transformer的语言模型，直接在文本中标记出细菌、古菌、真菌提及的位置。
    *   **命名实体标准化**：将识别出的微生物提及字符串映射到标准化的分类标识符（Taxonomic Identifier），本质上是一个实体链接任务，使用深度学习模型替代传统的纯规则/字典方法。
    *   **评估指标**：NER采用精确率、召回率、F1值；NEN采用准确率。通过标注者间一致性来估计金标准质量。
*   **部署与应用**
    *   将最优模型应用到全部可获取的开放获取全文微生物组文章（n=6,927），提取出每个领域（共14个）显著过度代表的分类群。

### 3. 实验设计

*   **数据集与场景**
    *   **训练/验证集**：1,410篇全文的自动管道注释，总计包含90,150个注释（含长格式和缩写）。
    *   **金标准测试集**：288篇独立文档，由人工严格按照相同规范标注。标注者间一致性：NER 99.52%，NEN 88.31%。
    *   **全文献集**：6,927篇可获取的开放获取微生物组全文，用于最终的下游荟萃分析。
*   **基准对比方法**
    *   **基于规则和字典的自动注释管道**：作为NER和NEN的传统方法基线。
    *   **不同Transformer语言模型**：虽然摘要未列举全部模型名，但明确提及对基于Transformer的模型进行了比较和筛选，最终微调后的BioBERT表现最优。
*   **评价体系**
    *   在金标准测试集上直接计算NER的F1值与NEN的准确率，并对比深度学习模型与传统管道的性能差异。同时记录单篇文献的处理耗时。

### 4. 资源与算力

*   文中**未明确说明**训练与推理所使用的具体硬件配置（如GPU型号、数量、显存大小）以及完整的训练时长。
*   仅提到模型在推理时，处理一篇完整全文的注释耗时仅需7秒，侧面反映了模型具备较高的推理效率，适合大规模文献处理。

### 5. 实验数量与充分性

*   **实验覆盖**
    *   实验涵盖了命名实体识别和标准化两个核心子任务。
    *   包含了基于规则/字典的传统方法与深度学习方法的直接对比。
    *   在NER上对比了不同Transformer模型的微调效果（选用BioBERT最优）。
    *   利用标注者间一致性量化了人工标注的可靠性。
*   **充分性与公平性评价**
    *   使用了独立的、规模可观（288篇）且人工精标的数据作为最终评测基准，避免了仅依赖自动管道评价所带来的偏差。
    *   对比方法涵盖经典方案，能够清晰体现深度学习方法的增益。
    *   然而，论文未详细报道消融实验（如对比不同规模的标注数据、冻结不同层的微调策略等），且NEN部分标注者间一致性仅为88.31%，可能限制了标准化准确率的上限。整体实验设计合理且客观，但内在细节披露尚不完整。

### 6. 论文的主要结论与发现

*   **模型性能**
    *   微调后的BioBERT在NER任务上达到**96%的F1值**，超过了传统规则/字典管道的94%。
    *   在更具挑战的NEN任务上，深度学习模型实现了**91%的准确率**，大幅领先传统管道的69%准确率。
*   **工程效能**
    *   算法实现了接近完美的精准度，并使整篇全文的注释过程加速到仅需7秒。
*   **生物领域洞见**
    *   通过对6,927篇开放获取全文进行全自动注释，成功识别出在14个不同研究领域中显著过度代表的微生物分类群，用实际用例证明该工具能够高效支撑大规模荟萃分析和文献综述工作。

### 7. 优点

*   **领域首创性**：创建了首个微生物组特化的高置信度文本语料库，为后续相关研究奠定了基础。
*   **任务完整性**：不仅解决实体识别，还进一步实现了从非结构化文本到标准化分类ID的端到端映射，实用性极强。
*   **显著的性能跃升**：在NEN任务上相比传统管道获得了22个百分点的巨大提升，展现了深度学习编码上下文语义信息的优势。
*   **高真实场景的验证**：通过处理近7,000篇全文的真实文献并提取领域关联分类群，有力证明了方法在现实支撑科研发现中的潜力。

### 8. 不足与局限

*   **训练与评估数据偏差**
    *   训练集的弱标签由自动管道生成，可能隐含原始规则的系统性偏差。
    *   测试集标注者间一致性在NEN上仅为88.31%，金标准并非完美，可能低估或限制了模型的真实上限。
    *   全文献分析仅限定于“开放获取”文章，可能引入选择偏差，无法覆盖所有微生物组出版物。
*   **方法对比的局限性**
    *   未与当前涌现的通用大语言模型的少样本或微调能力进行纵向比较。
    *   对于缩写识别、跨句指代消解、新发现菌株等挑战性场景，模块的鲁棒性未做详细报告。
*   **可复现性与细节缺失**
    *   关键的计算资源、超参数设置和具体训练迭代时长并未在摘要中给出，影响了复现的便利性。
*   **应用限制**
    *   模型专注于细菌、古菌、真菌；病毒、原生生物或其他微生物实体需要额外的扩展训练才能支持。

（完）
