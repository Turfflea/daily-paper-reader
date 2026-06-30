---
title: Untrained Convolutional Neural Networks as Feature Extractors for Structural MRI
title_zh: 未训练的卷积神经网络作为结构MRI的特征提取器
authors: "Encin, A., Pepe, I. G., Chatelain, Y., Dickie, E., Glatard, T."
date: 2026-06-10
pdf: "https://www.biorxiv.org/content/10.64898/2026.06.07.730652v1.full.pdf"
tags: ["query:mt"]
score: 9.0
evidence: 未经训练的CNN作为特征提取器在MRI任务上取得预测性能。
tldr: 结构MRI分析依赖特征提取，但预训练模型计算成本高、存在数据泄露风险且可重复性差。本文提出未经训练的卷积神经网络un-CNN，利用多通道输入、多尺度特征聚合和协方差池化从结构MRI中提取有效特征。在三个数据集和三种下游任务上，un-CNN的性能媲美或超越最先进的预训练基础模型，同时规避了训练开销，实现了轻量、隐私友好且高度可重复的特征提取。
source: biorxiv
selection_source: fresh_fetch
motivation: 预训练模型需要高计算与内存开销，存在数据泄露风险，且可重复性不佳，亟需替代方案。
method: 扩展3D CNN，引入多通道输入、分层编码器实现多尺度特征聚合，并结合协方差池化构建un-CNN。
result: 在三个结构MRI数据集和三个下游任务上，un-CNN特征提取性能达到或超越最先进预训练基础模型。
conclusion: un-CNN无需训练，降低计算与隐私风险，性能优越，为医学影像分析提供高效可重复方案。
---

## 摘要
我们证明，使用未训练的卷积神经网络（un-CNN）从结构MRI中提取的特征，在三个结构MRI数据集和三个下游任务上，其预测性能与最先进的预训练基础模型相当或更优。Un-CNN在经典3D CNN架构上进行了扩展，增加了多通道输入、具有多尺度特征聚合的分层编码器以及协方差池化。未训练的CNN规避了训练模型的几个关键限制，包括高计算成本和内存需求、分发大模型权重的需要、数据泄漏风险以及可重复性方面的挑战。

## Abstract
We demonstrate that features extracted from structural MRI using un-CNN, an untrained convolutional neural network, achieve predictive performance comparable to or exceeding that of state-of-the-art pretrained foundation models across three structural MRI datasets and three downstream tasks. Un-CNN extends a classical 3D CNN architecture with multi-channel inputs, a hierarchical encoder with multi-scale feature aggregation, and covariance pooling. Untrained CNNs circumvent several key limitations of trained models, including high computational cost and memory requirements, the need to distribute large model weights, risks of data leakage, and challenges in reproducibility.

---

## 论文详细总结（自动生成）

## 论文核心问题与整体含义
- **研究动机**：结构磁共振成像（sMRI）的特征提取是脑影像预测建模的关键步骤。传统方法（如 FreeSurfer 手工形态学管道）计算耗时，深度学习模型需要大规模训练或预训练，基础模型则需分发数 GB 的权重、存在数据泄漏风险，且可重复性差。作者偶然发现一个随机初始化的 CNN 能在脑龄预测和性别分类任务上取得与传统管道相当的性能，由此提出系统研究未训练 CNN 作为 sMRI 特征提取器的可能性。
- **整体含义**：本文旨在探索并验证一种完全不需要训练的 CNN 架构（un-CNN），仅依靠网络结构本身的归纳偏置与精心设计的预处理和多尺度池化，即可产出与先进预训练基础模型相当甚至更好的表型预测特征，从而大幅降低计算成本、避免隐私与可重复性问题。

## 提出的方法论
- **核心思想**：利用随机初始化且冻结的 3D CNN，通过多通道输入、分层编码器与协方差池化，从 sMRI 中提取具有判别力的固定维度特征向量，无需任何训练或微调。
- **un-CNN 架构关键技术细节**：
  - **多通道输入构建**：在 TurboPrep 预处理（N4 偏置校正、颅骨剥离、刚体配准到 MNI152、重采样到 1 mm 各向同性、白条带强度归一化）后，将脑图像进行稳健强度缩放，生成三种版本——原始强度图、中值秩滤波（保边平滑）图、Sobel 梯度幅值图，堆叠成 3×91×109×91 的输入张量。
  - **分层编码器**：四个顺序块，通道数依次为 64、128、256、512。第一块使用标准卷积，后续块使用深度可分离卷积。每块包含两次连续卷积、GroupNorm 和 ReLU，通过步幅 2 的平均池化下采样。从每个块的输出提取特征，而非仅使用最后块。
  - **协方差池化与多尺度读出**：对每个块，自适应平均池化到 2×2×2 空间网格得到均值特征（通道数×8）。同时，取前 32 个通道计算样本协方差矩阵，保留上三角元素（(32×31)/2 = 496 个协方差特征）。每个块均得到均值特征与协方差特征，拼接后组成 9664 维的嵌入。
  - **权重初始化**：所有参数使用 PyTorch 默认 Kaiming 均匀初始化，固定随机种子为 0，初始化后立即冻结。
- **推理流程**：预处理后直接单次前向传播，无训练循环、梯度计算或优化器状态。

## 实验设计
- **数据集**：
  - NKI（Nathan Kline Institute–Rockland Sample）：958 名健康寿命受试者（6–85 岁），用于优化架构。
  - HBN（Healthy Brain Network）：1000 名儿童/青少年（5–22 岁，富含学习与心理健康障碍），作为保留测试集。
  - PPMI（Parkinson’s Progression Markers Initiative）：894 名老年临床人群（帕金森病和对照），完全未参与优化，进一步测试泛化性。
  - 所有数据均为公开可用。
- **下游任务**：性别分类（平衡准确率）、脑龄回归（平均绝对误差，MAE）、BMI 回归（MAE，仅 NKI 和 HBN）。
- **基准方法**：
  - **预训练基础模型**：AnatCL（3D ResNet-18，解剖对比学习，3984 名健康人）、3D-Neuro-SimCLR（3D ResNet-18，SimCLR，44958 扫描）、BrainIAC（视觉 Transformer，SimCLR，48965 多参数扫描）、SwinBrain（SwinUNETR，掩码重建与对比学习，75861 扫描）。
  - **FreeSurfer 形态学特征**：基于 Desikan–Killiany 图谱（136 维）和 Schaefer 400 分区图谱（800 维）的皮层厚度与表面积。
- **评估方式**：所有模型输出固定长度特征，喂入统一的下游随机森林（200 棵树，最大深度 6，最小分裂样本 5），基于 90/10 随机划分、5 个不同种子的重复，报告均值±标准差。架构优化仅在 NKI 上进行，HBN 和 PPMI 作为完全保留集，BMI 为额外保留任务，避免过拟合。

## 资源与算力
- **un-CNN 推理成本**：在单线程 CPU 上，总耗时约 2.68 秒/图像（含预处理、特征提取、随机森林预测），参数仅 0.68 M。无训练成本。
- **对比模型预训练成本**：
  - AnatCL：约 100 小时（基于原论文信息估计）。
  - BrainIAC：72 小时。
  - SwinBrain：500 epochs × 8 NVIDIA A100 GPU。
  - 3D-Neuro-SimCLR：150 epochs × 12 NVIDIA H100 GPU。
  - FreeSurfer：每个受试者数小时（v6.0 约 5.2 小时，v7.3.2 约 4.3 小时，v8.0+ 约 2 小时）。
- 文中未提及 un-CNN 架构探索时的 GPU 消耗（因无需训练，主要算力开销为预处理和 CPU 推理）。

## 实验数量与充分性
- **基准实验**：3 个数据集、3 种下游任务（PPMI 不评 BMI，部分基础模型在 PPMI 因预训练数据重叠被排除），共计十多组对比实验，全面覆盖健康人群、青少年和老年临床人群。
- **消融研究**：在 NKI 上进行累计架构消融，共 10 个步骤（从默认 CNN 逐步添加多尺度读出、GroupNorm+AvgPool、百分位裁剪、深度可分离卷积、多通道输入、双卷积、秩滤波、更紧裁剪、协方差池化、稳健缩放），每个阶段评估性别分类与脑龄回归，纵向展示各组件贡献。
- **公平性**：优化集（NKI）与保留集（HBN、PPMI）严格分离，BMI 为额外保留任务，避免信息泄露。对比基线包括多种最新基础模型和传统基准，使用统一的下游学习器和评估协议。多次随机分割评估统计可靠性。
- 实验设计较为充分且客观。

## 主要结论与发现
- un-CNN 在三个数据集的所有任务上与预训练基础模型性能相当或更优：脑龄预测在 NKI 上 MAE 达 5.11 年（优化集），HBN 1.51 年，PPMI 5.74 年；性别分类准确率最高 85.7%（NKI）；BMI MAE 3.13（NKI）和 2.73（HBN），普遍优于所有对比基线。
- 多尺度读出和协方差池化是性能提升的关键，消融实验显示年龄 MAE 从默认架构的 11.23 年降至 5.11 年，协方差池化单独带来 6.17→5.74 年的提升。
- un-CNN 克服了训练模型的多个局限：无需训练、无数据泄漏风险、权重分发仅需代码和种子、计算可重复性高（数值不确定性低）、推理速度极快。
- 随机初始化的 CNN 凭借架构归纳偏置、手工设计的边缘/纹理输入与二阶统计量，能够编码足够的脑解剖信息。

## 优点
- **概念新颖**：系统性地将“未训练网络”引入 sMRI 特征提取，提出一整套专用架构设计。
- **性能突出且实用**：在多个数据集和任务上达到或超越大型预训练基础模型，同时大幅降低计算和时间成本，易于大规模部署。
- **隐私友好与可重复**：模型从未接触任何真实脑数据，无数据泄漏风险；固定种子确保特征确定性，数值稳定性高。
- **严谨的消融分析**：清晰量化每个架构组件和预处理步骤的性能贡献，可解释性强。
- **公平对比**：采用统一下游管道、保留测试集，与多种前沿基础模型和经典基准比较，结论可靠。

## 不足与局限
- **任务范围有限**：仅涉及性别、年龄和 BMI 这三项群体级回归/分类任务，未测试更细粒度的临床诊断、病灶分割、认知评分预测等任务，尚不能证明未训练特征在所有神经影像任务上的普适性。
- **模态限制**：仅验证在 T1 加权 sMRI 上，未拓展到功能 MRI、弥散张量成像等其他模态。
- **固定随机种子与超参**：仅报告种子 0 的结果，未分析不同随机种子对性能的影响；协方差池化只用前 32 通道，输入分辨率等也由 NKI 优化决定，缺乏对关键超参数的敏感性分析。
- **可能过拟合优化指标**：架构选择完全基于 NKI 上的性别和年龄任务，其他数据虽未用于训练，但优化标准可能与保留集任务相关，存在一定程度的选择偏倚。
- **依赖于配准与预处理**：需要将图像配准到标准空间，对扫描协议变化或病灶严重扭曲的大脑可能不够稳健。

（完）
