---
title: "Liberating Seen Classes: Boosting Few-Shot and Zero-Shot Text Classification via Anchor Generation and Classification Reframing"
title_zh: 释放已知类别：通过锚生成与分类重构增强少样本和零样本文本分类
authors: "Han Liu, Siyang Zhao, Xiaotong Zhang, Feng Zhang, Wei Wang, Fenglong Ma, Hongyang Chen, Hong Yu, Xianchao Zhang"
date: 2024-03-25
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/29827/31436"
tags: ["query:mt"]
score: 7.0
evidence: 少样本和零样本文本分类，自然语言处理文本分析
tldr: 针对少样本/零样本文本分类中类别差异导致知识迁移困难和稀少标签监督不足的问题，提出锚生成和分类重铸方法。通过生成锚样本和重构分类任务，有效弥合分布差异。实验在多个文本分类数据集上显著提升性能，为低资源场景下的文本分析提供了新途径。
source: AAAI-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29827/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1828, \"height\": 431, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29827/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 863, \"height\": 719, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29827/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 848, \"height\": 421, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29827/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 852, \"height\": 333, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29827/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1741, \"height\": 1133, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29827/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 857, \"height\": 516, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29827/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 888, \"height\": 323, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29827/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1773, \"height\": 385, \"label\": \"Table\"}]"
motivation: 已知类别与未知类别差异大，且稀有标签监督信号不足，导致迁移困难。
method: 提出锚生成策略减少类间差异，并重构分类框架以更好利用已知类别知识。
result: 在多个基准上，少样本/零样本分类性能显著提升。
conclusion: 该方法有效缓解少样本/零样本文本分类中的域偏移问题，提高了泛化能力。
---

## Abstract
Few-shot and zero-shot text classification aim to recognize samples from novel classes with limited labeled samples or no labeled samples at all. While prevailing methods have shown promising performance via transferring knowledge from seen classes to unseen classes, they are still limited by (1) Inherent dissimilarities among classes make the transformation of features learned from seen classes to unseen classes both difficult and inefficient. (2) Rare labeled novel samples usually cannot provide enough supervision signals to enable the model to adjust from the source distribution to the target distribution, especially for complicated scenarios. To alleviate the above issues, we propose a simple and effective strategy for few-shot and zero-shot text classification. We aim to liberate the model from the confines of seen classes, thereby enabling it to predict unseen categories without the necessity of training on seen classes. Specifically, for mining more related unseen category knowledge, we utilize a large pre-trained language model to generate pseudo novel samples, and select the most representative ones as category anchors. After that, we convert the multi-class classification task into a binary classification task and use the similarities of query-anchor pairs for prediction to fully leverage the limited supervision signals. Extensive experiments on six widely used public datasets show that our proposed method can outperform other strong baselines significantly in few-shot and zero-shot tasks, even without using any seen class samples.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **研究背景**：少样本（Few-Shot）和零样本（Zero-Shot）文本分类是自然语言处理中的关键挑战，尤其适用于标注稀缺的真实场景。
- **现有方法的痛点**：
  - 常规方法依赖从已见类（seen classes）向未见类（unseen classes）迁移知识。
  - 由于类别间固有差异，特征迁移困难且低效。
  - 少量标注的新类样本难以提供足够监督信号，模型难以从源分布平滑过渡到目标分布。
- **论文目标**：提出一种无需训练已见类样本的策略，通过生成伪样本并构建类别锚点，同时将多分类重构为二分类，从而在少样本与零样本条件下显著提升分类性能。

## 2. 方法论

### 2.1 核心思想
- 摆脱对已见类的依赖，仅利用类别描述和预训练语言模型（PLM）生成未见类的伪数据。
- 筛选最具代表性的生成样本作为“类别锚点”，并利用锚点间的二分类任务训练模型。
- 测试时通过查询样本与锚点的相似度进行预测。

### 2.2 锚点生成（Anchor Generation）
- **数据生成**：
  - 给定未见类 `y` 及其描述 `ŷ`，构造模板 `T(ŷ)`（如 “Here is news about <Category Description>: “”）。
  - 使用 GPT2‑XL 自回归生成文本 `x_g`，采用 top‑p（p=0.9）和 top‑k（k=40）采样增加多样性。
  - 采用引号结束策略（quotation‑mark‑end）控制生成长度。
  - 每类最多生成 1000 个样本。
- **锚点筛选**：
  - 用 BERT 编码生成样本得到嵌入向量 `e`。
  - 计算所有生成样本的均值向量作为类原型 `p_y`。
  - 基于每个 `e` 与 `p_y` 的欧氏距离构造概率分布，采样 `P` 个最接近原型的样本作为锚点。
  - 对于少样本情况，直接将 `K` 个支持样本加入锚点集合，每类共有 `P+K` 个锚点。

### 2.3 分类重构（Classification Reframing）
- **训练**：
  - 将锚点两两配对，若来自同一类则为正对（标签 `[1,0]`），否则为负对（标签 `[0,1]`）。
  - 输入格式为 `[CLS] x_i [SEP] x_j [SEP]`，用 BERT 提取特征，接 softmax 输出二维相似度向量 `v`。
  - 使用带标签平滑的交叉熵损失训练二分类器。
- **测试**：
  - 对测试样本 `x*` 与所有锚点构建查询‑锚点对，获得相似度分数 `ŝ = v^0 - v^1`。
  - 预测方式：
    - **Top‑One Score**：选择与 `x*` 相似度最高的锚点所属类别。
    - **Average Score**：对每类锚点的相似度取平均，选平均值最大的类别。

### 2.4 零样本与少样本统一框架
- 在零样本任务中，锚点集合仅由生成的 `P` 个样本构成（`K=0`）。
- 方法天然覆盖 N‑way K‑shot 和 N‑way 0‑shot 两种场景。

## 3. 实验设计

### 3.1 数据集
- **少样本数据集**：20News、Amazon、HuffPost、Reuters。
- **零样本数据集**：SNIPS、CLINC（意图分类）。
- 数据划分：
  - 少样本：五折类分割，仅使用测试未见类评估；每任务 5‑way，K=1 或 5，每类 25 个查询。
  - 零样本：按已见/未见类分割，仅在未见类上测试。

### 3.2 基准方法
- **少样本基线**：NN、Prototypical Network、MAML、Induction Network、TPN、DS‑FSL、MLADA、ContrastNet。
- **零样本基线**：CNN/LSTM/BERT 特征提取器 + 线性分类器、CDSSM、ZSDNN、CapsNet、CTIR 系列方法。
- 额外对比：有监督学习（用生成数据训练的多分类器）和 GPT‑3.5 零样本推理。

### 3.3 评估指标
- 准确率（Acc）和微平均 F1 值，多次运行（5 次）取平均。

## 4. 资源与算力

- 论文未明确说明使用的 GPU 型号、数量及训练时长。
- 提及使用 GPT2‑XL 作为生成模型，BERT 作为编码器和分类器，训练采用 AdamW 优化器。
- 仅说明在大连昇腾 AI 计算中心获得算力支持，但缺乏具体的硬件及时间成本细节。

## 5. 实验数量与充分性

- **对比实验**：在 6 个数据集上，分别对 1‑shot、5‑shot、0‑shot 进行测试，与超过 10 种基线方法比较，表格详尽。
- **消融实验**：
  - 锚点数量对性能的影响（5‑way 5‑shot 下变化 P 值），分析性能饱和趋势。
- **额外分析**：
  - 与监督学习及 GPT‑3.5 的零样本对比。
  - 在零样本设置下与 1‑shot 基线的性能对比（展示生成样本的弥补效果）。
- 总体实验丰富，覆盖多种数据规模和任务场景，对比全面且公平（采用相同数据划分和多次运行）。

## 6. 主要结论与发现

- 完全摆脱已见类后，基于类别锚点和二分类重构的方法在少样本和零样本文本分类上均超越多数强基线。
- 在零样本设置下，方法性能甚至可与部分 1‑shot 基线相比，证明了生成锚点的有效性。
- 在意图检测（CLINC）上显著超过 GPT‑3.5，表明精心设计的锚点筛选与任务重构能超越大型 LLM 的简单推理。

## 7. 优点

- **创新性强**：首次在少/零样本多分类文本任务中完全丢弃已见类，仅通过类别描述和生成模型完成任务。
- **方法简洁高效**：锚点生成和分类重构两步清晰，实现难度低，却能大幅提升性能。
- **统一框架**：自然地兼容少样本和零样本场景，无需额外调整。
- **充分利用预训练能力**：结合生成式 PLM 和判别式 PLM 的优势，且生成模板简单易扩展。
- **鲁棒性提升**：通过原型距离采样锚点，有效过滤低质量生成文本。

## 8. 不足与局限

- **依赖生成质量**：对 PLM 生成文本的准确性敏感，若类别描述不准确或模板设计不佳，可能生成噪声样本。
- **锚点数量敏感**：虽展示了饱和趋势，但最优 P 值可能因数据集而异，需手动调节。
- **模型规模与效率**：使用 GPT2‑XL 生成和 BERT 微调，未讨论在实际部署中的推理延迟和计算成本。
- **任务局限性**：仅验证了单标签多分类任务，未涉及多标签分类或更细粒度的语义理解。
- **零样本场景类别数影响**：当未见类数量极少（如 SNIPS 仅 2 类）时，分类重构的优势未能充分发挥。
- **实验公平性边界**：与基线的对比建立在已见类信息可用且训练充分的前提下，但方法本身完全依赖生成数据，未深入分析生成偏差可能带来的不公平优势（如信息泄露隐患）。

（完）
