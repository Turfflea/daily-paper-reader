---
title: "FeTS: A Feature-Aware Framework for Time Series Forecasting"
title_zh: FeTS：一种用于时间序列预测的特征感知框架
authors: "Le Wang, Jianyong Chen, Songbai Liu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39838/43799"
tags: ["query:mt"]
score: 10.0
evidence: 特征感知的深度学习框架，通过自适应特征提取实现时间序列预测
tldr: 时间序列中不同时间点和特征组合的预测重要性分布不均，统一处理方式效果不佳。为此提出FeTS，包含自适应特征提取模块动态发现每个时间块内最重要的特征并即时提取，同时双尺度前馈网络策略性融合细粒度和粗粒度时间表示，从而全面学习时间特征，在多个预测基准上展示了优越性，为深度时序模型提供了特征感知的新范式。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39838/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 872, \"height\": 524, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39838/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1798, \"height\": 868, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39838/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 874, \"height\": 453, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39838/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 878, \"height\": 861, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39838/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 858, \"height\": 738, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39838/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1827, \"height\": 599, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39838/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 871, \"height\": 329, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39838/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 849, \"height\": 333, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39838/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 869, \"height\": 325, \"label\": \"Table\"}]"
motivation: 时间序列数据预测重要性分布不均，统一处理容易忽略关键特征。
method: 提出自适应特征提取模块和双尺度前馈网络，实现动态特征发现与融合。
result: 在长短期预测任务上均优于现有先进模型，验证了特征感知设计的有效性。
conclusion: 特征感知自适应提取与多尺度融合是提升深度时序预测性能的关键策略。
---

## Abstract
Time series forecasting faces a fundamental challenge: the uneven distribution of predictive importance in time series data, where some specific time points and feature combinations carry disproportionately predictive power. As a result, uniform processing methods that treat all data alike inevitably fall short of optimal performance. To address this problem, we propose FeTS, a feature-aware framework that comprehensively learns temporal features through two key components: (i) Adaptive Feature Extraction (AdaFE), which dynamically discovers the most important features within each temporal patch and extracts them on the fly, yielding sharper and more focused local representations; and (ii) Dual-Scale Feed-Forward Network (DSFFN), which strategically integrates fine-grained local features with global long-term dependencies to achieve richer dual-scale representation learning. Extensive experiments on eight benchmark datasets demonstrate that FeTS achieves state-of-the-art performance in time series forecasting tasks, offering a novel solution to the challenge of uneven predictive importance in forecasting.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：时间序列数据中不同时间点和特征组合的预测重要性分布严重不均，部分时刻（如天气转季、温度突变）和特定模式对预测的贡献远高于其他平稳时段，而现有基于均匀处理（patch‑wise、point‑wise 或统一注意力）的方法无法有效聚焦这些关键特征，导致预测性能未达最优。
- **整体含义**：提出一种**特征感知框架 FeTS**，动态识别每个时间块（patch）内最具预测能力的特征并自适应提取，再将细粒度局部特征与全局长程依赖进行双尺度融合，从而解决“重要性不均衡”这一根本挑战，为时序预测提供新的建模范式。

## 2. 论文提出的方法论

FeTS 框架由两个核心模块构成：**自适应特征提取（AdaFE）** 和 **双尺度前馈网络（DSFFN）**。

### 2.1 自适应特征提取 (AdaFE)
- **混合基空间重要性评分**：利用 Fourier 级数（正弦/余弦）和多项式基函数构建混合表示空间，同时捕捉周期性模式和趋势性非线性变化，对每个 patch 内的时间点进行重要性打分。
- **阈值激活生成掩码**：计算每个位置的平均得分作为阈值，产生二值掩码，标记高预测贡献的关键位置。
- **掩码控制稀疏卷积**：仅在被激活的位置上执行深度可分离卷积，从而精细提取局部重要特征，抑制噪声和冗余信息。

### 2.2 双尺度前馈网络 (DSFFN)
- **局部分支**：使用 1D 卷积提取局部时序模式。
- **全局分支**：先沿时间轴全局平均池化，再扩展尺寸，获得长程上下文引导。
- **融合投影**：将局部和全局特征沿特征维度拼接，经融合卷积和 Dropout 投影，生成同时包含细节与全局语义的增强表示，用于最终预测。

整体流程：输入序列 → Patch 嵌入 → AdaFE 提取精细特征 → DSFFN 双尺度融合 → 输出预测。

## 3. 实验设计

### 3.1 数据集
8 个常用时间序列预测基准：ETTh1、ETTh2、ETTm1、ETTm2、Weather、Electricity、Traffic、Solar-Energy，均按 Autoformer/Informer 的标准协议划分训练、验证、测试集和归一化。

### 3.2 对比基准
涵盖三大范式的多个SOTA模型：
- **Transformer 类**：PatchTST、iTransformer、Crossformer、TimeXer
- **MLP 类**：DLinear、SparseTSF、TimeMixer、Amplifier
- **CNN 类**：TimesNet、ModernTCN、ConvTimeNet

### 3.3 评估设置
回溯窗口固定为 336，预测窗口 H ∈ {96, 192, 336, 720}，取各预测长度下 MSE 和 MAE 的平均值作为主要对比指标。

## 4. 资源与算力

- 论文**并未明确说明**使用的 GPU 型号、数量及总训练时长。
- 仅在效率分析图中对比了不同模型在 Weather 数据集上的**训练时间（秒/epoch）**和**显存占用**，显示 FeTS 使用约 928 MB 显存、每 epoch 约 35 秒，表现出较高的计算效率，但未给出具体硬件配置。

## 5. 实验数量与充分性

实验设计**较为充分且客观**，主要实验包括：
- **主对比实验**：8 数据集 ×（4 预测长度均值）与 11 个 Baseline 比较，全面覆盖不同领域和模型范式。
- **消融实验**：对 AdaFE 和 DSFFN 分别进行替换（用普通 CNN、FFN）和完全去除，在 5 个数据集上对比 MSE/MAE，证明两个模块的必要性。
- **模块泛化实验**：将 AdaFE 插入 PatchTST 和 TimesNet，在 4 个数据集上验证迁移增益。
- **超参数影响分析**：在 4 个数据集上测试嵌入维度 D ∈ {32, 64, 128, 256, 512}，对比 ModernTCN，显示模型对维度不敏感、主要收益来自特征感知机制。
- **变回溯窗口实验**：对不同回溯窗口（96, 192, 336, 720），在 4 个数据集上与 4 种模型对比 MSE，展示稳定性。
- **效率分析**：对比 7 个模型的训练时间和显存。

所有实验均采用统一数据划分和评价指标，比较公平；消融和泛化实验进一步削弱了“模型综合优势仅来自架构堆叠”的疑虑。

## 6. 论文的主要结论与发现

- 时间序列具有内在“重要性不均衡”特性，均匀处理会遗漏关键预测信息。
- FeTS 通过 AdaFE 动态聚焦高预测价值的局部特征，并与 DSFFN 的双尺度表示结合，在多数基准上取得最优或次优结果。
- 特征感知机制可轻松迁移至其他 patch‑based 模型，明显提升其预测精度。
- 模型对嵌入维度不敏感，效率高，可在低计算开销下实现优异性能。

## 7. 优点

- **问题洞察新颖**：首次在这类架构中显式定义并解决“预测重要性分布不均”问题，有理论（互信息分析）和实验支撑。
- **方法创新性强**：AdaFE 的 Fourier‑Poly 混合评分 + 掩码稀疏卷积设计巧妙，DSFFN 简洁高效地融合多尺度信息，整体端到端可学习。
- **实验充分全面**：多数据集、多基线、多维度消融和泛化实验，且与多种先进模型横向对比，结论可信。
- **实用性好**：模型轻量，训练快，显存小，便于部署，且性能领先。

## 8. 不足与局限

- **算力细节缺失**：未报告 GPU 型号、训练总时长等，难以精准复现资源需求。
- **仅限于多变量长期预测**：未在短期预测、分类、异常检测等 Time Series 任务上验证，应用范围有待拓宽。
- **特征重要性评分机制的可解释性有限**：虽可视化展示了激活掩码的效果，但缺少对具体哪些时序模式被标记为“重要”的详细解释。
- **Traffic 数据集表现未全面领先**：在 Traffic 上 MSE 略高于 iTransformer、MAE 略差于 SparseTSF，暗示对极强空间相关性的序列，特征感知设计可能需与变量关系建模进一步结合。
- **未探索 AdaFE 内参数（F、P）的影响**：傅里叶最大频率 F 和多项式最高阶 P 设为超参数，但未进行敏感性分析，可能影响不同数据集的调参成本。

（完）
