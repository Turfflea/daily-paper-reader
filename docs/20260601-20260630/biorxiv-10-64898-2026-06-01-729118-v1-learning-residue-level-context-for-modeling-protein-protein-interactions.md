---
title: Learning residue-level context for modeling protein-protein interactions
title_zh: 学习残基层面的上下文以建模蛋白质-蛋白质相互作用
authors: "Zhang, Z., Yang, Z., Liu, A., Yu, K.-H., Zhao, J., Yang, Y., Neale, B., Chen, S."
date: 2026-06-04
pdf: "https://www.biorxiv.org/content/10.64898/2026.06.01.729118v1.full.pdf"
tags: ["query:mt"]
score: 9.0
evidence: 基于Transformer的深度学习框架预测蛋白质相互作用扰动
tldr: 蛋白质语言模型常用于相互作用预测，但多聚合全蛋白信息，限制残基级解析。我们提出ReCLIP框架，通过融合残基邻域与伙伴条件表征，学习残基特异性交互。ReCLIP在突变扰动（AUROC=0.973）、翻译后修饰（AUROC=0.822）及零样本肽-MHC结合（最高0.972）上性能领先。该工作提供可推广与可解释框架，揭示残基上下文如何决定结合特异性及驱动临床扰动。
source: biorxiv
selection_source: fresh_fetch
motivation: 现有蛋白语言模型在相互作用预测中常聚合整条蛋白信息，缺乏残基水平分辨率，限制可解释性。
method: 提出ReCLIP Transformer框架，结合蛋白内残基邻域与交互伙伴残基条件表示，学习残基水平交互特异性。
result: ReCLIP在突变效应、翻译后修饰及肽-MHC结合预测中均获高AUROC（最高0.973），残基邻域匹配结合决定因子。
conclusion: ReCLIP提供可推广且可解释的残基水平蛋白相互作用建模框架，揭示上下文如何塑造特异性，并关联临床变异扰动。
---

## 摘要
蛋白质语言模型（PLMs）通过从序列中学习残基层面的特征来预测蛋白质特性，然而大多数基于PLM的蛋白质-蛋白质相互作用方法在整个蛋白质上聚合信息，限制了分辨率和可解释性。在此，我们提出了ReCLIP，一个基于Transformer的框架，它通过结合蛋白质内部残基邻域与相互作用伙伴的残基条件化表示，在单个残基层面学习相互作用特异性表示。我们表明，以残基为中心的上下文为在各种生物背景下建模蛋白质相互作用提供了一个通用框架。ReCLIP准确地预测了突变诱导的扰动（AUROC = 0.973），泛化到不改变序列的翻译后修饰（AUROC = 0.822），并实现了在未见等位基因上肽-MHC结合的零样本预测（AUROC高达0.972）。对学习到的残基邻域的分析揭示了与已知结合决定因素一致的结构和功能连贯模式。应用于临床注释的遗传变异，ReCLIP识别了疾病相关的相互作用扰动，将致病变异与特定的分子相互作用背景联系起来。我们的结果建立了一个可推广和可解释的蛋白质相互作用建模框架，并提供了关于残基层面上下文如何塑造相互作用特异性及其扰动的见解。

## Abstract
Protein language models (PLMs) enable prediction of protein properties by learning residue-level features from sequence, yet most PLM-based approaches to protein-protein interactions aggregate information across entire proteins, limiting resolution and interpretability. Here we present ReCLIP, a transformer-based framework that learns interaction-specific representations at the level of individual residues by combining intra-protein residue neighborhoods with residue-conditioned representations of interaction partners. We show that residue-centered context provides a general framework for modeling protein interactions across diverse biological settings. ReCLIP accurately predicts mutation-induced perturbations (AUROC = 0.973), generalizes to post-translational modifications that do not alter sequence (AUROC = 0.822), and enables zero-shot prediction of peptide-MHC binding across unseen alleles (AUROC up to 0.972). Analysis of learned residue neighborhoods reveals structurally and functionally coherent patterns aligned with known determinants of binding. Applied to clinically annotated genetic variants, ReCLIP identifies disease-associated interaction perturbations that link pathogenic variants to specific molecular interaction contexts. Our results establish a generalizable and interpretable framework for modeling protein interactions and provide insights into how residue-level context shapes interaction specificity and its perturbation.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **核心问题**：现有蛋白质语言模型（PLMs）在建模蛋白质-蛋白质相互作用（PPIs）时，普遍采用全蛋白级别的信息聚合（如序列拼接或固定嵌入），这导致决定结合特异性的残基级细节被稀释，模型的分辨率和可解释性受限。
- **整体含义**：本研究提出将PPI建模重新定义为以残基为中心、上下文依赖的问题，而非蛋白级分类任务。通过显式学习每个残基的“功能邻域”及其伙伴的条件化表示，框架能够更精准地捕捉局部相互作用决定因子，从而实现跨突变、翻译后修饰、肽-MHC结合等多种生物场景的通用且可解释的预测。

### 2. 论文提出的方法论
- **核心思想**：ReCLIP（Residue-level Context Learning for Interacting Proteins）利用预训练PLM（ESM-2）的注意力图，为查询残基构建内部功能邻域和外部伙伴条件化表征，最后用轻量分类器预测相互作用结果。
- **关键技术细节**：
    - **蛋白质内表示**：从ESM-2最后一层的20个注意力头中，对每个查询残基，选择其自注意力权重最高的top-*k*个残基（*k*=5），并按权重降序拼接对应的残基嵌入，形成头专属邻域向量，再跨头拼接得到整体邻域表示。
    - **蛋白质间表示**：借助MINT模型编码目标蛋白与伙伴蛋白，提取查询残基对伙伴残基的跨链注意力权重，归一化后作为伙伴残基的重要性加权分布，加权求和伙伴残基嵌入，得到查询残基条件化的伙伴表示。
    - **联合表示与预测**：将上述两种特征拼接，输入固定的XGBoost分类器（决策树集成）进行多分类或二分类预测。该方法不依赖预定义的结构信息或序列窗口，完全由注意力机制学习残基间依赖。

### 3. 实验设计
- **数据集与场景**：
    - **突变诱导扰动预测**：IntAct数据库，56,429个突变-相互作用对，区分为破坏、减弱、增强、无影响四类。
    - **PTM调控扰动预测**：PTMint数据库，3,052个磷酸化修饰-相互作用对，二分类（增强/抑制结合）。
    - **肽-MHC结合预测**：171,560对pMHC数据，采用零样本设定，评估对未见MHC等位基因的泛化。
    - **临床变异分析**：ClinVar中144,837个错义变异，与HINT数据库中最多5个伙伴蛋白配对，评估预测扰动与致病性的关联。
- **对比方法**：包括MINT（交互感知PLM）、SWING（序列窗口模型）、BERT-baseline（全局池化）、AlphaMissense、PrimateAI-3D（单蛋白变异效应预测器），并在二分类突变任务中额外比较了PLM-Interact和eSIG-Net。
- **评估指标**：多分类/二分类AUROC、AUPRC、平衡准确率、精确度、召回率、F1分数，以及Top-K排序精度。统计显著性通过配对自助法（bootstrap）和Wilcoxon符号秩检验判定。

### 4. 资源与算力
- 论文未明确说明使用的GPU型号、数量及训练总时长。
- 所用预训练模型为650M参数的ESM-2（esm2_t33_650M_UR50D），特征提取和XGBoost训练仅报告了不同邻域大小下的每折平均运行时间（如突变任务*k*=5时约316.5秒/折，PTM任务约21.5秒/折），以此体现计算开销的可控性，但未公布硬件配置细节。

### 5. 实验数量与充分性
- **多样化的任务与设定**：进行了三类核心预测任务（突变四分类/二分类、PTM二分类、pMHC零样本多等位基因），以及ClinVar大规模变异筛选，覆盖了不同类型的相互作用扰动和等位基因偏移。
- **系统性的消融与对照**：考察了功能邻域大小（*k*=3,5,7,10）、替换为随机残基的控制实验、不同ESM-2层嵌入选型等，验证了设计选择的必要性和稳健性。
- **广泛的比较基准**：对比了至少6种具有代表性的PLM基线和专用方法，并在各任务间保持一致的评估流程。
- **深入的机制分析**：对残基邻域进行了序列距离、3D结构距离、功能位点富集、域连贯性和氨基酸组成等多维度分析，提供了生物学合理性支撑。
- 实验整体充分、公平，多层次验证增强了结论的可信度。

### 6. 论文的主要结论与发现
- ReCLIP在突变扰动预测（总AUROC 0.973）上显著优于所有对比方法，特别是对少数类（如破坏、增强）的区分能力更强。
- 在不改变序列的PTM调控预测中同样表现最优（AUROC 0.822），证明其无需显式野生型-突变型对比的优势。
- 在pMHC零样本预测中，无论是在近端还是远端等位基因上均取得最高性能，展现出强大的等位基因泛化能力。
- 学习到的残基邻域在序列和三维结构上具有局部性，显著富集催化、结合等功能位点，且氨基酸组成偏向结合热点残基，表现出生物学一致性。
- 在ClinVar数据中，预测的相互作用扰动在致病/可能致病变异中显著富集，并能将变异与特定的疾病相关分子背景关联，提供机制见解。

### 7. 优点
- **残基级分辨率**：以残基为中心组织上下文，避免了早期全局池化造成的信息损失，尤其适合解析特定残基的扰动能造成的差异化结合结果。
- **仅依赖序列**：无需预知3D结构或显式界面定义，直接从PLM注意力中构建功能邻域和伙伴加权，使其能够应用于缺乏结构信息的海量PPI。
- **通用性与泛化性**：统一框架可处理突变、PTM和肽-MHC结合等多种扰动和相互作用类型，并在零样本场景下保持稳健。
- **可解释性强**：注意力邻域与已知结合热点、功能位点高度一致，便于生成可实验检验的假设，并且能将遗传变异与分子机制串联。
- **设计简洁高效**：使用固定XGBoost分类器，超参数不针对各任务单独优化，计算开销可控，易于复现。

### 8. 不足与局限
- **依赖底层PLM**：表示质量受限于预训练ESM-2可能存在的偏差和局限性，虽对不同层选择稳健，但未系统评估不同PLM架构的影响。
- **评估数据偏差**：所用数据集（IntAct、PTMint）偏向于被充分研究的蛋白质和易实验检测的扰动，某些少数类（如结合增强突变、抑制性PTM）样本量较小，可能影响性能评估的普适性。
- **可解释性局限**：注意力权重不应等同于因果重要性，所揭示的邻域只能提供假设，尚需实验验证才能确认其决定结合特异性的真实机制。
- **应用范围**：当前仅验证了二元蛋白相互作用，能否直接扩展至多蛋白复合体或更复杂的细胞环境尚不清楚。
- **算力信息缺失**：未提供明确的GPU资源和训练时间开销，使得评估其大规模应用的计算成本较为困难。

（完）
