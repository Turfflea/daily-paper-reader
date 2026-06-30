---
title: "GraphOracle: Efficient Fully-Inductive Knowledge Graph Reasoning via Relation-Dependency Graphs"
title_zh: GraphOracle：基于关系依赖图的高效全归纳知识图谱推理
authors: "Enjun Du, Siyi Liu, Yongqi Zhang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38978/42940"
tags: ["query:mt"]
score: 8.0
evidence: 图神经网络用于全归纳知识图谱推理，关系数据建模
tldr: 针对知识图谱全归纳推理中测试时实体和关系均不可见的挑战，提出GraphOracle框架。将知识图谱转化为关系依赖图，利用多头注意力传播关系信息，再通过GNN进行推理。实验表明该方法在全归纳设置下显著优于现有方法，推动了知识图谱推理的泛化能力。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38978/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1473, \"height\": 590, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38978/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 838, \"height\": 416, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38978/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1298, \"height\": 471, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38978/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 833, \"height\": 385, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38978/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 863, \"height\": 457, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38978/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1537, \"height\": 297, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38978/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1726, \"height\": 358, \"label\": \"Table\"}]"
motivation: 全归纳场景下，测试时实体和关系均未知，传统方法难以泛化。
method: 构建关系依赖图编码关系间顺序模式，以查询关系为条件，通过多头注意力和第二个GNN进行推理。
result: 在全归纳知识图谱推理基准上达到最优性能，泛化能力强。
conclusion: GraphOracle为全归纳知识图谱推理提供了高效方案，可应用于动态关系数据。
---

## Abstract
Knowledge graph reasoning in the fully-inductive setting—where both entities and relations at test time are unseen during training—remains an open challenge. In this work, we introduce GraphOracle, a novel framework that achieves robust fully-inductive reasoning by transforming each knowledge graph into a Relation-Dependency Graph (RDG). The RDG encodes directed precedence links between relations, capturing essential compositional patterns while drastically reducing graph density. Conditioned on a query relation, a multi-head attention mechanism propagates information over the RDG to produce context-aware relation embeddings. These embeddings then guide a second GNN to perform inductive message passing over the original knowledge graph, enabling prediction on entirely new entities and relations. Comprehensive experiments on 60 benchmarks demonstrate that GraphOracle outperforms prior methods by up to 25% in fully-inductive and 28% in cross-domain scenarios. Our analysis further confirms that the compact RDG structure and attention-based propagation are key to efficient and accurate generalization

---

## 论文详细总结（自动生成）

## 1. 研究背景与核心问题
- **全归纳知识图谱推理**：测试时出现的实体与关系在训练阶段完全不可见，这要求模型具备零样本的泛化能力。
- **现有方法的瓶颈**：
  - 基于关系图的方法（如 INGRAM、ULTRA）构建的关系图过于密集（边数约 O(|R|²)），计算开销大（O(|R|³·L)），且忽略关系间的方向性，引入噪声连接。
  - 多数方法为每个关系学习一个固定嵌入，无法根据查询上下文动态调整语义，难以适应“同一关系在不同语境下含义不同”的场景。
- **研究目标**：设计一个既能捕获关系间有向组合依赖，又保持计算高效，并支持动态关系表示的全归纳推理框架。

## 2. 方法论
### 2.1 关系依赖图（RDG）构造
- **核心变换**：将知识图谱 G = (V, R, F) 转换为关系依赖图 G_R = (R, E_R)。
- **边提取**：通过实体中介的路径定义两个连续关系 ri → rj 的有向边：(e, ri, e′) ∧ (e′, rj, e″) ⇒ (ri, rj)。
- **偏序关系**：引入函数 τ(r) 为每个关系赋予层级位置，形成有向无环结构，仅保留前驱关系 N_past(rv) = {ru | (ru, rv) ∈ E_R 且 τ(ru) < τ(rv)}，从而大幅减少边数并突出组合依赖。

### 2.2 查询条件的关系表示学习
- **初始化**：用指示向量 h⁰_{rv|rq} 标记查询关系 rq。
- **多头注意力消息传递**（Lr 层）：
  - 每个注意力头对前驱邻居的消息进行加权聚合。
  - 注意力权重 ˆα^{ℓ,h}_{ru rv} 通过可学习的投影和参数向量 a 计算，显式建模 ru 对 rv 的方向性影响。
- **输出**：得到上下文相关的关系嵌入 Rq = {h^{Lr}_{r|rq}}，同一个关系在不同 rq 下可产生不同表示。

### 2.3 原始 KG 上的实体表示与预测
- **实体消息传递**：利用上述动态关系嵌入，在原始 KG 上执行 Le 层递归聚合。
- **实体更新**：
  - h^ℓ_{e|q} 由前一层的邻居实体嵌入和对应的关系嵌入拼接后，经注意力加权求和，再通过激活函数更新。
  - 注意力系数 α 基于当前实体、当前关系以及查询关系的嵌入计算。
- **最终得分**：取最后一层实体表示 h^{Le}_{e|q}，通过线性映射得到三元组得分，使用对比损失进行训练。

### 2.4 训练范式
- **预训练-微调**：先在三个通用 KG（NELL-995、CoDEx-Medium、FB15k-237）上多数据集交替预训练，再对目标 KG 进行极少 epoch（1~2 轮）的轻量微调，或直接零样本推理。
- **损失函数**：标准对比目标，最大化正样本概率，最小化负样本概率。

## 3. 实验设计
### 3.1 数据集与场景
- **共 60 个基准**，分为三类：
  - 传导与归纳推理：包含 16 个传导、18 个实体归纳、23 个全归纳数据集，沿用 ULTRA/TRIX/KG-ICL 的设置。
  - 跨域推理：生物医学 PrimeKG（蛋白质、药物、疾病预测）、推荐系统 Amazon-book（用户-物品）、地理知识图谱 GeoKG。

### 3.2 对比方法
- 覆盖全系列 SOTA：
  - 传导：ConvE, QuatE, DuASE, BioBRIDGE
  - 实体归纳：MINERVA, DRUM, AnyBURL, RNNLogic, RLogic, GraphRulRL, CompGCN, NBFNet, RED-GNN, A*Net, Adaprop, one-shot-subgraph
  - 全归纳：INGRAM, ULTRA, TRIX, KG-ICL
- 评价指标：平均倒数排名（MRR）、Hits@1、Hits@10。

## 4. 资源与算力
- **GPU**：单张 NVIDIA A6000（48 GB 显存）。
- **训练配置**：
  - 预训练步数：150,000 步，batch size 32，优化器 AdamW。
  - 预训练耗时：约 36 小时。
  - 微调耗时：15~60 分钟（取决于目标数据集）。
- 明确提供了算力开销，易于复现。

## 5. 实验数量与充分性
- **主实验**：在 60 个数据集上的全面对比，给出平均相对提升（全归纳 H@1 提升高达 25.36%）。
- **消融实验**：
  - 移除 RDG 或减少注意力头（H=1）的性能退化。
  - 替换为 INGRAM/ULTRA 的关系图构造或消息传递机制。
  - 预训练数据集数量对零样本性能的影响（1~6 个数据集，性能在 3 个时饱和）。
- **分析实验**：
  - 根据注意力权重移除高/低重要性关系边，验证组合依赖的关键作用。
  - 引入外部基础模型嵌入的增强版本 GraphOracle+，在 PrimeKG 上测试额外信息增益。
- 实验设计系统、完整，对比公平，多种角度的消融与分析确保了结论的可靠性。

## 6. 主要结论与发现
- GraphOracle 在所有推理设定中一致超越现有 SOTA，在全归纳和跨域场景提升尤为显著（平均 MRR 提升 13.28%，H@1 提升 25.36%）。
- 稀疏有向的 RDG 结合多头注意力，是高效泛化和准确推理的关键。
- 预训练三个异构 KG 后，零样本性能趋于饱和，表明模型已习得丰富的通用关系模式。
- 模型能有效利用外部信息（如基础模型嵌入），在生物医学等专业领域进一步提升性能。

## 7. 优点
- **创新性强**：首次将 KG 转换为有向的、基于偏序的关系依赖图，显式建模组合方向。
- **动态关系表示**：查询条件的关系嵌入使同一关系具备上下文感知能力。
- **高效与可扩展**：RDG 边数远少于全连接图，计算复杂度显著降低。
- **预训练-微调范式**：在多个 KG 上预训练，仅需极少步数微调即可适应新域，甚至支持零样本推理。
- **实验扎实**：60 个基准、多类对比、深入消融和扰动分析，结论可信度高。

## 8. 不足与局限
- **偏序定义模糊**：文中仅提及基于拓扑分析和共现模式，未给出 τ(r) 的具体计算算法，复现时可能存在歧义。
- **跨域微调依赖**：虽然微调仅需 1~2 epoch，但仍需要目标域训练集，并非完全的零样本无需调参。
- **评价指标单一**：主要使用 MRR 和 Hits，未涉及校准度、可解释性或链路预测的置信度评估。
- **未讨论大图扩展挑战**：虽声称有复杂度分析（附录），但正文未呈现在超大规模 KG（如亿级三元组）上的实际表现。
- **外部信息增益有限**：GraphOracle+ 依赖额外的预训练基础模型，增加了系统复杂度和资源需求。

（完）
