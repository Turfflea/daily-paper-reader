---
title: "SAIL: Sample-Centric In-Context Learning for Document Information Extraction"
title_zh: SAIL：面向文档信息抽取的以样本为中心的上下文学习
authors: "Jinyu Zhang, Zhiyuan You, Jize Wang, Xinyi Le"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/34780/36935"
tags: ["query:mt"]
score: 8.0
evidence: 通过以样本为中心的上下文学习进行文档信息抽取
tldr: 针对从视觉丰富文档中提取结构化信息的挑战，本文提出SAIL方法，采用以样本为中心的上下文学习，仅需少量示例即可让大型语言模型完成信息抽取。SAIL通过细粒度实体级表示有效处理文档中布局与文本的复杂关系，为预训练模型提供精确指导。实验表明，该方法在多个文档信息抽取基准上取得了强泛化性能，显著优于传统全训练方法，为无训练推理场景提供了高效解决方案。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34780/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 830, \"height\": 605, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34780/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1806, \"height\": 875, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34780/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 812, \"height\": 370, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34780/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 844, \"height\": 787, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34780/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1710, \"height\": 941, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34780/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 882, \"height\": 992, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34780/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 782, \"height\": 226, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34780/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1735, \"height\": 294, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34780/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 792, \"height\": 220, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34780/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 802, \"height\": 312, \"label\": \"Table\"}]"
motivation: 文档信息抽取任务中，全训练方法泛化能力有限，而训练自由方法面临理解布局与文本复杂关系及准确引导预训练模型的挑战。
method: 提出SAIL，引入细粒度实体级表示，通过以样本为中心的上下文学习策略，利用少量示例指导LLM进行文档信息抽取。
result: 在多个基准上，SAIL展现出强大的泛化性能，优于全训练方法和现有训练自由方法。
conclusion: SAIL兼顾了文档布局理解与模型引导，为视觉丰富文档的信息抽取提供了高效且泛化好的方法。
---

## Abstract
Document Information Extraction (DIE) aims to extract structured information from Visually Rich Documents (VRDs). Previous full-training approaches have demonstrated strong performance but may struggle with generalization to unseen data. In contrast, training-free methods leverage powerful pre-trained models like Large Language Models (LLMs) to address various downstream tasks with only a few examples. Nonetheless, training-free methods for DIE encounter two primary challenges: (1) understanding the complex relationship between layout and textual elements in VRDs, and (2) providing accurate guidance to pre-trained models. To address these challenges, we propose SAmple-centric In-context Learning (SAIL). SAIL introduces a fine-grained entity-level textual similarity to facilitate in-depth text analysis by LLMs and incorporates layout similarity to enhance the analysis of layouts in VRDs. Moreover, SAIL formulates a unified In-Context Learning (ICL) prompt template for various sample-centric examples, enabling tailored prompts that deliver precise guidance to pre-trained models for each sample. Extensive experiments on FUNSD, CORD, and SROIE benchmarks with various base models (e.g., LLMs) indicate that our SAIL outperforms training-free baselines, even closer to the full-training methods, showing the superiority and generalization of our method.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**： 文档信息抽取（DIE）旨在从视觉丰富文档（VRDs，如发票、表单）中提取结构化信息。传统全训练方法在训练数据分布外的泛化能力差，而基于大语言模型（LLM）的无训练方法虽泛化性更好，但面临两大挑战：
    1.  **布局与文本关系理解难**：VRDs中离散的文本元素与灵活的布局结构关系复杂，LLM难以有效捕捉。
    2.  **模型引导难**：如何为每个测试样本提供精确、有效的上下文提示，以充分发挥预训练LLM的性能。
- **整体含义**： 提出一种以样本为中心的上下文学习（In-Context Learning， ICL）方法，旨在无需微调模型的情况下，通过为每个测试样本动态生成定制化的提示，显著提升LLM在文档信息抽取任务上的表现和泛化能力。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**： 设计一个**以样本为中心**的上下文学习框架 **SAIL**，为每个测试样本动态选择多样化的相似示例，并整合进一个统一的提示模板中，从而为LLM提供精确、深入的引导。

- **关键技术细节**：
    - **多维度示例选择**： 为克服单一文本相似性的不足，SAIL选择了三类示例：
        1.  **文档级文本相似示例**： 将文档内所有文本拼接，用Sentence-BERT编码，通过余弦相似度检索整篇文档内容最相似的示例。
        2.  **实体级文本相似示例**： 过滤掉纯数字文本后，对文档中的每个文本实体单独编码，检索语义最相似的实体示例。这有助于LLM进行细粒度文本分析。
        3.  **布局相似示例**： 将所有文本框绘制在黑底图像上，裁剪并调整为统一尺寸，然后使用均方误差（MSE）计算图像间相似度，找到布局结构最相似的文档示例。
    - **统一的提示模板**： 构建一个包含五部分的统一提示模板，将上述三种示例有机整合：
        1.  **候选标签说明（Ccl）**： 枚举所有可能的标签及其含义。
        2.  **实体级文本示例（Cet）**： 展示相似实体及其对应的标签。
        3.  **布局示例（Cl）**： 提供布局相似文档的完整信息，并要求LLM **显式分析**这些文档中各类标签的空间位置规律，生成布局分析（Al），辅助其理解布局。
        4.  **文档级文本示例（Cdt）**： 以“问题-答案”对的形式展示相似文档及其正确标注。
        5.  **测试问题（φ(T, B））**： 将待抽取文档的文本和坐标信息格式化后作为最终问题。

- **算法流程与公式**：
        - 给定测试文档，提取其中的文本实体及其边界框。
        - 根据上述三种相似度度量方法，从训练池中分别检索出最相似的示例。
        - 将这些示例填充到统一提示模板的相应部分，构成最终的上下文提示（C）。
        - LLM基于该提示和测试问题，最大化生成正确标签序列的条件概率 `P(Y|T,B) = 1/ne * Σ PLM(lk|C, φ(T,B))`。
        - 从LLM的输出中提取预测的实体标签。

### 3. 实验设计：使用了哪些数据集/场景，它的benchmark是什么，对比了哪些方法
- **数据集/Benchmark**：
    - **FUNSD**: 扫描表单理解数据集，用于评估键值对抽取。
    - **CORD**: 收据理解数据集，标签层次化且数量多（30个），任务较复杂。
    - **SROIE**: 扫描发票信息抽取数据集，用于提取公司、日期、地址、总金额四个字段。
- **对比方法**：
    - **全训练方法**： BERT, LiLT, BROS, XYLayoutLM, LayoutLMv1/v2/v3等，作为性能上界参考。
    - **少样本方法**： 上述部分模型的少样本微调版本。
    - **无训练方法**： 标准ICL、ICL-D3IE。这是主要的竞争对手。
    - **多模态大模型（MLLMs）**： GPT-4o和LLaVA-v1.5-7B，通过提供文档图像和任务指令直接测试其零样本抽取能力。
- **评估指标**： 实体级别的F1分数、精确率（Precision）和召回率（Recall）。
- **基础LLM**： 使用不同级别的LLM来测试方法泛化性，包括 ChatGLM3、GPT-3.5、GPT-4o。

### 4. 资源与算力：如果文中有提到，请总结使用了多少算力
- **未明确说明**： 论文全文未提及具体的硬件配置（如GPU型号和数量）、模型推理耗时或任何算力消耗。由于该方法是无训练的，主要算力消耗在于调用API（GPT-3.5, GPT-4o）或本地推理开源模型（ChatGLM3）。

### 5. 实验数量与充分性：大概做了多少组实验，这些实验是否充分、是否客观、公平
- **实验数量**： 论文进行了较丰富的实验验证，大致包含以下几组：
    1.  **主实验**：在3个数据集、3个不同LLM上，与多种基线方法（全训练、少样本、无训练）进行全面对比。
    2.  **多模态LLM对比实验**：将SAIL与GPT-4o、LLaVA等MLLM直接比较。
    3.  **消融实验**：
        - 验证自适应示例 vs. 固定示例的有效性。
        - 验证三种不同相似度示例（文档级文本、实体级文本、布局）各自的作用。
        - 研究示例在提示中的排列顺序对性能的影响。
        - 研究布局分析步骤的有效性。
- **充分性与公平性**： 实验设计**充分且公平**。
    - **客观公平**： 对比基线广泛，涵盖了不同范式的SOTA模型。对ICL-D3IE等关键基线，在更换LLM时，使用了其官方代码进行评估，保证了公平性。
    - **充分性高**： 多数据集、多模型验证了方法的有效性和鲁棒性。详细的消融实验清晰地展示了各核心组件的贡献。

### 6. 论文的主要结论与发现
- **性能优越**： SAIL在所有数据集和基础LLM上（ChatGLM3, GPT-3.5, GPT-4）均稳定地（sometimes significantly）超越了训练自由基线方法（如ICL-D3IE），性能甚至接近于全训练方法。
- **强泛化性**： SAIL在不同LLM之间切换时性能下降较小，表现出更好的模型迁移能力和鲁棒性。
- **多模态LLM的局限性**： 发现即使是GPT-4o这样的强大MLLMs，在直接进行文档信息抽取任务时效果有限，凸显了本文方法的贡献和价值。
- **组件有效性**：
    - 为每个样本自适应的选择示例远比使用固定示例有效。
    - 实体级文本相似度和布局相似度是文档级文本相似度的有力补充，尤其对于长文档（FUNSD）和复杂布局文档（CORD, SROIE）至关重要。
    - 显式要求LLM进行**布局分析**能持续提升性能。
    - 按相似度降序排列示例更有利于LLM理解。

### 7. 优点：方法或实验设计上有哪些亮点
- **即插即用，无需训练**： 方法完全无需训练或微调，可灵活应用于任何支持ICL的LLM。
- **样本中心的设计**： 动态地为每个测试样本构建定制化提示，思想新颖，针对性更强。
- **细粒度的示例选择**： 创新性地引入实体级文本相似度和基于图像的布局相似度，从多角度捕捉文档特性，解决了单一文档级相似度信息不足的问题。
- **可引导的布局分析**： 将布局信息通过“要求LLM分析”并生成文本描述的方式融入提示，这是一种巧妙且有效的隐式注入layout先验的方法。
- **实验说服力强**： 对比基线全面，消融实验细致，多LLM、多数据集的验证充分证明了方法的有效性和鲁棒性。

### 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等
- **依赖OCR质量**： 方法强依赖于上游OCR系统输出的文本和坐标的准确性，这是一个潜在的误差传播点，但论文未讨论OCR噪声的影响。
- **推理成本与效率**： 为每个样本进行多次语义检索、布局比较以及多次调用LLM（特别是布局分析步骤），导致推理过程的计算和时间成本较高。
- **应用场景有限**： 目前仅在英语的发票、表单、收据等文档类型上验证，向其他语言、手写文档或结构更复杂的文档（如报纸、杂志）的泛化能力未知。
- **示例选择上限**： 方法的性能本质上受限于训练示例池的多样性和覆盖率。如果示例池中没有与测试样本足够相似的样例，性能可能会下降。
- **实体级相似度依赖启发式规则**： 该方法直接过滤掉了纯数字文本，这可能在某些“金额”、“数量”等数字为关键信息的场景下遗漏有效示例，选择机制较为简单。

（完）
