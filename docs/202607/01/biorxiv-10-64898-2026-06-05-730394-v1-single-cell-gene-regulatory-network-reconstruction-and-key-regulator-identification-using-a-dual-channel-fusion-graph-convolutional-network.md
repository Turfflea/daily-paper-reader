---
title: Single-cell gene regulatory network reconstruction and key regulator identification using a dual-channel fusion graph convolutional network
authors: "Tang, R., Liu, J., Zhang, P., Liang, X."
date: 2026-06-07
pdf: "https://www.biorxiv.org/content/10.64898/2026.06.05.730394v1.full.pdf"
tags: ["query:mt"]
score: 8.0
evidence: 使用双通道融合图卷积网络将基因调控关系建模为图，利用GNN进行关系数据建模。
tldr: 针对单细胞基因调控网络推断中难以同时利用表达相关性和先验知识的问题，提出一种双通道融合图卷积网络。模型通过两个通道分别处理不同来源信息并融合，有效重建基因调控网络并识别关键调控因子。实验表明该方法优于现有技术，为细胞状态转换机制研究提供了有力工具。
source: biorxiv
selection_source: carryover_cache
motivation: 使用双通道融合图卷积网络将基因调控关系建模为图，利用GNN进行关系数据建模。
method: 方法与实现细节请参考摘要与正文。
result: 结果与对比结论请参考摘要与正文。
conclusion: 总体而言，该工作在所述任务上展示了有效性，并提供了可复用的思路或工具。
---

## Abstract
Background and objectiveGene regulatory networks are formed by complex regulatory relationships between transcription factors and their target genes. A systematic understanding of these regulatory relationships is crucial for deciphering the molecular mechanisms that underlie cell state transitions under physiological and pathological conditions. Single-cell expression data can reveal cell-type-specific transcriptional regulation, and computational methods have recently been developed to infer gene regulatory networks from single-cell transcriptomics and prior regulatory knowledge. However, existing methods could not explore the common and specific information in expression correlations and prior regulatory knowledge, which can adversely affect prediction performance.

MethodsWe propose a novel method for inferring gene regulatory networks from single-cell RNA sequencing data. The proposed method consists of dual-channel graph neural networks and a weight-shared common graph neural network, enabling effective fusion of prior regulatory knowledge with gene co-expression patterns. Furthermore, we formulate a new computational framework built upon the proposed algorithm, which integrates differential gene expression profiles and regulatory changes to identify key regulators that distinguish different cell states.

ResultsExperimental results demonstrate that our method significantly improves the accuracy of regulatory inference across multiple datasets, outperforming other state-of-the-art approaches. Our method also exhibits robustness to noise and missing data. Analysis of two single-cell expression datasets suggests that the proposed framework could help identify key regulators involved in tumor metastasis and drug resistance.

ConclusionThese results indicate that the proposed method could advance the understanding of the biological mechanisms underlying diseases by reconstructing single-cell gene regulatory networks and identifying key regulators across different cell states.