---
title: Conditional Diffusion Model for Multi-Agent Dynamic Task Decomposition
title_zh: 面向多智能体动态任务分解的条件扩散模型
authors: "Yanda Zhu, Yuanyang Zhu, Daoyi Dong, Caihua Chen, Chunlin Chen"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40158/44119"
tags: ["query:mt"]
score: 8.0
evidence: 用于多智能体动态任务分解的条件扩散模型
tldr: 在部分可观测环境下，从头学习动态任务分解需要大量样本，探索联合动作空间困难。本文提出CD3T，一种两层分层多智能体强化学习框架，高层学习子任务表示并基于子任务效果生成选择策略，利用条件扩散模型捕捉效果。在复杂多智能体长时域任务中，该方法提高了学习效率和协作性能，为层次化MARL提供了新途径。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40158/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1825, \"height\": 722, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40158/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 856, \"height\": 441, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40158/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1837, \"height\": 738, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40158/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1843, \"height\": 1065, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40158/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1664, \"height\": 456, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40158/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1778, \"height\": 516, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40158/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 875, \"height\": 580, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40158/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1830, \"height\": 569, \"label\": \"Figure\"}]"
motivation: 在部分可观测环境下，从头学习动态任务分解需要大量样本，探索联合动作空间困难。
method: 设计两层分层框架，高层学习子任务表示并基于子任务效果生成选择策略，利用条件扩散模型捕捉效果。
result: 在复杂多智能体长时域任务中，相比现有方法提高了学习效率和协作性能。
conclusion: 提出的条件扩散模型有效建模子任务效果，促进了动态任务分解与协调。
---

## Abstract
Task decomposition has shown promise in complex cooperative multi-agent reinforcement learning (MARL) tasks, which enables efficient hierarchical learning for long-horizon tasks in dynamic and uncertain environments. However, learning dynamic task decomposition from scratch generally requires a large number of training samples, especially exploring the large joint action space under partial observability. In this paper, we present the Conditional Diffusion Model for Dynamic Task Decomposition (CD3T), a novel two-level hierarchical MARL framework designed to automatically infer subtask and coordination patterns. The high-level policy learns subtask representation to generate a subtask selection strategy based on subtask effects. To capture the effects of subtasks on the environment, CD3T predicts the next observation and reward using a conditional diffusion model. At the low level, agents collaboratively learn and share specialized skills within their assigned subtasks. Moreover, the learned subtask representation is also used as additional semantic information in a multi-head attention mixing network to enhance value decomposition and provide an efficient reasoning bridge between individual and joint value functions. Experimental results on various benchmarks demonstrate that CD3T achieves better performance than existing baselines.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **核心问题**：在复杂、长时域、部分可观测的多智能体协作任务中，从零学习动态任务分解面临两大困难：
  - 需要海量训练样本来探索巨大的联合动作-观测空间；
  - 难以自动发现有效的子任务结构和协调模式。
- **整体含义**：本文提出一种新颖的两级分层多智能体强化学习框架 **CD³T（Conditional Diffusion Model for Dynamic Task Decomposition）**，它利用条件扩散模型捕捉智能体动作对环境的效应，进而以数据驱动方式动态分解任务、分配子任务，并在值函数分解中注入子任务语义以提升信用分配，从而在多个困难场景下取得比现有方法更优的性能。

## 2. 方法论

### 2.1 动作表示学习（条件扩散模型）
- 为智能体的 one‑hot 动作学习一个稠密的潜在表示 \(z_{a_i}\)，以反映动作对环境的影响。
- 使用 **条件扩散模型** 对表示进行去噪重建：扩散过程逐步添加噪声，去噪网络则以智能体自身的局部观察 \(o_i\) 和其他智能体的联合动作 \(a_{-i}\) 为条件，预测噪声。
- 同时训练两个预测器，用学习到的动作表示预测下一步的局部观察 \(o_i'\) 和全局奖励 \(r\)，迫使表示编码环境效应。
- 联合优化扩散去噪损失 \(L_d\) 和预测损失 \(L_p\)（含奖励预测与观察预测）。

### 2.2 子任务动态分解
- 在训练初期（50K 步后），对收集的动作表示执行 **k‑means 聚类**，自动划分出 \(g\) 个子任务 \(\Phi\)，每个子任务对应一个受限的动作空间。
- 每个子任务拥有一个表示向量 \(z_{\phi_j}\)，由该子任务所包含动作的表示取平均得到。
- **两级层次结构**：
  - **高层子任务选择器**：每 \(\Delta T\) 步，根据智能体的局部历史 \(h_\tau\) 和子任务表示计算 \(Q_{\phi}(\tau_i, \phi_j) = z_{\tau_i}^T z_{\phi_j}\)，选择 Q 值最高的子任务分配给智能体。
  - **低层子任务策略**：在分配的受限动作空间内，智能体共享同一子任务的策略参数，使用与选择器类似的 Q 值计算方式（\(Q_i(\tau_i, a_{m_i}) = z_{\tau_i}^T z_{a_{m_i}}\)）。

### 2.3 信用分配与值分解
- 在混合网络中引入 **基于子任务表示的多头注意力**，计算智能体个体的信用权重 \(\lambda_{h,i}^\phi\)（高层）和 \(\lambda_{h,i}\)（低层），它们利用子任务表示 \(z_\phi\) 或动作表示 \(z_a\) 与全局状态 \(s\) 进行交互。
- 联合 Q 值通过单调混合分解得到，满足 IGM 原则：
  - 高层：\(Q_{tot}^\Phi = c_\phi(s) + \sum_{h=1}^H w_h^\phi \sum_{i=1}^N \lambda_{h,i}^\phi Q_{\phi}^{i}(\tau_i, \phi_{j_i})\)
  - 低层：\(Q_{tot} = c(s) + \sum_{h=1}^H w_h \sum_{i=1}^N \lambda_{h,i} Q_i(\tau_i, a_{m_i})\)
- 两个层级分别通过最小化 TD 误差来训练，使用相同的经验回放池。

## 3. 实验设计

- **测试环境/基准**：
  - **LBF**（Level‑Based Foraging）：2 种自定义场景（4 智能体‑2 食物，3 智能体‑3 食物）
  - **SMAC**：8 个地图，覆盖 **Easy**（8m, 2s3z, 3s5z）、**Hard**（5m_vs_6m, 2c_vs_64zg）和 **Super Hard**（3s5z_vs_3s6z, 6h_vs_8z, corridor）
  - **SMACv2**（补充材料验证）
- **对比方法**：
  - 经典值分解：VDN, QMIX, QTRAN, QPLEX
  - 子任务/角色相关：RODE, CDS, GoMARL, ACORM_oc（仅聚类一次，与 CD³T 一致）, DT2GS
- **评估指标**：平均测试回报（LBF）、测试胜率（SMAC），均报告 5 个随机种子的均值与标准差。

## 4. 资源与算力

文中 **未明确说明** 使用的 GPU 型号、数量、训练时长或计算资源规模。读者无法从文本获知训练过程中的算力消耗。

## 5. 实验数量与充分性

- **总体实验数**：至少涵盖 3 个基准、10+ 个不同难度的场景，每个场景与 10 种左右的方法对比，并进行了消融实验（移除扩散模型、移除基于子任务的注意力、不同子任务簇数 3/4/5）。
- **充分性与公平性**：
  - 对比方法包括主流值分解和任务分解类算法，对比基线全面。
  - 为公平对齐，将需要每步聚类的 ACORM 改为仅聚类一次（对齐 CD³T 的设置）。
  - 消融实验清晰验证了扩散模型、注意力机制和子任务数量的贡献。
  - 提供了可视化（动作表示聚类、动态子任务选择、动作空间缩减）增强可解释性。
- **潜在不足**：未在更多超参数或不同聚类时机下进行敏感性分析。

## 6. 主要结论与发现

- CD³T 在所有基准测试（尤其 **Super Hard** 场景）上均取得最佳或极具竞争力的表现，显著优于已有方法。
- **扩散模型** 能够捕捉动作对环境的影响规律，形成语义清晰的子任务表示，即使面对对称同质的环境（如 corridor）也能正确聚类。
- **子任务动态切换** 使智能体可以自适应地扮演诱敌、风筝、集火等不同角色，实现高效协作。
- **子任务‑注意力信用分配** 利用全局状态与子任务表示协作，有效缓解了全局状态与联合 Q 值之间的虚假关联问题。

## 7. 优点

- 首次将条件扩散模型引入多智能体任务分解，利用其强大的生成建模能力提取动作语义。
- 完全基于数据驱动的子任务发现（聚类），无需人工定义或额外正则化即可获得差异性明显的子任务。
- 两级层次结构和动作空间掩码机制显著压缩有效动作空间，提高探索效率和学习速度。
- 信用分配模块与子任务表示深度绑定，提升了值分解的准确性和可解释性。
- 丰富的可视化（聚类效果、子任务选择时序、动作空间大小）直观显示了方法的有效性。

## 8. 不足与局限

- **计算开销未提及**：扩散模型的预训练和推理可能引入较大的计算负担，但论文未报告任何时间或显存开销。
- **子任务数量需预设**：聚类数目 \(g\) 是超参数，需针对不同任务手动调整，缺乏自动选取机制。
- **聚类时机固定**：仅在 50K 步后聚类一次，后续不再更新子任务划分，可能不适用于环境动态持续剧烈变化的场景。
- **实验覆盖有限**：仅在标准化对抗/觅食环境上评测，缺少在连续控制、机器人协作等不同类任务上的验证。
- **依赖全局状态**：训练中仍需全局状态 \(s\) 进行信用分配，在全局状态不可获取或高度不完全的任务中可能受限。
- **理论保证不足**：子任务分解的收敛性或最优性未提供形式化分析。
- **对比中的公平性细节**：ACORM 改为一轮聚类可降低其性能，但未讨论其他聚类相关超参数对各方法的影响，可能存在偏差。

（完）
