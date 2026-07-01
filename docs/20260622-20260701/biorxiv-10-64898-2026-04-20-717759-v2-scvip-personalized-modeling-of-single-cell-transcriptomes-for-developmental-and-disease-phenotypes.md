---
title: "scVIP: personalized modeling of single-cell transcriptomes for developmental and disease phenotypes"
authors: "Lai, H.-Y., Yoo, Y., Tjaernberg, A., Li, R., He, Z., Travaglini, K. J., Qiao, Q., Agrawal, A., Kana, O., van Velthoven, C., Carroll, J. B., Gillespie, M., Mukherjee, S., Fardo, D. W., Li, X.-j., Lein, E., Gabitto, M. I."
date: 2026-06-27
pdf: "https://www.biorxiv.org/content/10.64898/2026.04.20.717759v2.full.pdf"
tags: ["query:mt"]
score: 9.0
evidence: 利用深度生成模型从单细胞转录组学预测表型
tldr: 该论文针对单细胞转录组数据难以关联个体表型的问题，提出生成模型scVIP，通过细胞类型感知的多实例学习架构整合基因表达、细胞组成和表型测量，实现发育年龄等表型的精准预测（Pearson r=0.95）并定位关键细胞群。该方法为单细胞水平的个性化建模提供了可解释的框架，对疾病表型分析具有重要价值。
source: biorxiv
selection_source: fresh_fetch
motivation: 利用深度生成模型从单细胞转录组学预测表型。
method: 方法与实现细节请参考摘要与正文。
result: 结果与对比结论请参考摘要与正文。
conclusion: 总体而言，该工作在所述任务上展示了有效性，并提供了可复用的思路或工具。
---

## Abstract
Single-cell transcriptomics resolves cellular heterogeneity within individuals, but connecting molecular states to individual-level phenotypes requires frameworks that explicitly bridge these scales. We present scVIP, a generative model that links gene expression, cell-type composition, and phenotypic measurements within a single probabilistic model, which enables accurate phenotype prediction and interpretable trajectory inference. A cell-type-aware multi-instance learning architecture learns donor embeddings that capture progression while localizing phenotype-associated signals to specific cell populations. Applied across four settings, scVIP accurately predicts cortical developmental age (Pearson r = 0.95), characterizes Huntingtons disease progression (concordance correlation coefficient = 0.90), integrates two Alzheimers disease cohorts recovering disease-relevant microglial and astrocytic programs, and distinguishes healthy from ACPA-positive individuals and non-progressors from early RA individuals, identifying inflammatory T cell programs associated with disease. scVIP enables principled analysis of how cellular states collectively shape organism-level phenotypes across development and disease.