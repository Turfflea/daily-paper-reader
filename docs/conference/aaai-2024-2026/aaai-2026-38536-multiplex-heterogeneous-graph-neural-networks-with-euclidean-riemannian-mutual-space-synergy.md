---
title: Multiplex Heterogeneous Graph Neural Networks with Euclidean-Riemannian Mutual Space Synergy
title_zh: 欧几里得-黎曼空间协同的多重异构图神经网络
authors: "Xiang Li, Yuan Cao, Zhongying Zhao, Guoqing Chao, Yanwei Yu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38536/42498"
tags: ["query:mt"]
score: 9.0
evidence: 用于非欧空间关系建模的多重异构图神经网络
tldr: 针对现有多重异构图神经网络在关系解耦和非欧拓扑捕获上的局限，本文提出一种欧几里得-黎曼互空间协同的模型。它分别在两种几何空间中处理节点交互，通过空间融合有效区分异构关系并保留复杂语义，在节点分类和链路预测等任务上取得领先性能，为复杂关系网络建模提供了新范式。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38536/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1830, \"height\": 588, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38536/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 857, \"height\": 516, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38536/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 885, \"height\": 763, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38536/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 879, \"height\": 317, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38536/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1833, \"height\": 866, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38536/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1838, \"height\": 866, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38536/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 884, \"height\": 354, \"label\": \"Table\"}]"
motivation: 现有方法难以在保持复杂语义的同时有效捕获非欧几里得拓扑和关系模式。
method: 设计双空间图神经网络，在欧式和黎曼空间中分别建模，并通过空间协同融合。
result: 在多项异构图基准上，本方法在节点分类和链路预测中性能超越已有工作。
conclusion: 利用双几何空间互补优势，显著提升了对多重异构关系的建模能力。
---

## Abstract
Multiplex heterogeneous networks are common in real-world scenarios, where entities interact through diverse types of relations across multiple semantic layers. Recent advances in multiplex heterogeneous graph neural networks have achieved remarkable results by incorporating node and relation types into message passing and designing relation-aware architectures. However, most existing methods either decouple relations and risk losing complex semantics or require handcrafted relation patterns, which limit scalability. Moreover, prevailing models are typically restricted to Euclidean space, making it difficult to capture non-Euclidean topologies and to distinguish complex interactions among heterogeneous nodes and relations. Standard GNN message passing, grounded in the homophily assumption, also proves inadequate for the intricate, coupled structures in multiplex heterogeneous graphs. To address these challenges, we propose MRiemGNN, a novel multiplex heterogeneous graph neural network that synergizes Euclidean and Riemannian spaces through a geometry-aware, relation-specific message passing scheme and cross-space mutual learning. Experiments on multiple real-world datasets show that MRiemGNN achieves superior performance, efficiency, and scalability on both node classification and link prediction tasks.

---

## 论文详细总结（自动生成）

## 1. 研究动机与核心问题

现实世界中大量数据天然构成 **多重异构图**（multiplex heterogeneous graphs），即节点和边的类型多样、关系层次丰富、语义结构复杂。现有图神经网络（GNN）在处理这类图时存在三个关键瓶颈：
- **关系语义丢失**：多数方法将不同类型的关系解耦后简单聚合，无法刻画复合关系（多个关系同时出现）的耦合语义；另一类方法依赖手工设计的元路径或关系模式，扩展性差。
- **几何空间受限**：主流模型仅在欧几里得空间中建模，难以同时表达密集局部连接和层级化、树状的全局拓扑，也无法有效区分异构节点与交互的复杂模式。
- **同配性假设失效**：传统消息传递建立在同配性（相似节点相连）之上，在类型差异巨大的异构图中会导致表示坍塌和信息混淆。

因此，论文的核心问题是：**如何在一个统一框架下，同时建模多重异构图中的复杂关系语义与非欧几何结构，并让不同几何空间的学习相互增强**。

## 2. 方法论

### 总体架构
MRiemGNN 包含四大模块：
1. **关系感知核消息传递**
2. **欧几里得‑黎曼互空间协同**
3. **双空间嵌入聚合**
4. **任务自适应解码器**

### 关键技术细节
- **关系感知核消息传递**  
  不仅标记单一边类型，还将所有实际出现的 **关系组合（复合关系）** 显式编码为抽象关系标签。对每个几何空间 \(s \in \{\text{Euc}, \text{Rie}\}\) 和每个关系 \(r\)，采用 **随机傅里叶特征（RFF）核** 进行非线性变换：
  \[
  \phi_s^r(h_j^s) = \sqrt{\frac{2}{D}} \cos(\omega_s^r \cdot h_j^s + b_s^r)
  \]
  然后按关系邻域加权聚合：
  \[
  \tilde{h}_i^s = \sum_{r} \alpha_s^r \cdot \frac{1}{|N_i^r|} \sum_{j \in N_i^r} \phi_s^r(h_j^s) \cdot W_s^r
  \]
  通过堆叠多层，捕获高阶、关系特定的结构模式。

- **欧几里得‑黎曼互空间协同**  
  在最后层获得两空间嵌入 \(z_i^{\text{Euc}}\) 和 \(z_i^{\text{Rie}}\) 后，计算温度缩放的 softmax 分布：
  \[
  q_i^s = \text{softmax}(z_i^s / \tau)
  \]
  并用对称 KL 散度对齐两空间的高层语义分布：
  \[
  \mathcal{L}_{\text{mut}} = \sum_i \big[ \text{KL}(q_i^{\text{Euc}} \| q_i^{\text{Rie}}) + \text{KL}(q_i^{\text{Rie}} \| q_i^{\text{Euc}}) \big]
  \]
  这使两个几何空间在保持各自归纳偏置的同时实现知识互补。

- **双空间嵌入聚合**  
  通过一个可调系数 \(\beta\) 融合对齐后的嵌入：
  \[
  z_i = \beta \cdot \tilde{z}_i^{\text{Euc}} + (1-\beta) \cdot \tilde{z}_i^{\text{Rie}}
  \]
  最终得到兼具局部密集建模（欧式优势）与全局层级建模（双曲/黎曼优势）的统一表示。

- **任务自适应解码器**  
  - 节点分类：线性层 + softmax，交叉熵损失。
  - 链接预测：构建边特征向量（拼接、点乘、差），经 MLP 计算边分数，二分类交叉熵损失。
  总损失为任务损失与互蒸馏损失加权和。

## 3. 实验设计

- **数据集**  
  5 个公开异构图数据集：
  - IMDB（非多重异构图）
  - Alibaba、Amazon、Taobao、Mag（多重异构图，其中 Mag 为大规模 ogbn‑mag 改编版）
  详细统计见表 1，涵盖节点数、边数、节点/边类型数、是否多重等。

- **任务与指标**  
  - 节点分类：Macro‑F1、Micro‑F1
  - 链接预测：AUROC、AUPR
  所有实验均划分训练/验证/测试，采用早停防止过拟合。

- **对比方法**  
  两类别共 15 个基线：
  - 欧式方法：FAME、MHGCN、DMG、MGDCR、CoCoMG、BPHGNN、MGHC
  - 黎曼/双曲方法：HGCN、SELFMGNN、HIE、RRN、MotifRGC、CUSP、GraphMoRE、H‑EDML

## 4. 资源与算力

- **硬件**：Intel Xeon Gold 5320T CPU（2.3GHz），1 块 NVIDIA RTX 3090 GPU。
- **实现框架**：Deep Graph Library + PyTorch。
- **训练时长**：论文给出了详细的运行时比较表（Table 4），MRiemGNN 在所有数据集上单轮及总训练时间均为最短，例如在大规模 Mag 数据集上总运行时间仅约 287 秒，相较次优基线提速 **3～8 倍**。训练超参数通过网格搜索确定（学习率、权重衰减、隐层维度、层数等）。

## 5. 实验数量与充分性

实验覆盖程度很高，主要实验组包括：
- **性能对比实验**：节点分类（4 个数据集 vs. 15 个基线）、链接预测（5 个数据集 vs. 15 个基线），每组均含 Macro‑F1/Micro‑F1 或 AUROC/AUPR 两个指标。
- **消融实验**：针对四个核心模块（去掉欧式空间、去掉黎曼空间、去掉互蒸馏、去掉关系核）分别在四个数据集上进行节点分类测试。
- **效率分析**：四个数据集上记录单轮与总运行时间，对比 4 个强基线。
- **超参数敏感性分析**：分别分析 GNN 层数 \(L\)、融合系数 \(\beta\)、互蒸馏损失权重 \(\lambda_1/\lambda_2\) 对性能的影响，覆盖五个数据集。
- 实验设置中采用早停、多次实验并进行 t‑test 显著性验证（标记 *，p<0.01），保证了客观性与公平性。

## 6. 主要结论与发现

- MRiemGNN 在所有数据集的节点分类和链接预测任务上均 **显著优于** 当前最先进的欧式和黎曼基线模型，平均性能提升达 **11.57%（节点分类）** 和 **3.68%（链接预测）**。
- 在基线已接近饱和的高精度区间（如 Taobao 链接预测 AUROC > 0.99）仍能取得领先，显示出模型的鲁棒性。
- **双重几何空间的互补性** 至关重要：去除任一空间均会导致明显性能下降；互蒸馏模块作用最为关键，单纯双空间而无协同效果最差。
- 关系感知核消息传递显著提升了对复合关系的建模能力，但其重要性略低于空间协同。
- 模型在 **运行效率** 上同样领先，训练时间大幅缩短，具有良好的可扩展性。
- 适当深度的 GNN 层（2 层）与适中融合比例（\(\beta\approx0.6\)）能取得最佳效果，互学习损失权重在 0.1‑0.5 之间较为稳健。

## 7. 方法亮点

- **创新的双空间融合范式**：首次在异构图学习中同时使用欧式与黎曼空间并引入互蒸馏，实现语义对齐与几何互补，既保留局部紧邻结构又捕获全局层级拓扑。
- **显式建模复合关系**：通过关系标签扩展和随机傅里叶特征核，无需手工设计元路径即可自动捕捉简单及组合语义，增强模型对多重关系的表达能力。
- **高效可扩展**：核方法与轻量级双空间融合设计使得运行时间远低于多数基线，尤其适合大规模多重异构图。
- **实验全面扎实**：多项任务、多样数据集、详尽的消融与参数分析，验证了各模块的独立贡献及模型的泛化性。

## 8. 不足与局限

- **几何空间种类有限**：当前仅考虑欧式与双曲（Riemannian）两种几何，未探索球面、伪黎曼等更复杂的流形，理论扩展性尚有空间。
- **复合关系依赖于观测**：关系标签的枚举需要数据中实际出现的组合，若某些复合关系在训练集中缺失，模型无法学习其语义，在动态或流式场景下可能存在局限。
- **对无特征或特征极稀疏图的适用性未讨论**：实验数据集均带有初始节点特征，未验证在无特征或仅结构信息的图中的表现。
- **超参数敏感性的讨论限于几个关键参数**：整体超参数搜索范围虽有一定覆盖，但未系统分析核近似维度、温度 τ 等参数的影响。
- **缺少失败案例分析**：未展示哪些类型的节点或边预测不佳，也未讨论模型在极端异质性或极高噪声条件下的鲁棒性。
- **公平性偏差风险**：所有数据集皆为英文场景下的公开数据，未讨论潜在的社会偏见或分布外泛化问题。

（完）
