---
title: Deep learning based design of buried hydrogen bond networks with HBDesigner
authors: "Dieckhaus, H., Harvey, B. T., Mulikova, T., Horenstein, J. T., Nicely, N. I., Randolph, N. Z., Kuhlman, B."
date: 2026-06-11
pdf: "https://www.biorxiv.org/content/10.64898/2026.06.08.730848v1.full.pdf"
tags: ["query:mt"]
score: 8.0
evidence: 基于深度学习的采样和评分设计蛋白质氢键网络
tldr: 针对蛋白质设计中氢键网络计算设计不足的问题，提出HBDesigner算法，结合深度学习采样和原子级能量评分，在多种蛋白支架上设计了连通的氢键网络。实验表明，HBDesigner在设计含隐藏极性相互作用的单体蛋白和界面氢键网络二聚体方面优于现有工具，扩展了蛋白质设计的精度。
source: biorxiv
selection_source: carryover_cache
motivation: 基于深度学习的采样和评分设计蛋白质氢键网络。
method: 方法与实现细节请参考摘要与正文。
result: 结果与对比结论请参考摘要与正文。
conclusion: 总体而言，该工作在所述任务上展示了有效性，并提供了可复用的思路或工具。
---

## Abstract
Accurate design of hydrogen-bonding (H-bonding) interactions is a longstanding goal in protein design, as they can facilitate specific protein-protein interactions while improving the solubility of the proteins in the unbound state. Despite this, computational design of H-bond networks remains underexplored in the deep learning era. Here, we present HBDesigner, a novel algorithm for H-bond network design. Through a combination of deep learning-based sampling and atomistic energy scoring, HBDesigner outperforms existing tools in designing connected H-bond networks onto protein scaffolds. We demonstrate the usefulness of HBDesigner by creating monomeric proteins with buried polar interactions and homodimers with extended interface H-bond networks, and by installing specificity into a family of homologous heterodimers where prior design tools fail to do so. The ability to design H-bond networks into arbitrary protein scaffolds should be broadly useful for a wide range of design applications.