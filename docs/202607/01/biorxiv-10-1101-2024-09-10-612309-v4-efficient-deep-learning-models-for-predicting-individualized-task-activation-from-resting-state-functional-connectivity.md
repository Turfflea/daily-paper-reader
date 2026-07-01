---
title: Efficient Deep Learning Models for Predicting Individualized Task Activation from Resting-State Functional Connectivity
authors: "Madsen, S. J., Lee, Y.-E., Quah, S. K. L., Uddin, L. Q., Mumford, J. A., Barch, D. M., Fair, D. A., Gotlib, I. H., Poldrack, R. A., Kuceyeski, A., Saggar, M."
date: 2026-06-12
pdf: "https://www.biorxiv.org/content/10.1101/2024.09.10.612309v4.full.pdf"
tags: ["query:mt"]
score: 8.0
evidence: 使用深度学习从静息态功能连接预测个体任务激活，包括通道注意力和图神经网络架构
tldr: 针对从静息态功能磁共振预测任务激活的问题，系统评估了深度架构的效率。提出BrainSERF（通道注意力）和BrainSurfGCN（图网络）两种扩展，在Human Connectome Project数据上验证了改进的空间相关性和可扩展性，为无任务个体化脑映射提供新途径。
source: biorxiv
selection_source: carryover_cache
motivation: 使用深度学习从静息态功能连接预测个体任务激活，包括通道注意力和图神经网络架构。
method: 方法与实现细节请参考摘要与正文。
result: 结果与对比结论请参考摘要与正文。
conclusion: 总体而言，该工作在所述任务上展示了有效性，并提供了可复用的思路或工具。
---

## Abstract
Deep learning models have demonstrated the potential to predict task-evoked brain activation from resting-state fMRI, offering a pathway toward individualized brain mapping without requiring task-based data. In this study, we systematically evaluate architectural strategies for improving the efficiency and scalability of such models. Using data from the Human Connectome Project, we replicate the BrainSurfCNN framework and introduce two extensions: BrainSERF, which incorporates channel-wise attention through squeeze-and-excitation modules, and BrainSurfGCN, a graph-based model that leverages cortical mesh topology for efficient message passing. Across multiple evaluation metrics, including spatial correlation, Dice score, Dice AUC, and subject identification accuracy, all models achieve comparable predictive performance. Despite similar accuracy, the proposed models offer distinct advantages. BrainSERF provides modest improvements in capturing individual-specific features, while BrainSurfGCN achieves substantial reductions in model size and training time, highlighting a favorable trade-off between performance and computational efficiency. Beyond architectural comparisons, we investigate factors driving variability in prediction accuracy. We find that behavioral task performance, resting-state data quality, and inter-subject variability in task activation jointly constrain prediction fidelity. In particular, contrasts with lower signal reliability and higher variability exhibit reduced predictability across all models. Together, these findings demonstrate that incorporating topological and functional structural priors can improve the efficiency of deep learning models without sacrificing accuracy, while also emphasizing that prediction performance is fundamentally limited by the reliability of the underlying neural signals.