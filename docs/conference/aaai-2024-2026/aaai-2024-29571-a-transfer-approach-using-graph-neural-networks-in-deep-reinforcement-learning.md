---
title: A Transfer Approach Using Graph Neural Networks in Deep Reinforcement Learning
title_zh: 基于图神经网络的深度强化学习迁移方法
authors: "Tianpei Yang, Heng You, Jianye Hao, Yan Zheng, Matthew E. Taylor"
date: 2024-03-25
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/29571/30958"
tags: ["query:mt"]
score: 8.0
evidence: 图神经网络迁移学习用于强化学习顺序决策
tldr: 针对强化学习中状态-动作空间不匹配的多源迁移难题，提出TURRET方法，利用图神经网络泛化能力实现策略迁移，提高RL在新任务上的效率与通用性。实验表明，该方法在不同的状态-动作空间任务上均有效，为RL的跨任务泛化提供了新途径，在管理科学中的多场景决策迁移有应用潜力。TURRET通过将策略网络表示为图，利用GNN的消息传递机制实现跨状态-动作空间的策略复用，显著减少了新任务的训练时间，并在多个连续控制环境中验证了其优势。
source: AAAI-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29571/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 876, \"height\": 562, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29571/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1833, \"height\": 817, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29571/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1848, \"height\": 845, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29571/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 886, \"height\": 504, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29571/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1717, \"height\": 584, \"label\": \"Figure\"}]"
motivation: RL迁移学习通常要求状态-动作空间一致，限制了多源不同任务间的知识复用，亟需突破空间不匹配的迁移方法。
method: 提出TURRET，利用GNN将策略网络表示为图，通过消息传递实现不同状态-动作空间间的策略迁移。
result: 在多个连续控制环境中验证了TURRET的有效性，显著减少了新任务的训练时间并提升了泛化性能。
conclusion: TURRET为跨状态-动作空间的RL迁移提供了有效方案，在管理科学的多任务决策迁移中具有广阔应用前景。
---

## Abstract
Transfer learning (TL) has shown great potential to improve Reinforcement Learning (RL) efficiency by leveraging prior knowledge in new tasks. However, much of the existing TL research focuses on transferring knowledge between tasks that share the same state-action spaces.  Further, transfer from multiple source tasks that have different state-action spaces is more challenging and needs to be solved urgently to improve the generalization and practicality of the method in real-world scenarios. This paper proposes TURRET (Transfer Using gRaph neuRal nETworks), to utilize the generalization capabilities of Graph Neural Networks (GNNs) to facilitate efficient and effective multi-source policy transfer learning in the state-action mismatch setting.  TURRET learns a semantic representation by accounting for the intrinsic property of the agent through GNNs, which leads to a unified state embedding space for all tasks. As a result, TURRET achieves more efficient transfer with strong generalization ability between different tasks and can be easily combined with existing Deep RL algorithms. Experimental results show that TURRET significantly outperforms other TL methods on multiple continuous action control tasks, successfully transferring across robots with different state-action spaces.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：深度强化学习（DRL）虽然取得显著成功，但样本效率低下，需要大量环境交互。迁移学习（TL）是缓解这一问题的有效手段，但现有工作大多要求源任务与目标任务的**状态‑动作空间相同**，限制了在真实场景（如不同形态机器人）中的泛化能力。
- **多源跨域迁移挑战**：当多个源任务与目标任务的状态‑动作空间均不同时（跨域设定），如何有效提取并传递知识更具挑战，且现有方法或无法支持多源迁移，或难以应对源任务数量变化。
- **整体含义**：论文提出 **TURRET**（Transfer Using gRaph neuRal nETworks），利用图神经网络（GNN）的泛化能力，在多源跨域策略迁移中构建统一的状态嵌入空间，实现自适应、高效的知识传递。

### 2. 论文提出的方法论

- **核心思想**：将机器人形态建模为图，利用 GNN 的消息传递机制处理不同尺寸的输入输出，并通过注意力机制和集合变换器（set transformer）读取函数将所有任务的状态映射到统一嵌入空间，从而度量跨域状态相似性，指导多源策略迁移。

- **关键技术细节**：
  - **结构化策略网络（Structured Policy Network）**：
    - **输入模型** \(F_{\text{in}}\)：对每个关节节点的观测向量用 MLP 编码，得到初始节点表示。
    - **传播模型 \(P\)**：采用图注意力机制（GAT）计算相邻节点的权重系数 \(\alpha_{vu}^{k+1}\)，聚合邻居信息更新节点表示，缓解传统 MPNN 的信息损失。
    - **读取模型 \(F_{\text{read}}\)**：使用集合变换器将所有节点表示聚合为全局状态嵌入 \(S_{\text{emb}}\)，该嵌入处于跨任务统一的语义空间。
    - **输出模型 \(F_{\text{out}}\)**：基于 \(S_{\text{emb}}\) 预测所有关节的动作分布（高斯均值），标准差为可训练向量。
  - **自适应策略迁移**：
    - 计算当前状态在目标任务的表示 \(S_{\text{emb}}\) 与在各源任务中得到的表示 \(S_{\text{emb}}^i\) 的欧氏距离，通过 Softmax 生成权重 \(\omega_i\)（距离越小，权重越大）。
    - 在目标网络的隐藏层进行横向连接：\(z_j^\pi = p \, z_j + (1-p) \sum_{i=1}^N \omega_i z_j^i\)，其中 \(p\) 随时间增加，逐步减少对源策略的依赖。
    - 整个框架无需额外优化目标，可直接与 PPO 等现有算法结合。

### 3. 实验设计

- **环境与数据集**：MuJoCo 连续控制任务，包括标准任务（Walker2d, Ant, Humanoid, HalfCheetah）和具有多关节的 **Centipede‑n** 任务（n = 4, 6, 8, 12, 16, 20）。
- **基准方法对比**：
  - 从头训练的 **PPO**；
  - 多源跨域迁移方法 **CAT**；
  - 基于 GNN 的 **NerveNet** 及其扩展 **Snowflake**，以及它们的微调版本（用源模型权重初始化）；
  - 基于 Transformer 的多任务策略 **SWAT**。
- **实验类型**：
  - **大小迁移**：源任务为 Centipede‑4 和 Centipede‑6，目标为 Centipede‑{12, 16, 20}。
  - **形态迁移**：如 {Hopper & Centipede‑4 → Walker2d}、{HalfCheetah & Ant → Centipede‑8}、{HalfCheetah & Ant → Humanoid}。
  - **可视化分析**：通过 t‑SNE 将轨迹投影至三维空间，计算源‑目标轨迹间欧氏距离，验证嵌入空间对齐效果。
  - **消融实验**：去除注意力机制（TURRET w/o attention）和改用平均性能作为相似性度量（TURRET w/o distance）。

### 4. 资源与算力

- 论文未明确提及使用的 GPU 型号、数量及具体训练时长。
- 仅说明每个算法运行 5 个随机种子，每个种子在目标环境中交互 1000 万步（10 million timesteps）。
- 模型网络结构与参数细节可在附录中找到，但正文未直接描述算力开销。

### 5. 实验数量与充分性

- **实验组数**：涵盖 3 种大小迁移、3 种形态迁移组合、1 组消融实验（2 个变体）以及定性可视化分析，总计约 10 组以上独立实验配置。
- **充分性**：实验覆盖了不同关节数、不同形态的机器人，并对比了多种代表性方法（传统 RL、跨域迁移、GNN 策略）。每个实验运行 5 个种子并报告标准差，保证了统计稳定性。消融实验验证了两个关键设计的有效性。因此实验设计较为全面和客观。
- **潜在不足**：源任务数量固定为 2（作者提及为方便和计算复杂度），未展示更多源任务（>2）的结果，附录虽有提及但正文未展开；目标环境交互步数上限为 1000 万步，可能不足以完全收敛更大的智能体。

### 6. 论文的主要结论与发现

- TURRET 在所有测试环境中均显著优于对比方法，尤其是当目标任务状态‑动作空间极大或形态差异显著时（如 Centipede‑20、Humanoid）。
- 注意力机制有效减少了 GNN 聚合过程的信息损失，提升了大规模智能体的训练和迁移性能。
- 基于统一嵌入空间的状态距离度量能更精细、自适应地衡量各源策略在当前状态下的迁移价值，优于基于平均性能的权重分配方式。
- 可视化分析证实结构化策略网络确实学到了跨任务的语义共性，缩小了源‑目标映射距离。

### 7. 优点

- **首创性**：首次将图注意力与集合变换器读取结合，构建跨域统一嵌入空间，解决多源跨域策略迁移的状态空间不匹配问题。
- **自适应迁移**：状态级别的相似性度量使迁移更精准，随训练进程逐渐独立，避免负迁移。
- **即插即用**：无需修改已有 DRL 算法的损失函数，易于集成。
- **实验全面**：覆盖大小、形态迁移等多类型任务，对比方法丰富，消融和可视化分析增强可解释性。

### 8. 不足与局限

- **源任务数量固定**：文中实验主要使用两个源任务，扩展到更多源任务时的性能与效率未充分验证。
- **目标相似性假设**：方法假设不同任务的目标相似，状态相似性即能反映迁移价值；当前方法未处理目标空间不同的场景（如开门 vs 按按钮），需引入额外度量。
- **状态结构依赖**：要求状态由明确可分离的关节观测构成（结构化信息），对非结构化或含噪声、无关特征的状态输入适应性不足，需结合表示学习或自动结构发现。
- **算力与收敛性**：对大规模智能体（如 Centipede‑20）仍可能欠收敛，且未报告计算开销，实际部署成本未知。
- **仅连续控制验证**：实验局限于 MuJoCo 连续控制任务，未在离散动作环境或其他领域（如自动驾驶、推荐系统）中测试。

（完）
