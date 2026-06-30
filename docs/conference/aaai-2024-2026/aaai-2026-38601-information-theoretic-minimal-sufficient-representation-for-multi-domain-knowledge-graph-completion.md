---
title: Information-Theoretic Minimal Sufficient Representation for Multi-Domain Knowledge Graph Completion
title_zh: 面向多领域知识图谱补全的信息论最少充分表示
authors: "Jiawei Sheng, Taoyu Su, Weiyi Yang, Linghui Wang, Yongxiu Xu, Tingwen Liu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38601/42563"
tags: ["query:mt"]
score: 8.0
evidence: 基于信息论的最少充分表示学习用于多领域知识图谱补全
tldr: 针对多领域知识图谱补全中冗余信息掩盖任务相关信息的问题，提出IMKGC，利用信息论原则学习最少充分表示，在保留各领域上下文的同时去除冗余；在跨领域预测中取得了优异性能，为实现高效可扩展的多源知识图谱补全提供了新途径。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38601/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 857, \"height\": 288, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38601/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 843, \"height\": 297, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38601/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1636, \"height\": 570, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38601/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 851, \"height\": 329, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38601/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 864, \"height\": 530, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38601/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 847, \"height\": 257, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38601/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1848, \"height\": 585, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38601/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1845, \"height\": 591, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38601/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 864, \"height\": 445, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38601/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 873, \"height\": 384, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38601/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 874, \"height\": 262, \"label\": \"Table\"}]"
motivation: 现有方法融合多领域表示时受冗余信息影响，阻碍了可扩展预测。
method: 提出IMKGC框架，基于信息瓶颈原则，显式保留每个KG内的情境信息并压缩冗余。
result: 在多领域知识图谱补全任务上，优于现有基于融合的方法，尤其在KG数量增加时。
conclusion: 信息论最少充分表示为实现高效可扩展的多源知识图谱补全提供了新途径。
---

## Abstract
Multi-domain knowledge graph completion (MKGC) seeks to predict missing triples in a target KG by leveraging triples from multiple KGs in different domains (e.g., languages or sources). Existing studies typically learn and fuse multi-domain KG representations solely with alignments or fusion modules, which can be affected by redundant information within KGs. This issue can conceal task-relevant information in representations, impeding further improvements when scaling to numerous KGs. To this end, we propose IMKGC, an information-theoretic MKGC framework to learn minimal sufficient representations. In particular, IMKGC learns entity representations by explicitly preserving endogenous contextual information within each KG, exogenous complementary information from other KGs, and consistent information of equivalent entities, while suppressing redundant information through variational constraints. Furthermore, we achieve compressed relation representations with a devised relation reasoning decoder that captures relatedness among relations, also improving triple prediction. Extensive experiments on 14 KGs in three benchmark datasets demonstrate that IMKGC significantly outperforms previous state-of-the-art methods, especially in redundant scenarios.

---

## 论文详细总结（自动生成）

### 1. 核心问题与研究背景

- **任务定义**：多领域知识图谱补全（MKGC）旨在借助多个不同领域（如不同语言或来源）的知识图谱，预测某一目标图谱中的缺失三元组。
- **现有方法局限**：主流方法依靠实体对齐损失或注意力融合来传递跨图谱信息，但容易受图谱中冗余信息（如重复的平凡三元组）的污染。冗余信息会掩盖任务相关特征，在扩展至多个知识图谱时尤为严重。
- **信息论视角**：作者指出，神经网络倾向于学习超出任务所需的信息，即“过拟合虚假线索”。因此，从信息瓶颈（Information Bottleneck）角度看，需要学习对预测目标“最少但充分”的表示。

### 2. 方法论

- **整体框架 IMKGC**：基于信息瓶颈原则，约束表示保留三类关键信息（内生、外生、一致），同时压缩冗余。
- **三类关键信息**：
  - **内生信息**：当前图谱内与任务相关的上下文线索。
  - **外生信息**：其他图谱提供的互补知识。
  - **一致信息**：等价实体跨图谱的共享语义。
- **信息约束公式**：构建包含内生项 \(I(Z_i;Y_i)\)、外生项 \(I(Z_j;Y_i)\)、一致项 \(I(Z_j;Z_i)\) 及最小项 \(I(Z_j;X_j)\) 的损失函数，通过变分上下界实现可微分优化。
- **变分实体编码器**：采用关系图神经网络（ARGNN）编码实体邻居，并通过重参数化生成高斯变分表示 \(z\)。最终融合所有图谱的等价实体表示进行预测。
- **关系推理解码器（RRD）**：基于残差向量量化（RVQ），将关系分解为若干共享的“子关系”编码，以捕捉关系间语义联系，同时压缩关系表示。预测分数采用翻译模型 \(s_\phi(\overline{z}_h, r_q, \overline{z}_{t'}) = ||\overline{z}_h + r_q - \overline{z}_{t'}||_2\)。
- **训练目标**：联合最小化预测损失与四项信息约束（内生、外生、一致、最小），共同优化编码器、解码器及码本。

### 3. 实验设计

- **数据集**：三个基准数据集共14个知识图谱。
  - **DBP-5L**：5种语言（希腊语、英语、西班牙语、法语、日语）的DBpedia图谱。
  - **E-PKG**：6种语言（德语、英语、西班牙语、法语、意大利语、日语）的电子商务产品信息图谱。
  - **DWY**：多来源（DBpedia、YAGO、Wiki）的3个图谱。
  - 所有图谱均预先给定等价实体连接，关系统一。
- **对比方法**：
  - **单领域**：TransE、DistMult、RotatE、KG-BERT。
  - **多领域**：KEnS、CG-MuA、AlignKGC、SS-AGA、LSMGA、GLKGC。
  - **基于大模型（LLM）**：ChatGPT-3.5、KICGPT、MKGL。
- **评估指标**：Hits@1、Hits@10、MRR（平均倒数排名），以验证集平均MRR选择最优模型。

### 4. 资源与算力

- 论文未明确提及GPU型号、数量或训练时长。实现基于PyG框架，超参数经网格搜索调优，但无具体算力统计。

### 5. 实验充分性

- **主实验对比**：在三个数据集上对比了10余种基线，覆盖单领域、多领域、LLM方法，结果详尽。
- **消融实验**：对编码器（替换GCN）、关系解码器（去除RRD）以及各项信息约束（分别去除内生、外生、一致、全约束）进行了消融分析，验证各模块贡献。
- **敏感性与鲁棒性测试**：
  - 分析超参数（α, β, γ, ω）的影响。
  - 模拟冗余场景：逐步增加辅助图谱数量，观察性能变化。
  - 资源限制实验：随机保留20%、50%、80%的等价实体，评估强度。
  - 关系推理步骤数 K 的影响。
- **案例分析**：展示关系分解后共享子关系的语义相近性（如 friend–partner，senators–president）。
- 总体实验种类丰富、覆盖关键变量，对比公平，消融与敏感性实验充分验证了方法的有效性。

### 6. 主要结论与发现

- IMKGC在所有数据集上显著优于现有最优方法，如DBP-5L平均MRR提升3.8个百分点。
- 信息约束有效挖掘任务相关信息，抑制冗余：尤其是当辅助图谱数量增多时，现有方法性能停滞甚至下降，IMKGC持续提升。
- 在等价实体稀少时，IMKGC仍保持较强鲁棒性，证明其能抓住关键跨图谱信息。
- 关系推理解码器通过残差量化捕捉关系语义相关，从而提升预测准确度。

### 7. 优点

- **理论创新**：将信息瓶颈原则系统引入MKGC，从信息论角度定义和精炼最少充分表示，视角新颖。
- **方法完备**：同时优化实体表示（三元信息约束）和关系表示（压缩与分解），形成统一框架。
- **实验扎实**：覆盖多种语言和来源的14个图谱，消融与敏感性分析全面，验证了各模块的必要性。
- **鲁棒性强**：在冗余场景、资源受限场景下均表现出色，实用潜力大。

### 8. 不足与局限

- **计算开销未分析**：未提供模型参数规模、训练时间、显存占用等资源消耗数据，难以评估实际落地成本。
- **关系约束局限**：假定所有图谱使用统一关系模式，现实场景中不同领域的关系可能不兼容或需要更复杂的映射。
- **仅处理尾实体预测**：任务设置为给定头实体和关系预测尾实体，未扩展至头实体预测或更一般化的补全形式。
- **LLM结合有限**：仅简单对比LLM方法，未探索将IMKGC与大模型相结合的方式，可能限制了对开放世界知识的利用。
- **等价实体依赖**：仍需要预先对齐的等价实体，当种子对齐稀少或不可靠时，性能可能受限（尽管实验展示了较好的鲁棒性）。

（完）
