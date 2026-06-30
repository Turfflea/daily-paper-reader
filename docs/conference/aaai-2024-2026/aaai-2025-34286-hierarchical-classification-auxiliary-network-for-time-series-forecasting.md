---
title: Hierarchical Classification Auxiliary Network for Time Series Forecasting
title_zh: 用于时间序列预测的层次分类辅助网络
authors: "Yanru Sun, Zongxia Xie, Dongyue Chen, Emadeldeen Eldele, Qinghua Hu"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/34286/36441"
tags: ["query:mt"]
score: 10.0
evidence: 应用层次分类辅助网络改进时间序列预测的深度学习
tldr: 深度时间序列预测常用均方误差损失会导致过度平滑，难以捕捉高变异性数据中的复杂模式。本文提出层次分类辅助网络HCAN，将时间序列值离散化并引入交叉熵损失，作为通用组件可与任何预测模型集成，显著提升了预测精度，为管理情景下的波动性序列预测提供了有效技术。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34286/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 854, \"height\": 512, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34286/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1739, \"height\": 801, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34286/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 841, \"height\": 373, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34286/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 839, \"height\": 666, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34286/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 874, \"height\": 759, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34286/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1834, \"height\": 467, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34286/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1836, \"height\": 820, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34286/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1297, \"height\": 391, \"label\": \"Table\"}]"
motivation: 均方误差损失使深度预测模型输出过于平滑，无法学习高熵时间序列的动态特征。
method: 提出模型无关的HCAN，将时序值离散化为令牌，通过层次分类和交叉熵损失辅助训练。
result: 在多个真实时间序列数据集上，集成HCAN的模型预测误差明显降低。
conclusion: 通过分类范式重构预测损失，可有效缓解过平滑问题，对管理预测建模有直接帮助。
---

## Abstract
Deep learning has significantly advanced time series forecasting through its powerful capacity to capture sequence relationships. However, training these models with the Mean Square Error (MSE) loss often results in over-smooth predictions, making it challenging to handle the complexity and learn high-entropy features from time series data with high variability and unpredictability. In this work, we introduce a novel approach by tokenizing time series values to train forecasting models via cross-entropy loss, while considering the continuous nature of time series data. Specifically, we propose a Hierarchical Classification Auxiliary Network, HCAN, a general model-agnostic component that can be integrated with any forecasting model. HCAN is based on a Hierarchy-Aware Attention module that integrates multi-granularity high-entropy features at different hierarchy levels. At each level, we assign a class label for timesteps to train an Uncertainty-Aware Classifier. This classifier mitigates the over-confidence in softmax loss via evidence theory. We also implement a Hierarchical Consistency Loss to maintain prediction consistency across hierarchy levels. Extensive experiments integrating HCAN with state-of-the-art forecasting models demonstrate substantial improvements over baselines on several real-world datasets.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **研究背景与动机**：深度学习通过均方误差（MSE）损失训练时间序列预测模型，虽能捕获序列关系，但常导致预测结果“过度平滑”，难以捕捉高变异性、不可预测数据中的高熵特征。这在实际应用中（如风速预测）会抹平极端值，降低下游任务可用性。
- **核心问题**：如何缓解 MSE 损失带来的特征空间压缩和过度平滑问题，使模型能够学习更复杂、高熵的特征表示，提升预测精度。
- **整体含义**：论文提出将时间序列预测重新定义为层次分类问题，利用交叉熵损失引入高熵特征，并设计一个与模型无关的辅助网络（HCAN），可集成到任意预测模型中，以提升其在真实世界数据集上的预测性能。

### 2. 方法论
- **核心思想**：将连续时间序列值离散化为类别令牌（token），用交叉熵损失训练分类器，同时保留连续性的相对预测。通过多层次（粗、细粒度）分类结构引入多粒度高熵特征，并结合注意力机制将其融入预测特征中。
- **关键技术细节**：
    - **层次分类辅助网络（HCAN）**：分为三个层次——原始序列、粗粒度分类（2 类）、细粒度分类（4 类）。通过值的大小划分为区间进行离散化。
    - **不确定性感知分类器（UAC）**：使用证据理论（Dirichlet 分布）估计类别不确定度，避免 Softmax 的过度自信；提出不确定性感知损失函数，对难分类样本赋予更大权重。其损失由不确定性权重部分和 KL 正则化部分组成。
    - **层次一致性损失（HCL）**：通过最小化细粒度和粗粒度分类器输出分布之间的对称 KL 散度，缓解离散化带来的类边界效应，强制不同粒度间预测一致。
    - **层次感知注意力（HAA）**：利用粗细粒度特征与原始时间特征通过注意力机制融合，然后与 backbone 输出残差连接，将高熵特征集成到预测流程中。
- **总损失函数**：`L = LHIER + β * LHCL + γ * LMSE`，其中 `LHIER` 包含粗细粒度分类的相对预测损失，`β`, `γ` 为超参数。

### 3. 实验设计
- **数据集**：10 个公开真实世界多元时间序列数据集：ETT（4个子集：ETTh1, ETTh2, ETTm1, ETTm2）、Exchange-Rate、Weather、ILI、Electricity、Traffic、Solar Wind。
- **基准方法（backbones）**：7 种先进模型，涵盖不同架构：
    - Transformer 类：Informer、Autoformer、PatchTST、iTransformer
    - CNN 类：SCINet
    - MLP 类：DLinear、FITS
- **对比方式**：每个 backbone 在有无 HCAN 的情况下，分别在多元和单变量预测任务上对比 MSE 和 MAE。所有基线均在相同设置下重新运行。
- **预测长度设置**：多数实验采用预测长度 T ∈ {96, 192, 336, 720}，回溯窗口视模型而定（PatchTST、DLinear、SCINet 为 L=336，其余 L=96）。

### 4. 资源与算力
- **硬件**：单张 NVIDIA RTX 3090 24GB GPU。
- **框架**：PyTorch。
- **训练细节**：与基线训练相同 epoch 数；优化器为 ADAM；初始学习率通过网格搜索从 {5e-3, 1e-3, 5e-4, 1e-4, 5e-5, 1e-5} 选择；超参数 β 从 {1, 0.1, 0.01}，γ 从 {1, 0.1, 0.01} 网格搜索确定；所有实验重复 5 次（固定随机种子）取平均。
- **未明确说明总训练时长**。

### 5. 实验数量与充分性
- **实验组数**：
    - 多元预测：7 个 backbone × 10 个数据集（实际汇总表列出 10 个数据集/变体），每个数据集包含多个预测长度，总比较用例数目众多（文中指出 66/70 例获得提升）。
    - 单变量预测：在 ETT 完整基准和 Solar Wind 数据集上评估，同样覆盖多个预测长度。
    - 消融实验：以 Informer 为 backbone，在 Weather 和 Solar Wind 数据集上逐步验证 UAC、层次结构、HCL、HAA 各模块效果。
    - 定性分析：t-SNE 可视化特征、预测曲线对比。
- **充分性与公平性**：实验设计全面，数据集多样，涵盖主流 backbone，对比基线均重新运行，使用网格搜索调参，多次重复取平均，确保了客观性和公平性。消融实验清晰展示了每个模块的贡献。

### 6. 主要结论与发现
- HCAN 作为模型无关组件，能显著提升各类时间序列预测模型的性能，在大部分情况下均降低 MSE。
- 将预测任务重构为层次分类能有效引入高熵特征，缓解过度平滑。
- 不确定性感知分类器和层次一致性损失成功减轻了离散化带来的边界效应。
- 层次感知注意力机制优于简单的特征拼接，有助于合理地融合多粒度特征。

### 7. 优点
- **创新性**：将分类范式与回归损失相结合，并采用多粒度层次，为时间序列预测提供了新视角。
- **通用性**：模型无关设计，可灵活集成到现有各类预测模型中。
- **可靠性**：引入证据理论的不确定性估计，增加对困难样本的关注，提升鲁棒性。
- **实验扎实**：覆盖多个数据集、多种先进 backbone、多元/单变量设置，消融完整，可视化有说服力。

### 8. 不足与局限
- **类别数量固定**：粗、细粒度分类数固定为 2 和 4，未探究最优类别数对不同数据集的敏感性。
- **超参数敏感性**：β、γ 等权重需网格搜索，实际部署中可能需要专门调优。
- **计算开销**：增加了多个分类头和注意力模块，虽未详细报告训练时长，但可能增加计算成本。
- **应用限制**：主要针对数值型时间序列预测，未涉及类别型或混合类型时序，且极端事件预测的效果未单独深入分析。
- **对比局限性**：未与近期其他离散化或分类辅助回归的通用方法（如 Ordinal Entropy）进行直接比较。

（完）
