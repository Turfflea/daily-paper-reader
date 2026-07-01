---
title: Incorporating Surfaced-Induced Dissociation Mass Spectrometry Data into an AlphaFold-derived deep learning network improves protein structure prediction
authors: "Bolz, R. M., Day, E. H., Drake, Z. C., Harvey, S. R., Wysocki, V. H., Lindert, S."
date: 2026-06-29
pdf: "https://www.biorxiv.org/content/10.64898/2026.06.26.734850v1.full.pdf"
tags: ["query:mt"]
score: 9.0
evidence: 整合实验质谱数据的深度学习网络用于蛋白质结构预测
tldr: 针对现有蛋白质结构预测未能有效利用质谱连接信息的问题，提出 SIDFold，将表面诱导解离质谱数据嵌入 AlphaFold 衍生网络。实验表明，多聚体结构预测准确度显著提升，证实了融合实验数据可增强深度学习模型。
source: biorxiv
selection_source: fresh_fetch
motivation: 整合实验质谱数据的深度学习网络用于蛋白质结构预测。
method: 方法与实现细节请参考摘要与正文。
result: 结果与对比结论请参考摘要与正文。
conclusion: 总体而言，该工作在所述任务上展示了有效性，并提供了可复用的思路或工具。
---

## Abstract
Surface-Induced Dissociation native Mass Spectrometry (SID-nMS) is a tandem MS activation method that yields information on the connectivity and stoichiometry of protein complexes. While insufficient for direct structure elucidation, the data derived from SID-nMS has considerable potential to inform multimeric protein structure prediction. We hypothesized that incorporating this data into a machine-learning framework could improve multimer prediction accuracy beyond that of existing deep-learning methods. To this end, we developed SIDFold, a novel AlphaFold-based deep-learning network. SIDFold is the first AlphaFold-like network to leverage experimental data during protein complex prediction, and the first deep-learning network to utilize nMS data for structure prediction. We benchmarked SIDFold on the BETA protein set, and observed an improvement in RMSD in 138 of 227 cases including 27 targets in which the predicted structure attained near-native accuracy. We then evaluated the network on 20 proteins with experimental SID-nMS data, yielding an improved RMSD in 18 cases, with five of these cases improving to a high-accuracy complex. Finally, we tested SIDFold against a previously published SID-guided Rosetta docking method, where we saw improvement in 13 of 16 proteins. SIDFold is freely available on GitHub, with example files and commands available in the Supplementary Information.