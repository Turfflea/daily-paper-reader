---
title: Contextual Structure Knowledge Transfer for Graph Neural Networks
title_zh: 图神经网络的上下文结构知识迁移
authors: "Zhiyuan Yu, Wenzhong Li, Zhangyue Yin, Xiaobin Hong, Shijian Xiao, Sanglu Lu"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/34381/36536"
tags: ["query:mt"]
score: 7.0
evidence: 图神经网络迁移学习，解决同配性偏移，用于关系数据建模
tldr: 针对图迁移学习中源域和目标域同配性结构差异导致的性能下降问题，提出上下文结构图神经网络(CS-GNN)。通过定制注意力机制捕获局部结构线索，促进结构知识跨域迁移。实验表明该方法在有限标签目标域上优于现有迁移方法，增强了图模型在异质关系数据上的适应性。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34381/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 873, \"height\": 323, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34381/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1829, \"height\": 543, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34381/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 735, \"height\": 693, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34381/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 861, \"height\": 249, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34381/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1825, \"height\": 243, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34381/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 859, \"height\": 295, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34381/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 874, \"height\": 156, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34381/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1726, \"height\": 491, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34381/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1714, \"height\": 494, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34381/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1731, \"height\": 501, \"label\": \"Table\"}]"
motivation: 现有图迁移学习方法忽视同配性偏移，导致源域到目标域迁移时性能大幅下降。
method: 设计CS-GNN，利用注意力机制学习局部结构线索，通过自我网络模块实现结构知识迁移。
result: 在跨域图数据上，分类准确率显著提高，有效缓解同配性偏移问题。
conclusion: CS-GNN为图迁移学习提供了结构感知的解决方案，适用于多领域关系数据建模。
---

## Abstract
Graph transfer learning endeavors to develop a Graph Neural Network (GNN) model in a fully-labeled source domain, with the intention of deploying it on a target domain that has limited labeled data for inference. We reveal that prevalent graph transfer learning methods are susceptible to the homophily shift problem. This issue arises from the divergence in homophily structures between the source and target graphs, leading to a notable deterioration in the performance of GNN models. In this paper, we introduce a novel Contextual Structural Graph Neural Network (CS-GNN) method, leveraging a tailored attention mechanism to apprehend a variety of local structural cues, facilitating structural knowledge transfer across domains. It features an ego-network module to distill local structural diversity and a moment-based approach to gauge structural patterns without needing ground-truth labels. CS-GNN crafts a feature smoothness matrix from node attributes, guiding a customized attention mechanism for feature aggregation. A group-wise fairness loss is employed to balance learning across various structural patterns, enhancing the model's ability to transfer knowledge across domains.  Comprehensive experiments conducted on six benchmark datasets substantiate the superiority of CS-GNN over the state-of-the-art methods, demonstrating significant improvements in accuracy and robustness against homophily shifts.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义

- **研究问题**：图迁移学习（graph transfer learning）旨在利用有标签的源图知识，帮助标签稀缺的目标图进行推断。现有方法大多忽视**同配性偏移（homophily shift）**问题——即源域与目标域在同配性（相似节点相连的倾向）结构上的差异，导致模型性能大幅下降。
- **背景与动机**：
  - 现实世界的图数据集中，同配性差异普遍存在（如表1所示），且随着差异增大，下游分类准确率最多可下降10%（图1）。
  - 尽管同一网络中不同子图的结构模式也存在显著多样性，但现有迁移方法仍未能有效利用这部分**上下文结构知识**来提升跨域泛化性。
- **整体含义**：论文提出一种**上下文结构图神经网络（CS‑GNN）**，通过捕获局部结构多样性、量化和利用结构模式，从而在跨域迁移中缓解同配性偏移，提升目标域性能。

### 2. 方法论：核心思想、关键技术细节、算法流程

CS‑GNN 的核心思路是：在同一网络中，不同节点周围存在多样的局部结构；通过在源域和目标域之间**隐式匹配相似的结构模式**，赋予模型结构感知的注意力机制，促进知识迁移。主要包含以下四个模块：

- **K-跳自我网络构建（Ego‑Network Construction）**  
  - 为每个中心节点构造其 *k*-跳自我图（包含距离 ≤ *k* 的所有节点及相应边），以捕获局部结构多样性。后续的消息传递限制在该自我网络内。

- **基于矩的特征平滑度（Moment‑based Feature Smoothness）**  
  - 定义特征平滑度来衡量节点与其邻居间的特征差异程度。为刻画不同属性维度上的差异分布，引入**多阶矩**（原点矩、中心矩、标准化矩），形成每个图的特征平滑度矩阵 `F_G`。
  - 该度量不依赖真实标签，因此可直接在无标签目标域中计算，用于指导后续聚合。

- **上下文消息传递（Contextual Message Passing）**  
  - 不同于标准图注意力机制（对所有节点使用同一套参数），CS‑GNN 根据节点的自我网络特征平滑度矩阵 `F_gu` 生成**个性化的查询（query）向量**：
    ```
    Q_u = [q⊙σ(W_src F_gu) ‖ q⊙σ(W_dst F_gu)]
    ```
  - 然后分别与邻居信息构成的键（key）向量计算注意力权重，由此在不同结构模式下定制不同的邻居聚合方式。
  - 这一设计相当于一种**隐式结构匹配机制**：具有相似上下文结构的节点会被赋予相似的注意力计算逻辑，从而跨越域传递知识。

- **分组公平损失（Group‑Wise Fairness Loss）**  
  - 为避免模型过度依赖图上的主流结构模式，先使用 METIS 图划分算法将图分成多个不重叠的子图。
  - 对每个子图计算交叉熵损失，并动态加权：权重初始为 `1/K`，然后按上一轮训练中子图损失的反比进行 softmax 调整。损失较大的子图将获得更高权重。
  - 这种设计推动模型在所有结构模式上均衡学习，增强迁移能力。

**整体流程**：输入源图和目标图 → 为每个节点构造 *k*-跳自我网络 → 计算基于矩的特征平滑度矩阵 → 通过上下文注意力进行消息传递 → 用分组公平损失联合训练 GNN 编码器和分类器。

### 3. 实验设计

- **数据集**：6 个真实世界基准——
  - 引文网络（Citation）、社交网络（Social）、WebKB（多子域）、机场网络（Airport，含多个美国机场子域）、ARXIV、CORA。
  - 部分实验还通过向源图添加不同比例的异配边（5%~20%）来人工构造不同级别的同配性偏移。

- **基准方法**：
  - **基础 GNN**：GCN、GAT、GCNII、JKNet。
  - **图迁移学习方法**：EERM、MIXUP、DANE、UDAGCN、GRADE、STRURW（均以标准 GCN 为骨架）。

- **实验设定**：
  - **直接迁移（direct transfer）**：在源域全监督训练，目标域无任何标签，直接评估。
  - **微调（fine‑tune）**：在 Citation 数据集上，利用目标域 0.1%、1%、10% 的标签微调，考察少样本场景。
  - 所有方法统一深度为 2 层，隐藏维数 64，基于 PyTorch Geometric 实现。超参数参照原文或公平调参。报告 5 次运行的均值和标准差。

### 4. 资源与算力

论文中**未明确说明**所使用的 GPU 型号、数量或训练时长。无任何有关计算资源或能耗的讨论。

### 5. 实验数量与充分性

- **主要实验表格（表2‑4）**：覆盖 6 个数据集上 20+ 个迁移任务（包括 Citation、Social、WebKB 多对子域、Airport 的 6 对迁移、ARXIV、CORA）。直接迁移与微调均有系统对比。
- **超参数分析**：对矩阶数（1~5）、自我网络跳数（1~5）、图划分数（2~32）、温度参数 τ（0.1~100）分别探索，结果以 4 张图呈现。
- **消融实验**：移除自我网络构建、上下文消息传递、分组公平损失三个组件，单独检验每部分贡献。
- **可视化**：定性展示上下文注意力在不同结构模式下的权重分配。

整体实验设计**充分且客观**：对比方法涵盖基础 GNN 和主流迁移方法；消融和超参分析验证了各组件有效性及敏感性；多次运行报告方差，排除了随机性干扰。但未涉及时间复杂度或大规模图上的效率测试，也缺少显著性检验。

### 6. 主要结论与发现

- CS‑GNN 在所有直接迁移和微调场景下均**显著优于**现有最优方法，平均准确率相对 GNN 骨架提升约 5.8%，在 Citation 数据集上比最优迁移方法高最多 4.0%。
- 当目标域标签极度稀少（0.1%）时，CS‑GNN 的优势尤为突出（约 10% 准确率增益），证明其有效缓解了同配性偏移并提取了可迁移的结构知识。
- 消融与超参分析证实了三个模块的协同增益，且方法对超参数鲁棒性较好。

### 7. 论文的优点

- **问题新颖**：首次明确聚焦图迁移学习中的同配性偏移，指出该问题的普遍性和严重性。
- **方法论创新**：
  - 基于矩的特征平滑度不需要标签即可量化局部结构，突破了半监督场景的限制。
  - 上下文注意力实现“结构匹配”式的个性化聚合，优于传统统一注意力。
  - 分组公平损失迫使模型学习多样结构模式，避免过拟合主流模式，提升泛化。
- **实验扎实**：多数据集、多迁移对、大量对比、丰富消融和参数分析，结果可信。
- **可复现性**：提供了代码链接，超参数和实验配置描述清晰。

### 8. 不足与局限

- **计算开销未评估**：自我网络构造、矩计算和分组损失可能带来额外开销，但缺乏时间或内存消耗报告，实际部署的可行性需进一步验证。
- **图划分依赖**：分组公平损失依赖 METIS 划分结果，该方法本身引入额外预处理步骤，对极大规模图可能成为瓶颈。
- **假设限制**：假设源域和目标域共享特征空间，否则需额外投影，文中未深入讨论该场景。
- **领域多样性**：实验集中于同配性差异明显的引文、社交、Web 等网络，尚未在更多异质性图（如分子图、推荐系统）上验证。
- **对比方法与调参**：仅以标准 GCN 为骨架实现迁移方法，未探索与其他强骨架（如 GAT）组合的潜力；部分基线可能未调至最优。
- **公平性与可解释性**：尽管引入公平损失，但未分析各子图权重变化对特定结构模式的偏向性，缺乏模型决策的进一步解释。

（完）
