---
title: "MSGNet: Learning Multi-Scale Inter-series Correlations for Multivariate Time Series Forecasting"
title_zh: MSGNet：学习多尺度序列间相关性用于多元时间序列预测
authors: "Wanlin Cai, Yuxuan Liang, Xianggen Liu, Jianshuai Feng, Yuankai Wu"
date: 2024-03-25
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/28991/29883"
tags: ["query:mt"]
score: 10.0
evidence: 多尺度序列间相关性学习用于多元时间序列预测
tldr: 针对多元时间序列中不同时间尺度下序列间相关性变化被忽视的问题，提出MSGNet模型，利用频域分析和自适应图卷积捕获多尺度序列间相关，在多个预测基准上取得最优性能，为管理科学中的金融、需求等多变量预测提供了先进方法。
source: AAAI-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-28991/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 892, \"height\": 473, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-28991/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1820, \"height\": 564, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-28991/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 898, \"height\": 498, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-28991/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 890, \"height\": 673, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-28991/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1837, \"height\": 1405, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-28991/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 876, \"height\": 359, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-28991/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1648, \"height\": 258, \"label\": \"Table\"}]"
motivation: 现有方法忽略了多元时间序列在不同时间尺度上序列间相关性的变化。
method: 提出MSGNet，通过频域分析分解时间尺度，并利用自适应图卷积建模多尺度序列间依赖。
result: 模型能有效捕捉跨尺度关联，在多个数据集上显著提升预测精度。
conclusion: MSGNet为复杂多元时间序列预测提供了强大工具，可直接应用于管理科学中的经济、运营等预测场景。
---

## Abstract
Multivariate time series forecasting poses an ongoing challenge across various disciplines. Time series data often exhibit diverse intra-series and inter-series correlations, contributing to intricate and interwoven dependencies that have been the focus of numerous studies. Nevertheless, a significant research gap remains in comprehending the varying inter-series correlations across different time scales among multiple time series, an area that has received limited attention in the literature. To bridge this gap, this paper introduces MSGNet, an advanced deep learning model designed to capture the varying inter-series correlations across multiple time scales using frequency domain analysis and adaptive graph convolution. By leveraging frequency domain analysis, MSGNet effectively extracts salient periodic patterns and decomposes the time series into distinct time scales. The model incorporates a self-attention mechanism to capture intra-series dependencies, while introducing an adaptive mixhop graph convolution layer to autonomously learn diverse inter-series correlations within each time scale. Extensive experiments are conducted on several real-world datasets to showcase the effectiveness of MSGNet. Furthermore, MSGNet possesses the ability to automatically learn explainable multi-scale inter-series correlations, exhibiting strong generalization capabilities even when applied to out-of-distribution samples.

---

## 论文详细总结（自动生成）

### 1. 核心问题与研究动机
- **研究背景**：多元时间序列预测在金融、气象、运营等领域至关重要。真实数据常呈现序列内和序列间的复杂依赖。
- **现存局限**：现有深度学习方法（如RNN、Transformer、GNN）未能充分捕捉不同时间尺度下序列间相关性的动态变化。例如，资产价格在经济危机与平稳期的相关性模式截然不同。
- **核心问题**：如何自动发现并建模多元序列在不同时间尺度（如日周期、周周期）上变化的交互依赖，是已有研究中的显著空白。

### 2. 方法论
- **整体框架**：提出 **MSGNet**，由多个 ScaleGraph 块堆叠而成，每个块通过频域分析识别多尺度周期，并联合自适应图卷积与自注意力建模。
- **关键技术细节**：
  - **输入嵌入与残差连接**：使用1D卷积、位置编码和可学习的时间戳嵌入，将输入标准化后映射到高维空间。网络以残差方式堆叠。
  - **尺度识别**：对嵌入信号进行快速傅里叶变换（FFT），提取振幅最强的 k 个频率分量。对应的周期长度 $s_i = L/f_i$ 作为主导时间尺度。
  - **多尺度重塑**：依据每个尺度 $s_i$ 将输入 padding 后重塑为 3D 张量 $\mathbf{X}^i \in \mathbb{R}^{d_{model} \times s_i \times f_i}$。
  - **自适应图卷积**：通过可学习参数 $\mathbf{E}^i_1, \mathbf{E}^i_2$ 生成尺度特异邻接矩阵 $\mathbf{A}^i = \text{SoftMax}(\text{ReLU}(\mathbf{E}^i_1 \mathbf{E}^{i\top}_2))$。采用 MixHop 图卷积（借由多跳邻域混合）捕获序列间高阶依赖。
  - **时序注意力**：在每个尺度的张量上应用多头部自注意力（沿尺度维度），捕捉序列内时间相关性。
  - **尺度聚合**：将不同尺度结果重塑回原始长度，用 FFT 振幅计算 SoftMax 权重，实现多尺度特征的专家混合（MoE）聚合。
  - **输出**：通过线性投影得到预测序列。

### 3. 实验设计
- **数据集**：8 个真实数据集，包括 **Flight**（航班数据）、**Weather**、**ETT**（4个子集：h1, h2, m1, m2）、**Exchange-Rate**、**Electricity**。
- **对比基准**：6 种主流方法——Informer、Autoformer、MTGnn、DLinear、NLinear、TimesNet（当前最优）。
- **评估设置**：回顾窗口固定为 96，预测长度 $T \in \{96,192,336,720\}$；评价指标为 MSE 和 MAE。

### 4. 资源与算力
- **硬件**：1 块 NVIDIA GeForce RTX 3090 24GB GPU。
- **训练细节**：学习率 $10^{-4}$，批大小 32，训练 10 个 epoch，采用早停法。未提及总训练时长。

### 5. 实验数量与充分性
- **主实验**：在 8 个数据集、4 种预测长度下对比 6 个基线，共产生上百组 MSE/MAE 结果，覆盖充分。
- **消融实验**：移除自适应图卷积（w/o-AdapG）、移除多尺度图（w/o-MG）、移除注意力（w/o-A）、替换 MixHop 为传统 GCN（w/o-Mix），在 3 个数据集上验证各组件贡献。
- **泛化性实验**：修改 Flight 数据集划分（训练集仅含疫情前数据，测试集为疫情后 OOD 样本），评估模型对分布外样本的鲁棒性。实验设计公平，结论可信。

### 6. 主要结论与发现
- **性能优势**：MSGNet 在 8 个数据集的平均排名居首（Avg Rank=1.813），尤其在 Flight 数据集上将 MSE 降低 21.5%（相较 TimesNet）。
- **可解释性**：可视化显示不同尺度学习到截然不同的机场间关联（如长周期强相关 vs. 短周期弱相关），符合实际物理距离规律。
- **鲁棒性**：在 OOD 泛化测试中性能下降最小（21.3% vs. 其他方法 26%~46%），证明多尺度图结构能抵抗外部冲击。

### 7. 优点
- **创新性强**：首次将频域多尺度分解与自适应图学习结合，解决序列间相关性时间尺度依赖的盲点。
- **设计巧妙**：MixHop 图卷积与多头部注意力并行，同时捕获序列间高阶交互与序列内模式；尺度聚合的 MoE 机制动态调整不同周期的重要性。
- **实验扎实**：涵盖典型基准与全新 Flight OOD 场景，消融分析和可视化充分支撑模型有效性。
- **可解释性**：直接输出各时间尺度的邻接矩阵，为领域专家提供直观的变量依赖关系。

### 8. 不足与局限
- **计算复杂度**：FFT、多尺度重塑和多次图卷积可能增加显存和时间开销，在大规模变量（$N$ 很大）场景下未分析。
- **尺度预设**：尺度数量 $k$ 作为超参数需人工指定，缺乏自动选择机制。
- **泛化边界**：仅在一个数据集上测试 OOD 能力，未探讨更多样化的分布偏移（如传感器故障、概念漂移）。
- **对比局限**：未纳入 PatchTST、Crossformer 等近期优秀模型，可能遗漏更全面的竞争力评估。

（完）
