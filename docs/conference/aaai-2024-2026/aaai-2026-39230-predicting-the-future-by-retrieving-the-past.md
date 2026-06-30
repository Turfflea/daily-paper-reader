---
title: Predicting the Future by Retrieving the Past
title_zh: 检索过去以预测未来
authors: "Dazhao Du, Tao Han, Song Guo"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39230/43191"
tags: ["query:mt"]
score: 9.0
evidence: 基于全局记忆检索的单变量时间序列预测
tldr: 当前深度时间序列预测模型仅利用局部滑动窗口信息，未能充分利用全局历史模式。提出PFRP方法，构建全局记忆库，在推理时显式检索相关历史片段，与局部上下文融合预测。实验表明，PFRP在单变量预测任务上显著优于Transformer、TCN等主流模型，为长序列预测提供了新的记忆检索范式，可应用于管理科学中的销售预测、库存需求等时间序列分析。PFRP通过检索与当前最相关的历史片段，有效缓解了长程依赖问题，在多个公开数据集上均取得领先结果。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39230/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 876, \"height\": 645, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39230/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 832, \"height\": 726, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39230/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1649, \"height\": 765, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39230/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 805, \"height\": 586, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39230/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1797, \"height\": 659, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39230/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 836, \"height\": 460, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39230/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 834, \"height\": 441, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39230/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 855, \"height\": 297, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39230/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1836, \"height\": 512, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39230/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 748, \"height\": 545, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39230/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 750, \"height\": 382, \"label\": \"Table\"}]"
motivation: 现有深度预测模型仅在参数中隐式存储历史，推理时无法显式利用全局历史，导致信息利用不充分。
method: 提出PFRP，构建全局记忆库，在推理时检索相似历史片段，与局部窗口信息融合进行预测。
result: PFRP在单变量时间序列预测中显著超越MLP、Transformer、TCN等模型，提升了长序列预测精度并有效缓解长程依赖。
conclusion: PFRP通过显式记忆检索引入全局视角，为时间序列预测提供新范式，适用于金融、供应链等管理预测任务。
---

## Abstract
Deep learning models such as MLP, Transformer, and TCN have achieved remarkable success in univariate time series forecasting, typically relying on sliding window samples from historical data for training. However, while these models implicitly compress historical information into their parameters during training, they are unable to explicitly and dynamically access this global knowledge during inference, relying only on the local context within the lookback window. This results in an underutilization of rich patterns from the global history. To bridge this gap, we propose Predicting the Future by Retrieving the Past (PFRP), a novel approach that explicitly integrates global historical data to enhance forecasting accuracy. Specifically, we construct a Global Memory Bank (GMB) to effectively store and manage global historical patterns. A retrieval mechanism is then employed to extract similar patterns from the GMB, enabling the generation of global predictions. By adaptively combining these global predictions with the outputs of any local prediction model, PFRP produces more accurate and interpretable forecasts. Extensive experiments conducted on seven real-world datasets demonstrate that PFRP enhances the average performance of advanced univariate forecasting models by 8.4%.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **核心问题**：现有的深度时间序列预测模型（如 MLP、Transformer、TCN）虽然在训练时通过滑动窗口隐式地学习了历史模式，但在推理阶段无法显式、动态地访问全局历史知识，仅依赖当前有限的回溯窗口进行预测。这导致了全局历史数据的利用不足。
- **整体含义**：论文提出了一种名为 **PFRP (Predicting the Future by Retrieving the Past)** 的新框架。其核心思想是将时间序列预测重新定义为一种“检索”任务，通过显式存储和检索与当前窗口相似的全局历史片段，来弥补局部上下文信息的局限性，从而提高预测的准确性和可解释性。

### 2. 论文提出的方法论
PFRP 的流程分为两个主要阶段，其核心组件及算法思想如下：

-   **第一阶段：全局记忆库 (Global Memory Bank, GMB) 构建**
    -   **预测对比学习 (Predictive Contrastive Learning, PCL)**：
        -   训练一个 MLP 编码器，将回溯窗口序列映射为特征向量。
        -   提出了一种新的正负样本选择策略：基于**预测窗口的相似性**（MSE）选择正样本，而非传统的基于回溯窗口的相似性。直观上，这能促使具有相似未来的序列在特征空间中更接近。
        -   损失函数采用对比学习中的 InfoNCE 形式。
    -   **K-medoids 聚类**：
        -   利用训练好的编码器将所有历史样本编码后，应用 K-medoids 聚类以减少冗余、提高检索效率。
        -   选择 K-medoids 而非 K-means，因为它使用真实的历史样本作为聚类中心，保证了记忆库中存储的模式是真实且连贯的。
-   **第二阶段：检索过去以预测未来 (Predicting by Retrieving)**
    -   **检索过程**：
        -   将当前回溯窗口编码为查询向量，在 GMB 中通过余弦相似度检索 top-k 个最相似的历史特征（键），并取出其对应的预测序列（值）。
    -   **权重调制与全局预测生成**：
        -   **置信门 (Confidence Gate)**：将检索到的历史预测序列与当前回溯窗口拼接，通过一个 MLP + Sigmoid 来学习该组合序列存在的概率。该概率用于动态调整检索到的 top-k 序列的权重。
        -   **输出门 (Output Gate)**：基于当前回溯窗口，通过 MLP 生成尺度 (α) 和平移 (β) 参数，对初始的加权全局预测进行仿射变换，使其更贴合当前尺度与基线。
    -   **动态融合 (Dynamic Fusion)**：
        -   同时，将当前回溯窗口输入任意一个“局部预测模型”得到局部预测结果。
        -   将调制后的 top-k 权重输入一个 MLP + Softmax，动态计算全局预测和局部预测的融合权重，加权求和得到最终的预测结果。
        -   该框架与模型无关，可适配并增强任意局部预测模型。

### 3. 实验设计
-   **数据集**：在跨 3 个领域的 7 个真实世界单变量时间序列数据集上进行评估：
    -   **交通/能源**：Traffic, Electricity
    -   **天气**：Weather
    -   **电力变压器**：ETTh1, ETTh2, ETTm1, ETTm2
-   **基准设置**：遵循标准预测协议，回溯窗口 L 固定为 96，预测长度 H ∈ {96, 192, 336, 720}。评估指标为 MSE 和 MAE。
-   **对比方法**：
    -   **主流基线模型**：MLP-based (DLinear, SparseTSF)，Transformer-based (PatchTST)，CNN-based (TimesNet)。评估 PFRP 对它们的性能增强效果。
    -   **大型预训练模型**：LLM-based (TimeCMA)，基础模型 (Moirai, Sundial)，验证 PFRP 对先进大模型的兼容性。
    -   **检索增强预测方法**：直接对比了 RATD 和 RAFT 等同类 RAG-based 方法。

### 4. 资源与算力
-   论文**未明确提及**所使用的 GPU 型号或具体数量。
-   关于时间效率，文中仅在消融实验部分（Figure 7）提及了在 Electricity 数据集上的相对时间成本：构建 GMB 耗时固定为 186 秒（其中 PCL 134秒，K-medoids 52秒）。与基线模型相比，训练时间的增加微乎其微，模型大小仅增加 1.57 MB。

### 5. 实验数量与充分性
论文进行了大量且相对充分的实验，主要分为以下几类：

-   **主实验结果**：在 7 个数据集、4 种预测长度下，对 4 种不同类型的基线模型进行有无 PFRP 的对比，包含数十组对比实验。
-   **可视化分析**：通过图表展示了预测结果对比、检索到的历史序列，以及周期性与融合权重的相关性。
-   **对比实验**：与 2 种大型模型和 2 种 RAG 方法进行了性能与效率的对比。
-   **消融实验 (Ablations)**：
    -   **关于 GMB**：探究了不同检索标准（特征相似度 vs. DTW/MSE/PCC）、不同编码器（MLP vs. PatchTST vs. TimesNet）、不同编码器训练策略（PCL vs. CL vs. 预测学习）的影响。
    -   **关于 PFRP 组件**：分别移除了置信门、输出门、局部预测模型，分析各模块的贡献。
-   **效率分析**：对比了集成 PFRP 前后各模型的训练时间和模型大小。
实验设计客观，通过控制变量法（使用相同回溯窗口和预测长度）对比，且每个实验重复 3 次取平均值，保证了公平性。

### 6. 论文的主要结论与发现
-   PFRP 能普遍提升 SOTA 单变量预测模型的性能，平均提升 8.4%，对于 DLinear 和 SparseTSF 等简单模型的提升尤为显著。
-   在周期性强的数据集（如 Traffic, Electricity）上，PFRP 的提升效果（提升最高达 17.4%）远强于周期性弱的数据集，表明其有效利用了周期性重复模式。
-   动态融合机制的有效性：在强周期性数据上，模型会自适应地提高全局预测的融合权重，反之亦然。
-   与其它 RAG 方法相比，PFRP 不仅精度更高，而且因仅在固定大小的记忆库中检索，推理效率更高。

### 7. 优点
-   **新颖性**：将检索增强生成（RAG）概念创造性地引入单变量时间序列预测，通过“以过去检索预测未来”的范式解决局部上下文信息不足的问题。
-   **模型无关性 (Model-Agnostic) 与即插即用**：PFRP 作为一个框架，可以无缝附加到任何现有的局部预测模型（从简单 MLP 到大型基础模型）之上，显著提升其性能，而几乎不增加模型大小和训练成本。
-   **可解释性**：通过显式检索并可视化相关历史序列，让预测过程不再是一个黑箱，增强了结果的可解释性。
-   **高效性**：通过 K-medoids 构建固定大小的记忆库，避免了在全部历史数据上的穷举检索，在精度和效率间取得了平衡。
-   **精巧的训练策略**：提出的 PCL 方法，通过关注“相似的未来”来学习特征表示，比传统基于“相似的过去”的学习策略更契合检索式预测的目标。

### 8. 不足与局限
-   **对随机性强、周期性弱的数据效果有限**：论文自己也证实，在 ETT 等周期性不显著的数据集上，PFRP 的提升幅度相对较小。
-   **未在多变量场景下评估**：实验仅限于单变量预测，其方法在多变量时间序列预测中的有效性及对变量间依赖关系的建模能力尚待验证。
-   **超参数敏感性**：方法引入了 GMB 大小（K）、top-k 检索数量等额外超参数，论文未详细讨论这些参数在不同数据集上的敏感性及调优策略。
-   **编码器选择依赖经验**：GMB 所使用的编码器最优结构（MLP, PatchTST, TimesNet）因数据集而异，文中未提供选择指南，增加了实际应用的调优成本。
-   **全局记忆库对非平稳性的鲁棒性**：当数据分布发生剧烈变化（如概念漂移）时，基于全部历史构建的 GMB 可能会检索到过时或不再相关的模式，带来负面干扰，文中未探讨此情景。

(完)

### 9. 未来工作与开放问题
- **推广到多变量与时空预测**：论文仅评估了单变量场景，未来可探索 PFRP 在多变量时间序列及包含空间维度的时空预测任务中的表现，特别是如何定义和检索多变量片段、以及如何在记忆库中保持变量间相关性。
- **在线学习与记忆库更新**：当前 GMB 为静态构建，不适用于持续演化、存在概念漂移的数据流。设计能够在线更新、淘汰过时模式的动态记忆库，将提升 PFRP 在非平稳环境中的鲁棒性。
- **可学习的检索策略**：目前基于 k-NN 的检索使用固定的余弦相似度，未来可考虑通过学习得到的哈希或强化学习驱动的检索策略，进一步提升检索质量与多样性。
- **大规模部署与压缩**：虽然 K-medoids 已经降低了记忆库规模，但在处理长达数年、高频采样数据时，记忆库仍可能过大。需要探索层级化记忆、乘积量化等压缩技术。
- **与其他模态的结合**：PFRP 目前只利用序列本身的信息，若能将文本描述、时间戳元数据等辅助信息纳入检索与融合过程，可望进一步提升在复杂真实场景中的预测精度。

（完）
