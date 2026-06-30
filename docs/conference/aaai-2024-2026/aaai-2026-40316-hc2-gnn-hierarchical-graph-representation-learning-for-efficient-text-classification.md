---
title: "HC2-GNN: Hierarchical Graph Representation Learning for Efficient Text Classification"
title_zh: HC2-GNN：面向高效文本分类的层次图表示学习
authors: "jiejie fan, Xiaojuan Ban, Zhiyan Zhang, Xi Sun"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40316/44277"
tags: ["query:mt"]
score: 9.0
evidence: 基于GNN的层次聚类文本分类方法
tldr: 针对现有GNN文本分类方法计算效率低且难以同时建模细粒度局部结构和序列信息的局限，提出HC2-GNN，采用轻量图聚类算法C2GC，在保持文本顺序和子图拓扑一致性的同时实现高效分类。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40316/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 836, \"height\": 195, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40316/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1742, \"height\": 888, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40316/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 874, \"height\": 452, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40316/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 855, \"height\": 317, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40316/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 875, \"height\": 259, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40316/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 839, \"height\": 221, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40316/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 871, \"height\": 264, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40316/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 874, \"height\": 249, \"label\": \"Table\"}]"
motivation: 基于图的文本分类模型存在计算开销大、无法很好捕捉文本顺序和局部结构的问题。
method: 设计了一种层次聚类与粗化图神经网络，并创新性地提出折衷电导图聚类算法。
result: 在多个文本分类数据集上表现出高效性和竞争性的分类精度。
conclusion: 所提框架为图神经网络在大规模文本分析中的实用化提供了可能。
---

## Abstract
Graph Neural Networks (GNNs) offer superior modeling capabilities for text classification by capturing complex spatial features within semantic representations. However, existing graph-based approaches often suffer from computational inefficiency and limited ability to model both fine-grained local structures and the sequential nature of text. To address these challenges, we propose HC2-GNN, a Hierarchical Clustering and Coarsening Graph Neural Network, which introduces a novel lightweight graph clustering algorithm called Compromise Conductance Graph Clustering (C2GC). C2GC enables efficient graph clustering while simultaneously preserving both the textual order and the topological coherence of subgraphs. Furthermore, it incorporates a virtue cluster mechanism that expands each subgraph with semantically relevant neighbors, explicitly enabling cross-cluster information propagation without compromising local structural integrity. HC2-GNN aggregates local and global features by combining subgraph-level and full-graph representations, enhancing semantic discriminability for classification. Extensive experiments on benchmark datasets demonstrate that HC2-GNN consistently outperforms existing state-of-the-art text classification methods.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有的图神经网络（GNN）文本分类方法虽然能够捕捉复杂的语义关系，但存在计算效率低下、难以同时建模文本的细粒度局部结构和序列特性的问题。
- **研究动机**：
  - 图结构虽然适合表示文本中的复杂依赖，但密集或静态图导致计算成本高。
  - 传统图聚类或粗化方法（如基于电导的切割）往往忽略文本的顺序性，或破坏局部句法结构。
  - 层级式 GNN 中，子图之间的消息传递受限，形成信息瓶颈。
- **整体含义**：该论文旨在提出一种兼顾计算效率、文本顺序保持和跨子图信息流通的层次图表示学习框架 HC2-GNN，以提升文本分类的性能。

### 2. 论文提出的方法论

- **核心思想**：构建一个轻量级的层次聚类和粗化图神经网络（HC2-GNN），通过折衷电导图聚类（C2GC）将文本图划分为有序的子图，利用虚拟簇扩展机制实现跨簇信息传播，最后通过双分支 GNN 整合局部子图与全局图表示进行分类。
- **关键技术细节**：
  - **图构建**：根据词共现关系为每个文本构建无向图，节点按文本中的顺序排列。
  - **C2GC 算法（Compromise Conductance Graph Clustering）**：
    - 沿文本顺序累积节点，计算每个潜在分割点的电导。
    - 在电导最小的位置分割子图，迭代直到达到目标子图数。
    - 引入超参数 β 控制簇大小，避免过小或过大。
    - 与全局最优切割不同，C2GC 在维持顺序和拓扑一致性的前提下追求局部最优。
  - **虚拟簇（Virtue Cluster）扩展**：
    - 对每个子图，选择 n-hop 邻居并按累计边权重排序。
    - 逐节点加入并重新计算电导，直到电导不再下降或上升为止。
    - 生成扩展后的虚拟簇，允许跨簇消息传递，同时通过掩码保持原始子图的顺序结构。
  - **图粗化**：
    - 使用映射矩阵和分配矩阵生成子图邻接矩阵和粗化全局邻接矩阵。
    - 对全局粗化矩阵进行归一化和剪枝（阈值 α），并补充必要边以保持连通性。
  - **图学习与合并**：
    - **子图编码**：对虚拟簇使用 GGNN + GRU 堆叠 t 次，获取顺序敏感的局部特征；再通过掩码恢复原结构并用 GRU 更新。
    - **全局图编码**：使用多层 GCN，第一层用原始邻接矩阵，后续层用粗化后的邻接矩阵。
    - **合并**：将每个子图的两种节点表示拼接后输入 GAT，再经平均+最大池化得到子图级表示；最后拼接所有子图表示得到全文向量，经全连接层和 softmax 分类。
- **损失函数**：交叉熵损失。

### 3. 实验设计

- **数据集**：五个公开基准文本分类数据集——MR（短文本）、Ohsumed（医学长文本）、20NG（长文本）、R8、R52（简单文本）。
- **对比方法（Benchmarks）**：
  - **传统模型组**：TextGCN、TextRNN、FastText、SWEM、TextING、SGC。
  - **Transformer 增强模型组**：BertGCN、BertGAT、MSABertGCN、ConTextING-BT（对应模型 HC2-GNN-GT 替换内部模块为 Graph Transformer）。
- **评价指标**：分类准确率（Accuracy）。
- **实验设置**：使用预训练 GloVe 300d 词向量，9:1 划分训练/验证集，Adam 优化器，学习率 0.005，dropout 0.5，早停阈值设为节点数 25。

### 4. 资源与算力

- **提及算力**：论文提到“使用一台配备 64 GB 内存和 Nvidia 4090 GPU 的工作站进行效率分析”。
- **训练时长**：仅报告了在 20NG 数据集上每两个 epoch 的运行时间（图 4），用于对比不同变体及代表性基线；未给出完整训练的精确 GPU 小时数。
- **未明确说明**：未给出主要实验结果的总训练轮次、单次训练的精确 GPU 消耗等具体算力开销。

### 5. 实验数量与充分性

- **主要实验**：
  - 在 5 个数据集上对比 10 个基准模型（传统模型 6 个，Transformer 增强模型 4 个），共 2 张主表。
  - 消融实验：
    - **聚类消融**（表 3）：四种变体（去掉聚类、仅用图形拓扑聚类、去掉虚拟簇、完整模型）在 20NG 和 20NG-L 上比较。
    - **合并消融**（表 4）：四种变体（去掉全局分支、去掉全局和顺序性、刚性电导聚类、完整模型）在 20NG 和 20NG-L 上比较。
  - **效率分析**（图 4）：对比多个内部变体及代表性基线在 20NG 上每两轮 epoch 的耗时。
- **充分性与客观性**：
  - 实验覆盖多个文本长度和难度的数据集，对比覆盖传统方法和 SOTA Transformer 方法，消融设计针对两个核心模块，较为全面。
  - 所有对比均使用统一的数据划分和词向量，标准偏差报告多次运行结果（10 次或 3 次），具有客观性。
  - 公平性：对于 Transformer 增强组，作者将内部模块替换为 Graph Transformer 以保证一致性。

### 6. 论文的主要结论与发现

- HC2-GNN 在所有传统基准上均取得最优准确率，特别是在 Ohsumed（+2.7%）和 20NG（+4.1%）等长文本或难数据集上提升显著。
- HC2-GNN-GT 在 Transformer 增强组中也表现最优，尤其在 20NG（91.26%）和 Ohsumed（75.32%）上大幅领先。
- 消融实验证明：
  - C2GC 聚类模块通过保持文本顺序和拓扑一致性明显提升长文本性能。
  - 虚拟簇扩展机制有效缓解了信息瓶颈。
  - 双分支设计（局部子图 GGNN+GRU 和全局 GCN）及顺序感知编码对性能至关重要。
- 效率分析显示，HC2-GNN（传统 GNN 版本）的额外聚类和粗化开销很低，训练时间显著低于 Transformer 增强方法，实现了效率与精度的平衡。

### 7. 优点

- **方法设计亮点**：
  - C2GC 算法在计算效率和结构保持之间取得了良好折中，巧妙融合了文本顺序和拓扑电导。
  - 虚拟簇机制开创性地在层次 GNN 中显式实现跨簇信息传播，且不引入结构性噪声。
  - 框架与具体 GNN 架构解耦，可灵活替换为 Transformer 等。
- **实验设计亮点**：
  - 引入 20NG-L 长文本极端子集，有效验证长文本场景下的优势。
  - 多维度消融实验清晰分离了各部分贡献。
  - 同时对比传统和 Transformer 增强两条路线，证明了模型在两者上的通用性。

### 8. 不足与局限

- **实验覆盖**：
  - 仅在英文文本分类数据集上评测，未涉及中文或其他语言；未在超大图数据或领域外任务上验证泛化性。
  - 效率分析仅展示每两轮 epoch 时间，缺少总体训练时间和收敛轮数的详细对比。
- **偏差风险**：
  - 超参数 β（簇大小控制）和 α（剪枝阈值）的敏感性分析未在文中展开。
  - 所有实验基于 GloVe 词向量，如果用更现代的预训练 Transformer 嵌入初始化，结果可能不同。
- **应用限制**：
  - 对极短文本（如 MR 平均 18 词）提升较小，因为图聚类和粗化能提取的结构信息有限。
  - 虽然训练速度优于 Transformer 模型，但相对于简单基线仍有框架复杂度，用于大规模在线低延迟服务可能需进一步工程优化。
- **未明确说明**：论文未提供模型推理时间的分析。

（完）
