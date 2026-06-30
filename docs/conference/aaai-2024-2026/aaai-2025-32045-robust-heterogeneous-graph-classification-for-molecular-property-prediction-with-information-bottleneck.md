---
title: Robust Heterogeneous Graph Classification for Molecular Property Prediction with Information Bottleneck
title_zh: 基于信息瓶颈的异质图分类用于分子性质预测的鲁棒性研究
authors: "Zhibin Ni, Chang Liu, Hai Wan, Xibin Zhao"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/32045/34200"
tags: ["query:mt"]
score: 8.0
evidence: 使用异质图神经网络与信息瓶颈进行稳健的分子性质预测
tldr: 异构图神经网络在分子性质预测上表现优异，但易受对抗攻击。本文首次研究异质分子图的鲁棒表示学习，提出基于信息瓶颈的鲁棒异构图分类框架RHGC，通过提取信息最丰富且噪声最低的子图，显著增强了模型抗攻击能力，为安全敏感的医药与化工管理决策提供了可信赖的方法。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32045/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 880, \"height\": 834, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32045/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1846, \"height\": 694, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32045/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 836, \"height\": 455, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32045/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1829, \"height\": 664, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32045/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 854, \"height\": 609, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32045/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 846, \"height\": 259, \"label\": \"Table\"}]"
motivation: 异构图神经网络用于分子预测时缺乏对抗鲁棒性，此前未被研究。
method: 提出基于信息瓶颈的鲁棒异构图分类框架RHGC，识别最具信息且低噪的子结构。
result: RHGC在受到对抗攻击时预测性能下降幅度远小于普通HGNN。
conclusion: 提升鲁棒性对分子性质预测的工业应用至关重要，思想可迁移至管理领域的图数据风险分析。
---

## Abstract
Heterogeneous Graph Neural Networks (HGNNs) have achieved state-of-the-art performance in classifying molecular graphs, capitalizing on their ability to capture rich semantics. However, HGNNs for molecule property prediction exhibit significant susceptibility to adversarial attacks—a challenge that prior research has entirely overlooked. To fill this gap, this paper introduces the first study focused on robust graph-level representation learning tailored for heterogeneous molecular graphs. To achieve this goal, we propose a comprehensive Robust Heterogeneous Graph Classification (RHGC) framework grounded in the Information Bottleneck principle, which aims to identify the most informative and least noisy heterogeneous subgraphs to derive robust, holistic representations.  This is specifically accomplished through a dedicated Node Semantic Purifier, which enhances node-level and semantic-level robustness by eliminating label-irrelevant interference using graph stochastic attention and the Hilbert-Schmidt Independence Criterion, along with a Global Graph Disentanglement method, which improves graph-level robustness by addressing information leak. Experiments on three molecular benchmarks demonstrate that RHGC enhances accuracy by an average of 5.06% under all three attack settings and meanwhile by 4.33% on clean data.

---

## 论文详细总结（自动生成）

### 1. 论文核心问题与整体含义
- **研究背景**：异质图神经网络（HGNNs）在分子性质预测任务中表现突出，因为它能捕捉分子图丰富的异质语义（如不同原子类型与键型）。然而，这类模型极易受到对抗攻击的干扰，例如将醛中的羰基碳替换成其他原子会彻底改变分子性质，这种脆弱性此前完全未被研究。
- **核心问题**：HGNN 面临两类鲁棒性挑战：
  - **全局直接扰动影响更大**：异质分子图包含依赖异质性的隐式模式，结构扰动会同时破坏这些模式。
  - **局部中间扰动暴露更多漏洞**：基于元路径的 HGNN 将异质图拆分为多个中间同质图，攻击者可以针对单个中间图设计扰动，破坏语义互补性。
- **整体含义**：论文首次将分子性质预测重构为鲁棒异质图分类问题，填补了该领域对抗鲁棒性的研究空白，为药物发现等安全敏感场景提供可信赖的图级表示。

### 2. 方法论：Robust Heterogeneous Graph Classification (RHGC)
核心思想基于信息瓶颈（IB）原理，目标是识别信息最丰富且噪声最低的异质子图，得到鲁棒的整体表示。RHGC 包含两个模块：

#### 2.1 Node Semantic Purifier (NSP) —— 节点语义纯化器
- **节点级鲁棒性**：对每条元路径诱导的邻接矩阵，使用两个子图提取器（基于 GSAT 的图随机注意力）分别提取与标签相关（\(G_T\)）和无关（\(G_S\)）的子图。具体做法：对每条边 \((u,v)\)，拼接节点表示后映射到概率 \(p_{uv}\)，通过 Gumbel-Softmax 重参数化进行可微采样，得到边掩码 \(\alpha_T\)、\(\alpha_S\)，进而获得干净/噪声子图邻接矩阵 \(A_T, A_S\)。
- **语义级鲁棒性**：对不同元路径下的节点表示，利用 **希尔伯特-施密特独立性准则（HSIC）** 最小化它们之间的依赖，增强互补性。HSIC 目标为：
  \[
  \mathcal{L}_{\text{NSP}} = \sum_{i\neq j} \text{HSIC}(Z_T^{\Phi_i}, Z_T^{\Phi_j}) + \sum_{i\neq j} \text{HSIC}(Z_S^{\Phi_i}, Z_S^{\Phi_j})
  \]
  最后通过语义级注意力融合得到最终的 \(Z_T\) 与 \(Z_S\)。

#### 2.2 Global Graph Disentanglement (GGD) —— 全局图解耦
- **动机**：现有 IB 方法存在信息泄漏，依赖需要多轮调节的权衡超参数 \(\beta\)。
- **解耦图信息瓶颈目标**：
  \[
  \mathcal{L}_{\text{GGD}} = -I(G_T;Y) - I(G;G_S,Y) + I(G_S;G_T)
  \]
  该目标被分解为三个任务：
  - **任务 I**：最大化 \(I(G_T;Y)\)，保证 \(G_T\) 包含足够的标签相关信息，用分类损失 \(\mathcal{L}_{\text{clf}}\) 实现。
  - **任务 II**：最大化 \(I(G;G_S,Y)\)，保证 \((G_S,Y)\) 能覆盖图的完整信息，用图重建损失 \(\mathcal{L}_{\text{rec}}\)（GAE 风格）实现。
  - **任务 III**：最小化 \(I(G_S;G_T)\)，通过对抗训练实现。引入判别器 \(d\) 区分样本来自联合分布 \(p(G_S, G_T)\) 还是边缘分布乘积 \(p(G_S)p(G_T)\)，损失为：
    \[
    \mathcal{L}_{\text{dis}} = \mathbb{E}_{p(G_S)p(G_T)} \log d(G_S, G_T) + \mathbb{E}_{p(G_S, G_T)} \log(1 - d(G_S, G_T))
    \]
- 整体损失：\(\mathcal{L} = \lambda \mathcal{L}_{\text{NSP}} + \mathcal{L}_{\text{clf}} + \mathcal{L}_{\text{rec}} + \mathcal{L}_{\text{dis}}\)，通过对抗训练联合优化。

### 3. 实验设计
- **数据集**：三个分子/生物信息学基准数据集——OGB-molbace、MUTAG、PROTEINS。按 80%/10%/10% 划分训练/验证/测试集。
- **攻击设置**：
  - **非目标攻击**：
    - 结构攻击：选择度最高的节点中 20% 随机增/删一条边。
    - 特征攻击：对所有节点特征添加高斯噪声（幅度系数 \(m=2.0\)）。
  - **目标攻击（逃避攻击）**：基于 GRABNEL 的黑盒逃避攻击，扰动预算为每条图边数的 10%，允许翻转最多 \(\Delta\) 条边。
- **对比方法**：
  - 异质图分类方法：muxGNN、HeGCL。
  - 鲁棒同质图分类方法：数据增强类（NodeSam、G-Mixup）、图表示学习类（InfoGraph、MGRL）、IB 类（GIB、VIB-GSL、PGIB）。
- **评估指标**：分类准确率，报告 10 次不同随机种子运行的均值和标准差。

### 4. 资源与算力
论文仅在实现细节中提到使用 PyTorch 2.1 框架和 Ubuntu 22.04 操作系统，未说明 GPU 型号、数量、训练时长或显存消耗。计算成本信息缺失。

### 5. 实验数量与充分性
- **主实验**：3 个数据集 × 3 种场景（干净、结构攻击、特征攻击）共 9 组对比实验（表1），与 9 个基线方法比较；再加 3 个数据集的目标攻击实验（表2）。
- **消融实验**：对 3 个数据集分别测试 GSAT 基础模型、仅加 NSP、仅加 GGD、完整 RHGC 四个版本（表3）。
- **超参数分析**：对关键系数 \(\lambda\) 进行 5 个值的遍历实验（图3）。  
实验覆盖了不同攻击类型和多个数据集，采用随机种子多次运行，对比基线丰富，消融与分析较为完整，整体实验客观、公平且充分。

### 6. 主要结论与发现
- RHGC 在所有干净数据集上均超越所有基线（平均提升 4.33%）。
- 在非目标攻击下，RHGC 平均准确率仅下降 4.94%（基线下降 5.68%~8.12%），相对于基线平均提升 5.53% 的鲁棒准确率。
- 在目标逃避攻击下，RHGC 同样显著优于代表性基线（平均准确率超出 4.89%），验证了层次化鲁棒设计的有效性。
- 消融实验证明 NSP 和 GGD 两个模块各自均带来增益，组合后效果最佳。

### 7. 优点
- **首次探索**：首次将异质图分类的鲁棒性引入分子性质预测，问题定位清晰且针对性强。
- **全层次防护**：同时覆盖节点级、语义级和图级三个维度的鲁棒性，设计完整且有理论依据。
- **新目标函数**：提出解耦图信息瓶颈目标，克服了传统 IB 的信息泄漏与超参数依赖问题。
- **模块化与可解释性**：随机注意力+HSIC 的组合清晰约束了无关信息，模块职责明确。
- **实验扎实**：多个数据集、多种攻击、丰富基线，消融与参数分析齐全。

### 8. 不足与局限
- **攻击类型覆盖不全**：目标攻击仅考虑逃避攻击，未涉及投毒攻击（论文也承认该点）。
- **图重建器较简单**：采用了基础的 GAE 方案，未探索更适合异质图的先进重建技术。
- **计算资源未披露**：缺少对模型训练效率、GPU 开销的讨论，难以评估实际部署成本。
- **泛化性未知**：实验局限于三个较小规模的分子/蛋白质数据集，在其他领域异质图上的表现尚待验证。
- **超参数 λ 仍依赖经验**：虽进行了搜索，但最优值 0.1 可能随数据集变化，缺乏自适应机制。

（完）
