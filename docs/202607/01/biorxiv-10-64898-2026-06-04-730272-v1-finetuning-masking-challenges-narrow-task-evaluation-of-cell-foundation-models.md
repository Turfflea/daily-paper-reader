---
title: Finetuning masking challenges narrow-task evaluation of cell foundation models
authors: "Shakeel, M. H., Shen, M., Mangiola, S."
date: 2026-06-06
pdf: "https://www.biorxiv.org/content/10.64898/2026.06.04.730272v1.full.pdf"
tags: ["query:mt"]
score: 8.0
evidence: 自监督细胞基础模型用于表示学习
tldr: 针对单细胞基础模型，本文评估预训练数据量对下游性能的影响，发现微调会掩盖数据规模效应，挑战了“更多数据带来更好表示”的假设。实验显示，大幅减少预训练数据后，下游任务性能几乎不变，揭示了微调掩蔽效应。这表明仅靠数据扩展不一定提升表示质量，需要更全面的评估基准。
source: biorxiv
selection_source: carryover_cache
motivation: 自监督细胞基础模型用于表示学习。
method: 方法与实现细节请参考摘要与正文。
result: 结果与对比结论请参考摘要与正文。
conclusion: 总体而言，该工作在所述任务上展示了有效性，并提供了可复用的思路或工具。
---

## Abstract
Single-cell foundation models are large, self-supervised deep learning networks pretrained on millions of cellular transcriptomes. These models promise to deliver cell representations that are transferable across diverse biological domains and, when used in specific tasks, would outperform narrowly scoped models. A central assumption is that more pretraining data translates to better downstream performance. However, despite its centrality, this assumption remains largely untested. Here, we tested downstream performance on gold-standard benchmarking tasks across massive dataset reductions, showing that performance was largely insensitive to pretraining data size once finetuning was allowed. This trend reveals a finetuning masking effect that offsets differences in representation quality induced by pretraining, making the benefit of additional pretraining scale largely invisible under current benchmark settings. These findings challenge current benchmarking standards, which rely on closed-ended finetuning tasks that are too narrow to expose the full representational value of pretraining. They also challenge the main driving force in single-cell foundation-model development when evaluated through common narrow tasks. We propose that the next generation of foundation models should be assessed less by performance on highly optimised finetuning tasks and more by their ability to support open-ended biological inference, frozen-representation evaluation and zero-shot capability.