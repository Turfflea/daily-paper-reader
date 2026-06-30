---
title: Self-Supervised Contrastive Re-Learning for Multi-Graph Multi-Label Classification
title_zh: 面向多图多标签分类的自监督对比重学习方法
authors: "Meixia Wang, Yuhai Zhao, Zhengkui Wang, Yejiang Wang, Miaomiao Huang, Fenglong Ma, Fazal Wahab, Wen Shan, Xingwei Wang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39842/43803"
tags: ["query:mt"]
score: 9.0
evidence: 针对多图多标签分类的自监督对比重学习方法，在无标签数据上学习表示
tldr: 多图多标签学习依赖大量标注数据，自监督对比学习虽减少标签依赖，但直接应用面临未建模标签相关性和结构扰动改变语义两大挑战。为此提出自监督对比重学习方法，通过对比重学习策略保留标签相关性并维持语义一致性，在无标签数据上学习用于多图多标签分类的有效表示，显著提升了对稀缺标签的鲁棒性。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39842/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1842, \"height\": 640, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39842/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1841, \"height\": 1817, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39842/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1841, \"height\": 1845, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39842/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1804, \"height\": 322, \"label\": \"Table\"}]"
motivation: 多图多标签场景标注成本高，现有自监督对比学习忽视标签相关性和结构扰动影响。
method: 设计对比重学习模块，同时保持标签相关性和语义不变性，用于无标签表示学习。
result: 在多个多图多标签数据集上，所提方法在少标签甚至零标签下均取得最优分类性能。
conclusion: 自监督对比重学习为标签稀缺的多图多标签问题提供了一种有效的表征学习方法。
---

## Abstract
Multi-graph multi-label learning (MGML) represents each object as a bag-of-graphs with multiple labels, but demands large-scale labeled data whose acquisition is often difficult and costly. Self-supervised contrastive learning (SCL) mitigates label dependence by leveraging data augmentation to construct discriminative pretext tasks, proving effective for multi-instance learning. However, when applied to MGML, SCL faces two key challenges: (1) it distinguishes individual instances by their differences, whereas MGML requires modeling label correlations; (2) it assumes semantic invariance under augmentation, but structural perturbations in MGML alter label semantics. To tackle these challenges, we propose a self-suPervised contrastive rE-learning framework for mulTi-grAph multi-labeL classification (PETAL). Specifically, to model label correlations, we first define a unified label space to learn label prototypes and align features with them, yielding prototype-aligned representations. We then design a multi-granularity contrastive loss over these representations, which captures label dependencies by contrasting at the bag level, graph level, and bag-graph level. Moreover, to ensure semantic invariance, we develop a contrastive re-learning strategy based on prototype-aligned representations to generate augmentation-free positive samples. This guarantees consistent multi-label distributions without structural perturbations. Experiments on six datasets demonstrate that PETAL achieves an average improvement of 4.12% over state-of-the-art self-supervised and supervised baselines.

---

## 论文详细总结（自动生成）

### 论文核心问题与整体含义（研究动机和背景）
- **核心问题**：多图多标签学习（Multi-Graph Multi-Label Learning, MGML）将每个对象表示为一个包含多个标签的图包（bag-of-graphs），但其性能高度依赖大规模、高质量的标注数据。获取完全标注的MGML数据极其困难且成本高昂。
- **研究动机**：自监督对比学习（Self-Supervised Contrastive Learning, SCL）能够通过数据增强从无标签数据中学习判别性表示，从而减少对人工标注的依赖。然而，直接将SCL应用于MGML面临两大挑战：
    - **标签相关性建模缺失**：传统SCL旨在区分单个实例的差异，而MGML中每个样本关联多个相互依赖的标签，因此必须有效建模标签间的复杂依赖关系。
    - **语义不变性难以保证**：SCL假设数据增强后的视图在语义上与原样本一致。但对于图结构数据，轻微的结构扰动（如增删节点或边）就可能破坏关键子结构，改变图乃至图包的标签语义，无法生成语义一致的正样本对。
- **整体含义**：本文旨在提出一种能适应MGML场景的自监督学习框架，在无需真实标签的情况下，学习出既包含判别性特征又嵌入了标签相关性的图包表示，以用于下游的多标签分类任务。

### 论文提出的方法论（PETAL框架）
- **核心思想**：提出名为 **PETAL** （自监督对比重学习框架），通过“多粒度对比损失”显式建模标签相关性，并借助“对比重学习策略”在不依赖数据增强的情况下生成语义不变的正样本。
- **关键技术细节与算法流程**：
    1.  **双粒度特征提取**：
        - **初始表示**：使用图核函数（Graph Kernel）将每个图映射为核向量，以保留图内和图间的结构相似性。
        - **可学习表示**：将核向量输入一个基于多层感知机（MLP）的编码器-解码器架构，生成可训练、高保真的图级表示 \( Z^{G_{ij}} \)。
        - **包级聚合**：通过最大池化操作将包内所有图的表示聚合成粗粒度的包级表示 \( Z^{B_i} \)。
    2.  **标签特定原型表示**：
        - **统一标签空间**：定义一个具有 \( L' \) 个伪标签的统一标签空间，并为每个标签初始化一个独热向量。
        - **原型学习**：通过原型编码器 \( f_\delta \) 将这些向量编码为可学习的标签原型（Label Prototypes）\( C \)，作为类中心。
        - **原型对齐表示**：通过计算图/包表示与所有标签原型之间的相似度（点积），并经 softmax 归一化，得到原型对齐的图级表示 \( P^G \) 和包级表示 \( P^B \)。
    3.  **多粒度对比重学习**：
        - **对比重学习策略**：为解决语义不变性问题，不直接扰动输入图/包。而是利用最优传输算法 \( f_c \) 在锚点（原型对齐表示 \( Z^G, Z^B \)）和语义原型（\( C \)）之间计算最小成本传输计划，生成保证多标签分布一致的“重学习”正样本（\( Q^G, Q^B \)）。
        - **多粒度对比损失**：在生成的增强自由正负样本对上，计算三部分对比损失的加和：
            - \( L1 \)：细粒度图级对比损失。
            - \( L2 \)：粗粒度包级对比损失。
            - \( L3 \)：跨粒度（图-包）对比损失，通过拉近包内图的正样本和包表示等操作，整合跨粒度的语义互补信息。

### 实验设计
- **数据集**：在六个公开的MGML基准数据集上进行评估：**MSRC v2、VOC12、Scene、Dblp(ai)、Dblp(cv)、Dblp(db)**。
- **评价指标**：采用六种标准的多标签分类指标：**One Error、Hamming Loss、Coverage、Ranking Loss、Average Precision、Macro-F1**。
- **对比方法（基线）**：
    - **监督学习方法**（6种）：包括4种先进的多示例多标签（MIML）算法（M3MIML, KISAR, MIMLFast, MIML-LLMC）和2种最先进的MGML方法（cfMGML, cfMGNML）。
    - **自监督学习方法**（6种）：包括5种先进的图对比学习方法（GRAPHCL, JOAO v2, SimGRACE, GALOPA, PaGCL）和1种自监督MIL方法（SMILES）。

### 资源与算力
- **硬件**：实验在单个 **A5000 GPU** 上进行。
- **超参数**：文中列出了部分关键超参数，如批次大小为128，学习率为 \( 1 \times 10^{-5} \)，伪标签数量 \( L' = 200 \)，温度参数 \( \tau = 0.09 \)。未明确提及模型训练的总时长。
- **图核选择**：MSRC v2 数据集使用 Weisfeiler-Lehman 核，其他数据集使用 Graph-Hopper 核。

### 实验数量与充分性
- **实验数量**：实验设计较为全面，总计对比了12种方法，覆盖监督和自监督两大范式。
- **充分性与公平性**：
    - **对比充分**：与多种主流基线在六个数据集和六个指标上进行了全面比较。
    - **消融实验**：通过移除多粒度损失（\( L1, L2, L3 \)）、对比重学习策略（\( f_c \)）和标签原型（\( C \)）来验证每个核心组件的有效性。
    - **评估方式合理**：与监督方法比较时采用十折交叉验证，与自监督方法比较时采用反向五折交叉验证（20%训练，80%测试），所有方法均使用标准的RakelD分类器进行下游评估，确保了公平性。

### 论文的主要结论与发现
- PETAL作为一种自监督学习方法，在所有六个基准数据集上一致且显著地超越了所有监督和自监督基线方法，平均性能提升达 **4.12%**。
- 通过多粒度对比损失有效建模了标签相关性，消融实验证明跨粒度（包-图）损失尤为重要。
- 对比重学习策略成功地在不依赖数据增强的情况下生成了语义一致的正样本，这对性能提升至关重要，其移除会导致性能大幅下降。
- 标签原型的引入对捕获标签依赖关系和确保有效分类起到了关键作用。

### 优点
- **问题新颖性与首创性**：首次尝试以自监督方式学习多图包表示，用于下游多标签分类，填补了研究空白。
- **方法设计巧妙**：
    - “**对比重学习**”策略巧妙地规避了图数据增强破坏语义的固有问题，是一个通用的、可跨模态扩展的思想。
    - “**多粒度对比损失**”和“**标签原型**”设计有效地将单标签SCL扩展到了多标签场景。
- **实验说服力强**：实验设计全面，同时与监督和自监督方法比较，并通过详细的消融实验验证了各模块的有效性。

### 不足与局限
- **计算效率未详述**：虽然提及了时间和空间复杂度，但缺少与基线的实际训练时间、资源消耗对比，难以评估其效率优劣。
- **伪标签数量敏感**：方法引入了伪标签数量（\( L' \)）作为一个关键超参数，文中未探讨其敏感性及对不同数据集的选取策略。
- **对比重学习机制泛化性**：文中采用的最优传输算法作为重学习策略的实例，其效果是否最优，以及该通用策略在其他生成机制下的潜力，尚需更多验证。
- **应用场景初步**：主要在图分类的学术基准数据集上验证，其在更复杂、更大规模现实场景（如算力网络）中的应用效果有待进一步探索。

（完）
