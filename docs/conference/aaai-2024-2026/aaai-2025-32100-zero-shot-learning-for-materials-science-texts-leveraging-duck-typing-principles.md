---
title: "Zero-Shot Learning for Materials Science Texts: Leveraging Duck Typing Principles"
title_zh: 基于鸭子类型原则的材料科学文本零样本学习
authors: "Xin Zhang, Peiliang Zhang, Jingling Yuan, Lin Li"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/32100/34255"
tags: ["query:mt"]
score: 9.0
evidence: 利用大语言模型的零样本材料科学文本挖掘
tldr: 针对材料科学文本挖掘中描述符在不同任务间含义不同的问题，提出MatDuck方法，利用大语言模型结合鸭子类型原则，零样本地从科学文献中提取属性与合成操作，无需任务特定微调，为管理科学中的跨领域文本分析提供了灵活的范式。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32100/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 861, \"height\": 632, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32100/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1825, \"height\": 898, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32100/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 160, \"height\": 50, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32100/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 857, \"height\": 617, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32100/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 863, \"height\": 517, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32100/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 863, \"height\": 512, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32100/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 798, \"height\": 445, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32100/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1826, \"height\": 1123, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32100/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1754, \"height\": 432, \"label\": \"Table\"}]"
motivation: 材料科学文本中描述符因任务而异，现有微调方法难以泛化。
method: 提出MatDuck，利用LLM唤起材料领域知识进行零样本文本挖掘。
result: 在属性提取和合成操作检索等任务上达到良好性能。
conclusion: 该方法展示了LLM在领域文本挖掘中的潜力，可迁移至管理科学文献分析。
---

## Abstract
Materials science text mining (MSTM), involving tasks like property extraction and synthesis action retrieval, is pivotal for advancing research by deriving critical insights from scientific literature. Descriptors, serving as essential task labels, often vary in meaning depending on researchers' usage purposes across different mining tasks. (e.g., 'Material' can refer to both synthesis components and participants in fuel cell experiment). This meaning difference makes it difficult for existing methods, fine-tuned to specific task, to handle the same descriptors in other tasks. To overcome above limitation, we propose MatDuck, a simple and effective approach for Zero-Shot MSTM by evoking material knowledge within Large Language Models (LLMs). Specifically, inspired by the Duck Typing principles in programming languages, we present a ClassDefinition-Style Descriptor generation method that evokes task-specific characteristics to address usage variation. Subsequently, we introduce code-style in-context learning for zero-shot tasks, reframing them into code to leverage LLMs' proficiency in code understanding. Extensive experiments on eight benchmark datasets demonstrate that MatDuck, as a plug-and-play approach, significantly improves the Zero-Shot MSTM performance of LLMs by an average of 11.3% across seven tasks.

---

## 论文详细总结（自动生成）

### 论文核心问题与整体含义
- **研究动机**：材料科学文本挖掘（MSTM）中，描述符（如“Material”）在不同任务中因使用目的不同而含义各异（例如既可指合成组分，也可指燃料电池实验参与者）。这导致现有针对特定任务微调的模型难以泛化，重新标注或微调成本高，尤其对无 NLP 背景的材料研究者不友好。
- **整体含义**：提出**MatDuck**，一种即插即用的零样本 MSTM 方法，利用大型语言模型（LLM）中蕴含的材料知识和代码理解能力，不依赖任何任务特定微调或外部知识库，实现跨任务的描述符意义锚定与文本挖掘。

### 方法论
- **核心思想**：受编程语言中“鸭子类型”原则启发，提炼两条原则：  
  **原则 I**：若对象类型未知，可通过其特征属性识别；  
  **原则 II**：若描述符意义不明确，可通过相关任务文本推断。  
  基于此，通过“类定义风格描述符”和“代码风格上下文学习”增强 LLM 的零样本能力。
- **关键技术细节**：
  1. **类定义生成**：对每个描述符 `d`，利用 LLM 思维链生成多种潜在类定义，包括类注释 `cd_i` 和属性 - 注释对 `(P, C_p)_i`，形成 `γ_i`，作为描述符的多种意义表示。
  2. **ClassDefinition-Style 描述符适配**：从任务文本中随机采样，统计各类定义在文本中的实例出现次数，计算适配度 `Φ`，选出最佳适配的定义作为任务专属描述符。
  3. **代码风格任务推理**：将识别任务重构为 Python 代码形式，把描述符定义为任务类的子类，文本作为变量，利用 LLM 的对象类识别能力进行零样本分类。
- **算法流程**（伪代码见 Algorithm 1）：
  - 对每个描述符生成多个类定义（含属性和注释）；
  - 初始化对象计数器，遍历任务文本统计实例出现次数；
  - 选适配度最大的类定义作为 ClassDefinition-Style 描述符；
  - 推理阶段将文本和描述符打包为代码格式提示，预测类别。
- **查询复杂度**：总查询次数 ≈ `n + m·h·(1+r)·(1+k)`，其中 `n` 为样本数，`m` 为描述符数，`h`、`r`、`k` 为生成参数，主要与样本量线性相关，可接受。

### 实验设计
- **数据集**：8 个公开材料科学文本基准数据集，涵盖 7 个任务：命名实体识别（NER）、关系抽取（RE）、事件属性抽取（EAE）、段落分类（PC）、合成动作检索（SAR）、句子分类（SC）、槽填充（SF）。描述符总误用率达 31.6%，NER 任务高达 43.9%。
- **对比方法**：
  - **API‑LLM**：GPT‑4、GPT‑3.5（ChatGPT）、Claude3.5；
  - **开源 LLM**：LLaMA3‑8B；
  - **材料域 LLM**：HoneyBee‑7B/13B（在材料指令数据上微调）；
  - **微调 BERT**：MatBERT、MatSciBERT。
- **评价指标**：Macro‑F1 和 Micro‑F1。
- **实现细节**：GPT‑4 版本为 GPT‑4-Turbo，Claude3.5 为 Sonnet，LLaMA3‑8B 在两块 NVIDIA RTX‑4090 GPU 上评估。属性数量默认 5，任务适配随机采样 10% 文本。

### 资源与算力
- 文中明确指出使用两块 **NVIDIA RTX‑4090 GPU** 评估开源模型 LLaMA3‑8B。其余商业模型通过 API 访问，未提及其计算资源。未报告训练时长或具体能耗（因为 MatDuck 为纯推理，无训练阶段）。

### 实验数量与充分性
- **主实验**：表 2 对比了 MatDuck（基于三种 backbone）与多个零样本基线及微调 BERT 在 7 个任务上的性能（Macro‑F1 和 Micro‑F1）。
- **消融实验**：表 3 逐模块分析“去除属性”“去除任务适配”“去除代码风格”的影响。
- **超参数分析**：图 4 展示属性数量从 1 到 8 时的性能变化。
- **适配有效性分析**：图 5 分析按适配度排序后真实最优类定义的位置分布。
- **评估层面**：对比方法覆盖匿名、开源、商业和领域微调模型，指标统一，实验设计客观完整，消融研究充分验证了各组件贡献。

### 主要结论与发现
- MatDuck 在零样本设置下，将 LLaMA3‑8B 的整体 Macro‑F1 平均提升 **11.3%**；以 GPT‑4 为 backbone 提升 **10.4%**，以 Claude3.5 提升 **8.6%**。
- 在 Claude3.5 加持下，MatDuck 的 Macro‑F1 达到 **0.697**，超越微调的 MatSciBERT（0.671）。
- 增强后的 LLaMA3‑8B 性能（0.629）超过原始 GPT‑4（0.557）和 Claude3.5（0.584），为资源有限者提供高性价比方案。
- 消融结果证实：**属性定义**、**任务适配**和**代码风格**各自对性能有显著贡献；属性数量存在最优区间（4‑6 个），过少或过多均会削弱效果。
- 所提任务适配方法能将最优类定义大概率排在前两位，有效缓解描述符意义偏移问题。

### 优点
- **创新性**：首次将鸭子类型原则引入 MSTM，设计出任务无关的零样本 In‑Context Learning 框架，无需微调或外部知识。
- **技术亮点**：通过类定义生成和代码风格提示，充分挖掘 LLM 的对象识别与代码理解能力，实现即插即用。
- **实验扎实**：在 7 类任务、8 个数据集上全面评估，与多种基线对比，并进行多维度消融，结果可靠。
- **实用性强**：对非 NLP 专业的材料研究者友好，开源模型加轻量查询即可获得媲美甚至超越商业 API 的性能。

### 不足与局限
- **查询成本**：虽然复杂度可接受，但每次推理仍需多次调用 LLM，对超大文本量可能累积较高成本；未与直接提示等更简洁的零样本方法进行成本对比。
- **领域泛化**：仅在材料科学数据集上验证，未探索其他科学领域的迁移效果。
- **依赖 LLM 内部知识**：性能受 backbone 模型自身材料知识储备影响，若 LLM 缺乏相关知识，类定义生成可能不准确。
- **超参数敏感性**：属性数量、采样比例等需要根据任务调整，自动化程度不足。
- **评估局限**：缺少多次运行的置信区间或统计显著性检验；未分析不同描述符误用率下的细分表现。
- **资源限制**：开源模型实验仅在 4090 上进行，未报告推理时间，可能低估了在 CPU 或低资源环境下的实际开销。
- **潜在偏差**：类定义适配所使用的随机文本采样可能引入选择偏差，影响适配稳定性。

（完）
