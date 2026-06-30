---
title: "FreqCycle: A Multi-Scale Time-Frequency Analysis Method for Time Series Forecasting"
title_zh: FreqCycle：一种面向时间序列预测的多尺度时频分析方法
authors: "Boya Zhang, Shuaijie Yin, Huiwen Zhu, Xing He"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40042/44003"
tags: ["query:mt"]
score: 10.0
evidence: 基于深度学习的多尺度时频分析方法用于先进时间序列预测
tldr: 现有时间序列预测大多专注低频模式，忽略中高频信息。为此提出FreqCycle框架，整合滤波器增强的周期预测模块显式学习时域共享周期模式，以及分段频域模式学习模块通过可学习滤波和自适应加权增强中高频能量比例，同时处理偶耦合多周期现象，在各类时间序列上均表现出超越主流方法的预测精度，为深度时序模型提供了新的频率增强思路。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40042/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 871, \"height\": 426, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40042/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 869, \"height\": 420, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40042/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1814, \"height\": 907, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40042/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 862, \"height\": 566, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40042/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 870, \"height\": 435, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40042/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 884, \"height\": 820, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40042/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1820, \"height\": 475, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40042/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 854, \"height\": 313, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40042/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 917, \"height\": 328, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40042/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 853, \"height\": 335, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40042/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 771, \"height\": 296, \"label\": \"Table\"}]"
motivation: 传统深度学习时序模型忽视中高频模式，限制了预测性能的进一步提升。
method: 提出滤波器增强周期预测模块和分段频域模式学习模块，分别处理低高频特征。
result: 在多个基准数据集上取得优于最新方法的预测误差，验证了中高频建模的有效性。
conclusion: 全面利用时频特征、尤其增强中高频信息可显著提升深度时序模型的预测能力。
---

## Abstract
Mining time-frequency features is critical for time series forecasting. Existing research has predominantly focused on modeling low-frequency patterns, where most time series energy is concentrated. The overlooking of mid to high frequency continues to limit further performance gains in deep learning models. We propose FreqCycle, a novel framework integrating: (i) a Filter-Enhanced Cycle Forecasting (FECF) module to extract low-frequency features by explicitly learning shared periodic patterns in the time domain, and (ii) a Segmented Frequency-domain Pattern Learning (SFPL) module to enhance mid to high frequency energy proportion via learnable filters and adaptive weighting. Furthermore, time series data often exhibit coupled multi-periodicity, such as intertwined weekly and daily cycles. To address coupled multi-periodicity as well as long lookback window challenges, we extend FreqCycle hierarchically into MFreqCycle, which decouples nested periodic features through cross-scale interactions. Extensive experiments on seven diverse domain benchmarks demonstrate that FreqCycle achieves state-of-the-art accuracy while maintaining faster inference speeds, striking an optimal balance between performance and efficiency.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机与背景）
- 时间序列预测任务中，现有深度学习方法大多聚焦于建模**低频成分**（如长周期趋势），因为大部分信号能量集中于此，却忽视了对**中高频成分**（短时波动、非周期特征）的有效提取。
- 这种忽视限制了模型性能的进一步提升，尤其对于具有复杂非平稳特性或多尺度耦合周期（如日、周周期叠加）的数据。
- 论文旨在设计一种**多尺度时频分析框架**，既能显式学习时域共享周期模式，又能增强频域中高频的能量表示，从而突破现有局限，实现精度与效率的平衡。

### 2. 论文提出的方法论（核心思想与关键技术）
#### 2.1 整体架构：FreqCycle
- 由两大核心模块组成：**滤波器增强的周期预测模块（FECF）** 和 **分段频域模式学习模块（SFPL）**。
- 输入序列先经FECF提取并去除低通滤波后的周期成分，得到残差；残差再经SFPL预测未来值；最后周期分量与残差预测相加得到最终输出。

#### 2.2 滤波器增强周期预测（FECF）
- 预设一化全局可学习的周期基 $Q \in \mathbb{R}^{W \times D}$（$W$为周期长度，如24小时或168小时），通过周期性重复生成历史与未来的周期成分 $c_{t-L+1:t}$ 和 $c_{t+1:t+H}$。
- 对周期成分应用 **自适应频域滤波器**：先FFT，与可学习参数 $\theta_c$ 逐元素相乘，再IFFT，以强化低频、抑制中高频噪声。
- 从输入中减去历史周期成分得残差 $r_{t-L+1:t}$。

#### 2.3 分段频域模式学习（SFPL）
- 对残差序列做 **滑动窗口分割**（$s$个子段），两端补零，堆叠得到 $R \in \mathbb{R}^{s \times L \times D}$。
- 对每个子段执行FFT，得到频域表示 $F$；通过可学习的自适应权重 $\Theta_F$ 对各子段加权求和，得到融合的频域特征 $f$，实现类似于 **短时傅里叶变换（STFT）** 的效果，增强中高频能量占比。
- 经iFFT还原时域，再通过FFN层输出残差预测 $r_{t+1:t+H}$。

#### 2.4 多尺度扩展：MFreqCycle
- 针对长回溯窗口与耦合多周期问题，引入 **基础周期模块**（如日周期）和 **周周期模块** 并行处理。
- 周周期模块先对长序列池化降采样，再经与FreqCycle相同的处理流水线，最后通过可学习权重融合两个尺度的预测输出。
- 该设计解耦嵌套周期特征，提升对长距离依赖的建模能力。

### 3. 实验设计
- **数据集**：7个公开基准——ETTm1、ETTm2、ETTh1、ETTh2、Electricity (ECL)、Weather、Traffic，涵盖能源、天气、交通等领域。
- **对比方法**：
  - 线性/MLP类：DLinear、CycleNet、TimeMixer；
  - 频率学习方法：FreTS、FilterNet、FITS、Amplifier；
  - Transformer类：iTransformer、PatchTST。
- **评估指标**：均方误差（MSE）和平均绝对误差（MAE），统一回溯窗口 $L=96$，预测长度 $H \in {96,192,336,720}$，取平均结果。

### 4. 资源与算力
- 所有实验基于 **PyTorch 2.6.0**，使用一张 **NVIDIA RTX 4090（24GB显存）**。
- 论文未明确报告训练时长或总计算量，仅在图4中通过“Training speed”散点图体现相对训练速度（FreqCycle处于较优位置）。未给出绝对时间及GPU数量。

### 5. 实验数量与充分性
实验设计较为全面，包含：
- **主实验**（Table 1）：7数据集 × 4预测长度，与9种SOTA方法全面比较；
- **长窗口消融**（Table 2）：对比不同回溯窗口（96 vs 168/672）下MFreqCycle性能；
- **模块消融**（Table 3）：去掉FECF、用MLP替代SFPL；
- **频率处理方法对比**（Table 4）：将SFPL替换为PaiFilter或LPF；
- **分解策略对比**（Table 5）：FECF vs 移动平均分解 vs 局部分解；
- **效率分析**（Figure 4）：MSE-内存-训练速度三维对比；
- **可视化**：频谱增强效果、学习到的周期模式、预测结果曲线。
实验覆盖充分，对比公平，且公开了代码，具有可复现性。

### 6. 论文的主要结论与发现
- 通过显式学习共享周期模式（FECF）能有效捕获低频规律，显著降低预测误差。
- SFPL通过分段STFT与自适应加权，显著提升了中高频成分的能量占比，补偿了传统方法对短时波动建模的不足。
- MFreqCycle成功解耦多尺度周期特征，在长回溯窗口下带来明显增益。
- 综合框架在多个领域数据集上取得了最优精度，同时保持较快的推理速度，实现了性能与效率的最佳平衡。

### 7. 优点（方法与实验亮点）
- **问题切中要害**：直接指出忽视中高频是现有模型瓶颈，并提出针对性解决方案。
- **创新的频率增强机制**：SFPL借鉴STFT思想，用可学习分段加权替代传统固定滤波器，有效增强中高频。
- **周期建模简洁高效**：FECF通过可学习周期基+自适应滤波，避免复杂长程依赖架构，计算开销低。
- **模块化与可扩展性**：FreqCycle可独立运行，也可扩展为MFreqCycle应对多周期场景，设计灵活。
- **实验全面扎实**：数据集多样、对比方法覆盖主流类别、多维度消融和可视化，说服力强。
- **代码开源**，利于社区复现与跟进。

### 8. 不足与局限
- **周期先验依赖**：FECF和MFreqCycle需预设周期长度 $W$（如24、168），对不存在明显固定周期的数据集（如高频金融数据）可能不适用。
- **长周期扩展受限**：MFreqCycle目前仅验证了日+周周期，未探索更长周期（如月、年），且文中指出因数据集限制无法验证年度周期。
- **算力报告不完整**：未给出具体训练时间、FLOPs或参数量对比，仅凭GPU型号和相对速度图难以完全评判计算成本。
- **数据集覆盖范围**：主要集中在常规预测基准，缺乏大规模工业数据集或小样本场景，泛化边界有待进一步探索。
- **方法组合的必要性验证**：未设计仅含SFPL（无周期建模）的基线，难以独立判断SFPL在纯非周期数据上的边际贡献。

（完）
