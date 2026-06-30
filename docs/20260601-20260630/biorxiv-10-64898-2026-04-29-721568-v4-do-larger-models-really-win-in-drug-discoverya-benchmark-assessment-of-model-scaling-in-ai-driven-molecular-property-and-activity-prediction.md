---
title: Do Larger Models Really Win in Drug Discovery?A Benchmark Assessment of Model Scaling in AI-Driven Molecular Property and Activity Prediction
title_zh: 更大的模型在药物发现中真的胜出吗？AI驱动的分子性质与活性预测中模型规模化的基准评估
authors: "Guo, J., Ding, S."
date: 2026-06-08
pdf: "https://www.biorxiv.org/content/10.64898/2026.04.29.721568v4.full.pdf"
tags: ["query:mt"]
score: 10.0
evidence: 基准测试图神经网络与大模型在分子性质预测中的表现，直接针对GNN用于分子性质预测及可迁移性。
tldr: 大规模预训练模型在药物发现中是否优于经典机器学习尚无定论。本研究在26个ADME、毒性及生物活性端点上，采用三种不同难度的数据分割协议进行系统基准测试。结果显示经典机器学习在整体比较中仍占优势，大模型在特定低数据场景下表现突出，但性能提升依赖于模型与任务及验证场景的匹配，而非单纯规模。该研究为理性选择分子预测模型提供了量化依据。
source: biorxiv
selection_source: fresh_fetch
motivation: 检验大模型在药物分子性质与活性预测中是否确优于经典机器学习。
method: "在26个端点、165,541条记录上，用三种分割协议进行156项系统比较。"
result: "经典ML胜出47.4%，预训练序列模型28.8%，GNN 21.8%，LLM仅1.9%，且大模型优势受读出设置影响。"
conclusion: 专用紧凑模型仍高效，大模型在低数据推理中有附加价值，性能取决于任务-模型-场景匹配而非规模。
---

## 摘要
分子基础模型和大语言模型（LLM）的快速发展，催生了以规模为中心的人工智能药物发现观，即预期更大的预训练模型将取代基于经典机器学习（classical ML）和图神经网络（GNN）、针对单个任务训练的紧凑型化学信息学模型。我们在26个终点上检验这一假设，这些终点分为ADME、毒性和生物活性类别，涵盖165,541条终点级别的化合物标签记录。基准测试包含78个终点与分割条目，对应26个终点在三种分割协议下的评估结果：随机分割、Murcko骨架分割和结构分离的五折交叉验证（CV）。按从易到难的顺序，这些分割近似于封闭库的回顾性评估、从苗头化合物到先导化合物的骨架扩展，以及新颖化学型的库扩展。每个条目提供两项任务与指标对照，总计156项比较。在这些比较中，经典机器学习提供了最高比例的最佳表现条目（47.4%），其次是预训练分子序列模型（28.8%）、GNN（21.8%）和基于LLM的SAR基线（1.9%）。经典机器学习在随机分割插值中占主导地位，并且仍是整体上的最大赢家类别。在主要最优留出读数下，GNN和序列模型在选定的较难分割协议中具有竞争力，但在固定最终窗口读数下，其严格胜出比例下降，表明部分收益依赖于训练设置和模型选择。配对自助法分析表明，各模型之间微小的数值差异不应被视为决定性胜利。来自训练折叠的SAR知识改善了许多GPT5.5-SAR和Opus4.7-SAR的指标，但并未使基于规则的推理成为监督预测器的普适替代。紧凑的专用模型在分子性质和活性预测方面仍然非常有效。较大模型在低数据场景下为SAR解释和推理增添价值，但预测性能取决于模型、任务与验证场景的契合度，而非仅仅依赖规模。

## Abstract
The rapid growth of molecular foundation models and large language models (LLMs) has encouraged a scale centred view of AI in drug discovery, in which larger pretrained models are expected to supersede compact cheminformatics models based on classical machine learning (classical ML) and graph neural networks (GNNs) trained for individual tasks. We test this assumption across 26 endpoints grouped into ADME, toxicity and bioactivity classes, covering 165,541 endpoint level compound label records. The benchmark contains 78 endpoint and split entries, corresponding to 26 endpoints evaluated under three split protocols: random, Murcko scaffold and structure separated 5-fold cross validation (CV). Ordered from easiest to hardest, these splits approximate retrospective evaluation on a closed library, scaffold expansion in hit to lead, and library expansion on novel chemotypes. Each entry contributes two task and metric comparisons, giving 156 comparisons in total. Across these comparisons, classical ML provides the largest share of best performing entries (47.4%), followed by pretrained molecular sequence models (28.8%), GNNs (21.8%) and LLM based SAR baselines (1.9%). Classical ML dominates random split interpolation and remains the largest winner family overall. GNN and sequence models are competitive in selected harder split protocols under the primary optimal held-out readout, but their strict winner shares decrease under a fixed final-window readout, indicating that some of these gains depend on training settings and model selection. Paired bootstrap analyses indicate that small numerical differences between individual models should not be read as decisive victories. SAR knowledge from training folds improves many GPT5.5-SAR and Opus4.7-SAR metrics but does not make rule based reasoning a universal substitute for supervised predictors. Compact specialized models remain highly effective for molecular property and activity prediction. Larger models add value for SAR interpretation and reasoning in low data settings, but predictive performance depends on the fit among model, task and validation scenario, not on scale alone.

---

## 论文详细总结（自动生成）

好的，作为一名资深学术论文分析助手，我将基于提供的论文内容，以结构化的 Markdown 格式为您进行深入、客观的总结。

### **1. 论文核心问题与整体含义**

*   **研究动机与背景**: 随着分子基础模型和大语言模型（LLM）在药物发现领域的快速发展，形成了一种“以规模为中心”的观点，即普遍认为参数更大的预训练模型理应取代基于经典机器学习（如随机森林）或图神经网络（GNN）的紧凑型模型。这种观点默认了“模型越大，性能越好”。
*   **核心问题**: 本研究旨在系统性地检验上述假设，核心问题是：“在药物发现相关的分子属性与活性预测任务中，更大的模型是否真的能带来更优的预测效果？”。它并非寻找一个全局最优的模型家族，而是探究在不同数据场景下，哪种模型的“模型-任务契合度”最高。
*   **整体含义**: 论文通过全面的基准评估挑战了“唯规模论”的流行叙事。其核心含义是，模型规模并非一个可靠的性能排序原则，预测性能更多地取决于分子表征、归纳偏置、数据规模、终点生物学特性和验证场景之间的复杂匹配，而非单纯的参数数量或预训练语料库大小。

### **2. 论文方法论**

本研究的核心思想是建立一个多维度的严格评估体系，而非提出新的模型架构。其关键技术细节和算法流程体现在以下方面：

*   **模型家族与分子表征**: 对比了四大类模型，其主要区别在于分子表征方式和归纳偏置：
    *   **经典机器学习 (Classical ML)**：使用固定分子表征（ECFP4/6、MACCS指纹、RDKit描述符）作为输入，训练随机森林 (RF)、极端随机树 (ExtraTrees)、梯度提升树 (GBDT) 等模型。参数规模在 \(10^3-10^5\) 级别。
    *   **图神经网络 (GNN)**：将分子视为图（原子为节点、化学键为边），通过消息传递机制学习任务特定的分子表征。包括 GCN、GAT、GIN 和 Ligandformer。参数规模在 \(10^5-10^6\) 级别。
    *   **预训练分子序列模型 (Sequence Models)**：将SMILES序列视为化学语言，对预训练的Transformer模型（ChemBERTa、ChemBERTa2、MoLFormer）进行微调。参数规模覆盖10M、47M和77M。
    *   **基于LLM的构效关系基线 (LLM-SAR)**：利用前沿LLM（GPT5.5和Claude Opus 4.7）的化学推理能力，**一次性** 生成确定的SAR规则库和评分标准，然后用这些规则对验证分子进行 **确定性评分**，而无需再次调用LLM或进行梯度更新。其参数规模属于LLM级别 (\(>10^{11}\))。
*   **主要评估协议**:
    *   **核心指标**: 分类任务优先使用 **PR-AUC**（精确率-召回率曲线下面积），因为其对不平衡数据集更敏感，辅以ROC-AUC、EF@K%等；回归任务使用 **MAE** 和 **Pearson相关系数**。
    *   **读出方式**: 针对GNN和序列模型，采用两种互补的读出策略：
        1.  **主要最优留出法**: 在预设的训练轮次内，选择在留出验证折上性能最优的那个epoch，用于主排名表。
        2.  **固定最终窗口法**: 取训练日志中最后三个epoch性能的均值。用于敏感性分析，检验胜出结果对模型选择的依赖性。
    *   **统计检验**: 采用 **配对自助法** 对预测结果进行1000次重采样，计算性能指标差值的95%置信区间，并使用 **Benjamini-Hochberg (BH) FDR校正**（q=0.05）来控制多重比较的错误发现率。

### **3. 实验设计**

*   **数据集与场景**: 基准测试覆盖 **26个终点**，分为三大类，总计 **165,541条** 终点级别的化合物-标签记录。
    *   **ADME相关 (6个)**: 如Caco2肠渗透性、血脑屏障穿透、CYP3A4抑制、亲脂性、水溶性等。
    *   **毒性相关 (17个)**: 如AMES致突变性、hERG心脏毒性（来自两个独立来源）、12个Tox21核受体和应激反应通路测定、DRD2受体结合等。
    *   **生物活性相关 (3个)**: EGFR激酶抑制、抗结核H37Rv细胞活性、抗疟Pf.3D7/Dd2细胞活性。
*   **基准测试与对比方法**: 对比了上述四大模型家族（Classical ML, GNN, Sequence, LLM-SAR）共数十个具体模型的表现。
*   **分割协议**: 采用 **三种不同难度** 的5折交叉验证，模拟现实药物发现的不同阶段：
    1.  **随机分割 (Random CV)**: 最容易，近似于在一个封闭化学库内的回顾性评估。
    2.  **Murcko骨架分割**: 中等难度，同一骨架的分子被分入同一折，测试模型向新骨架的泛化能力，近似苗头化合物到先导优化阶段。
    3.  **结构分离分割**: 最难，基于ECFP4指纹聚类进行折划分，确保训练和测试集在化学空间上分离，测试模型对全新化学型的泛化能力，近似全新库筛选。

### **4. 资源与算力**

*   论文未明确提及进行基准测试所使用的具体GPU型号、数量或总训练时长。
*   文中提及了不同模型的固定训练配置，如GNN的批量大小(256)、训练轮次(多达80)、优化器(Adam，学习率 \(10^{-3}\))，以及序列模型微调的批量大小(8或64)、轮次(多达30)和优化器(AdamW, 学习率 \(2\times10^{-5}\))，但这些信息不足以推断出总体算力消耗。

### **5. 实验数量与充分性**

*   **实验数量**: 评估极为系统。26个终点 × 3种分割协议 = **78个终点-分割条目**。每个条目参与分类或回归任务，比较两个主要指标，得到 **总计156项比较**。这还构成了家族级赢家分析的基础。此外，还包括了数据规模消融（4个代表性任务）、不同读出方法的敏感性分析、虚拟筛选富集因子分析和校准分析，总计成百上千组对照实验。
*   **充分性与公平性**: 实验设计非常充分和客观。
    *   **公平性**: 每个模型家族内部维持固定的超参数设置，避免了为特定模型进行不公平调优。
    *   **严谨性**: 引入了固定最终窗口法来揭示“最优”胜出的随机性；通过BH-FDR校正的配对自助法提供统计支持，防止过度解读微小差异。
    *   **场景真实性**: 三种难度的分割协议模拟了不同的行业应用场景，使得结论更具实践指导意义。

### **6. 主要结论与发现**

*   **经典机器学习整体领先**: 在156项比较中，经典ML模型的胜出比例最高（47.4%），其次是序列模型（28.8%）、GNN（21.8%）。LLM-SAR表现最弱（1.9%）。这表明，在没有经过仔细任务匹配的情况下，紧凑专用模型依然是极其强大的基线。
*   **“唯规模论”被证伪**: 参数数量和预训练语料库大小并不能可靠地预测某个终点上的性能排名。在不同任务和数据分割下，胜出的模型家族各不相同。
*   **模型-任务-场景的契合度是关键**:
    *   **简单场景（随机分割）**: 经典ML（尤其是基于指纹的树集成模型）拥有压倒性优势，因其能有效捕捉局部SAR模式。
    *   **困难场景（结构分离分割）**: GNN和序列模型的竞争力相对提升，在某些ADME和毒性任务中可与经典ML持平甚至超越，但这种优势对模型选择（读出方式）敏感。
    *   **低数据或推理场景**: LLM-SAR 在绝对性能上表现不佳，但其受数据分割难度的影响最小，在化学分布偏移时表现出更好的稳健性。其价值在于 **可解释性、规则生成和假设提出**，而非作为独立的预测器。
*   **统计支持分层**: 家族层面的性能差异具有统计学意义，但端点层面具体模型间微小的排名差异往往不显著，应视为“排名猜测”而非“明确的赢家”。

### **7. 优点**

*   **系统性基准设计**: 覆盖广泛的终点类型和模型家族，评估维度全面。
*   **方法学严谨**: 采用了“固定最终窗口法”和配对自助法进行敏感性分析和统计显著性检验，极大地增强了结论的可信度。
*   **实践导向**: 分割协议的设计紧贴药物发现的实际应用场景（封闭库筛选、骨架跃迁、新化学空间探索）。
*   **批判性视角**: 有力地挑战了“模型越大越好”的流行观念，为行业提供了理性选择模型的量化依据。
*   **清晰的定位**: 准确评估了LLM在分子预测中的位置，即其在 **可解释性与推理支持** 方面的价值高于其在 **直接预测** 上的能力。

### **8. 不足与局限**

*   **未报告算力需求**: 论文没有提供实验消耗的具体硬件资源和时间，这使得其他研究者复现或评估其成本效益变得困难。
*   **端点和数据集的代表性**: 尽管覆盖了26个终点，但仍属于现有公开数据集的一个子集，可能无法代表所有药物发现中的化学和生物多样性。
*   **模型实现的通用性**: 虽然旨在保持公平，但各模型家族使用了一套固定（而非针对单个任务优化）的超参数。这可能导致某些模型（特别是对超参数敏感的GNN和序列模型）的性能被低估，但这也恰好强化了论文的核心论点——即未经特殊调优的大模型不比经典ML更好。
*   **LLM-SAR设计的局限**: LLM-SAR的规则是单一时间点生成且确定性执行的，无法完全代表未来LLM在交互式、动态推理方面的潜力。其设计也反映了当前LLM在直接输出可靠概率预测方面的限制。
*   **结论的时效性**: 该基准测试的性能对比是基于特定版本的模型和训练范式，随着新模型（尤其是更强大的GNN和分子基础模型）的出现，具体排名可能发生变化，但论文的核心方法论和结论——“模型-任务契合度胜过单纯规模”——具有持久的参考价值。

（完）
