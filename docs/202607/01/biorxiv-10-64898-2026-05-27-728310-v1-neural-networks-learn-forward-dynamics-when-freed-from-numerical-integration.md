---
title: Neural networks learn forward dynamics when freed from numerical integration
authors: "Bahdasariants, S., Yakovenko, S."
date: 2026-06-01
pdf: "https://www.biorxiv.org/content/10.64898/2026.05.27.728310v1.full.pdf"
tags: ["query:mt"]
score: 8.0
evidence: 神经网络学习前向动力学以预测肢体运动。
tldr: 针对人机交互中肌肉骨骼动力学预测的数值不稳定问题，发现当神经网络从数值积分中解放出来时，能够学习准确的前向动力学模型。该方法直接从肌肉力或关节力矩预测肢体姿势随时间的变化，避免了传统统计映射导致的预测不稳定。这一发现为开发稳健的运动控制接口提供了新思路。
source: biorxiv
selection_source: carryover_cache
motivation: 神经网络学习前向动力学以预测肢体运动。
method: 方法与实现细节请参考摘要与正文。
result: 结果与对比结论请参考摘要与正文。
conclusion: 总体而言，该工作在所述任务上展示了有效性，并提供了可复用的思路或工具。
---

## Abstract
Seamless interaction between humans and machines requires interfaces that remain robust to the variability inherent in biological signals and physical environments. Advanced human-machine interfaces (HMIs) increasingly rely on machine learning to predict or control limb dynamics. These systems must learn input-to-output mappings between control variables and limb state, such as the mapping from muscle forces or joint torques acting about segmented arm joints to limb posture over time. Such statistical input-to-output transformations can result in numerical instability of predicted musculoskeletal kinematics and dynamics. Achieving the robustness of biological motor control requires solving both forward and inverse dynamics problems; however, these problems are computationally asymmetric because they entail opposing operations-integration and differentiation. Since we have previously shown that neural networks solve the inverse dynamics problem when trained to map kinematic to dynamic signals during reaching, we hypothesized that representing separately the approximation of equations of motion (EOM) and their temporal numerical integration may capture the relevant computational structure of the forward dynamics problem. We tested this hypothesis by comparing a conventional direct-mapping recurrent neural network (RNN) with a two-stage model, the artificial physics engine (APE). When predicting the state of a two-segment system under external perturbations not encountered during training, the direct-mapping, monolithic model produced large prediction errors inconsistent with the expected interaction torque, whereas the APE maintained low error and remained stable under novel initial conditions and perturbations. Mapping system dynamics in the terms of the EOM improves robustness against intrinsic and extrinsic sources of variability by imposing a causal, physics-based structure on HMI design.