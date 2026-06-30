---
title: Attribute-guided Dynamic Prompt Learning for Graph Neural Networks
title_zh: 面向图神经网络的属性引导动态提示学习方法
authors: "Zhuomin Liang, Liang Bai, Xian Yang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39515/43476"
tags: ["query:mt"]
score: 8.0
evidence: 属性引导的动态提示学习用于提升图神经网络在图结构扰动下的鲁棒性
tldr: 图神经网络在训练图结构与测试图结构不一致时性能大幅下降，现有方法改进鲁棒性但忽略表征偏移。为此提出属性引导的动态提示学习模型，通过生成提示向量近似节点内在信息，使训练好的GNN在结构扰动下仍能保持性能，增强了对图结构变化的容忍度，为图学习在不可靠图数据上的应用提供了新途径。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39515/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 872, \"height\": 266, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39515/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 880, \"height\": 334, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39515/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 870, \"height\": 399, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39515/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1576, \"height\": 623, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39515/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 866, \"height\": 784, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39515/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 870, \"height\": 445, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39515/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 869, \"height\": 429, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39515/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1614, \"height\": 1168, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39515/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 828, \"height\": 615, \"label\": \"Table\"}]"
motivation: 图神经网络在扰动图上性能骤降，现有方法忽视结构变化导致的表征偏移。
method: 利用节点属性动态生成提示向量，在推理阶段注入GNN以补偿结构偏移。
result: 在多个图结构扰动场景下，提示学习显著提升了节点分类和链接预测的鲁棒性。
conclusion: 动态提示可作为一种轻量级即插即用模块，有效提升GNN在图结构变化时的泛化能力。
---

## Abstract
Graph Neural Networks (GNNs) have achieved remarkable success in analyzing graph-structured data, with their performance dependent on the graph structure. However, models trained on high-quality graph structures often suffer a significant performance drop when evaluated on perturbed graphs. Existing methods tackle this problem by improving the robustness of GNNs, but they often overlook representation deviation caused by structural changes. To address this limitation, we propose an attribute-guided dynamic prompt learning model that generates prompt vectors to approximate the intrinsic information of nodes. With these prompt vectors, the trained GNNs are expected to maintain their performance under perturbed graph structures. Unlike previous prompt-based methods that learn unified prompt vectors for all nodes, we obtain node-level prompts by encoding node attributes that provide unique information. Given the diversity of perturbed graph structures during inference, we introduce a structure-aware adaptation mechanism that adjusts the prompt vectors based on the input graph. Furthermore, we apply gradient-based attacks to generate perturbed graphs, encouraging the model to generalize to unseen structures. Extensive experiments on multiple benchmark datasets demonstrate the effectiveness and robustness of our model.

---

## 论文详细总结（自动生成）

好的，请查收根据您提供的论文内容生成的结构化中文总结。

### 1. 论文的核心问题与整体含义

*   **核心问题**：图神经网络（GNNs）的性能高度依赖图结构的质量。然而，在推理阶段，图结构常因动态环境或对抗攻击等发生未知且不可预测的扰动（如边增加或删除），导致**结构退化**。这会使训练好的GNN性能显著下降，其根源是结构变化引起了**表征偏移**——节点特征向其错误类别的中心漂移。
*   **整体含义**：现有方法主要关注提升GNN自身的鲁棒性（如重训练或图结构重构），但忽视了表征偏移问题。本文旨在解决这一局限，提出一种即插即用的方法，通过**属性引导的动态提示学习**，在不修改预训练GNN参数的前提下，补偿因结构扰动造成的信息损失，维持模型在扰动图上的性能。

### 2. 论文提出的方法论

*   **核心思想**：借鉴提示学习的思想，为每个节点学习一个独特的**提示向量**，并将其注入到节点的原始属性中。该提示向量旨在近似节点因局部结构缺失而损失的内在信息，从而引导冻结的GNN在扰动图上做出与原始图一致的预测。
*   **关键技术细节（AttrPrompt模型）**：
    *   **提示学习模块**：为解决节点属性高维稀疏难以捕捉局部结构模式的问题，首先基于节点属性构建一个**K近邻图**。然后，使用一个GNN编码器在此KNN图上生成固定的、包含上下文信息的节点级提示向量P。
    *   **提示适配模块**：为使固定的提示向量能适应多样化的结构扰动，引入一个结构感知的适配机制。该模块通过一个线性变换，从扰动后的邻接矩阵中学习一个缩放因子γ，并以元素级乘法的方式动态调整提示向量，生成个性化提示向量 ˆP。
    *   **对抗图扰动**：为训练模型泛化到最坏情况，采用基于梯度的攻击方法生成扰动图。具体来说，通过最大化原始图和扰动图预测结果之间的KL散度损失，利用快速梯度符号法更新一个可微的扰动矩阵∆，进而生成对抗性扰动邻接矩阵。
    *   **损失函数**：
        *   **KL散度损失**：对齐模型在原始图和扰动图上的soft预测结果，确保预测一致性。
        *   **互信息损失**：使用CLUB方法估算并最小化提示向量ˆP与扰动图G'之间的互信息上界，鼓励提示向量学习与图结构互补而非冗余的信息。
*   **算法流程**：整个训练过程交替进行扰动图的更新和模型参数的优化。

### 3. 实验设计

*   **数据集**：在四个同配性真实世界基准数据集上进行实验——**Cora、Citeseer、Amazon-Computer、Amazon-Photo**。
*   **任务与场景**：**半监督节点分类任务**。模型在原始图结构上训练，在模拟结构退化的**扰动图**上测试，具体包括随机增加50%/75%的边和随机删除20%/50%的边。
*   **对比方法**：与五种专门设计用于提升GNN鲁棒性的先进方法进行了比较——**EERM、CITGCN、StruRW、CaNet和GOOD-AT**。实验使用了GCN作为基础GNN模型，并在部分实验中验证了在APPNP上的泛化性。

### 4. 资源与算力

*   论文中未明确提及所使用的GPU型号、数量或具体的模型训练时长。

### 5. 实验数量与充分性

*   **实验数量**：实验设计较为全面，主要包括约24组主实验。具体为4个数据集 × 4种扰动设置 × 约6种对比方法的节点分类精度评估。此外，还包括提示方法对比实验、消融实验、表征一致性分析、在APPNP上的泛化性实验和参数敏感性分析等多个维度的实验。
*   **充分性与客观性**：实验覆盖了多种数据集、扰动类型和扰动率，并与多个最新的代表性方法进行了比较，结果报告了多次运行的均值和标准差，整体设计是**充分、客观且公平的**。

### 6. 论文的主要结论与发现

*   **性能显著提升**：AttrPrompt在所有数据集和不同扰动设置下，一致性地取得了**最佳或次优**的性能，尤其在边删除场景下表现更佳，因为节点属性可以有效补偿缺失的结构信息。
*   **模块有效性验证**：消融实验证明，提示适配模块和互信息损失对提升模型鲁棒性至关重要，完整的AttrPrompt模型效果最优。
*   **表征一致性增强**：通过分析余弦相似度，发现AttPrompt生成的提示向量能使模型在扰动图上产生的节点表征与原始表征高度一致，有效缓解了表征偏移问题。
*   **架构泛化能力**：基于APPNP的实验表明，该方法可以轻松适配不同的GNN架构，具有良好的泛化性。

### 7. 优点

*   **创新性强**：首次将提示学习范式应用于解决GNN的图结构鲁棒性问题，视角新颖。
*   **即插即用**：方法不改变预训练GNN的参数，冻结主干网络，参数效率高，部署灵活。
*   **动态自适应**：提示向量是节点级的，并能根据输入的扰动图结构动态调整，比统一或静态的提示方法更具适应性和个性化。
*   **理论基础扎实**：模型设计有清晰的动机和理论支持，例如引入互信息正则化让提示学习到与结构互补的信息。

### 8. 不足与局限

*   **对抗训练成本**：引入对抗图扰动来模拟最坏情况，可能在训练过程中引入了额外的计算开销。
*   **图类型限制**：研究聚焦于**同配性图**。对于节点与非同配邻居交互的异配性图，该方法是否有效尚待探索。
*   **扰动类型**：对抗训练主要面向全局随机扰动或基于梯度的对抗攻击。对于更复杂的、符合特定分布规律的图结构漂移，其鲁棒性有待进一步验证。

（完）
