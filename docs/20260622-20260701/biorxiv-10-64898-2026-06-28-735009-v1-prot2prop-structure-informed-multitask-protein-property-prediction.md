---
title: "Prot2Prop: Structure-informed multitask protein property prediction"
authors: "Gharaie Amirabadi, D., Jackson, C., Kim, D. S., Sprang, M., Amani, K."
date: 2026-06-29
pdf: "https://www.biorxiv.org/content/10.64898/2026.06.28.735009v1.full.pdf"
tags: ["query:mt"]
score: 9.0
evidence: 多任务深度学习模型联合预测蛋白质的六项性质
tldr: 针对蛋白质工程中开发相关性质预测效率低下的问题，Prot2Prop基于冻结的ProstT5编码器加适配器，统一联合预测材料产量、溶解度、温度稳定性等六项性质，在AUROC和Spearman相关系数上取得优异表现，为多任务预测建模提供了可参考架构。
source: biorxiv
selection_source: fresh_fetch
motivation: 多任务深度学习模型联合预测蛋白质的六项性质。
method: 方法与实现细节请参考摘要与正文。
result: 结果与对比结论请参考摘要与正文。
conclusion: 总体而言，该工作在所述任务上展示了有效性，并提供了可复用的思路或工具。
---

## Abstract
Protein engineering often relies on separate models for related developability properties, limiting efficiency and transfer across tasks. We present Prot2Prop, a multitask framework based on a frozen ProstT5 encoder with shared and task-specific adapters for joint prediction of six protein properties: material production, solubility, temperature stability, aggregation propensity, expression yield, and folding stability. Across held-out test data, Prot2Prop achieved strong performance on both classification and regression tasks, including AUROC values ranging from 0.86 to 0.98 for classification endpoints and Spearman correlations ranging from 0.73 to 0.86 for regression endpoints. The model achieved particularly strong performance for temperature stability (AUROC = 0.98) and aggregation propensity (Spearman = 0.86). Post-hoc calibration further improved regression accuracy, reducing folding stability MAE from 0.67 to 0.48. These results demonstrate that parameter-efficient multitask adaptation of protein language models can provide accurate and unified prediction of diverse protein developability properties.