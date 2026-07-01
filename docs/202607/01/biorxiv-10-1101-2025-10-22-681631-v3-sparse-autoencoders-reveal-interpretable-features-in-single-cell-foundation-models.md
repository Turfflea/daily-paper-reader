---
title: Sparse Autoencoders Reveal Interpretable Features in Single-Cell Foundation Models
authors: "Pedrocchi, F., Barkmann, F., Joudaki, A., Boeva, V."
date: 2026-06-16
pdf: "https://www.biorxiv.org/content/10.1101/2025.10.22.681631v3.full.pdf"
tags: ["query:mt"]
score: 8.0
evidence: 稀疏自编码器从单细胞基础模型中无标签地学习可解释特征
tldr: 针对单细胞基础模型内部机制不透明的问题，在三种常用模型（scGPT、scFoundation、Geneformer）的隐藏表示上训练稀疏自编码器，发现学习到的特征蕴含丰富的生物和技术信号，且与模型行为相关。该工作为理解和利用大规模细胞表示提供了新工具，有助于下游预测和注释任务。
source: biorxiv
selection_source: carryover_cache
motivation: 稀疏自编码器从单细胞基础模型中无标签地学习可解释特征。
method: 方法与实现细节请参考摘要与正文。
result: 结果与对比结论请参考摘要与正文。
conclusion: 总体而言，该工作在所述任务上展示了有效性，并提供了可复用的思路或工具。
---

## Abstract
Single-cell foundation models (scFMs) hold promise for applications in cell type annotation, data integration, and prediction of the effects of cell perturbations, but their internal mechanisms remain poorly understood. We investigate the structure of these models by training sparse autoencoders (SAEs) on the hidden representations of three widely used scFMs: scGPT, scFoundation, and Geneformer.The learned features reveal diverse and complex biological and technical signals, which emerge even in pre-trained models. We also observe that the encoding of this information differs between scFMs with distinct training protocols and architectures. Finally, we demonstrate that SAE-derived features are functionally related to model behavior and can be intervened upon. Suppressing batch-associated features reduces unwanted technical variation and improves data integration while preserving the core biological signal. Activating drug-encoding features steers control cells toward drug-perturbed states in a concentration-dependent manner. These findings provide a path toward more interpretable and controllable single-cell foundation models.