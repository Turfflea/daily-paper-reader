---
title: "DEALT: LLM-driven Diversity-Enhanced Data Augmentation for Long-Tail Text Classification"
title_zh: DEALT：大语言模型驱动的多样性增强长尾文本分类数据增强
authors: "Wayne Lu, Xiaoxi Cui"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40508/44469"
tags: ["query:mt"]
score: 9.0
evidence: 自然语言处理长尾文本分类的数据增强
tldr: 针对长尾文本分类中稀有类别数据稀疏导致性能不佳的问题，提出DEALT框架，利用大语言模型模拟‘识别-探索-生成-优化’认知过程，系统化生成多样化增强样本。实验表明，DEALT显著提升了尾部类别的分类性能，为低资源文本分类提供了经济有效的数据增强方案，对管理科学中的用户评论分析、客服工单分类等场景具有重要应用价值。DEALT通过认知循环不断优化生成质量，减少了传统数据增强的语义局限，在多个长尾文本分类数据集上取得了最优结果。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40508/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1834, \"height\": 1035, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40508/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 876, \"height\": 356, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40508/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 866, \"height\": 381, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40508/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 872, \"height\": 412, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40508/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 883, \"height\": 349, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40508/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1826, \"height\": 704, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40508/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 883, \"height\": 430, \"label\": \"Table\"}]"
motivation: 长尾文本分类中大量类别数据稀疏，导致模型性能严重下降，现有数据增强方法生成样本多样性不足且忽略隐式长尾。
method: 提出DEALT，利用LLM模拟认知过程，通过识别、探索、生成和优化循环，系统化增强长尾类别样本的多样性。
result: DEALT在多个长尾数据集上显著提升尾部类别分类准确率，超越现有数据增强方法，且成本效益高。
conclusion: DEALT为长尾文本分类提供了高效数据增强方案，其认知循环机制可推广至管理科学中的稀疏类别建模任务。
---

## Abstract
Real-world text classification datasets frequently exhibit long-tail distributions, where numerous classes have sparse data, significantly degrading model performance on these underrepresented categories. While Large Language Models (LLMs) offer promise for data augmentation, existing methods often produce semantically limited samples, neglect "implicit long-tails" (sparse sub-patterns within classes), and lack cost-effective optimization. To address these challenges, we propose \textbf{DEALT (LLM-driven Diversity-Enhanced Data Augmentation for Long-Tail Text Classification)}, a novel cognitive-inspired framework emulating the human learning process of "recognize, explore, generate, and optimize." DEALT systematically enhances augmented data diversity by first detecting both explicit and implicit long-tails. It then employs an LLM for diversity-aware planning of augmentation strategies, followed by conditional generation. A low-overhead quality and diversity validator filters the synthetic data, and an adaptive incremental sampler refines future augmentation efforts based on proxy model feedback, ensuring efficient and budget-aware optimization. Extensive experiments on multiple public text classification datasets demonstrate DEALT's superiority over state-of-the-art methods in improving tail-class performance and overall model robustness by generating more diverse and high-fidelity augmented data.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义  
- **研究动机**  
  现实文本分类数据集普遍呈现长尾分布，大量“尾部”类别仅拥有极少量样本，导致深度学习模型在稀有类别上泛化能力急剧下降。现有数据增强方法（尤其是基于大语言模型的）面临三个核心瓶颈：  
  - 语义多样性不足：生成样本往往只是原句的浅层改写，难以引入真正新颖的子概念。  
  - 忽视“隐式长尾”：即使数据看似充足的头部类别，其内部仍可能存在某些稀疏子模式（隐式长尾），现有方法未针对性处理。  
  - 高成本与缺乏系统优化：生成策略多为启发式提示，验证数据效用需昂贵的LLM调用与全量下游模型重训，难以形成闭环调优。  

- **整体含义**  
  本文提出 DEALT（LLM‑driven Diversity‑Enhanced Data Augmentation for Long‑Tail Text Classification），一个受人类“识别‑探索‑生成‑优化”认知过程启发的框架，旨在系统性提升增强数据的多样性、保真度和资源效率，从而显著改善长尾文本分类的性能与鲁棒性。

## 2. 方法论  
DEALT 由五个协同工作的模块构成，形成从异常检测到闭环优化的完整流程。  

- **Long‑tail Distribution Detector (LDT)**  
  自动识别两类数据稀疏：
  - 显式长尾类：基于全局样本数阈值（如低于25%分位数）。
  - 隐式长尾模式：对每类样本用 Sentence‑BERT 获取嵌入，通过 DBSCAN 聚类发现内部子簇，尺寸小于阈值（如5个样本）则标记为隐式长尾。  
  从显式和隐式尾部中选取代表性种子样本组成增强目标集。

- **Diversity Planning (DP)**  
  利用 LLM 的 Chain‑of‑Thought 能力，将单一种子样本转化为 N 条结构化的多样性生成指令。先通过 LLM 构建领域知识池，再基于预定义的多样性维度（词汇替换、句法重组、语义场景变换）生成 N 条互不重复且可执行的操作指令。

- **Conditional Diversity Generation (CDG)**  
  将 DP 输出的每条指令与种子样本、标签一并送入生成 LLM，生成原始增强样本（Daug_raw）。提示模板明确要求“根据指令生成符合原标签的新样本”。

- **Quality and Diversity Validator (QDV)**  
  分级验证流水线：
  - 代理模型检查标签一致性。
  - 通过余弦距离和 1‑BLEU 计算样本与种子及已接受样本的复合多样性分数。
  - 当代理置信度或多样性低于阈值时，触发 LLM 进行最终裁决。  
  通过的样本进入验证集 Daug_val。

- **Adaptive Incremental Sampler and Evaluator (AISE)**  
  闭环优化机制：将验证样本加入临时训练集，用代理模型在验证集上评估性能变化和错误分析，结合剩余预算自动调整下一轮增强的目标（优先性能差或错误率高的类/子簇），并可调整后续策略权重。迭代直至预算耗尽或性能增益低于容忍度。

## 3. 实验设计  
- **数据集**  
  - AG News (IF=100) 和 Yelp Reviews Polarity (IF=50)：人工构建的长尾版本。  
  - CLINC150 和 HWU64：10‑shot 低资源意图分类。  
  - TREC (20‑shot) 和 Symptoms (15‑shot)：模拟低资源场景。  
  六个数据集涵盖情感、主题、意图、症状等任务，兼顾人工和自然长尾。

- **对比基线**  
  - 无增强（Original）。  
  - 传统增强：EDA、回译。  
  - 经典长尾方法：随机过采样、类别平衡损失。  
  - 简单 LLM 增强：零样本 / 少样本 LLM 同义改写。  
  - SOTA LLM 增强：AugGPT、DoAug、Self‑LLMDA、TARDiS、ZeroGen（大多数为重现或模拟）。  

- **评估指标**  
  总体 Accuracy 和 Macro‑F1；AG News 和 Yelp 的头部/中部/尾部 F1；语义不相似度（余弦距离）、词汇多样性（distinct‑1/2）；人工评估标签一致性和 Fleiss’ Kappa；LLM 调用次数。

## 4. 资源与算力  
论文未明确报告实验使用的 GPU 型号、数量或模型训练时长。下游分类器采用 bert‑base‑cased 微调（学习率 2e‑5，batch size 16，5 epochs），DEALT 中的 LLM 组件使用 GPT‑4o‑mini，通过 API 调用完成生成和验证，未提及本地计算资源的具体需求。

## 5. 实验数量与充分性  
- 主要结果：6 个数据集 × 12 种方法（含 DEALT），共 72 个实验点（表1），每个重复 3 次并汇报均值与标准差。  
- 消融实验：在 CLINC150 和 AG‑News 上移除/禁用各模块（LDT、DP、隐式尾部处理、QDV、AISE）共 5 种消融（图2）。  
- 多样性与保真度评估：在 Symptoms 数据集上对比 7 种方法（表2），包含人工标注。  
- 效率分析：在 TREC 上统计各方法 LLM 调用次数（表3）。  
- 隐式长尾专项分析：对比完整方法与仅处理显式尾部的版本。  
- 超参数敏感性：分析 kimplicit 和 N 的影响（图3），以及 kexplicit 和 AISE 迭代轮数（图4）。  
实验覆盖面广，比较基线丰富且多数为 SOTA，消融和敏感性分析充分，随机种子和标准差增强了结果可靠性，整体设计客观且公平。

## 6. 论文的主要结论与发现  
- DEALT 在所有六个数据集上均取得了最高的 Macro‑F1 和 Accuracy，尤其在尾部类别性能上提升显著（如 AG News 尾部 F1 从 29.35% 提升至 58.10%）。  
- 消融实验表明，LDT 和 QDV 对性能贡献最大，但全部模块不可或缺。  
- DEALT 生成的样本在语义多样性（Aug‑Aug 和 Seed‑Aug 距离）和词汇多样性上均高于所有基线，同时保持极高的标签一致性（97.2%），实现了多样性与保真度的兼顾。  
- 相较于需要昂贵 DPO 微调的 DoAug 等方法，DEALT 的一次增强成本虽略高，但远低于需要额外训练的方法，且通过 AISE 可进一步优化效率。

## 7. 优点  
- **认知闭环设计**：首次将“识别‑探索‑生成‑优化”认知过程显式应用于 LLM 驱动的长尾增强，流水线清晰合理。  
- **多样性规划创新**：利用 LLM 的思维链能力生成结构化的多样性策略，超越简单的同义改写和随机操作。  
- **低开销验证**：轻量级代理模型 + 分级 LLM 裁决，在保证质量的同时大幅减少昂贵调用。  
- **显隐尾部双重覆盖**：同时处理宏观类别不平衡和微观子模式稀疏，针对性更强。  
- **实验扎实**：覆盖多种数据集、任务类型和低资源场景，对比方法全面，消融和敏感性分析充分，结果统计可靠。

## 8. 不足与局限  
- **语言与领域局限**：仅在英文文本分类上验证，未扩展到多语言、多模态或结构化数据任务。  
- **阈值依赖**：DBSCAN 参数、kimplicit、kexplicit 等需要针对数据集调参，缺乏自适应机制。  
- **生成质量依赖 LLM**：使用 GPT‑4o‑mini 作为后端，其生成能力上限可能限制多样性，且闭源 API 的可复现性受限。  
- **人工评估规模未明确**：表2 中标签一致性评估由人工完成，但未报告样本量，可能影响结论的统计效力。  
- **与基线的不完全公平**：DEALT 的 LLM 调用次数多于一次性生成方法，但未提供在同等预算下的对比，公平性可进一步加强。  
- **对下游训练的潜在影响未分析**：如增强数据的引入是否导致训练时间大幅增加或过拟合风险，未作讨论。  
- **AISE 的代理偏差**：代理模型性能变化与实际全量模型可能不完全一致，优化方向存在偏差风险。

（完）
