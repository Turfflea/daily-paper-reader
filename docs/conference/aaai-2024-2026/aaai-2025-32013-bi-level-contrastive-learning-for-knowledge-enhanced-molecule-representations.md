---
title: Bi-level Contrastive Learning for Knowledge-Enhanced Molecule Representations
title_zh: 面向知识增强的分子表示的双层对比学习
authors: "Pengcheng Jiang, Cao Xiao, Tianfan Fu, Parminder Bhatia, Taha Kass-Hout, Jimeng Sun, Jiawei Han"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/32013/34168"
tags: ["query:mt"]
score: 9.0
evidence: 双层对比学习用于知识增强的分子表示
tldr: 现有GNN方法难以捕捉分子的完整复杂性，未能充分利用分子在知识图谱中的角色。本文提出Gode，一种双层对比学习方法，通过预训练两个GNN分别编码分子图和知识图谱，并设计双层对比目标进行联合学习。在分子性质预测和副作用预测任务上，Gode优于现有方法，表明双层结构有效融合多源生化知识，提升了分子表示的泛化能力。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32013/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1839, \"height\": 742, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32013/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1174, \"height\": 456, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32013/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 860, \"height\": 607, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32013/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1819, \"height\": 380, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32013/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1836, \"height\": 611, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32013/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1832, \"height\": 544, \"label\": \"Table\"}]"
motivation: 现有GNN方法难以捕捉分子的完整复杂性，未能充分利用分子在知识图谱中的角色。
method: 提出Gode，通过预训练两个GNN分别编码分子图和知识图谱，并设计双层对比目标进行联合学习。
result: 在分子性质预测和副作用预测任务上优于现有方法。
conclusion: 双层结构有效融合多源生化知识，提升了分子表示的泛化能力。
---

## Abstract
Molecular representation learning is vital for various downstream applications, including the analysis and prediction of molecular properties and side effects. While Graph Neural Networks (GNNs) have been a popular framework for modeling molecular data, they often struggle to capture the full complexity of molecular representations. In this paper, we introduce a novel method called Gode, which accounts for the dual-level structure inherent in molecules. Molecules possess an intrinsic graph structure and simultaneously function as nodes within a broader molecular knowledge graph. Gode integrates individual molecular graph representations with multi-domain biochemical data from knowledge graphs. By pre-training two GNNs on different graph structures and employing contrastive learning, Gode effectively fuses molecular structures with their corresponding knowledge graph substructures. This fusion yields a more robust and informative representation, enhancing molecular property predictions by leveraging both chemical and biological information. When fine-tuned across 11 chemical property tasks, our model significantly outperforms existing benchmarks, achieving an average ROC-AUC improvement of 12.7% for classification tasks and an average RMSE/MAE improvement of 34.4% for regression tasks. Notably, Gode surpasses the current leading model in property prediction, with advancements of 2.2% in classification and 7.2% in regression tasks.

---

## 论文详细总结（自动生成）

## 论文核心问题与整体含义  
- 分子表示学习对预测分子性质、副作用等下游任务至关重要。  
- 图神经网络（GNN）虽被广泛用于分子建模，但常难以捕捉分子表示的完整复杂性。  
- 分子具有双重结构：既是一个由原子与键构成的图，又作为一个节点存在于更大的生物化学知识图谱（KG）中。传统方法往往只关注分子图本身，忽略了分子在KG中的上下文关联。  
- 本文旨在将**分子图**与**以该分子为中心的知识图谱子图**进行融合，从而获得更丰富、更鲁棒的分子表示，提升性质预测性能。

## 方法论  
### 整体框架：GODE（Graph as a Node）  
- 将分子视为“图中的图”，设计**双级自监督预训练 + 对比学习**的框架。  
- 四个阶段：  
  1. **分子级预训练（M‑GNN）**  
     - 在1100万未标记分子图上预训练一个GNN（基于GROVER）。  
     - 自监督任务：节点级上下文性质预测（多分类）和图级功能团基序预测（多标签分类）。损失函数为两者的联合。  
  2. **KG级预训练（K‑GNN）**  
     - 为每个分子提取以该分子为中心的κ‑hop知识图谱子图（来自新构建的MolKG）。  
     - 使用另一个GNN（GINE）对该子图进行预训练，初始化时使用TransE得到的实体和关系嵌入。  
     - 自监督任务：边类型预测（多分类）、节点类别预测（多分类）、中心分子的功能团基序预测（多标签分类）。联合损失函数。
  3. **对比学习**  
     - 将同一分子的分子图（来自M‑GNN）和KG子图（来自K‑GNN）作为正样本对，其他分子的作为负样本。  
     - 负样本包括随机采样的子图和邻居分子子图。  
     - 采用InfoNCE损失最大化正样本对的相似度，实现跨模态知识对齐。  
  4. **下游任务微调**  
     - 拼接三个向量：微调后M‑GNN的分子嵌入、RDKit提取的额外分子特征、冻结的K‑GNN分子嵌入。  
     - 输入MLP预测目标分子性质（分类或回归）。  
### 关键公式与符号  
- 分子图 \(G_m = (V_m, E_m)\)，KG \(G_k = (V_k, E_k)\)。  
- M‑GNN输出 \(h_{MG}\)，K‑GNN输出 \(h_{KG}\)。  
- 对比损失：\(L_{InfoNCE} = -\frac{1}{N} \sum_{i=1}^N [y_i \log(\text{sim}(f(m_i), g(s_i))) + (1-y_i)\log(1-\text{sim}(...))]\)。  
- 最终联合表示：\(h_{joint} = h_{MG} \oplus h_f \oplus h_{KG}\)。

## 实验设计  
- **数据集**  
  - 下游任务：MoleculeNet中的6个分类数据集（BBBP、SIDER、ClinTox、BACE、Tox21、ToxCast）和5个回归数据集（FreeSolv、ESOL、Lipophilicity、QM7、QM8）。  
  - 分子级预训练数据：ZINC15 + ChEMBL（约1100万分子）。  
  - KG级预训练数据：自建MolKG（源自PubChemRDF和PrimeKG，含约6.5万分子、2.5M三元组、7类实体、39种关系）。  
- **基准方法**  
  - 传统GNN：GCN、GIN、SchNet、MPNN、DMPNN、MGCN。  
  - 先进基线：N‑GRAM、Hu et al.、GROVER、MGSSL、MolCLR、KGE NFM（结合MolKG）、知识增强模型KANO。  
- **评估设置**  
  - 分类任务指标：ROC‑AUC；回归任务指标：RMSE（FreeSolv、ESOL、Lipophilicity）或MAE（QM7、QM8）。  
  - 划分方式：scaffold splitting，train/valid/test = 8:1:1，3个随机种子取平均值和标准差。

## 资源与算力  
- 文中明确提及硬件配置：  
  - 2× AMD EPYC 7513 32‑Core CPU  
  - 528 GB RAM  
  - 8× NVIDIA A6000 GPU  
  - CUDA 11.7  
- 未提供训练总时长或单次实验耗时。

## 实验数量与充分性  
- **大量系统性实验**：  
  - 在11个分子性质预测任务上与十余种基线对比，展示主结果（Table 2）。  
  - 针对不同模块的消融实验（Figure 2），对比了9种配置（如是否使用KG嵌入、KG预训练、对比学习、不同嵌入组合等）。  
  - 进一步分析了双级预训练各自的作用（Table 3a）。  
  - 研究了剔除MolKG中特定关系对性能的影响（Table 3b）。  
  - 提供了t‑SNE嵌入可视化，用Davies–Bouldin指数定量评估聚类质量（Figure 3）。  
- **实验设计与评估方式较为客观、公平**：采用标准基准、scaffold split多随机种子，并与SOTA模型（KANO）直接比较。消融实验覆盖关键组件，结论可信。

## 论文的主要结论与发现  
- GODE在所有分类任务和4/5个回归任务上取得最优性能，相比基线平均提升幅度显著（分类12.7%，回归34.4%），并在分类和回归上分别超出SOTA模型KANO 2.2%和7.2%。  
- KG集成、KG级预训练和对比学习均对性能有正向贡献，且三者叠加效果最优。  
- 对比学习能有效将KG知识迁移至分子图表示，仅用增强后的M‑GNN嵌入（不显式拼接KG嵌入）已大幅优于纯分子图模型。  
- MolKG中特定关系（如xlogp3相关、互变异构计数等）对特定性质预测影响显著。  
- 嵌入可视化显示GODE产生的分子表示聚类更清晰，不同scaffold间重叠更少。

## 优点  
- **创新范式**：首次将分子建模为“图中的图”，同时利用分子结构和其在知识图谱中的上下文，方法通用性强。  
- **有效的多源知识融合**：双级自监督预训练 + 对比学习的组合有效融合了化学结构与生物化学知识，显著提升表示鲁棒性。  
- **构建了专用KG**：MolKG直接整合PubChemRDF和PrimeKG，为分子知识增强提供了数据基础。  
- **实验扎实**：在多个标准基准上与大量基线对比，并进行了详尽的消融和关系贡献分析，结果具有说服力。  
- **可视化支撑**：通过t‑SNE直观展示了表示质量的提升。

## 不足与局限  
- **计算开销不透明**：虽列出硬件资源，但未给出训练时间或模型效率指标，难以评估实际应用成本。  
- **图谱规模限制**：MolKG仅包含约6.5万分子，对于更大规模分子库的覆盖能力未知，可能限制对罕见分子的表示效果。  
- **对KG质量的依赖**：性能受MolKG构建质量和覆盖范围影响较大，扩展到其他KG或领域时需重新处理。  
- **负采样策略**：对比学习中随机邻居负采样的有效性未作深入分析，可能对难负样本敏感。  
- **下游任务单一**：仅在分子性质预测上验证，未探索反应预测、药物‑靶标相互作用等其他重要任务。  
- **可解释性不足**：未分析模型是否真正学到了有化学意义的子图，决策过程仍然黑箱。

（完）
