---
title: Stage-Aware Graph Contrastive Learning with Node-oriented Mixture of Experts
title_zh: 面向节点的专家混合模型阶段感知图对比学习
authors: "Xiangkai Zhu, Yeyu Yan, Saiqin Long, Chao Li, Guanwen Chen, Longsheng Su"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38695/42657"
tags: ["query:mt"]
score: 9.0
evidence: 使用阶段感知的图对比学习与专家混合模型对文本属性图进行自监督表示学习
tldr: 文本属性图表示学习中，大型语言模型与图神经网络结合时常因架构异质导致性能差异。本文提出阶段感知的图对比学习框架，并设计面向节点的专家混合模型，分阶段协调两类模型，实现更有效的自监督表示，为融合文本和图结构信息提供了新方案，可用于管理领域的知识图谱表示。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38695/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 859, \"height\": 489, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38695/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1485, \"height\": 636, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38695/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 862, \"height\": 447, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38695/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 868, \"height\": 428, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38695/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 840, \"height\": 405, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38695/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1824, \"height\": 571, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38695/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1839, \"height\": 584, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38695/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 887, \"height\": 630, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38695/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 884, \"height\": 313, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38695/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 884, \"height\": 289, \"label\": \"Table\"}]"
motivation: 不同语言模型架构与图神经网络不匹配，导致文本属性图表示学习性能波动。
method: 提出阶段感知的对比学习框架，引入节点级专家混合模型协调语言与图编码器。
result: 在多个文本属性图数据集上，方法显著优于单一模型和直接拼接方案。
conclusion: 分阶段协调多模态信息可提升文本图表示质量，对管理文本关系建模有参考价值。
---

## Abstract
Text-attributed graphs (TAGs), which associate rich textual descriptions with each node, are widely employed to represent complex relationships among real-world textual entities.
Currently, representation learning for TAGs leverages large language models (LLMs) to transform node-matched textual descriptions into node features or labels, followed by the message passing in graph neural networks (GNNs) that further improves the expressiveness of graph representation learning. Nevertheless, a simple experiment we conducted demonstrates that not all LLMs are readily compatible with GNNs.
A salient finding indicates that  architectural heterogeneity among LLMs manifests as substantial performance gap across diverse TAGs representation learning.
Moreover, the node semantics encoded by LLMs are often misaligned with the message passing in GNNs, causing performance collapse. 
Motivated by this observation, we propose a novel  self-supervised graph learning framework called Stage-Aware Graph Contrastive Learning (SAGCL).
In particular, we propose the node-oriented mixture of experts (NodeMoE) to assign suitable candidate experts for each node. 
It flexibly balances the strengths of different language experts by low-rank decomposition and reparameterization strategies. 
Subsequently, to align the inductive biases of graph structures with the semantic perception capabilities of LLMs, the message passing in GNNs is decoupled into the feature transformation  stage and the feature propagation  stage.  Given the two stage views, stage-aware graph contrastive learning is proposed to match the node semantics encoded by the LLM with the locally aware topological patterns within the GNN via self-supervised contrastive learning. Experiments on eight datasets and three downstream tasks demonstrate the effectiveness of SAGCL.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义

- **研究背景**：文本属性图（TAG）结合了图拓扑与丰富的节点文本描述（如引用网络、推荐场景），当前主流方法利用大型语言模型（LLM）编码文本作为 GNN 的输入特征，随后进行消息传递以增强表示学习。
- **核心问题**：简单实验发现，**不同 LLM 与 GNN 之间并非总能兼容**：LLM 架构异质性导致在不同 TAG 表示学习任务上性能差距显著；且 LLM 编码的节点语义常与 GNN 的消息传递机制 **语义失配**，引发性能崩塌（甚至不如原始浅层特征）。
- **研究动机**：由此引出两个关键研究问题：
  1. 如何协调多个 LLM 的优势，为每个节点生成最有利于下游任务的语义表示？
  2. 如何将 LLM 编码的节点语义与 GNN 中局部感知的拓扑模式对齐？
- **整体含义**：本文提出一种自监督图学习框架 **SAGCL**，通过面向节点的专家混合（NodeMoE）协调多 LLM，并将 GNN 消息传递解耦为特征变换（FT）和特征传播（FP）两个阶段，利用阶段感知的图对比学习实现语义对齐，从而释放 LLM 对 GNN 的潜力。

### 2. 论文提出的方法论

- **整体框架**：SAGCL 包含两大模块：NodeMoE 与阶段感知图对比学习。
- **NodeMoE（节点级专家混合）**：
  - 目的：为每个节点自适应融合多个 LLM 专家（如 Llama、Qwen）编码的文本嵌入。
  - 方法：不同于传统门控 MoE，NodeMoE 通过**预训练阶段**获得各专家嵌入，再学习节点特定的协同系数 Ψ 进行加权求和。
  - 低秩重参数化：为避免直接学习 N×M 的巨大矩阵，将 Ψ 分解为节点依赖矩阵 S_m 和节点无关参数矩阵 Θ，S_m 由专家嵌入通过可学习的非线性变换得到，最终通过 softmax 归一化得到融合权重。
- **阶段感知图对比学习**：
  - 解耦消息传递：GNN（以 SGC 为例）被划分为**特征变换（FT）** 阶段（MLP 映射 X→H）和**特征传播（FP）** 阶段（聚合邻居信息得到 Z）。
  - 对比对齐设计：
    - **FT 阶段**：对齐 NodeMoE 融合后的 LLM 语义 z^llm 与 GNN 的 FT 输出 h，使用三元组损失，负样本来自随机打乱行。
    - **FP 阶段**：
      - 对齐原始 FT 特征 h 与传播后表示 z^gnn（保持原始特征结构化偏差），并引入上界损失防止过度平滑。
      - 对齐 LLM 语义 z^llm 与传播后表示 z^gnn，同样采用三元组损失。
  - 总损失：L_total = ω(L_ft + L_fp1) + L_fp2 + L_up_fp1，ω 平衡浅层与 LLM 增强语义。

### 3. 实验设计

- **数据集**：8 个图数据集，包括 **Cora、CiteSeer、PubMed、WikiCS、Instagram、Photo、Computer、ogbn-arxiv**，覆盖小规模引文网络与大规模商品共现/社交网络。
- **下游任务**：节点分类（NC）、节点聚类（NClu）、链接预测（LP）。
- **Baseline 方法**：共 12 种自监督方法，分为两类：
  - 图对比学习：DGI、GRACE、BGRL、GREET、SUGRL、ROSEN。
  - 图自编码器：MaskGAE、GiGaMAE、S2GAE、Bandana、BSG、TEDMAE。
- **评估指标**：NC 用 Accuracy；NClu 用 NMI 和 ARI；LP 用 AUC。
- **实现细节**：使用 3 个 LLM 专家（Llama‑8B、Qwen‑7B、Qwen‑2.5‑7B）；低秩 r=3；损失权重 ω=10。数据划分遵循 LLMNodeBed 标准或固定比例。5 次随机种子取平均与标准差。

### 4. 资源与算力

- 论文**未明确提及**使用的 GPU 型号、数量或具体训练时长。文中仅指出使用了 Llama‑8B、Qwen‑7B 等较大的语言模型进行文本编码，而 GNN 训练部分的参数规模未给出硬件消耗说明。实验环境仅指向代码仓库，无详细算力记录。

### 5. 实验数量与充分性

- **实验规模**：
  - 8 个数据集 × 3 个下游任务，每个任务与 12 个基线方法进行了全面比较（部分方法在大型数据集上出现 OOM）。
  - 包含消融实验（去除 FT 对齐、FP 对齐、上界约束）验证各模块贡献。
  - 参数敏感性分析（α、β）展示耦合行为。
  - 针对 NodeMoE，分析了不同专家数量（M=1 至 5）的影响，并使用 Sentence‑Bert 等较小模型验证泛化性；还可视化节点级专家权重分布。
- **公平性与客观性**：统一数据划分，公开代码，多次运行报告标准差，基线选择覆盖主流自监督学习范式，对比较为公平。消融实验与超参数分析补充充分，证明设计有效性。

### 6. 论文的主要结论与发现

- SAGCL 在所有 8 个数据集、三项下游任务上**一致超越** 12 种基线方法，尤其在节点分类与聚类任务上提升显著。
- NodeMoE 的低秩重参数化使每个节点能自适应选择最合适的语言专家组合，专家数量存在最优值（M≈3），过多会引入噪声。
- 阶段感知的对比学习有效对齐了 LLM 语义与图拓扑，FT 与 FP 阶段的对齐在不同数据集上贡献各异，说明语义对齐与结构保持需根据图特性动态平衡。
- 上界损失（L_up_fp1）缓解了深层 GNN 的过平滑问题，增强表示稳定性。

### 7. 优点（亮点）

- **新颖框架**：首次提出解耦 GNN 消息传递为 FT/FP 阶段并通过自监督对比损失同时对齐 LLM 语义与图拓扑，不依赖标签。
- **节点级专家分配**：NodeMoE 通过低秩分解实现参数高效的节点特定融合，避免了全局单一专家或稠密权重矩阵的缺陷。
- **实验全面扎实**：覆盖多类型数据集、多任务、多基线，消融与敏感性分析细致，结论可信度高。
- **实用性**：无需修改 GNN 结构，可即插即用于各种图自监督学习场景。

### 8. 不足与局限

- **计算资源说明缺乏**：未报告使用 LLM 编码及训练所需的硬件成本，实际部署门槛不明。
- **LLM 专家依赖**：方法需要预编码多个 LLM 的节点嵌入，存储与预处理开销大，且对 LLM 选择敏感，超参数（α、β、ω）需按数据集调参。
- **模型扩展性局限**：仅在小到中等规模图（如 Cora、Photo）上验证，大型引用网络（ogbn‑arxiv）上相对提升有限，部分基线直接 OOM 说明框架扩展能力仍有待验证。
- **阶段分解简化**：基于线性 SGC 的 FT/FP 解耦，对更复杂的注意力或异配 GNN 的泛化性未探索。
- **理论分析不足**：缺乏对对比损失收敛性或表示对齐的理论保证。

（完）
