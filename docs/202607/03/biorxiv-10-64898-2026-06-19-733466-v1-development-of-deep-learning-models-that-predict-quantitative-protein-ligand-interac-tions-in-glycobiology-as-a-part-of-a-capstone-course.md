---
title: Development of Deep-Learning Models that Predict Quantitative Protein-Ligand Interac-tions in Glycobiology as a part of a Capstone Course
authors: "Yin, H., Liu, W., Zhou, W., Chang, Z., Carpenter, E. J., Satyajith, A., Haregu, S., Greiner, R., Derda, R."
date: 2026-06-24
pdf: "https://www.biorxiv.org/content/10.64898/2026.06.19.733466v1.full.pdf"
tags: ["query:mt"]
score: 8.0
evidence: 预测糖生物学中蛋白-配体相互作用强度的深度学习模型
tldr: 为解决无法通用预测糖与蛋白结合强度的问题，开发基于混合数据驱动和分子输入的深度学习模型。该模型首次实现了从氨基酸序列和糖结构直接预测结合力，为糖生物学研究提供了新工具。
source: biorxiv
selection_source: carryover_cache
motivation: 预测糖生物学中蛋白-配体相互作用强度的深度学习模型。
method: 方法与实现细节请参考摘要与正文。
result: 结果与对比结论请参考摘要与正文。
conclusion: 总体而言，该工作在所述任务上展示了有效性，并提供了可复用的思路或工具。
---

## Abstract
Glycans coat the surface of all cells, and every glycan is recognised by specific glycan-binding proteins (GBPs). There are no general tools that can accurately estimate the binding strength between glycan and GBP from the amino acid sequence of the GBP and the molecular structure of the glycan, represented as SMILES string. We describe models for predicting such binding strengths developed as a part of a Capstone Course at the University of Alberta. The models are trained on a dataset that combines BindingDB, a published database of small-molecule protein interactions, and data from glycan arrays measured by Consortium of Functional Glycomics (CFG). In this hybrid dataset of protein-ligand interactions the ligands are both glycans from CFG and small molecules from BindingDB; similarly, proteins include GBP and proteins from BindingDB. Three models are presented (i) ProMax which fuses ESM-2, MolFormer, and MolCLR features; (ii) APEX which constrains learning to a predetermined form, a physical model of binding; (iii) UltraMax adds inter-atomic distances for the ligands. To address the datasets severe long-tail distribution, the models employ tail-aware losses for rare high-binding instances. Trained and evaluated on approximately one million protein-ligand pairs using hold-out splits for unseen molecules, the three models provide a unified framework for quantitative glycan-protein binding prediction. We observed that learning glycan-protein binding is harder than the similar task of learning small-molecule-protein interactions. Simple mirror-inversion tests led us to postulate that insufficient use of chiral features is an important source of difficulty in learning these interactions.