---
title: "vDeepInsight: an injective three-dimensional voxel carrier for tabular-feature neighborhood learning"
authors: "Jia, S., Lysenko, A., Boroevich, K. A., Sharma, A., Tsunoda, T."
date: 2026-06-26
pdf: "https://www.biorxiv.org/content/10.64898/2026.06.22.733711v1.full.pdf"
tags: ["query:mt"]
score: 8.0
evidence: 通过保持特征邻域的单射3D体素载体实现表格数据深度学习
tldr: 针对表格数据转换图像时特征邻域失真问题，提出 vDeepInsight，通过流形嵌入和优化分配生成单射 3D 体素载体，再用稀疏 3D 卷积网络学习。在生物数据上，该方法比 2D 方法更忠实保留邻域，提升了预测性能。
source: biorxiv
selection_source: carryover_cache
motivation: 通过保持特征邻域的单射3D体素载体实现表格数据深度学习。
method: 方法与实现细节请参考摘要与正文。
result: 结果与对比结论请参考摘要与正文。
conclusion: 总体而言，该工作在所述任务上展示了有效性，并提供了可复用的思路或工具。
---

## Abstract
DeepInsight-style methods make tabular feature relationships accessible to convolutional networks by placing each feature at a fixed position on an image carrier. An open design question is how the carrier geometry should be constructed when feature neighborhoods themselves carry part of the signal. We introduce vDeepInsight, an injective three-dimensional (3D) voxel carrier that preserves feature neighborhoods more faithfully than matched two-dimensional (2D) carriers while keeping a one-to-one mapping from each feature to a single voxel. Genes are embedded with t-SNE or UMAP, assigned one-to-one to a sparse voxel grid by linear-sum assignment, and processed by a submanifold sparse 3D convolutional network. We evaluate the carrier on gene expression through four linked analyses. First, representation-quality metrics show that 3D layouts reduce gene-neighborhood distortion relative to matched 2D layouts before any model is trained. Second, controlled synthetic tasks show that a sparse 3D convolution can exploit this preserved locality, but only when the supervised signal is constructed to depend on co-located genes and the receptive field spans adjacent voxels. Third, on real omics tasks the 3D carrier matches or exceeds tuned tabular baselines and consistently exceeds matched 2D carriers; the margin is small on marker-type classification, where individual genes already carry much of the label (tissue, lineage and cancer-type classification), and larger on program-type tasks, where the target depends on coordinated, pathway-level multi-gene activity (drug-response regression, TCGA immunogenomic-context regression and mechanism-of-action classification). Fourth, because the assignment is injective, voxel attribution maps directly back to genes, enabling gene-level attribution and pathway-level functional interpretation without voxel-to-gene deconvolution. Overall, the added carrier dimension improves the fidelity of feature-neighborhood representation and translates this improvement into prediction gains that are largest when the signal is distributed across local gene programs rather than dominated by individual marker genes.