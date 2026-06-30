---
title: Adaptive and Context-rich Generative Self-supervised Learning on Graphs
title_zh: 图上自适应且上下文丰富的生成式自监督学习
authors: "Yijun Tian, Chuxu Zhang, Ziyi Kou, Zheyuan Liu, Xiangliang Zhang, Nitesh V Chawla"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39792/43753"
tags: ["query:mt"]
score: 9.0
evidence: 自适应且上下文丰富的图自监督学习生成方法
tldr: 针对生成式图自监督学习中存在的节点重要性不均、全局信息利用不足、表示空间语义知识忽略以及重构不稳定等问题，本文提出ACE-GSL框架。该框架从自适应性、完整性、互补性和协作性四个角度出发，通过自适应掩码、上下文增强和语义对齐等机制，有效提升了图表示学习的质量。实验表明，ACE-GSL在多个图基准任务上取得领先性能，为图数据上的自监督学习提供了更稳健和全面的解决方案。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39792/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1836, \"height\": 827, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39792/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 883, \"height\": 362, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39792/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 880, \"height\": 367, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39792/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1651, \"height\": 711, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39792/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1800, \"height\": 682, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39792/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 875, \"height\": 299, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39792/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1608, \"height\": 565, \"label\": \"Table\"}]"
motivation: 现有生成式图自监督学习方法忽视节点重要性差异、未充分利用全局信息、忽略表示空间语义知识且重构不稳定。
method: 提出ACE-GSL框架，从自适应性、完整性、互补性和协作性角度设计，包含自适应掩码、上下文增强和语义对齐模块。
result: 实验证明，ACE-GSL在图分类等多种任务上超越了现有方法，实现了更稳定和有效的表示学习。
conclusion: ACE-GSL通过多维度改进，显著提升了图自监督学习的性能与鲁棒性，为图数据预训练提供了新范式。
---

## Abstract
Generative self-supervised learning on graphs has emerged as a popular learning paradigm and demonstrated its efficacy in handling non-Euclidean data. However, several remaining issues limit the capability of existing methods: 1) the disregard of uneven node significance in masking, 2) the underutilization of holistic graph information, 3) the ignorance of semantic knowledge in the representation space due to the exclusive use of reconstruction loss in the output space, and 4) the unstable reconstructions caused by the large volume of masked contents. In light of this, we propose ACE-GSL, an adaptive and context-rich graph self-supervised learning framework to address these issues from the perspectives of adaptivity, integrity, complementarity, and consistency. Specifically, we first develop an adaptive feature mask generator to account for the unique significance of nodes and sample informative masks (adaptivity). We then design a ranking-based structure reconstruction objective joint with feature reconstruction to capture holistic graph information and emphasize the topological proximity between neighbors (integrity). After that, we present a bootstrapping-based similarity module to encode the high-level semantic knowledge in the representation space, complementary to the low-level reconstruction in the output space (complementarity). Finally, we build a consistency assurance module to provide reconstruction objectives with extra stabilized consistency targets (consistency). Extensive experiments demonstrate that ACE-GSL achieves state-of-the-art performance over 28 methods on 20 datasets across 3 tasks.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- 背景：图自监督学习（特别是生成式方法，如掩码自编码器）已成为处理非欧数据的有效范式。然而，现有方法存在四个核心局限性，限制了其表征能力。
- 四个关键问题：
  1. **掩码时忽视节点重要性的不均一性**：随机掩码假设所有节点均匀分布，未考虑节点承载的信息量差异，导致次优掩码策略。
  2. **全图信息利用不足**：仅重构边或特征不足以完整编码图的结构与内容，缺少对节点间拓扑邻近性的显式强调。
  3. **忽略表示空间中的语义知识**：训练目标仅依赖输出空间的重构损失，而下游任务实际使用的是中间层学到的表示，两者之间存在鸿沟。
  4. **大量掩码内容导致的重建不稳定**：高掩码率引入隐式不确定性，导致模型性能波动。
- 研究目标：提出一个自适应、上下文丰富的图自监督学习框架（ACE-GSL），从自适应性、完整性、互补性和一致性四个角度同时解决上述问题。

### 2. 论文提出的方法论
ACE-GSL 包含四个创新组件，分别对应四个解决视角：
1. **自适应特征掩码生成器（Adaptivity）**  
   - 核心思想：根据节点的重建困难程度分配不同的掩码概率，使模型重点重建信息丰富、难以重构的节点。  
   - 技术细节：  
     - 利用辅助采样网络（MHA + FFN + Softmax）输出所有节点的概率分布 \(P\)。  
     - 通过类别分布无放回采样获得掩码节点集 \(\tilde{V}\)（大小由特征掩码率 \(p_f\) 决定）。  
     - 引入可学习的掩码token \([FMASK]\) 替换被掩码节点特征；编码器得到表示 \(H_1\) 后再施加与 \([FMASK]\) 索引一致的 \([DM]\) token 进行二次掩码，解码器输出重建特征 \(Z_1\)。  
     - 特征重建损失为缩放余弦误差：  
       \[
       \mathcal{L}_{FR} = \frac{1}{|\tilde{V}|}\sum_{v\in\tilde{V}}\left(1 - \frac{X_v \cdot Z_{1v}^\top}{\|X_v\| \times \|Z_{1v}^\top\|}\right)^\alpha
       \]  
     - 采样过程不可微，采用 REINFORCE 优化：  
       \[
       \mathcal{L}_{sample} = -\mathbb{E}[\mathcal{L}_{FR}] = -\sum_{v\in\tilde{V}}\log(P_v)\cdot \mathcal{L}_{FR}^v
       \]  
       通过最大化期望重建误差来优化生成器，梯度停止传播至编解码器以避免重复计算。

2. **基于排序的结构重建（Integrity）**  
   - 核心思想：利用相对排序损失替代严格的二分类边重建，捕获整体图信息并强调邻居间的拓扑邻近性。  
   - 技术细节：  
     - 随机生成结构掩码（伯努利分布，掩码率 \(p_s\)），移除部分边得到 \(\tilde{E}\)。  
     - 编码器输出 \(H_2\)，解码器得到 \(Z_2\)。  
     - 损失函数为：  
       \[
       \mathcal{L}_{SR} = \sum_{(v_i,v_j)\in\tilde{E}}\max\{0,\, 1 - \text{sim}(Z_2^{v_i}, Z_2^{v_j}) + \text{sim}(Z_2^{v_i}, Z_2^{v_{j'}})\}
       \]  
       鼓励相连节点相似度高于负采样节点。

3. **基于自助法的相似性模块（Complementarity）**  
   - 核心思想：在表示空间捕捉高层语义知识，弥补输出空间低层重建的不足。  
   - 技术细节：  
     - 引入动量编码器 \(f_E^*\)（参数为原编码器 \(f_E\) 的指数移动平均）。  
     - 将特征掩码图和结构掩码图分别输入动量编码器得到动量表示 \(H_1^*\) 和 \(H_2^*\)。  
     - 原编码器表示 \(H_1, H_2\) 经过投影网络得到 \(H_1', H_2'\)。  
     - 交叉掩码相似性损失：  
       \[
       \mathcal{L}_{BS} = -\frac{1}{|V|}\sum_{v\in V}\left[\frac{(H_1')_v \cdot (H_2^*)_v}{\|(H_1')_v\|\times\|(H_2^*)_v\|} + \frac{(H_1^*)_v \cdot (H_2')_v}{\|(H_1^*)_v\|\times\|(H_2')_v\|}\right]
       \]  
       迫使不同掩码视图下的表示对齐。

4. **一致性保障模块（Consistency）**  
   - 核心思想：通过自蒸馏提供稳定的额外监督，缓解因大量掩码导致的重建不稳定。  
   - 技术细节：  
     - 引入动量解码器 \(f_D^*\)（参数为原解码器 \(f_D\) 的指数移动平均）。  
     - 动量编码特征 \(H_1^*\) 输入 \(f_D^*\) 得到动量重建 \(Z_1^*\)。  
     - 一致性损失为：  
       \[
       \mathcal{L}_{CA} = \frac{1}{|\tilde{V}|}\sum_{v\in\tilde{V}}\left(1 - \frac{Z_1^v \cdot (Z_1^*)_v}{\|Z_1^v\|\times\|(Z_1^*)_v\|}\right)^\beta
       \]  
       使原始重建向动量重建对齐。

最终总损失联合以上各项进行训练。

### 3. 实验设计
- **数据集**：共 20 个，涵盖 3 类任务。
  - 节点分类：Cora、CiteSeer、PubMed、Ogbn-arxiv、PPI（共5个）。
  - 图分类：IMDB-M、IMDB-B、PROTEINS、COLLAB、MUTAG、REDDIT-B、NCI1（共7个）。
  - 迁移学习（分子性质预测）：BBBP、Tox21、ToxCast、SIDER、ClinTox、MUV、HIV、BACE（共8个）。
- **基准对比方法**：总计 28 种，包括监督方法（GCN、GAT、GIN、DiffPool等）、传统生成式（GAE、GPT-GNN、GATE）、对比式（DGI、MVGRL、GRACE、BGRL、InfoGCL、CCA-SSG、GraphCL、JOAO等）、图核方法（WL、DGK）、图自监督预训练（AttrMasking、ContextPred、InfoMax、GraphLoG等）、近期掩码自编码器方法（GraphMAE、GraphMAE2、S2GAE、AUG-MAE）。
- **评测设置**：
  - 节点分类采用归纳（PPI）或直推（其他）设定，评价指标为准确率（PPI用Micro-F1）。
  - 图分类采用准确率，特征使用节点标签或度数。
  - 迁移学习使用分子支架分割，评价指标为ROC-AUC。
  - 所有实验重复 5/10 次，报告均值与标准差。

### 4. 资源与算力
- 论文未明确说明 GPU 型号、数量及训练时长。
- 仅提及实现框架为 PyTorch 与 DGL，优化器为 Adam。

### 5. 实验数量与充分性
- **实验组数丰富**：主实验覆盖 3 项任务 × 20 个数据集，对比 28 种方法，总计超过 60 个主要对比结果。
- **消融研究**：分析各组件贡献（AM、SR、BS、CA）及解码器骨干网络（MLP、GCN、GIN、GAT）的影响。
- **参数敏感性分析**：探究特征掩码率 \(p_f\) 和结构掩码率 \(p_s\) 对性能的影响。
- **公平性**：与 SOTA 方法采用相同实验设置（如划分方式、评价指标），多数据集的平均提升较为一致，实验设计客观、充分。

### 6. 论文的主要结论与发现
- ACE-GSL 在所有 20 个数据集上的 3 项任务中均取得最优性能，显著超越当前基于掩码自编码器的图自监督方法（如 GraphMAE2、S2GAE）。
- 四个组件各自均有正向贡献，验证了自适应性、结构重建、语义互补和一致性保障的必要性。
- 模型对结构掩码率不敏感，鲁棒性较强。
- 迁移学习表现大幅领先，表明预训练表征具有良好的泛化性和迁移能力。

### 7. 优点（方法论或实验设计的亮点）
- 首次系统性地从四个维度诊断并解决生成式图自监督学习的关键缺陷。
- 自适应掩码通过REINFORCE直接以重建难度为导向，更符合信息论直觉。
- 排序结构损失避免了二分类边重建的刚性，更灵活地保留相对邻近关系。
- 引入动量编解码器进行跨视图表示对齐和重建蒸馏，巧妙融合了对比范式与生成范式的优点。
- 实验覆盖面广，在节点、图、迁移三大类任务上均获得一致提升，验证了方法的普遍有效性。
- 解码器骨干可灵活替换，方法具有良好的可扩展性。

### 8. 不足与局限
- 未提供计算资源消耗分析，缺乏训练效率、参数量、GPU 显存等对比，难以评估实际部署成本。
- 特征掩码自适应采样网络的设计相对简洁，未探讨更复杂的分布假设或替代梯度估计方法（如 Gumbel-Softmax）。
- 对某些极小型数据集（如 MUTAG）的提升不显著，且存在较大方差，小样本场景下的稳定性有待加强。
- 所有组件联合训练可能增加调参难度，论文未给出各损失项权重的消融或详细设定过程。
- 迁移学习仅限分子图领域，未探索其他跨域迁移场景。

（完）
