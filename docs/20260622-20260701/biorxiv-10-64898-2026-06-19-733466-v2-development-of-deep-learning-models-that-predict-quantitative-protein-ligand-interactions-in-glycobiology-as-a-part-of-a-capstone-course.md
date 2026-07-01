---
title: Development of Deep-Learning Models that Predict Quantitative Protein-Ligand Interactions in Glycobiology as a part of a Capstone Course
authors: "Yin, H., Liu, W., Zhou, W., Chang, Z., Carpenter, E. J., Satyajith, A., Haregu, S., Greiner, R., Derda, R."
date: 2026-06-26
pdf: "https://www.biorxiv.org/content/10.64898/2026.06.19.733466v2.full.pdf"
tags: ["query:mt"]
score: 9.0
evidence: 深度学习模型从蛋白质序列和聚糖SMILES预测结合强度
tldr: 针对缺乏从蛋白质序列和聚糖SMILES预测结合强度通用工具的局限，本文开发了深度学习模型，在整合BindingDB和聚糖阵列数据的混合数据集上训练，实现了糖生物学中蛋白质-配体相互作用的定量预测。该工作填补了糖生物学预测工具的空白，具有实际应用价值。
source: biorxiv
selection_source: fresh_fetch
motivation: 深度学习模型从蛋白质序列和聚糖SMILES预测结合强度。
method: 方法与实现细节请参考摘要与正文。
result: 结果与对比结论请参考摘要与正文。
conclusion: 总体而言，该工作在所述任务上展示了有效性，并提供了可复用的思路或工具。
---

## Abstract
Glycans coat the surface of all cells, and every glycan is recognised by specific glycan-binding proteins (GBPs). There are no general tools that can accurately estimate the binding strength between glycan and GBP from the amino acid sequence of the GBP and the molecular structure of the glycan, represented as SMILES string. We describe models for predicting such binding strengths developed as a part of a Capstone Course at the University of Alberta. The models are trained on a dataset that combines BindingDB, a published database of small-molecule protein interactions, and data from glycan arrays measured by Consortium of Functional Glycomics (CFG). In this hybrid dataset of protein-ligand interactions the ligands are both glycans from CFG and small molecules from BindingDB; similarly, proteins include GBP and proteins from BindingDB. Three models are presented (i) ProMax which fuses ESM-2, MolFormer, and MolCLR features; (ii) APEX which constrains learning to a predetermined form, a physical model of binding; (iii) UltraMax adds inter-atomic distances for the ligands. To address the datasets severe long-tail distribution, the models employ tail-aware losses for rare high-binding instances. Trained and evaluated on approximately one million protein-ligand pairs using hold-out splits for unseen molecules, the three models provide a unified framework for quantitative glycan-protein binding prediction. We observed that learning glycan-protein binding is harder than the similar task of learning small-molecule-protein interactions. Simple mirror-inversion tests led us to postulate that insufficient use of chiral features is an important source of difficulty in learning these interactions.