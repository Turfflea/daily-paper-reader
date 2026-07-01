---
title: "LNGCN: a distance-aware continuous-time graph protocol for prioritizing protein-protein interaction candidates"
authors: "Xiao, Y., Zheng, Y., Hua, Y., Peng, J., Liu, J., Qu, Y., Xu, J., Fu, R., Qian, Q., Zhao, M., Zhang, X., Zhao, J., Yao, Y., Kosar, M., Ke, Y., Chi, Y."
date: 2026-06-25
pdf: "https://www.biorxiv.org/content/10.64898/2026.04.30.721835v2.full.pdf"
tags: ["query:mt"]
score: 9.0
evidence: 图神经网络（LNGCN）通过距离感知连续时间图协议进行分子性质预测（蛋白相互作用）。
tldr: LNGCN提出一种距离感知的连续时间图协议，将残基层结构图与液体神经动力学相结合，以残基径向距离驱动图演化，并采用分层校准将输出转化为可解释的交互概率。该方法解决了现有图方法消息传递离散、表示过平滑和置信度校准差的问题，在多个基准数据集上实现了高精度的蛋白质相互作用预测，为分子性质预测提供了可迁移的图神经网络架构。
source: biorxiv
selection_source: fresh_fetch
motivation: 图神经网络（LNGCN）通过距离感知连续时间图协议进行分子性质预测（蛋白相互作用）。
method: 方法与实现细节请参考摘要与正文。
result: 结果与对比结论请参考摘要与正文。
conclusion: 总体而言，该工作在所述任务上展示了有效性，并提供了可复用的思路或工具。
---

## Abstract
Accurate high-throughput prediction of protein-protein interactions (PPIs) is essential for mapping cellular mechanisms and prioritizing experimental validation. Current graph-based methods often rely on discrete message passing, suffer from representation over-smoothing, and provide poorly calibrated confidence scores. We present LNGCN, a distance-aware continuous-time graph protocol that integrates residue-level structural graphs with liquid neural dynamics to model spatially heterogeneous interaction patterns. Residue radial distance is used as an explicit driving signal for continuous graph evolution, while hierarchical calibration converts raw model outputs into interpretable interaction probabilities. Across balanced, highly imbalanced, and cross-species benchmarks, LNGCN achieved robust predictive performance. Importantly, the calibrated scores supported biologically coherent prioritization in the FGF23-FGFR1c--Klotho complex, SHP2-associated signaling interactions and Tdk1 oligomeric-state-dependent binding. In a TPR-centered experimental case study, LNGCN recovered known TPR-associated partners and prioritized ELAVL1-TPR and RALY-TPR, whose physical interactions were subsequently confirmed via experimental validation. These results indicate that LNGCN can serve as a practical prioritization protocol for PPI candidates.