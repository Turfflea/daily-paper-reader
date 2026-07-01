---
title: Computational reconstruction of hierarchical cis-regulatory networks reveals synergistic transcription control and disease-associated rewiring
authors: "Zhu, X., Zhou, X., Zhang, Y., Cai, G., Zhao, W., Zhou, B., Zhou, J., Tang, Z., Liu, J., Zhu, Q., Cao, J., Yang, B., Gu, X., Zhou, Z."
date: 2026-06-26
pdf: "https://www.biorxiv.org/content/10.64898/2026.06.24.734159v1.full.pdf"
tags: ["query:mt"]
score: 9.0
evidence: "深度学习框架将顺式调控建模作为潜在图推理任务,用于关系数据建模"
tldr: "该论文针对顺式调控元件如何协同调控基因转录的难题,提出ORIGAMI框架,将调控建模转化为潜在图推理任务。该框架融合DNA序列、表观基因组和三维染色质先验,推断功能性而非仅结构性的调控图谱。通过转录输出约束,ORIGAMI能复原噪声抑制的调控网络,揭示协同调控模式,并在疾病样本中发现调控重连接,为基因调控网络重建提供了新范式。"
source: biorxiv
selection_source: fresh_fetch
motivation: "深度学习框架将顺式调控建模作为潜在图推理任务,用于关系数据建模。"
method: 方法与实现细节请参考摘要与正文。
result: 结果与对比结论请参考摘要与正文。
conclusion: 总体而言，该工作在所述任务上展示了有效性，并提供了可复用的思路或工具。
---

## Abstract
Gene regulation emerges from coordinated interactions among dispersed cis-regulatory elements, yet how these elements integrate into functional regulatory networks and collectively regulate gene transcription remains poorly understood. Here, we present ORIGAMI, a multi-omics, gene-centric deep learning framework that reconstructs functional cis-regulatory networks constrained by transcriptional output. ORIGAMI formulates cis-regulatory modeling as a latent graph inference task, which integrates DNA sequence, epigenomic signals, and three-dimensional chromatin priors to infer denoised regulatory graphs that capture functional interactions rather than structural proximity alone. The inferred regulatory graphs exhibit distinct topological regimes, where hierarchical and modular organization encodes cell-state-specific functional demands and enables synergistic transcriptional control. Furthermore, we show that these regulatory architectures undergo measurable state-dependent rewiring across disease contexts. Finally, ORIGAMI accurately predicts the transcriptional consequences of both cis- and trans-regulatory perturbations and links the rearrangement of regulatory architecture to perturbation response. Together, ORIGAMI advances a network-based view of gene regulation and establishes a foundation for virtual cell modeling of regulatory dynamics.