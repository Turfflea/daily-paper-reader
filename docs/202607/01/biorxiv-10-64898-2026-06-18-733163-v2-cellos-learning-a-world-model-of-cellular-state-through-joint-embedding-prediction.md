---
title: "CellOS: Learning a World Model of Cellular State through Joint Embedding Prediction"
authors: "Zhou, Q., Le, Y., Qi, X., Chang, S., Lu, H., Wu, Y., Wang, H., Ran, R., li, x."
date: 2026-06-25
pdf: "https://www.biorxiv.org/content/10.64898/2026.06.18.733163v2.full.pdf"
tags: ["query:mt"]
score: 8.0
evidence: 自监督多视图基础模型，从配对的表达和感知视图学习细胞表征
tldr: 针对现有单细胞基础模型仅利用单一基因表达视图的局限，提出CellOS多视图基础模型，通过因果细胞句子语言建模、功能保持扰动和联合嵌入预测三阶段训练，融合表达与感知互补视图。在多个单细胞数据集上表明其能学习更丰富的细胞状态表征，为虚拟细胞建模提供了新范式。
source: biorxiv
selection_source: carryover_cache
motivation: 自监督多视图基础模型，从配对的表达和感知视图学习细胞表征。
method: 方法与实现细节请参考摘要与正文。
result: 结果与对比结论请参考摘要与正文。
conclusion: 总体而言，该工作在所述任务上展示了有效性，并提供了可复用的思路或工具。
---

## Abstract
Foundation models learned from single-cell transcriptomes are central to the prospect of AI virtual cell that can represent, query and predict cellular state. However, most current single-cell foundation models learn from a single view of gene expression and are optimized primarily through reconstruction or next-token prediction. As a result, they capture expression abundance but cannot explicitly reconcile complementary views of cellular state. Here we present CellOS, a multi-view foundation model that learns cellular representations from paired expression and perception views. CellOS integrates complementary views through a scalable three-stage training strategy that combines causal cell-sentence language modelling, function-preserving dense-to-mixture-of-experts expansion and latent-space alignment via an LLM-JEPA objective. Using this framework, we trained a 12-billion-parameter model on 390.5 million single-cell transcriptomes. Across diverse benchmarks spanning cell-state annotation, batch integration and perturbation-response prediction, CellOS consistently outperformed state-of-the-art single-cell foundation models. Together, these results suggest that predictive alignment between complementary cellular views provides a scalable path toward representation-centric cellular world models and transferable AI virtual cells.