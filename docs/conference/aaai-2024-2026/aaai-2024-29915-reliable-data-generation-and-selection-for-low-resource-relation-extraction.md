---
title: Reliable Data Generation and Selection for Low-Resource Relation Extraction
title_zh: 面向低资源关系抽取的可靠数据生成与选择方法
authors: "Junjie Yu, Xing Wang, Wenliang Chen"
date: 2024-03-25
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/29915/31600"
tags: ["query:mt"]
score: 8.0
evidence: 自监督数据生成与选择方法用于低资源关系抽取，属于自然语言处理文本挖掘技术
tldr: 关系抽取标注昂贵，低资源场景下性能受限。提出自监督可靠数据生成与选择方法Self-RDGS，先利用三元组知识通过大语言模型生成句子，再提出排序式数据选择法过滤噪声，并将数据选择与关系抽取模型训练整合于自监督迭代框架内，在三个低资源数据集上显著超越基线，为文本挖掘中的高效数据增强提供了可行路径。
source: AAAI-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29915/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 856, \"height\": 466, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29915/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 812, \"height\": 658, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29915/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 843, \"height\": 615, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29915/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 841, \"height\": 593, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29915/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 748, \"height\": 220, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29915/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1695, \"height\": 574, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29915/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1781, \"height\": 650, \"label\": \"Table\"}]"
motivation: 关系抽取依赖人工标注，低资源情况下数据稀缺且自动生成噪声大。
method: 利用三元组知识通过LLM生成数据，设计排序法筛选可靠样本，并与模型训练迭代进行。
result: 在多个低资源关系抽取数据集上取得优越性能，证明了所提数据生成与选择策略的有效性。
conclusion: 自监督数据生成与选择可在低资源条件下有效提升关系抽取，降低标注依赖。
---

## Abstract
Automated construction of annotated data holds significant importance in Relation Extraction (RE) tasks due to the hardness and cost of human annotation. In this work, we propose Self-RDGS, a method for Self-supervised Reliable Data Generation and Selection in low-resource RE tasks. At first, we fully utilize the knowledge of triplets as prompts to generate sentences by employing the Large Language Models (LLMs). Since the auto-generated data contains noise, we then propose a ranking-based data selection method to select reliable sentences. Finally, we integrate the data selection and RE model training within a self-supervised iterative framework. Through experimentation on three datasets with low-resource settings, we demonstrate the effectiveness of our proposed approach in constructing annotated data and achieving noteworthy improvements in comparison to multiple baselines. Code, data and models are available at https://github.com/jjyunlp/GenerationRE.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- 研究背景：关系抽取标注成本高，低资源场景下模型性能受限。现有自动标注方法（如精确匹配远程监督 EM-DS）噪声大，而自动文本生成方法（ATG）未充分利用实体与关系三元组的完整知识。
- 核心问题：如何在低资源条件下，自动生成高质量、低噪声的标注数据，以提升关系抽取模型的效果。
- 论文目标：提出一种自监督的可靠数据生成与选择方法，融合远程监督和文本生成优势，并通过迭代训练提升数据质量。

### 2. 方法论
- **整体框架**：`Self-RDGS`，包含数据生成、可靠数据选择及自监督迭代训练三个核心部分。
- **数据生成（ATG-DS）**：
  - 利用三元组（头实体、关系、尾实体）作为提示，让大语言模型生成包含指定实体并表达目标关系的句子。
  - 两种生成模式：基于**上下文学习**（ICL）直接提示LLMs（如LLaMa2-7B-chat）；基于**微调**（用种子数据微调GPT-2等），使模型学会按模板生成句子。
  - 后过滤：只保留同时包含头尾实体的句子，每个三元组最多生成 K=8 个候选句。
- **数据选择（排序式去噪）**：
  - 假设：每个三元组的候选句中多数表达目标关系，少数为噪声。
  - 利用关系编码器（当前的RE模型）得到每个句子的关系表征 f(s)。
  - 定义全局分布分数：候选句中某句子与池中所有其他句子的平均余弦相似度：
    \[
    \text{score}(s_i, f, S) = \frac{1}{|S|} \sum_{s_j \in S} \cos(f(s_i), f(s_j))
    \]
  - 选择得分最高的句子作为该三元组的可靠代表句。
- **自监督迭代训练**：初始化RE模型（预训练BERT），每轮用当前模型计算句子表征、重新选择句子，再用新选数据训练RE模型，循环直至验证集性能不再提升，最终输出最佳RE模型及所选数据。

### 3. 实验设计
- **数据集**：
  - SemEval-2010 Task 8（人标，9类关系 + “NA”）
  - Re-TACRED（人标修订版，39类关系 + “NA”）
  - NYT10m（远程监督自动标注，25类关系，含人标测试集）
- **低资源模拟**：每类关系随机抽取 20 个人工标注句子作为种子数据，剩余作为测试；三元组来自训练集中未被使用的句子。
- **基线方法**：
  - `SUPERVISED`（低资源仅用种子、高资源用全量）
  - `LLM Predict`（LLaMa2-7B-chat / GPT-2-large 通过上下文学习或微调直接预测关系）
  - `STAD`（自训练RE系统，利用无标注句子）
  - `RELATION PROMPT`（只将关系作为提示，LLaMa2生成句子）
- **选择策略对比**：`MERGE ALL`（全部使用）、`FIRST ONE`（取首句）、`RAND ONE`（随机选一句）、`MIN ONE`（最低分布分）、`MAX ONE`（最高分布分，即本文方法）
- **评价指标**：Micro-F1，多次运行（5个随机种子）报告平均与标准差。

### 4. 资源与算力
- 文中提及使用的模型：`BERT-base`（RE模型）、`GPT-2-large`（生成器微调）、`ChatGLM2-6B`、`LLaMa2-7B-chat`（LLM生成）。
- **未明确给出**：GPU型号、数量、训练时长、显存占用等具体算力消耗未在论文中说明。

### 5. 实验数量与充分性
- **主要实验组数估算**：
  - 3个数据集 × 多种系统（约6种基线 + 2种生成器下的Self-RDGS）= 至少20个主要结果组合。
  - 数据选择方法消融：2种生成器 × 5种选择策略 × 3个数据集 = 30组对比。
  - 附加消融：掩码分析（仅上下文/仅实体mention）、与EM-DS数据的对比、长尾分布分析、不同LLM生成对比。
- **充分性与客观性**：实验覆盖不同规模与来源的数据集，对比了强基线，进行了大量消融研究；采用多轮随机种子和标准指标，统计显著性虽未报告，但偏差控制合理。整体实验设计较充分、公平。

### 6. 主要结论与发现
- `Self-RDGS` 在所有数据集上显著优于各类基线（如相比STAD在平均测试F1上提升8.9个点），缩小了与全量人工标注的差距。
- 排序式数据选择方法能有效识别高质量句子，远好于直接使用全部生成数据或随机选择；最低分布分的选择表现劣于随机选择，证明评分有效。
- `ATG-DS` 生成的数据在质量和覆盖度上优于传统的 `EM-DS` 数据，且缓解了长尾问题，使排序选择发挥更大作用。
- 方法对大模型（LLaMa2）和小模型（GPT-2-large）均有效，具有一定通用性。

### 7. 优点
- **创新性**：首次将三元组知识完整用于指导LLM生成关系抽取数据，并提出基于全局分布分数的句子选择方案。
- **框架设计**：自监督迭代使数据选择与模型训练相互促进，无需额外标注。
- **实验扎实**：多数据集、多基线、丰富的消融与分析法，验证全面。
- **开源**：提供代码与数据，便于复现。

### 8. 不足与局限
- **算力报告缺失**：未披露训练所需GPU资源和时间，难以评估实际成本。
- **依赖三元组**：需要已有或可获取的三元组资源，若知识图谱质量差可能影响生成效果。
- **生成器偏置**：LLM生成的句子可能带有模型自身偏置，虽经选择仍可能残留噪声，未深入分析生成文本的多样性或事实正确性。
- **领域局限**：仅在新闻等通用关系抽取数据集上验证，未探索专业领域（如生物医学）的泛化性。
- **迭代稳定性**：自监督循环可能受初始模型影响，未讨论收敛性或对初始种子的敏感度。

（完）
