---
title: Reconstructing living materials as a computable design space with multi-agent reasoning
authors: "Xiao, Y., Zeng, X., Yang, Z., Gu, J., Lu, Y., Wang, Y., Wen, H., Chen, M., Huang, Z., Hu, J., Liu, J., Sha, C., Xie, J., Li, H., Zhu, X., Zheng, S., Zhang, J., Zong, W., He, Z., Xu, Y., Zhou, X., Li, F., Liu, H., He, Q., Liu, L., Yu, Z."
date: 2026-06-09
pdf: "https://www.biorxiv.org/content/10.64898/2026.02.15.705954v2.full.pdf"
tags: ["query:mt"]
score: 8.0
evidence: 多智能体推理构建活体材料知识图谱；材料知识表示
tldr: 本文提出LiveMat，一个多智能体推理框架，将非结构化文献转化为活体材料的可计算设计空间。它标准化34215条记录，构建连接活体成分、非生物基质、功能输出与评价条件的知识图谱，集成微生物和聚合物数据，实现了活体材料的系统化知识表征，为数据驱动的活体材料发现奠定基础。
source: biorxiv
selection_source: carryover_cache
motivation: 多智能体推理构建活体材料知识图谱；材料知识表示。
method: 方法与实现细节请参考摘要与正文。
result: 结果与对比结论请参考摘要与正文。
conclusion: 总体而言，该工作在所述任务上展示了有效性，并提供了可复用的思路或工具。
---

## Abstract
Artificial intelligence is increasingly used to accelerate scientific discovery, but most successful frameworks operate within well-defined molecular, protein or materials spaces. Living materials present a more formidable computational problem because functions emerge from context dependent coupling among cells, matrices, fabrication processes and evaluation conditions. Here we introduce LiveMat, a multi-agent reasoning framework that transforms unstructured literature into a computable design space for living materials. LiveMat standardizes 34,215 living material records, integrating 16,769 microorganism and 17,446 polymer entries into a knowledge graph linking living components, abiotic matrices, functional outputs, evaluation contexts and performance metrics. Benchmarking across five large language models shows that living material reasoning is limited mainly by cross-domain feature integration rather than coarse classification. LiveMat overcomes this limitation through constraint decomposition, provenance-aware extraction, consistency checking and expert-anchored ranking. In a prospective wound-healing task, it prioritizes a four-component design with state-of-the-art in vivo performance, establishing a scalable infrastructure for interpretable, evidence-grounded living material discovery.