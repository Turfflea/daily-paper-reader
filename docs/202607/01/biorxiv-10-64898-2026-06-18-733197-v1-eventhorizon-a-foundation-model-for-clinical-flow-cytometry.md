---
title: "EventHorizon: A Foundation Model for Clinical Flow Cytometry"
authors: "Medina Grespan, M., Morrison, M., O'Fallon, B., Shean, R., Spies, N. C., Ng, D."
date: 2026-06-22
pdf: "https://www.biorxiv.org/content/10.64898/2026.06.18.733197v1.full.pdf"
tags: ["query:mt"]
score: 8.0
evidence: 用于临床流式细胞术的自监督基础模型，从异构多面板数据学习统一的样本级表征
tldr: 针对临床流式细胞术依赖专家解读且现有ML模型泛化性差的难题，提出EventHorizon自监督基础模型，采用两级层次变换器和标志物感知分词，将不同抗体面板的细胞嵌入统一潜空间。预训练后仅需少量标签即可实现稳健分类，有望提升血液恶性肿瘤诊断的自动化水平。
source: biorxiv
selection_source: carryover_cache
motivation: 用于临床流式细胞术的自监督基础模型，从异构多面板数据学习统一的样本级表征。
method: 方法与实现细节请参考摘要与正文。
result: 结果与对比结论请参考摘要与正文。
conclusion: 总体而言，该工作在所述任务上展示了有效性，并提供了可复用的思路或工具。
---

## Abstract
Flow cytometry is an essential tool for diagnosis of hematologic malignancies, but existing clinical workflows are highly dependent on expert manual interpretation. Existing machine learning approaches typically require extensive labeled data and are sensitive to variability in panel design, instrumentation, and laboratory workflows, limiting their generalizability. We present EventHorizon, a self-supervised foundation model for clinical flow cytometry that produces unified specimen-level representations from heterogeneous multi-panel data. EventHorizon employs a two-stage hierarchical transformer architecture with marker-aware tokenization, enabling seamless integration of cells measured across different antibody panels into a single shared latent space. We pre-train the model using a DINO-inspired self-distillation strategy with a variety of flow cytometry-specific augmentations on a dataset of more than 100,000 clinical specimens across 17 distinct panels. We evaluate the resulting embeddings on three clinically relevant classification tasks spanning common and rare panels, demonstrating that simple k-nearest neighbor probing of frozen EventHorizon embeddings achieves performance comparable to a fully supervised baseline model and a prior panel-specific self-supervised model. To ensure EventHorizon is not simply shortcut learning on features such as the markers/panels run for a given specimen, we perform a graph-theoretic analysis of EventHorizons latent space which argues that specimen embeddings are organized primarily by biological diagnosis. Taken together, these results demonstrate that EventHorizon produces biologically meaningful, panel-agnostic specimen representations from clinical flow cytometry data which, with further development and validation, could provide a potential basis for scalable, reproducible diagnostic support across diverse clinical laboratory settings.