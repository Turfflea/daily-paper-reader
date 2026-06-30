---
title: SSL-STMFormer Self-Supervised Learning Spatio-Temporal Entanglement Transformer for Traffic Flow Prediction
title_zh: SSL-STMFormer：面向交通流预测的自监督时空纠缠Transformer
authors: "Zetao Li, Zheng Hu, Peng Han, Yu Gu, Shimin Cai"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/33321/35476"
tags: ["query:mt"]
score: 10.0
evidence: 自监督时空纠缠Transformer用于交通流预测
tldr: 为解决交通流预测中长距离时空依赖、异质性和纠缠建模不足的问题，提出SSL-STMFormer框架，利用自监督学习与时空纠缠Transformer，有效捕捉全局时空交互，在真实交通数据上取得领先性能。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33321/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 795, \"height\": 892, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33321/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1626, \"height\": 762, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33321/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 880, \"height\": 308, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33321/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 859, \"height\": 206, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33321/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 857, \"height\": 535, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33321/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1789, \"height\": 639, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33321/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 868, \"height\": 232, \"label\": \"Table\"}]"
motivation: 现有模型难以捕捉长距离时空相似性和时空异质性，且无法充分建模时空纠缠。
method: 提出基于自监督学习的时空纠缠Transformer框架，设计自监督预训练任务增强时空表征。
result: 在真实交通数据集上，预测性能显著超越基线模型，验证了方法的有效性。
conclusion: 该方法为智能交通系统中的精确流量预测提供了先进解决方案。
---

## Abstract
Traffic flow prediction remains a critical issue in intelligent transport systems. Despite significant efforts in traffic flow modeling, existing approaches exhibit several notable limitations: (i) Most models fail to capture traffic flow similarities over long distances and extended periods; (ii) They struggle to account for spatio-temporal heterogeneity induced by varying traffic flow patterns; (iii) Due to their static modeling approach, they struggle to effectively capture the intricate spatio-temporal entanglement. To address these challenges, we propose a traffic flow prediction framework based on self-supervised learning spatio-temporal entanglement transformer(SSL-STMFormer). This framework adopts a self-supervised learning paradigm, leveraging a transformer architecture that captures richer spatio-temporal information to better represent traffic flow patterns. Specifically, a temporal attention module and a spatial attention module are employed to capture the spatio-temporal dependencies of traffic dynamics, respectively, and spatio-temporal entanglement-aware methods are introduced to allow the model to perceive spatio-temporal entanglement and thus better modelling of real traffic environments. Furthermore, to achieve adaptive spatio-temporal self-supervised learning, adaptive data augmentation is applied to the input traffic flow data, and the traffic flow prediction task is enhanced with temporal heterogeneity module and spatial heterogeneity module. Extensive experimental evaluations conducted on six publicly available real-world transportation datasets demonstrate that our method achieves substantial improvements across these datasets.

---

## 论文详细总结（自动生成）

### 1. 论文核心问题与研究动机  
- 交通流预测是智能交通系统的关键任务，但现有方法存在三重局限：  
  - **无法捕捉长距离、长周期的流量相似性**（如相隔较远的区域可能因功能相似而具有一致的模式，但该相似性会动态变化）；  
  - **难以建模由不同流量模式引发的时空异质性**（例如不同功能区在相同时段表现出截然不同的规律）；  
  - **静态建模无法刻画复杂的时空纠缠**（即时空依赖与异质性随出行模式动态演化）。  
- 因此，论文提出 **SSL‑STMFormer**，利用自监督学习与时空纠缠 Transformer，从全局、动态的角度提升交通流预测的准确性与鲁棒性。

### 2. 方法论  
#### **核心架构：SSL‑STMFormer**  
- **数据嵌入层**：将原始流量数据映射到高维空间，同时融合空间图拉普拉斯特征向量（刻画路网结构）、时间周期嵌入（捕捉日、周周期性）和时序位置编码，得到综合嵌入 \(X_{\text{emb}}\)。  
- **时空编码器层**：  
  - **空间注意力（SA）**：沿节点维度计算自注意力，动态捕捉不同时刻的空间依赖。  
  - **时间注意力（TA）**：沿时间维度计算自注意力，建模跨时段的动态时序依赖。  
  - **空间/时间纠缠感知模块**：通过多种 Mask 策略（随机掩码、管道掩码、块掩码、短期/长期图掩码以及未来时间掩码）限制注意力交互，强制模型捕捉真实交通环境中的关键节点对和时间依赖。  
  - **多头融合**：将不同掩码策略下的空间、时间注意力输出拼接并投影，经前馈网络、归一化与残差连接得到编码输出。  
- **输出投影**：对每层编码器的输出进行 1×1 卷积，求和后获得最终隐状态 \(X_{\text{hid}}\)。  
- **自适应数据增强**：  
  - 对流量张量按伯努利分布掩码不相关时间步，降低噪声；  
  - 对路网图进行增强，缓解低相关连接偏差，并捕获长距离依赖。  
- **空间异质性建模（SHM）**：基于软聚类的自监督任务，将区域映射到多个隐因子（对应城市功能区），通过比较原始与增强嵌入的聚类分配一致性，强化区域嵌入对空间功能差异的感知。  
- **时间异质性建模（THM）**：将原始与增强嵌入融合，构建城市级时间步表征，以同一步的（区域嵌入，城市嵌入）为正对，不同步的为负对，用对比损失强化时间步差异的捕捉。  
- **联合训练**：优化目标由预测损失（MSE）与两个异质性建模损失相加构成。

#### **关键公式流程（文字说明）**  
1. 嵌入层输出 \(X_{\text{emb}}\) 由流量线性变换、空间图拉普拉斯嵌入、周/日周期嵌入和时序位置编码求和得到。  
2. 空间注意力计算：\(Q_t^{(S)} = X_{t::}W_Q\)，\(K_t^{(S)} = X_{t::}W_K\)，\(V_t^{(S)} = X_{t::}W_V\)；注意力得分 \(A_t^{(S)} = (Q_t^{(S)}(K_t^{(S)})^\top)/\sqrt{d'}\)，经 Softmax 后与 Value 相乘。  
3. 纠缠增强：对 \(A_t^{(S)}\) 施加各类掩码矩阵（如随机掩码、管状掩码、短期掩码等），再进行加权求和。  
4. 时间注意力对称处理，并对未来时间步使用掩码强制从历史重建。  
5. SHM 中，区域嵌入 \(h_{i,n}\) 与聚类中心 \(c_k\) 的点积得到相似性，通过预测的分配概率与真实软分配间的交叉熵进行优化。  
6. THM 中，对比损失鼓励同一 \(t\) 的区域嵌入与城市嵌入接近，不同 \(t\) 的远离。

### 3. 实验设计  
- **数据集**：  
  - 图结构高速交通：PeMS04（307 节点）、PeMS07（883 节点）、PeMS08（170 节点），时间间隔 5 min。  
  - 网格化城市交通：NYCTaxi（75 区域）、CHIBike（270 区域）、T-Drive（1024 区域），时间间隔 30 min 或 60 min。  
- **基准方法**：19 个基线，分为四类：  
  - 网格模型：STResNet、DMVSTNet、DSAN。  
  - 图神经网络：DCRNN、STGCN、GWNET、MTGNN、STSGCN、STFGNN、STGODE、STGNCDE。  
  - 自注意力模型：STTN、GMAN、TFormer、PDFormer、ASTGNN。  
  - 自监督/预训练：ST-SSL、GraphST、STD‑MAE。  
- **评价指标**：MAE、MAPE、RMSE。  
- **数据处理**：图数据集按 6:2:2 划分，用过去 12 步预测未来 12 步；网格数据集按 7:1:2，用过去 6 步预测未来 1 步。

### 4. 资源与算力  
- 论文未提及具体的 GPU 型号、数量、训练时长或计算量（如 FLOPs）。仅给出了代码仓库链接。因此，算力使用情况无明确说明。

### 5. 实验数量与充分性  
- **主实验**：6 个数据集 × 19 个基线 × 3 个指标 = 至少 342 组对比结果，表格完整呈现。  
- **消融实验**：在 PeMS08 和 T‑Drive 上对比去除自监督、去除纠缠模块、仅空间/时间纠缠、融合纠缠等变体，共 5 组变体。  
- **参数敏感性**：在 PeMS08 和 T‑Drive 上研究隐层维度 {8,16,32,64,128}、编码器深度 {2,4,6,8,10}、掩码率 {0.05~0.25} 的影响。  
- 实验覆盖全面，对比公平（统一划分、一致指标），消融充分，能够验证各个组件的独立贡献。

### 6. 主要结论与发现  
- SSL‑STMFormer 在所有数据集上均以明显优势超越所有基线，图数据上平均相对提升约 5%，网格数据上提升超过 10%（尤其 MAE、RMSE）。  
- 消融实验表明，自监督异质性建模和时空纠缠感知模块对性能提升至关重要，缺少任一模块都会导致指标恶化。  
- 参数分析显示，适中的隐层维度（32 或 64）、合理的编码器深度（4～6 层）以及约 0.15 的掩码率能取得最佳效果。

### 7. 优点  
- **创新性**：首次将多种掩码策略与注意力结合，显式建模时空纠缠，同时利用自监督信号增强异质性捕捉。  
- **全面性**：同时处理空间异质性（软聚类）和时间异质性（时间步对比），并通过自适应增强提升鲁棒性。  
- **实证充分**：覆盖图与网格两类交通数据，对比最新颖的基线，消融与参数分析详尽，提升幅度显著。  
- **可复现性**：提供代码链接，使用公开数据集。

### 8. 不足与局限  
- **计算开销未审计**：未报告推理时间、参数量或 GPU 内存占用，实际部署的效率待验证。  
- **缺少极端场景测试**：如传感器大面积失效、突发事件下的预测鲁棒性未单独评估，虽然掩码策略有所涉及，但未系统分析。  
- **自监督任务设计依赖先验**：聚类中心数、长期邻居的 DTW 拓扑构建需要预设超参数，调参会增加成本。  
- **仅限交通流预测**：未展示在其他时空任务（如 PM2.5、犯罪率）上的泛化结果，虽然结论提及未来工作，但当前贡献未覆盖。  
- **数据增强的局限性**：流量掩码概率来自模型自身计算的注意力分数，可能导致错误的反馈循环。

（完）
