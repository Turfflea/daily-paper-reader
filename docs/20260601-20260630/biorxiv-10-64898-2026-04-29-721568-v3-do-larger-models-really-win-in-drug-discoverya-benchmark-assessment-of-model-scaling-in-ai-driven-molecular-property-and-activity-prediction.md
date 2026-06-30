---
title: Do Larger Models Really Win in Drug Discovery?A Benchmark Assessment of Model Scaling in AI-Driven Molecular Property and Activity Prediction
title_zh: 更大的模型真的在药物发现中胜出吗？AI驱动的分子性质与活性预测中模型扩展的基准评估
authors: "Guo, J., Ding, S."
date: 2026-06-08
pdf: "https://www.biorxiv.org/content/10.64898/2026.04.29.721568v3.full.pdf"
tags: ["query:mt"]
score: 10.0
evidence: 在药物发现中基准测试图神经网络和分子基础模型用于分子性质预测
tldr: "在大模型迅速发展的背景下，药物发现领域普遍认为更大的预训练模型将全面优于传统的紧凑化学信息学模型。为系统检验这一观点，本研究对26个ADME、毒性和生物活性终点进行了大规模基准测试，覆盖165,541条化合物标记数据，并设计随机、Murcko骨架和结构分离三种难度的数据分割协议，在156项模型对比中评估了经典机器学习、图神经网络、预训练分子序列模型以及基于GPT-SAR的LLM方法。结果表明，经典ML在47.4%的对比中取得最优，尤其主导随机分割场景；GNN和序列模型在更困难的骨架分离和结构分离中虽有竞争力，但在固定最终读出窗口下赢家比例降低，说明其部分优势依赖超参数调优。配对自助法分析进一步揭示微小数值差异不能视为决定性胜利。训练集SAR知识虽能提升GPT-5.5和Opus-4.7等LLM的指标，但基于规则的推理尚无法替代监督预测器。最终结论是，紧凑的专用模型在分子性质与活性预测中依然高度有效，大模型在低数据解释和推理任务中具有附加价值，但预测性能的核心在于模型、任务与验证场景的匹配，而非单纯模型规模。"
source: biorxiv
selection_source: fresh_fetch
motivation: 检验药物发现中‘越大越好’的假设，即更大预训练模型是否在分子性质与活性预测中必然优于传统紧凑模型。
method: "构建涵盖26个终点、165,541条记录的基准，采用随机、骨架和结构分离三种分割，对比经典ML、GNN、序列模型和GPT-SAR等LLM的156项性能。"
result: "经典ML以47.4%的最佳条目占比领先；GNN和序列模型在困难分割下的优势受训练窗口影响；LLM推理增益有限，不能替代监督预测。"
conclusion: 紧凑专用模型仍高效；大模型在低数据解释和推理中增值，但预测性能取决于模型与任务场景的匹配，而非规模。
---

## 摘要
分子基础模型和大语言模型的快速发展，催生了药物发现中以规模为中心的人工智能观，即预期更大的预训练模型将取代基于经典机器学习和图神经网络的紧凑型化学信息学模型。我们在涵盖ADME、毒性和生物活性类的26个终点上检验了这一假设，涉及165,541条终点级别的化合物标签记录。基准包含78个终点和划分条目，对应26个终点在三种划分方案下的评估：随机划分、Murcko骨架划分和结构分离的五折交叉验证。从易到难，这些划分分别近似于封闭库的回溯评估、从苗头化合物到先导化合物的骨架扩展，以及新型化学型的库扩展。每个条目包含两项任务和指标比较，共计156项比较。在这些比较中，经典机器学习提供了最大占比的最佳表现条目（47.4%），其次是预训练的分子序列模型（28.8%）、图神经网络（21.8%）和基于大语言模型的构效关系基线（1.9%）。经典机器学习在随机划分内插中占主导地位，并且总体上是最大的赢家族。在主要最优留出读出方案下，图神经网络和序列模型在选定的较难划分方案中具有竞争力，但在固定最终窗口读出方案下，其严格赢家份额下降，表明部分增益依赖于训练设置和模型选择。配对自助法分析表明，不应将个别模型间微小的数值差异视为决定性的胜利。来自训练折叠的构效关系知识改善了许多GPT5.5-SAR和Opus4.7-SAR指标，但并未使基于规则的推理成为监督预测器的通用替代。紧凑的专用模型在分子性质与活性预测中仍然高度有效。更大的模型在低数据环境下为构效关系解释和推理增添了价值，但预测性能取决于模型、任务和验证场景之间的契合，而非仅依赖于规模。

## Abstract
The rapid growth of molecular foundation models and large language models (LLMs) has encouraged a scale centred view of AI in drug discovery, in which larger pretrained models are expected to supersede compact cheminformatics models based on classical machine learning (classical ML) and graph neural networks (GNNs) trained for individual tasks. We test this assumption across 26 endpoints grouped into ADME, toxicity and bioactivity classes, covering 165,541 endpoint level compound label records. The benchmark contains 78 endpoint and split entries, corresponding to 26 endpoints evaluated under three split protocols: random, Murcko scaffold and structure separated 5-fold cross validation (CV). Ordered from easiest to hardest, these splits approximate retrospective evaluation on a closed library, scaffold expansion in hit to lead, and library expansion on novel chemotypes. Each entry contributes two task and metric comparisons, giving 156 comparisons in total. Across these comparisons, classical ML provides the largest share of best performing entries (47.4%), followed by pretrained molecular sequence models (28.8%), GNNs (21.8%) and LLM based SAR baselines (1.9%). Classical ML dominates random split interpolation and remains the largest winner family overall. GNN and sequence models are competitive in selected harder split protocols under the primary optimal held-out readout, but their strict winner shares decrease under a fixed final-window readout, indicating that some of these gains depend on training settings and model selection. Paired bootstrap analyses indicate that small numerical differences between individual models should not be read as decisive victories. SAR knowledge from training folds improves many GPT5.5-SAR and Opus4.7-SAR metrics but does not make rule based reasoning a universal substitute for supervised predictors. Compact specialized models remain highly effective for molecular property and activity prediction. Larger models add value for SAR interpretation and reasoning in low data settings, but predictive performance depends on the fit among model, task and validation scenario, not on scale alone.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
这篇论文的核心是**系统性质疑并检验药物发现领域中的一个普遍假设——“模型越大，预测性能越好”**。
- **研究背景**：近年来，图神经网络、基于SMILES的Transformer、分子基础模型以及大语言模型等规模庞大的AI模型迅速发展，带来了一种“以规模为中心”的观点，即预训练的大型模型应全面取代针对特定任务训练的经典机器学习或紧凑型模型。
- **核心问题**：研究直接挑战这一假设，旨在探究**在给定的分子性质与活性预测任务中，参数数量或预训练语料库大小是否是预测性能的可靠保证**。它不寻求一个全局最优模型家族，而是探究在不同任务、数据和验证场景下，不同复杂度的模型何时有效、何时失效。
- **整体含义**：其核心论点是，预测性能取决于**模型、任务与验证场景之间的“契合度”**，而非单纯由模型规模决定。紧凑的专用模型依然是强基准，大规模模型在特定场景（如低数据、解释性）有附加价值，但不能作为监督预测器的万用替代品。

### 2. 论文提出的方法论
论文的核心方法是构建一个严谨的多维度基准测试框架，通过控制实验变量来评估模型性能，而非提出一个新的算法。
- **核心思想**：这项基准测试通过对比四类具有不同分子表征和归纳偏好的模型家族，系统地评估它们在多种难度递增的数据划分协议下的表现。
- **关键组成——模型家族**：
    1.  **经典机器学习**：使用ECFP4/6、MACCS和RDKit描述符等固定分子指纹的随机森林和ExtraTrees等模型。
    2.  **图神经网络**：GCN、GAT、GIN和Ligandformer等将分子视为原子-键图进行学习的模型。
    3.  **预训练分子序列模型**：对ChemBERTa、MoLFormer等在大规模SMILES语料上预训练的Transformer进行微调。
    4.  **基于LLM的SAR基线**：一种新颖的对比方法，先利用GPT-5.5和Claude Opus 4.7等前沿LLM生成一套固定的、可解释的构效关系（SAR）规则库和评分细则，然后**在本地以确定性方式应用这些规则**进行预测，且在评估阶段不再调用LLM API，确保了可重复性。
- **关键组成——评估协议**：采用从易到难的三种五折交叉验证协议，以测试模型在不同程度的化学分布外泛化能力。
    - **随机划分**：近似于封闭项目库内的回溯评估。
    - **Murcko骨架划分**：保证测试集分子与训练集分子具有不同的核心骨架，模拟苗头化合物到先导化合物的优化阶段。
    - **结构分离划分**：通过聚类在化学空间层面隔离训练集和测试集，模拟在全新化学型上进行库扩展的难度。
- **统计严谨性**：采用配对自助法进行统计显著性检验，并使用Benjamini-Hochberg FDR校正多重比较。区分了“家族层级”和“模型层级”的显著性分析。

### 3. 实验设计
- **数据集**：基准涵盖了26个药学相关终点，划分为三大类：
    - **ADME类**：如Caco-2渗透性、血脑屏障穿透、CYP3A4抑制、亲脂性、水溶性等。
    - **毒性类**：如AMES致突变性、hERG心脏毒性、药物性肝损伤(DILI)，以及12个Tox21通路分析。
    - **生物活性类**：如EGFR激酶抑制、抗结核分枝杆菌(H37Rv)和抗疟原虫(3D7/Dd2)全细胞活性。
    - 总计**165,541条**终点级别的化合物标记记录。
- **场景/Benchmark**：核心benchmark就是上述**26个终点 × 3种划分协议 = 78个终点和划分条目**。每个条目同时进行分类和回归，总计形成**156项**任务和指标比较。
- **对比方法**：系统比较了四大家族下的多个具体模型，包括：
    - **经典机器学习**：RF、ExtraTrees、GBDT、LR等。
    - **图神经网络**：GCN、GAT、GIN、Ligandformer。
    - **序列模型**：ChemBERTa、ChemBERTa2、MoLFormer。
    - **LLM-SAR**：GPT5.5-SAR、Opus4.7-SAR及其融合训练集知识的变体。

### 4. 资源与算力
- **文中未明确说明具体使用的GPU型号、数量以及总训练时长**。
- 尽管论文比较了拥有数十亿参数的前沿LLM和百万参数的GNN/序列模型，但其主要计算开销在于数千次针对不同终点和划分协议的传统模型训练、微调以及LLM的推理调用。论文本身是一篇评估性。基准研究，而非提出一种消耗巨大算力的大模型训练方法。

### 5. 实验数量与充分性
- **实验数量巨大且充分**。研究进行了**156项**严格控制的头对头比较（26个终点 × 3种划分 × 2个指标），覆盖了不同化学空间、不同数据规模（从几百到上万）和不同生物学机理的任务。
- **客观性与公平性**：基准设计极具客观性和公平性。
    - **统一的评估协议**：所有模型在所有任务上均采用相同的五折交叉验证协议和主要评估指标（如PR-AUC, ROC-AUC）。
    - **全面的对比基线**：不仅包括了前沿的深度学习模型，还将精心调优的经典机器学习模型和具有明确化学机制的LLM-SAR规则作为基线。
    - **深度的敏感性分析**：研究不仅报告了最优轮次的结果，还通过“固定最终窗口读出”分析探讨了模型选择（早停法）对结果的影响，揭示了部分模型优势的脆弱性。
    - **多维度指标**：除排序指标外，还分析了校正（Calibration）、Top-K富集率（EF）等，提供了更全面的性能画像。

### 6. 论文的主要结论与发现
- **大小并非胜负手**：参数数量或预训练语料库大小不能线性预测模型在分子性质预测任务中的表现。经典机器学习模型（如基于ECFP指纹的随机森林）在47.4%的比较中表现最优，总体胜率最高。
- **场景决定优势**：
    - 在数据分布相对一致的**随机划分**下，经典ML占据绝对主导地位。
    - 在更具挑战性的**骨架分离和结构分离**中，GNN和分子序列模型竞争力提升，但其优势部分依赖于超参数调优和模型选择。
- **LLM的合理定位**：基于LLM的SAR基线模型在预测性能上最弱，但它在化学结构变化下的稳健性最好。其主要价值在于生成可解释的化学规则、科学假设和推理，作为“解释层”与监督预测器协同工作，而非独立的预测工具。
- **统计显著性警示**：家族层级的差异通常是显著的，但具体到任务级别的“第一名、第二名”差异，常因置信区间重叠而在统计上不显著，不应过度解读微小数值差异。
- **数据规模的影响**：即使在低数据量（如500个化合物）时，经典ML也表现出强大竞争力，简单地增加数据量并不自动保证更大模型获胜。

### 7. 优点
- **核心问题极具价值**：正面回答了一个对AI制药领域至关重要的“规模迷思”，为社区提供了理性的、基于证据的参考。
- **基准设计严谨、系统且全面**：通过控制化学空间分离程度的多种划分协议，系统测试了不同模型的泛化能力，超越了简单的随机划分基准。
- **对比基线新颖且有意义**：引入“基于LLM的SAR规则”作为对比基线，巧妙地将大模型从“预测器”转变为“知识来源与解释器”，开辟了评估其推理能力的新视角。
- **深入的方法论剖析**：通过“固定最终窗口读出”和“显著性检测”等敏感性分析，深刻揭示了基准测试中模型选择和结果解读的微妙之处，提升了结论的可靠性。

### 8. 不足与局限
- **算力信息缺失**：论文未报告所用的具体算力资源，使得评估其计算成本和可复现门槛变得困难。
- **模型与数据局限**：
    - **模型范围**：基准限于特定的几个预训练分子模型和GNN变体，无法穷尽所有先进模型。
    - **数据代表性**：结论的泛化性受限于所选的26个特定终点，可能无法完全推广到所有类型的药物发现任务。
    - **前瞻性验证不足**：性能评估基于历史数据的交叉验证，缺乏时间跨度上的前瞻性实验验证。
- **LLM规则方法的限制**：LLM-SAR方法的效能受限于其生成的规则库质量和LLM本身的化学推理能力，这种能力的普适性和上限仍有待探索。
- **结论的极端通俗化风险**：该研究结论“大模型并非总是赢家”可能被错误地用作反对尝试任何大规模模型的借口，而忽略了其在特定任务（如零样本或少样本学习）和解释推理方面的独特价值。

（完）
