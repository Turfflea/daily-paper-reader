---
title: "Predicting P-glycoprotein Substrate Status Using a Pretrained Graph Neural Network: A TDC Benchmark Study"
title_zh: 使用预训练图神经网络预测P-糖蛋白底物状态：一项TDC基准研究
authors: "Yan, J., Duan, W."
date: 2026-06-04
pdf: "https://www.biorxiv.org/content/10.64898/2026.06.01.729343v1.full.pdf"
tags: ["query:mt"]
score: 10.0
evidence: 自监督属性掩码预训练图神经网络用于分子性质预测
tldr: P-糖蛋白底物预测对药物发现至关重要，但小样本ADMET数据带来挑战。本研究采用在大规模分子库上经属性掩码自监督预训练的图同构网络GIN，微调后用于分类。在TDC基准上取得AUROC 0.937，排名第二，超越摩根指纹XGBoost基线。证明图表示迁移学习能有效提升小数据集ADMET预测性能。
source: biorxiv
selection_source: fresh_fetch
motivation: 精确预测P-gp底物状态对早期药物发现至关重要，而小数据集限制了传统模型的泛化能力。
method: 利用在约200万分子上通过属性掩码自监督预训练的GIN编码器，并附加MLP分类头进行微调。
result: 模型在TDC Pgp_Broccatelli基准上AUROC达0.937±0.004，排行榜第二，显著优于XGBoost基线的0.912。
conclusion: 基于图神经网络的迁移学习策略为小样本ADMET性质预测提供了高效方案，展现了预训练分子表示的潜力。
---

## 摘要
P-糖蛋白（Pgp/ABCB1）是一种关键的外排转运蛋白，对药物生物利用度和多药耐药性有显著影响。准确预测P-糖蛋白底物状态对于早期药物发现至关重要。在本研究中，我们在治疗数据共享平台（TDC）的Pgp_Broccatelli基准上，评估了一种采用属性掩码策略的预训练图同构网络（GIN）。我们的方法先对使用自监督属性掩码策略在约200万个分子上预训练的GIN编码器进行微调，然后添加一个多层感知机（MLP）分类头。在TDC基准上，经过五次独立运行，我们的模型取得了0.937±0.004的AUROC，截至2026年5月在排行榜上排名第二。我们进一步将该方法与使用摩根指纹的XGBoost基线方法（AUROC 0.912±0.007）进行了比较，展现了基于图的分子表示与迁移学习在小规模数据集ADMET预测任务上的优势。

## Abstract
P-glycoprotein (Pgp/ABCB1) is a critical efflux transporter that significantly impacts drug bioavailability and multidrug resistance. Accurate prediction of Pgp substrate status is essential for early-stage drug discovery. In this study, we evaluate a pretrained Graph Iso-morphism Network (GIN) with attribute masking on the Pgp_Broccatelli benchmark from the Therapeutics Data Commons (TDC). Our approach fine-tunes a GIN encoder pretrained on approximately 2 million molecules using a self-supervised attribute masking strategy, followed by a multilayer perceptron (MLP) classification head. On the TDC benchmark, our model achieves an AUROC of 0.937 {+/-} 0.004 across five independent runs, ranking second on the leaderboard, as of May 2026. We further compare this approach against an XGBoost baseline using Morgan fingerprints (AUROC 0.912 {+/-} 0.007), demonstrating the advantage of graph-based molecular representations with transfer learning for small-dataset ADMET prediction tasks.

---

## 论文详细总结（自动生成）

### 1. 核心问题与研究动机
- **问题背景**：P-糖蛋白（Pgp）是一种关键的外排转运蛋白，其底物识别直接影响口服药物生物利用度及癌细胞多药耐药性。因此，对候选化合物的 Pgp 底物状态进行准确预测，是早期药物发现中 ADMET 评估的重要环节。
- **核心挑战**：实验标注数据稀缺（该任务训练集仅 851 个分子），传统机器学习模型（如基于固定分子指纹）难以在小样本条件下学习到泛化能力强的表示。
- **研究动机**：探索一种利用大规模无标注分子进行自监督预训练的图神经网络（GNN）方法，通过迁移学习提升小规模 ADMET 数据集的预测性能，并为 TDC 基准提供新的强基线。

### 2. 方法论
- **总体框架**：采用 “预训练 GIN 编码器 + MLP 分类头” 的架构，并进行端到端微调。
- **分子图表示**：将分子定义为图 \(G=(V,E)\)，节点特征包括元素类型、度、形式电荷、杂化状态、芳香性及氢原子数，保留完整拓扑结构；对比方法使用 2048 位摩根指纹（ECFP4 样式）。
- **自监督预训练策略（属性掩码）**：
    - 在约 200 万个来自 ZINC15 和 ChEMBL 的分子上，随机掩码部分原子（节点）和键（边）的属性。
    - 预训练目标：根据周边分子上下文预测被掩码的属性，从而使 GIN 编码器学习到可迁移的分子表示。
- **下游微调与模型细节**：
    - 编码器：5 层图同构网络（GIN），使用 DeepPurpose / DGL / DGLLife 实现。
    - 分类头：两层 MLP，隐藏层维度为 [512, 128]。
    - 训练配置：Adam 优化器，学习率 0.0005，批大小 128，二元交叉熵损失，训练 100 轮。
- **基线对比**：XGBoost 使用 2048 位摩根指纹，通过网格搜索优化超参数（max_depth, learning_rate, n_estimators），并采用早停。

### 3. 实验设计
- **数据集与分割**：
    - 采用 TDC 提供的 Pgp_Broccatelli 数据集，共 1218 个分子（二分类：底物/非底物）。
    - 官方 scaffold 分割：训练/验证集 973 个分子，测试集 245 个分子。
    - 训练与验证进一步按 5 个不同随机种子（1–5）分割为 851 训练/122 验证。
- **评价指标**：以测试集 AUROC 为主要指标，同时报告 AUPRC 和 F1 分数；最终报告 5 次独立运行的均值与标准差。
- **对比方法**：
    - 本文核心方法：AttrMasking GNN（作者提交的版本）。
    - 内部基线：XGBoost + 摩根指纹。
    - 排行榜对比：MapLight + GNN、ZairaChem、MapLight、SimGCN、原始 AttrMasking 基线、ContextPred、RDKit2D + MLP 等。

### 4. 资源与算力
- **算力情况**：论文未明确说明所使用的 GPU 型号、数量或具体训练时长。
- **可获取信息**：仅提及采用 DGL、DGLLife 以及 DeepPurpose 库实现，训练轮数为 100 epochs，批大小 128，学习率 0.0005。未提供有关硬件消耗的进一步细节。

### 5. 实验数量与充分性
- **实验组数**：
    - 5 次不同种子下的独立训练，用于评估模型稳定性和统计方差。
    - 一组对比实验：AttrMasking GNN vs. XGBoost 基线。
    - 隐式对比：与 TDC 排行榜上多个已有方法进行 AUROC 比较。
- **充分性与公平性**：
    - **充分性**：通过多种子验证（n=5）展示了方法的可复现性与鲁棒性；与强基线（XGBoost + 指纹）进行对比，确证 GNN 方法的提升。
    - **公平性**：使用 TDC 官方数据分割，保证测试集固定、对比公平；但未进行更细粒度的消融实验（如不同预训练策略、不同分类头深度的影响等），也未对模型在不同分子属性（如分子量、logP）子集上的表现做分层分析，实验深度仍有扩展空间。
    - **局限性**：单数据集评估限制了结论的泛化性；未进行像预训练数据规模、掩码比例等敏感度分析。

### 6. 主要结论与发现
- 本文提出的 AttrMasking GIN 在 TDC Pgp_Broccatelli 基准上取得了 **AUROC 0.937±0.004**，在排行榜上位列第二，仅比第一名低 0.001。
- 相较于使用摩根指纹的 XGBoost 基线（AUROC 0.912），GNN 方法绝对提升 2.5 个百分点，表明基于图的表示能够更好地捕获与 Pgp 底物识别相关的结构信息。
- 属性掩码预训练带来的迁移学习明显缓解了小标注数据集上的过拟合风险。
- 微调策略（更大的 MLP 头、更低的初始学习率、更长训练）相较官方 AttrMasking 基线 (0.929) 有显著提升，说明下游微调配置的重要性。

### 7. 优点
- **方法简洁有效**：将成熟的属性掩码 GIN 预训练范式直接应用于 ADMET 小样本任务，获得了具有竞争力的性能。
- **公平的基准测试**：完全遵循 TDC 标准评估协议（固定测试集、多种子平均），结果可与其他方法直接比较。
- **对比清晰**：不仅与 XGBoost 等传统基线对比，还列出了排行榜上多种前沿方法的性能，突显了方法的优势位置。
- **可复现性**：提供了代码链接，并详细描述了训练超参数，便于复现。

### 8. 不足与局限
- **数据集规模单一**：仅在 Pgp_Broccatelli 这一个 ADMET 终点上验证，未扩展至 TDC 其他 21 个 ADMET 任务，结论的普适性有待进一步验证。
- **缺乏立体化学信息**：当前图表示未考虑手性、cis/trans 异构，可能遗漏对 Pgp 识别至关重要的三维结构特征。
- **实验严谨性不足**：
    - 未进行消融实验（如移除预训练、改变 GNN 层数、对比不同预训练策略）。
    - 未报告模型校正、假阳性/假阴性等实用指标。
    - 未分析预测错误的分子模式，难以揭示失败模式。
- **算力与成本信息缺失**：预训练和微调的实际计算开销未被描述，不利于评估方法在大规模工业场景下的可行性。
- **分布外泛化风险**：scaffold split 虽比随机 split 更严格，但仍无法保证模型在面对全新化学骨架时的预测稳定性，文中对此仅有讨论而未开展额外验证。

（完）
