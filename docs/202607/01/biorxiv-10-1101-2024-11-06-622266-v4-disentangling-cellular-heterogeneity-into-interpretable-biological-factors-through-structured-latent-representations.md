---
title: Disentangling cellular heterogeneity into interpretable biological factors through structured latent representations
authors: "Moinfar, A. A., Theis, F. J."
date: 2026-06-08
pdf: "https://www.biorxiv.org/content/10.1101/2024.11.06.622266v4.full.pdf"
tags: ["query:mt"]
score: 8.0
evidence: 无监督深度生成模型学习维度可解释表示，无需监督先验
tldr: 为解决单细胞组学数据中深度生成模型潜在空间难以解释的问题，该文提出DRVI，一种无监督深度生成模型，通过加性解码子网络和对数和指数池化，实现维度可解释的非线性表示学习，无需细胞类型或生物过程的监督先验。实验表明，DRVI在单细胞数据分析中既能保持非线性灵活性，又能提供可解释的生物学因子，为其他领域的高维数据表示学习提供了迁移方法。
source: biorxiv
selection_source: carryover_cache
motivation: 无监督深度生成模型学习维度可解释表示，无需监督先验。
method: 方法与实现细节请参考摘要与正文。
result: 结果与对比结论请参考摘要与正文。
conclusion: 总体而言，该工作在所述任务上展示了有效性，并提供了可复用的思路或工具。
---

## Abstract
Deep generative models have become central to single-cell omics analysis, but their latent spaces remain difficult to interpret biologically. Linear factor models offer dimension-wise interpretability, but often lack the nonlinear flexibility, scalability, and integration quality required for large multi-batch atlases. We bridge this gap with Disentangled Representation Variational Inference (DRVI), an unsupervised deep generative model that learns dimension-wise interpretable representations for single-cell omics without supervised priors on cell types or biological processes. DRVI achieves this through additive decoder subnetworks combined with log-sum-exp pooling, enabling disentangled nonlinear gene programs. Across atlases, perturbation screens, and developmental datasets, DRVI separates cell identity, signaling pathways, stress responses, developmental trajectories, and perturbation effects into distinct interpretable factors. These factors identify rare migratory dendritic cells and recover coherent combinatorial perturbation programs in CRISPR screens. Systematic benchmarks show that this interpretability does not reduce integration performance. DRVI recovers biological factors more accurately while maintaining competitive integration quality. Altogether, DRVI provides a practical route to nonlinear single-cell modeling with factor-level interpretability.