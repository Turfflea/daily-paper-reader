---
title: Learning Perturbation Effects Through Contrastive Alignment of Multimodal Biological Embeddings
authors: "Long, W., Liu, T., Szalata, A., Theis, F. J., Xue, L., Zhao, H."
date: 2026-06-26
pdf: "https://www.biorxiv.org/content/10.64898/2026.06.23.734145v1.full.pdf"
tags: ["query:mt"]
score: 9.0
evidence: CLIP式多模态框架用于生物扰动的自监督表征学习。
tldr: PertOmni采用CLIP式对比学习框架，将单细胞转录组扰动信号与文本（基因和化合物描述）及图像嵌入对齐，联合训练共享转录组编码器，实现跨模态自监督表征。该方法解决了现有方法仅针对单一模态且忽略外部语义知识导致的泛化局限，实验表明其能有效泛化至不同数据集和扰动类型，为多模态生物数据整合和扰动预测提供可迁移的表征学习方法。
source: biorxiv
selection_source: fresh_fetch
motivation: CLIP式多模态框架用于生物扰动的自监督表征学习。
method: 方法与实现细节请参考摘要与正文。
result: 结果与对比结论请参考摘要与正文。
conclusion: 总体而言，该工作在所述任务上展示了有效性，并提供了可复用的思路或工具。
---

## Abstract
Multimodal single-cell perturbation screens offer a scalable approach for characterizing the effects of genetic and chemical interventions on cellular state. However, most existing representation-learning methods are tailored to a single perturbation modality and fail to explicitly incorporate external semantic knowledge, which limits their ability to generalize across datasets and perturbation types. Here, we introduce PertOmni, a CLIP-style multimodal representation-learning framework that aligns transcriptomic perturbation signatures with text-derived embeddings of curated genes and compound descriptions, as well as image-derived embeddings from cell paintings. PertOmni jointly trains a shared transcriptomic encoder and dataset-specific text encoders using a masked contrastive objective that emphasizes within-cell-type discrimination while mitigating confounding effects arising from cell-type heterogeneity. We evaluate the produced joint embedding space on bi-directional retrieval, drug-gene interaction inference, and perturbation prediction across both small-molecule and CRISPRi perturbation datasets, and demonstrate consistent improvements over strong baseline methods.