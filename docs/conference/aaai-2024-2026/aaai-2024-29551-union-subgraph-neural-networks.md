---
title: Union Subgraph Neural Networks
title_zh: 并子图神经网络
authors: "Jiaxing Xu, Aihu Zhang, Qingtian Bian, Vijay Prakash Dwivedi, Yiping Ke"
date: 2024-03-25
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/29551/30921"
tags: ["query:mt"]
score: 9.0
evidence: 通过并子图和最短路径描述符增强图神经网络表达能力
tldr: 针对普通图神经网络表达能力受限于1维Weisfeiler-Leman测试的问题，本文提出并子图神经网络（Union Subgraph Neural Networks）。通过挖掘局部邻域中的并子图结构，并设计具有完美性质的最短路径描述符，该模型能够捕获边的一跳邻域完整连接信息，从而注入高阶连接性。实验表明，该方法显著提升了GNN在图分类和回归任务上的性能，为关系数据建模提供了更强大的表示学习工具。
source: AAAI-2024-Accepted
selection_source: conference_retrieval
motivation: 普通GNN的表达能力上限为1-WL测试，无法捕捉局部邻域内的复杂连接性，限制了图表示学习的性能。
method: 识别并利用并子图结构，设计基于最短路径的子结构描述符，将邻居连接性信息注入GNN的消息传递过程。
result: 实验证明，所提方法在图分类等任务上显著超越基线GNN，有效突破了1-WL限制。
conclusion: 并子图神经网络通过捕捉高阶连接性增强了GNN的表达能力，为图数据建模提供了新的思路。
---

## Abstract
Graph Neural Networks (GNNs) are widely used for graph representation learning in many application domains. The expressiveness of vanilla GNNs is upper-bounded by 1-dimensional Weisfeiler-Leman (1-WL) test as they operate on rooted subtrees through iterative message passing. In this paper, we empower GNNs by injecting neighbor-connectivity information extracted from a new type of substructure. We first investigate different kinds of connectivities existing in a local neighborhood and identify a substructure called union subgraph, which is able to capture the complete picture of the 1-hop neighborhood of an edge. We then design a shortest-path-based substructure descriptor that possesses three nice properties and can effectively encode the high-order connectivities in union subgraphs. By infusing the encoded neighbor connectivities, we propose a novel model, namely Union Subgraph Neural Network (UnionSNN), which is proven to be strictly more powerful than 1-WL in distinguishing non-isomorphic graphs. Additionally, the local encoding from union subgraphs can also be injected into arbitrary message-passing neural networks (MPNNs) and Transformer-based models as a plugin. Extensive experiments on 18 benchmarks of both graph-level and node-level tasks demonstrate that UnionSNN outperforms state-of-the-art baseline models, with competitive computational efficiency. The injection of our local encoding to existing models is able to boost the performance by up to 11.09%. Our code is available at https://github.com/AngusMonroe/UnionSNN.

---

## 论文详细总结（自动生成）

### 1. 研究动机与背景
- **核心问题**：标准图神经网络（GNN）的表达能力受限于 1-dimensional Weisfeiler-Leman (1-WL) 测试，因其在消息传递过程中将每个邻居同等看待，忽略了邻居节点之间复杂的连接关系（即局部子图结构信息）。
- **研究意义**：提升 GNN 对非同构图结构的区分能力，使其能够捕获边的一跳邻域内完整的高阶连接性，从而突破 1-WL 上限。

### 2. 方法论
- **核心思想**：通过定义一种新的局部子结构——**并子图**（union subgraph），捕获一条边两端节点的整个一跳闭邻域的连接全貌，并设计一种基于最短路径的子结构描述符，将邻域连接信息编码后注入消息传递过程。
- **关键技术细节**：
  - **局部子结构分析**：将一跳邻域内的边分为四类（共同邻居间、共同与独占邻居间、不同侧独占邻居间、同侧独占邻居间），指出并子图能够包含全部四类边，而重叠子图等只能捕获部分。
  - **并子图定义**：$S_{v\cup u}$ 为 $\tilde{N}(v) \cup \tilde{N}(u)$ 的诱导子图。
  - **子结构描述符**：计算并子图内所有节点对的最短路径长度，构成路径矩阵 $\mathbf{P}_{vu}$；该描述符满足大小感知、连接感知和同构不变性三个性质。
  - **网络设计（UnionSNN）**：
    - 对路径矩阵进行奇异值分解，取奇异值之和作为边 $(v,u)$ 的局部结构系数 $a_{vu}$。
    - 消息传递公式：$\mathbf{h}_v^{(l)} = \text{MLP}_1^{(l-1)}\big((1+\epsilon^{(l-1)})\mathbf{h}_v^{(l-1)} + \sum_{u\in\mathcal{N}(v)} \text{Trans}^{(l-1)}(\tilde{a}_{vu})\mathbf{h}_u^{(l-1)}\big)$，其中 $\tilde{a}_{vu}$ 为归一化系数，$\text{Trans}(\cdot)$ 为可学习的多层感知机 + softmax。
  - **插件式注入**：可将结构系数以逐元素乘或注意力偏置的形式灵活集成到任意 MPNN 或 Transformer 模型中。
- **理论保证**：
  - 证明并子图同构强于重叠子图同构。
  - 证明 UnionSNN 的表达能力严格强于 1-WL，并在某些情况下可区分 3-WL 无法区分的图。

### 3. 实验设计
- **数据集与场景**：
  - 图分类：8 个 TUDataset 数据集（MUTAG, PROTEINS, ENZYMES, DD, FRANKENSTEIN, Tox21, NCI1, NCI109）+ 2 个 OGB 数据集（OGBG-MOLHIV, OGBG-MOLBBBP），共计 10 个。
  - 图回归：ZINC10k, ZINC-full。
  - 节点分类：5 个数据集（Cora, Citeseer, PubMed, Computer, Photo）。
  - 循环检测：4-CYCLES, 6-CYCLES。
- **对比方法**：
  - 经典 MPNN：GCN, GIN, GraphSAGE, GAT, GatedGCN。
  - 高阶 WL 方法：3WL-GNN。
  - Transformer 类：UGformer, Graphormer, GPS。
  - 子图/曲率/结构编码方法：GraphSNN, CurvGN, GatedGCN-LSPE, NestedGIN, DropGIN, MEWISPool, GeniePath, CGN 等。
- **评价方式**：
  - 分类：准确率（Acc）或 ROC-AUC。
  - 回归：MAE。
  - 所有结果均附带标准差，使用 10 折交叉验证或多次运行。

### 4. 资源与算力
- 论文未明确提及所使用的 GPU 型号、数量或具体训练时长。
- 提供的时间复杂度分析：训练阶段为 $O(kmfd)$，与 GIN 和 GraphSNN 相当；预处理时间在多个数据集上与其他子结构描述方法进行了对比，给出秒级数据（如 PROTEINS 73 秒、DD 1226 秒），但未说明硬件环境。

### 5. 实验数量与充分性
- **主要实验**：在 10 个图分类数据集、2 个图回归数据集、5 个节点分类数据集和 2 个循环检测数据集上，共计约 19 种数据集/任务场景，与 15 种以上基线方法进行了全面比较。
- **消融实验**：在 6 个图分类数据集上，分别测试了局部子结构类型（重叠/并减/并子图）、子结构描述符（边介数、节点边计数、曲率、拉普拉斯矩阵）以及路径矩阵编码方法（矩阵和、最大特征值、奇异值和）的影响。
- **案例分析**：通过一个并子图实例，分析了增删节点/边时结构系数的变化规律，解释其如何反映局部连接密度。
- **效率分析**：比较了预处理时间和总运行时间（表格 11、12），覆盖不同规模的图数据集。
- **公平性**：所有对比均基于统一的数据集划分和评估协议，插件实验采用同一骨架模型注入结构系数前后对比，具备客观性。实验数量充足，消融维度完整。

### 6. 主要结论与发现
- UnionSNN 在 7/10 个图分类数据集上超越所有未使用该方法的基线，且在节点分类和图回归中同样领先。
- 结构系数作为插件注入现有 MPNN 或 Transformer 模型后，性能可提升最高 11.09%，并有效降低标准差（如 GCN 在 Cora 上的标准差从 4.41 降至 0.42）。
- 循环检测任务中，将结构系数加入标准 GCN 可使准确率从约 50%（随机猜测）提升至约 95%，表明该方法能有效捕获高阶连接性。
- 消融实验证实：并子图优于其他子结构，最短路径描述符优于边介数、曲率等描述方式，SVD 和奇异值和是更优的矩阵编码方法。
- 理论证明 UnionSNN 的表达力高于 1-WL，并在某些情形下强于 3-WL，但上限仍受限于 4-WL。

### 7. 优点
- **结构性创新**：首次提出并子图概念，能够完全刻画边的一跳邻域连接全貌。
- **描述符设计精巧**：最短路径矩阵具备大小感知、连接感知和同构不变三个优良性质，且使用 SVD 保证置换不变性。
- **高灵活度**：可作为插件无缝嵌入多种 GNN 框架（MPNN、Transformer），无需改变原始网络架构。
- **理论支撑扎实**：提供了关于同构关系、表达力比较的形式化定理，并给出与高阶 WL 的关系分析。
- **实验全面扎实**：覆盖多个任务、多种数据集类型，消融实验与效率分析充分，可解释性强（案例研究）。

### 8. 不足与局限
- **表达力上限**：方法仍受限于局部 3 跳邻域（并子图范围），因此理论上无法超越 4-WL，对长环结构（如长度 > 6 的环）区分能力不足。
- **预处理开销**：需要预先计算所有边的并子图路径矩阵和 SVD，虽然实验显示开销可接受，但在大规模动态图上可能成为瓶颈。
- **硬件细节缺失**：论文未给出 GPU 型号、训练轮数或显存消耗等信息，复现成本不够透明。
- **图类型局限**：实验集中于分子图、引文网络和共购网络等常见基准，缺少在异构图、有向图或大规模工业图上的验证。

（完）
