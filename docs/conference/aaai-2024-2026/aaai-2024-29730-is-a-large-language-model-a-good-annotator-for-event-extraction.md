---
title: Is a Large Language Model a Good Annotator for Event Extraction?
title_zh: 大型语言模型是事件抽取的好标注器吗？
authors: "Ruirui Chen, Chengwei Qin, Weifeng Jiang, Dongkyu Choi"
date: 2024-03-25
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/29730/31254"
tags: ["query:mt"]
score: 9.0
evidence: 大语言模型作为事件抽取的标注器以增强训练数据
tldr: 针对事件抽取任务面临的数据稀缺和不平衡问题，提出将大型语言模型作为专家标注器，利用基准数据集中的样本作为提示，生成与真实数据分布一致的增强样本，有效提升事件抽取性能，为管理科学中的非结构化文本分析提供了数据增强新思路。
source: AAAI-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29730/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1466, \"height\": 491, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29730/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1458, \"height\": 489, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29730/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 901, \"height\": 411, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29730/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 832, \"height\": 299, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29730/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 827, \"height\": 604, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29730/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 828, \"height\": 961, \"label\": \"Table\"}]"
motivation: 事件抽取任务存在数据稀缺和类别不平衡问题。
method: 将LLM作为标注器，在提示中包含训练样本以保持数据分布一致性，生成增强数据集。
result: 生成的增强数据有助于缓解数据稀缺，提高事件抽取性能。
conclusion: LLM辅助数据增强可有效应用于管理科学等领域的文本信息提取。
---

## Abstract
Event extraction is an important task in natural language processing that focuses on mining event-related information from unstructured text. Despite considerable advancements, it is still challenging to achieve satisfactory performance in this task, and issues like data scarcity and imbalance obstruct progress. In this paper, we introduce an innovative approach where we employ Large Language Models (LLMs) as expert annotators for event extraction. We strategically include sample data from the training dataset in the prompt as a reference, ensuring alignment between the data distribution of LLM-generated samples and that of the benchmark dataset. This enables us to craft an augmented dataset that complements existing benchmarks, alleviating the challenges of data imbalance and scarcity and thereby enhancing the performance of fine-tuned models. We conducted extensive experiments to validate the efficacy of our proposed method, and we believe that this approach holds great potential for propelling the development and application of more advanced and reliable event extraction systems in real-world scenarios.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **事件抽取面临的瓶颈**：事件抽取（Event Extraction）是 NLP 中从非结构化文本中挖掘事件信息的重要任务，包括事件检测（ED）和事件论元抽取（EAE）两个子任务。尽管已有较多工作，但 EAE 在句子级上的 F1 仍只有约 60%，ED 虽达 80%，但整体性能仍不令人满意。
- **数据稀缺与长尾分布**：主流数据集如 ACE 2005 存在严重的数据不平衡（某些事件类型仅有个位数标注样本），MAVEN 数据集虽较大但仍缺少论元标注，且同样面临长尾分布。这是制约性能提升的关键因素。
- **LLM 直接应用存在差距**：作者首先测试了 LLM 在零样本和一样本设置下的事件抽取性能，发现 LLM 无法达到微调专用模型的水平。原因包括：标注理解不一致、句级语义局限、产生未定义事件类型或额外论元、输出格式不稳定等。
- **核心思路**：不再将 LLM 直接作为抽取器，而是将其作为“专家标注器”来生成与基准数据集分布一致的高质量增强数据，缓解数据稀缺和不平衡问题，从而提升微调模型的性能。

### 2. 论文提出的方法论：核心思想、关键技术细节、算法或流程

- **LLM 作为标注器进行数据增强**：
  - 在提示（prompt）中提供一个来自训练集的真实标注样本作为参考，让 LLM 模仿格式和标注逻辑生成新的句子和标注。
  - 对不同的 LLM 设计了不同复杂度的提示：GPT 4 可在简单指令下工作；GPT‑3.5‑Turbo 和 PaLM 则需要更明确的格式要求和示例。
  - 论元标注时会列出该事件类型的所有可能角色，即使示例中没有出现某些角色，以促使 LLM 生成更完整的标注。
- **样本生成与选择策略**：
  - 对 ACE 2005，除了少数训练样本已较多的事件类型外，为其他事件类型各生成 3–5 条 GPT‑4 标注的样本，共增加 111 条。
  - 生成过程采用迭代方式：将上一条生成的标注样本用作下一条提示中的示例，以提高一致性。
  - 对 MAVEN，仅因预算原因使用 GPT‑3.5‑Turbo 和 PaLM 生成，选择训练样本数较少的类型生成数据，并通过过滤脚本去除格式错误或明显有问题的样本。
- **评估管道**：
  - 将增强后的训练集用于微调多种经典事件抽取模型（BERT+CRF、DMBERT、CLEVE、EEQA、Text2Event），比较有无增强时的性能变化。
  - 使用 OmniEvent 框架统一预处理和评估，避免因预处理差异带来的偏差。

### 3. 实验设计：使用数据集/场景、Benchmark、对比方法

- **数据集**：
  - ACE 2005：句子级事件抽取，33 种事件类型，包含 ED 和 EAE 标注。采用“ACE‑DYGIE”预处理策略，并排除时间等角色以减少语义混淆。
  - MAVEN：大规模 ED 数据集（168 种事件类型），缺少论元标注，仅用于 ED 评估。
- **LLM 性能测试场景**：
  - 直接抽取：零样本和一样本设置，分别测试管道式和联合式抽取，并探索一次提取所有事件和逐个事件提取的策略。
  - 评价指标：精确率（P）、召回率（R）、F1；对 ED 分别用严格匹配（事件类型和触发词均正确）和宽松匹配（仅事件类型）。
- **对比方法（基准模型）**：
  - 序列标注：BERT+CRF
  - 分类模型：DMBERT
  - 对比预训练：CLEVE
  - 问答式抽取：EEQA
  - 条件生成：Text2Event
- **增强对比**：同一模型在原始 ACE 2005 训练集与加入 LLM 生成样本后的增强训练集上训练的 ED 和 EAE 性能（分别用黄金触发词和预测触发词评估 EAE）。

### 4. 资源与算力

- 论文中**未明确提供 GPU 型号、数量或具体的训练时长**。
- 提到 LLM 的使用涉及 API 调用（GPT‑3.5‑Turbo、GPT‑4、PaLM），但因预算原因 MAVEN 上未使用 GPT‑4 生成数据。专用事件抽取模型的训练和评估基于 OmniEvent 框架，但算力细节未披露。

### 5. 实验数量与充分性

- **实验组数**：
  - 直接评估了 3 种 LLM（GPT‑3.5‑Turbo、GPT‑4、PaLM）在零样本和一样本下的 ED 和 EAE 表现。
  - 对 5 种基准模型在原始和增强后的 ACE 2005 上进行了 ED 和 EAE 的对比。
  - 在 MAVEN 上仅用 PaLM 进行了 ED 零样本评估，并用 GPT‑3.5‑Turbo 和 PaLM 生成的增强数据做了简单对比。
  - 未进行消融实验（如不同数量增强样本的影响、不同提示策略对比）。
- **充分性与公平性**：
  - 使用 OmniEvent 框架确保了预处理和评估的一致性，比较公平。
  - 虽然涵盖多种模型，但增强实验主要集中在 ACE 2005 且增强数据量不大（111 条）。对 MAVEN 的实验相对较弱，增强效果提升微小。
  - 对 LLM 直接抽取的评估指标虽然揭示了差距，但未深入分析错误类型的影响。

### 6. 论文的主要结论与发现

- **LLM 直接抽取存在显著性能差距**：即使是在一样本条件下，LLM 的 ED 和 EAE F1 仍远低于微调模型（如 ED 上 GPT‑4 最好仅 56.7%），不能直接替代专用模型。
- **LLM 作为标注器生成增强数据可行且有效**：
  - 加入 GPT‑4 生成的标注样本后，多个基准模型在 ED 和 EAE 上均有提升（如 CLEVE ED F1 从 71.0% 升至 72.8%），说明生成数据质量足够高。
  - GPT‑4 的生成质量优于 GPT‑3.5‑Turbo 和 PaLM，后者有时会重复示例或格式混乱。
- **增强效果受原有数据量和生成质量影响**：ACE 2005 上增益较明显，MAVEN 上因原有样本量极大、生成数据相对占比小且模型质量较低，增益有限。
- **实用价值**：该方法能有效缓解数据稀缺和长尾问题，并已公开增强后的标注样本供进一步研究。

### 7. 优点：方法或实验设计上的亮点

- **创新视角**：将 LLM 定位为数据增强的“专家标注器”，而非直接抽取器，绕过了 LLM 在严格评估下性能不足的问题。
- **提示策略设计**：通过在提示中包含基准数据集样本并迭代使用生成样本作为示例，较好地维持了生成数据与原始数据的一致性。
- **公平对比框架**：采用 OmniEvent 框架，统一预处理与评估，减少了此前事件抽取研究中因预处理差异导致的不可靠比较。
- **多模型验证**：测试了多种主流事件抽取模型（覆盖分类、序列标注、生成等方法），证明增强策略具有模型无关性。
- **数据公开**：作者承诺公开增强数据，有助于社区后续研究。

### 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等

- **试验规模有限**：仅对 ACE 2005 做了较充分的增强实验，MAVEN 的实验很有限，且仅 ED；未在其他事件抽取数据集（如 ERE、RAMS）上验证，泛化性待考证。
- **算力信息缺失**：未报告 GPU 资源和训练时间，可复现性受影响。
- **增强数据量未系统研究**：未探讨不同增强样本数量、不同采样策略对性能的边际影响，也未分析生成数据的多样性与噪声。
- **依赖昂贵 API**：GPT‑4 生成数据成本较高，限制了该方法在资源受限场景中的应用；更低成本的开源模型（如 LLaMA）仅在未来工作提及，未进行实验。
- **上下文依赖性未解决**：LLM 仍可能在生成中脱离原始文档上下文，导致样本语义偏差；对文档级事件抽取的适用性未验证。
- **评估指标严格**：现有精确匹配可能低估了 LLM 生成答案的正确性，但论文未探讨更灵活的评价方式。

（完）
