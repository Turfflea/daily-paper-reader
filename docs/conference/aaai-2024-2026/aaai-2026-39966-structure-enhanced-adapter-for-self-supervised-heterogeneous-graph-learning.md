---
title: Structure-Enhanced Adapter for Self-Supervised Heterogeneous Graph Learning
title_zh: 面向自监督异质图学习的结构增强适配器
authors: "Fengyu Yan, Di Jin, Xiaobao Wang, Qianhua Tang, Dongxiao He"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39966/43927"
tags: ["query:mt"]
score: 9.0
evidence: 自适应结构增强的自监督异质图学习
tldr: 针对异质图存在的缺失关键边和多关系语义冗余等结构噪声，提出结构增强适配器，以解耦方式集成到现有异质图神经网络中，自监督学习更鲁棒的节点表示；实验表明该方案显著优于现有方法，为异质图上的自监督学习提供了结构感知的增强范式。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39966/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 879, \"height\": 451, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39966/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 885, \"height\": 389, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39966/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1845, \"height\": 846, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39966/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 886, \"height\": 680, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39966/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 886, \"height\": 632, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39966/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 884, \"height\": 316, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39966/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1842, \"height\": 1434, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39966/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 884, \"height\": 623, \"label\": \"Table\"}]"
motivation: 现有异质图学习方法忽视原始图的结构缺陷，如缺失边和冗余边，导致次优表示。
method: 提出结构增强适配器，以解耦方式集成到现有异质图神经网络中，自适应地增强结构并减少噪声。
result: 在多个异质图数据集上的节点分类等任务中显著提升性能。
conclusion: 该方法为异质图上的自监督学习提供了结构感知的增强范式，适用于关系数据建模。
---

## Abstract
Real-world heterogeneous data is commonly modeled as heterogeneous information networks (HINs). Building upon advancements in graph neural networks (GNNs), existing research has significantly progressed in semi-supervised and self-supervised paradigms for heterogeneous GNNs (HGNNs). However, these methods overlook inherent structural deficiencies in raw heterogeneous graphs. We identifies unique structural noise in HINs: missing potential critical edges and multi-relational semantically redundant edges, which force existing HGNNs to learn suboptimal representations on fixed topologies. Crucially, prior limited studies address only partial noise while remaining architecturally entrenched and tightly coupled with specific models. To break this bottleneck, we propose a plug-and-play Heterogeneous graph Structure ADaPter (HSADP) that simultaneously resolves task/model decoupling challenges while accounting for HIN-specific structural properties with with two core components: a dynamic homogeneous subgraph enhancer recovering latent topology across semantic views and a learnable heterogeneous edge discriminator dynamically suppressing redundant edges while collaboratively optimizing semantic graphs. Extensive experiments across multi-domain datasets demonstrate our method’s effectiveness and compatibility. The adapter significantly boosts node classification accuracy for multiple SOTA approaches and surpasses specially designed heterogeneous graph structure learning models.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **研究动机**：真实世界数据常建模为异质信息网络（HIN），现有异质图神经网络（HGNN）无论在监督还是自监督范式下，都 **默认图结构是固定且正确的**，忽视了原始图结构中的固有缺陷。
- **核心问题**：论文指出异质图结构中存在 **两类独有的结构噪声**：
  - **边缺失**：潜在的关键边未被记录，导致信息传播不完整（如冷启动用户‑物品交互稀疏）。
  - **语义冗余**：多关系语义下存在大量任务无关或噪声边，稀释有效信息流（如热门商品购买关系淹没个性化差异）。
- **整体含义**：在不改变下游任务模型的前提下，设计一种 **即插即用的结构适配器**，对原始异质图结构进行自监督优化，解决边缺失和语义冗余，从而提升各类 HGNN 的表示质量和节点分类性能。

## 2. 论文提出的方法论

- **整体架构**：提出 **异质图结构适配器（HSADP）**，与预训练模型解耦，包含两条路径：
  1. **同质子图增强器**：针对边缺失，发现潜在有益连接。
  2. **异质边鉴别器**：针对语义冗余，抑制噪声边。
  两者协同优化，输出精炼的结构供任何 HGNN 预训练模型使用。

- **特征转换**：为每个节点类型使用独立 MLP，将不同维度、不同空间的原始特征投影到统一维度，得到 `hi`。

- **同质子图增强器（Homogeneous Subgraph Enhancer）**
  - 利用预定义元路径从原始异质图中提取多个语义不同的单关系同质子图 `G_{Pi}`。
  - 观察到这些子图的 **同配比**（相同类节点连接比例）越高，下游分类性能越好（图2验证）。
  - 设计两种机制来提升同配比、补全缺失边：
    - **基于特征相似度的邻居扩展**：在目标类型节点内，连接特征余弦相似度超过阈值 η 的节点对。
    - **基于多关系传播的动态邻居扩展**：若两个目标节点 `ai`、`aj` 同时高频连接到相似度超过 δ 的非目标节点 `bk`、`bl`，则在 `ai` 和 `aj` 之间建立新边。
  - 将新发现的潜在结构与原结构通过注意力机制融合，并做 top‑K 稀疏化，得到增强后的单关系子图 `A'_{Pi}`。

- **异质边鉴别器（Heterogeneous Edge Discriminator）**
  - 为每条异质边计算 **必要性概率** `pij = φᵣ(hi ∥ hj ∥ h_rel)`，其中 `φᵣ` 是关系特定的 MLP，`h_rel` 为关系嵌入。
  - 所有原始边都作为可学习参数，通过概率动态修剪冗余边，输出精炼后的多关系图 `E^t_Ri`。
  - 更新后的异质图既用于预训练，又通过动态邻接矩阵反馈到同质子图增强步骤中，形成迭代优化。

## 3. 实验设计

- **数据集**：五个广泛使用的异质图基准数据集，覆盖学术和商业场景。
  - ACM、DBLP、Freebase、Yelp、AMiner。
  - 节点数从 3,913 到 180,098，关系类型达 4–36 种。
- **任务与设置**：节点分类，采用 **10‑shot 和 20‑shot** 两种少样本场景。
- **对比方法** 四类基线：
  - 传统同质图方法：GCN、GAT。
  - 半监督异质图方法：HAN、HGT，以及仅用结构的 metapath2vec。
  - 异质图结构学习方法：HGSL。
  - 自监督异质图预训练方法：HeCo、HGCML、HGMAE、HERO。
  - HSADP 作为适配器分别嵌入上述自监督方法（标注 “+HSADP”）。
- **评估指标**：Macro‑F1、Micro‑F1、AUC。

## 4. 资源与算力

- 文中仅说明 **所有实验均在 NVIDIA RTX 3090 显卡上进行**，未说明使用的 GPU 数量、单次训练时长或总计算开销。

## 5. 实验数量与充分性

- **主要实验**：覆盖 5 个数据集 × 2 种少样本设置 × 10 种以上基线/方法组合，生成表2和表3两大张结果表，共约 80 行以上具体数据。
- **消融实验**：在 ACM 和 DBLP 上，对三种自监督预训练模型分别去除同质子图增强器（w/o HoSE）和异质边鉴别器（w/o HeED），共 18 组指标对比。
- **超参数分析**：考察了三个核心超参数（相似度阈值 η、跨类型相关阈值 γ 和 top‑K），在两个数据集上结合两种预训练模型绘制性能变化曲线（图5）。
- **充分性评价**：实验规模较大，覆盖多域、多范式、多指标，并配有消融与参数敏感性分析，且与半监督结构学习方法和纯半监督基线比对，整体 **充分且客观公平**。

## 6. 论文的主要结论与发现

- HSADP 在几乎所有数据集上 **一致提升** 了多种 SOTA 自监督 HGNN 的节点分类精度，尤其在 10‑shot 场景下提升更为显著。
- 该方法 **性能超过** 专门设计的异质图结构学习模型 HGSL，证明其结构学习能力与泛化优势。
- 双路径设计具有互补作用：同质子图增强器补全关键边，异质边鉴别器抑制冗余噪声，两者协同带来稳定增益。
- 插入 HSADP 后，即便在传统半监督模型（HAN、HGT）上也能获得明显提升，证实其 **即插即用** 的解耦特性。

## 7. 优点

- **首次实现自监督、解耦、即插即用的异质图结构适配器**，无需改变下游模型架构。
- **双路径针对性设计**：分别处理边缺失和语义冗余，契合异质图特有的多关系结构复杂性。
- **充分的实验验证**：涵盖多个领域数据集、不同预训练范式（对比/生成）、少样本设置，并且有消融和敏感度分析。
- **同配性分析驱动设计**：通过实证观察同配比与分类性能的正相关，指导增强器的构建，增强了方法可解释性。

## 8. 不足与局限

- **实验局限**：仅验证了 **节点分类** 任务，未探索链接预测、节点聚类等其他下游任务上的泛化能力。
- **跨数据集与迁移**：方法目前只限于在 **单个数据集内部** 进行结构增强，未能处理跨数据集的零样本或迁移学习场景（作者在结论中也指出这是未来工作）。
- **计算开销未量化**：论文未给出适配器引入的额外时间、内存消耗，对实际落地时的性价比评估不足。
- **元路径依赖**：同质子图增强器需要预定义元路径才能提取语义子图，可能不适用于无明确元路径或元路径难以设计的场景。
- **超参数敏感**：实验表明性能对相似度阈值等超参数敏感，实际应用可能需要额外调参成本。

（完）
