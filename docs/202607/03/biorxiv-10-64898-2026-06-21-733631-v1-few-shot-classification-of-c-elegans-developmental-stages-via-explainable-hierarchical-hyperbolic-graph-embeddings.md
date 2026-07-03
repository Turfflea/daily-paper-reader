---
title: Few-Shot Classification of C. elegans Developmental Stages via Explainable Hierarchical Hyperbolic Graph Embeddings
authors: "Khalid, N., Elliott, L., Obafemi-Ajayi, T., Wunsch, D., Scharf, A."
date: 2026-06-22
pdf: "https://www.biorxiv.org/content/10.64898/2026.06.21.733631v1.full.pdf"
tags: ["query:mt"]
score: 8.0
evidence: 双曲空间小样本学习框架利用深度图嵌入对发育阶段进行分类
tldr: 针对线虫发育阶段标注数据稀缺问题，提出HyperDev框架，利用庞加莱球嵌入自然编码阶段层次结构，实现基于形态图像的小样本分类。该方法结合生物学先验，在有限标注下实现准确且可解释的预测，为稀有样本分类任务提供了一种可迁移的深度学习方法。
source: biorxiv
selection_source: carryover_cache
motivation: 双曲空间小样本学习框架利用深度图嵌入对发育阶段进行分类。
method: 方法与实现细节请参考摘要与正文。
result: 结果与对比结论请参考摘要与正文。
conclusion: 总体而言，该工作在所述任务上展示了有效性，并提供了可复用的思路或工具。
---

## Abstract
Automated, accurate, and fast developmental-stage classification of C. elegans from microscopy-based morphological images is essential for aging research, drug screening, and disease modeling. However, it remains challenging due to morphological similarities between stages and the limited annotated data. In this work, we propose HyperDev, a hyperbolic few-shot learning framework that addresses these limitations by directly encoding developmental hierarchies in the embedding space, unlike conventional Euclidean approaches that treat stages as independent classes. HyperDev uses Poincare ball geometry, combined with a biologically informed developmental prior, to naturally represent stage relationships. We introduce our self-curated C. elegans dataset spanning seven developmental stages (Egg, L1-L4, Adult, Dauer) with extreme class imbalance (6-8 samples per minority class). HyperDev achieves competitive classification accuracy (76.9-88.3%) while providing intrinsic explainability across nine 7-way few-shot evaluation settings. The learned embeddings exhibited strong biological alignment (Pearson r = 0.669, p < 0.001), while significantly outperforming ProtoNet (r = 0.187), MatchingNet (r = 0.235), and RelationNet (r = 0.464). These results establish hyperbolic geometry as a principled approach to explainable few-shot learning in biological imaging, where understanding learned representations is as critical as predictive performance.

Clinical RelevanceBy enabling explainable, data-efficient developmental staging from scarce samples, HyperDev supports improved phenotype quantification for aging research, disease modeling, and drug screening.