---
title: "Integrative Transfer Network: Deep Transfer Learning Across Populations and Prediction Targets"
authors: "Gao, Y., Cui, Y."
date: 2026-06-16
pdf: "https://www.biorxiv.org/content/10.64898/2026.06.12.731936v1.full.pdf"
tags: ["query:mt"]
score: 8.0
evidence: 深度神经网络跨亚组多目标迁移学习；预测建模
tldr: 本文提出整合迁移网络(ITN)，一种可同时处理多个亚组和多种预测目标的深度神经网络。通过捕获共享和亚组特异性信息，ITN在临床时间-事件和分类任务上优于独立建模，为多群体多结局预测提供统一框架，有效提升复杂异构数据下的预测性能。
source: biorxiv
selection_source: carryover_cache
motivation: 深度神经网络跨亚组多目标迁移学习；预测建模。
method: 方法与实现细节请参考摘要与正文。
result: 结果与对比结论请参考摘要与正文。
conclusion: 总体而言，该工作在所述任务上展示了有效性，并提供了可复用的思路或工具。
---

## Abstract
Large-scale clinical and biomedical datasets increasingly contain both diverse subgroup attributes (e.g., demographic or clinical subgroups) and multiple prediction targets. Although various machine learning approaches can address subgroup differences or multi-target prediction, they often consider these aspects independently rather than jointly. To more effectively capture the shared and subgroup-specific information in such complex datasets, we propose the Integrative Transfer Network (ITN), a deep neural network designed to leverage data across subgroups and multiple related outcomes simultaneously. In extensive experiments, including time-to-event and classification tasks where demographic subgroups and multiple disease end-points are prevalent, ITN demonstrates consistent improvements in subgroup-specific prediction by borrowing strength from other subgroups and outcomes. We envision ITN as a unified frame-work for learning from heterogeneous datasets where subgroup-specific insights are critical.