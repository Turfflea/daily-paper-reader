---
title: Scaling SMILES-Based Chemical Language Models for Therapeutic Peptide Engineering
authors: "Feller, A. L., Secor, M., Swanson, S., Wilke, C. O., Deibler, K."
date: 2026-06-23
pdf: "https://www.biorxiv.org/content/10.64898/2026.01.06.697994v5.full.pdf"
tags: ["query:mt"]
score: 9.0
evidence: "在大规模分子数据集上自监督学习化学语言模型,实现无标签的表示学习"
tldr: "该论文针对治疗性肽在现有计算模型中的盲区,提出了PeptideCLM-2系列化学语言模型,在超过一亿分子上训练,可原生处理大型聚合物序列。该方法弥补了小分子与蛋白质模型之间的鸿沟,避免了静态描述符或复杂流程。实验表明,扩展SMILES语言模型能有效捕捉肽的化学细节,为肽类药物的计算设计提供了强大基础。"
source: biorxiv
selection_source: fresh_fetch
motivation: "在大规模分子数据集上自监督学习化学语言模型,实现无标签的表示学习。"
method: 方法与实现细节请参考摘要与正文。
result: 结果与对比结论请参考摘要与正文。
conclusion: 总体而言，该工作在所述任务上展示了有效性，并提供了可复用的思路或工具。
---

## Abstract
Therapeutic peptides occupy a unique middle ground in drug discovery, offering the high specificity of protein interactions with the chemical diversity of small molecules, yet they currently fall in a computational blind spot. Existing foundation models cannot handle them effectively: protein models are restricted to natural amino acids, while chemical models struggle to process large, polymer-like sequences. This disconnect has forced the field to rely on static chemical descriptors that fail to capture subtle chemical details or on complex multi-embedding pipelines that are custom tailored to specific datasets. To bridge this gap, we present PeptideCLM-2, a suite of chemical language models trained on over 100 million molecules to natively represent complex peptide chemistry. This modeling approach expands the available toolkit of machine learning models for therapeutic peptides. Benchmarking results show strong performance versus prior methods for predicting development endpoints including membrane diffusion, biological function, and half life.