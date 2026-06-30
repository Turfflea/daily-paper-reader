---
title: Scaling Combinatorial Optimization Neural Improvement Heuristics with Online Search and Adaptation
title_zh: 在线搜索与自适应的组合优化神经改进启发式规模化方法
authors: "Federico Julian Camerota Verdù, Lorenzo Castelli, Luca Bortolussi"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/34921/37076"
tags: ["query:mt"]
score: 9.0
evidence: 基于深度强化学习的组合优化改进启发式方法
tldr: 针对深度强化学习在组合优化中扩展性不足的问题，提出有限展开束搜索策略，结合预训练改进策略，在旅行商问题及变体上显著提升分布内性能和泛化到更大规模实例的能力，并支持离线与在线自适应，缩小了与构造式方法的差距。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34921/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 636, \"height\": 219, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34921/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 883, \"height\": 780, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34921/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 881, \"height\": 604, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34921/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1616, \"height\": 870, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34921/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 785, \"height\": 211, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34921/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1784, \"height\": 528, \"label\": \"Table\"}]"
motivation: 现有的DRL组合优化改进策略在性能和泛化能力上存在局限性。
method: 提出有限展开束搜索策略，结合预训练模型，通过在线搜索和自适应提高改进启发式的性能。
result: 在欧几里得TSP及变体上，最优性差距显著优于现有改进启发式，并缩小了与最优构造式方法的差距。
conclusion: 所提LRBS策略有效提升了DRL组合优化方法的可扩展性和适应性，为实际应用提供了新思路。
---

## Abstract
We introduce Limited Rollout Beam Search (LRBS), a beam search strategy for deep reinforcement learning (DRL) based combinatorial optimization improvement heuristics. 
Utilizing pre-trained models on the Euclidean Traveling Salesperson Problem, LRBS significantly enhances both in-distribution performance and generalization to larger problem instances, achieving optimality gaps that outperform existing improvement heuristics and narrowing the gap with state-of-the-art constructive methods.
We also extend our analysis to two pickup and delivery TSP variants to validate our results.
Finally, we employ our search strategy for offline and online adaptation of the pre-trained improvement policy, leading to improved search performance and surpassing recent adaptive methods for constructive heuristics.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义

- **研究背景**：深度强化学习（DRL）在组合优化（CO）中，特别是旅行商问题（TSP），已发展出两类主流神经启发式：**构造式**（逐步生成解）和**改进式**（迭代优化给定初始解）。改进式方法在复杂约束、已知良好算子等方面具有优势，但其**泛化能力**远不如构造式方法，即在训练时看到的小规模实例上学到的策略，难以直接迁移到大尺度问题上，这成为 DRL 改进启发式实际应用的主要瓶颈。
- **核心问题**：如何让基于 DRL 的改进启发式在推理阶段**有效扩展到大 10 倍以上的问题规模**，同时保持或提升分布内性能，并探索如何通过在线搜索与自适应策略进一步弥补预训练模型的泛化不足。
- **整体含义**：本文提出了一种新的束搜索策略（LRBS），使改进启发式在 TSP 及其变体上既能取得 SOTA 改进方法的性能，又能显著缩小与构造式方法的差距，尤其在大规模泛化上表现突出。

### 2. 论文提出的方法论

- **核心思想**：将改进式 MDP 的搜索过程看作遍历搜索树，利用有限长度的策略展开（rollout）来克服单一动作短视和长周期带来的探索困难，同时通过**限制搜索宽度与深度**平衡性能与计算成本。
- **关键技术细节**：
  - **改进 MDP 建模**：状态为当前解及历史最优解；动作为 2-opt 移动（或针对 PDTSP 的移除-重插入）；奖励仅为找到新的更优解时的长度改善。
  - **LRBS 算法流程（见 Algorithm 1）**：
    1. 初始化：从初始解 δ₀ 采样 α×β 个候选动作，得 β 束节点（即当前活跃边沿大小 β）。
    2. 对于每个束节点，执行 π 策略**展开 n_s 步**，到达拓展状态。
    3. 从 β×α 个展开后的状态中，依据目标函数值选择最优 β 个作为新束节点。
    4. 重复展开与选择，直至达到总步数 T_max，返回历史最优解。
  - 参数：β（束宽度）、α（每节点扩展动作数）、n_s（展开步数），通过固定 α×β = 60（TSP）控制预算，使搜索开销可控。
  - **自适应扩展**：将 Efficient Active Search (EAS) 嵌入 LRBS 的展开过程中。引入少量新参数 ϕ（添加在解码器末端），通过 RL 损失（式1）在展开的轨迹上更新 ϕ，实现**离线微调**（有少量同分布实例）或**在线自适应**（每个测试实例单独更新并重置）。梯度为 ∇_ϕ L_R(ϕ) = E_π[(R(δ_LRBS)-b)∇_ϕ log π_ϕ(δ_LRBS)] ，其中 b 为基线。
- **与 SGBS、MCTS 等对比**：SGBS 需要完整构造式 rollout 至叶节点，不适用于改进式长周期；MCTS 反向传播代价高；LRBS 通过有限展开避免了这些缺点。

### 3. 实验设计

- **数据集/场景**：
  - 欧几里得 TSP：节点坐标在单位正方形内均匀随机生成。测试集包含：
    - TSP100：10,000 实例（与 Costa 等 2020 相同）
    - TSP150：1,000 实例
    - TSP200：1,000 实例
    - TSP500、TSP1000：各 128 实例
  - Pickup and Delivery TSP (PDTSP) 及带后进先出约束版本 (PDTSPL)：随机生成 200 和 500 节点实例各 128 个。
- **Benchmarks**：
  - 精确/传统求解器：Concorde (TSP), LKH (PDTSP/PDTSPL) 作为最优参考。
  - 构造式 DRL 方法：POMO, EAS, SGBS+EAS, Meta-SAGE, GLOP, LEHD 等（仅提供上下文对比，不作直接改进类内对比）。
  - 改进式 DRL 方法：DACT, A-DACT (带数据增强), Costa (基础策略), SGBS+C (修改后适用于改进式的 SGBS), Beam Search。
  - 自适应场景：LRBS+OA (在线), LRBS+FT (离线微调)。
- **对比方法**：在同改进类中直接对比 Costa, DACT, A-DACT, SGBS+C, Beam Search；在构造类中对比 POMO, EAS, SGBS+EAS, Meta-SAGE 等（表1, 2, 4）。实验严格使用相同预训练模型（Costa 等, Ma 等），确保比较公平。

### 4. 资源与算力

- **硬件**：所有实验在单张 **NVIDIA Ampere GPU with 64 GB HBM2** 上运行。
- **训练/推理时间**：论文未提及预训练模型本身的训练时长（直接使用公开发布的检查点）。推理时间在结果表格中均有报告（如 TSP100 上 LRBS 用时 19 小时，TSP200 上 1.6–4.1 小时等），用户可据此推算。自适应微调在推理过程中进行，其时间已包含在报告的总 runtime 中。

### 5. 实验数量与充分性

- **实验组数**：
  - TSP 规模：100, 150, 200, 500, 1000 节点，共 5 组大型主实验（表1, 2），每组包含多种方法。
  - PDTSP/PDTSPL 规模：200, 500 节点，共 4 组实验（表4），每种方法不同步数（1K, 2K, 3K）。
  - 自适应场景：OA 和 FT 在 TSP200/500/1000 上各做了 2 种步数（2K/5K）实验。
  - 消融实验：针对 FT 场景，剔除 LRBS 搜索和剔除在线探索的对比（表3），覆盖 TSP150/200/500/1000。
  - 参数敏感性分析：在扩展版本附录中报告了 α, β 的影响（正文提及）。
- **充分性评价**：实验覆盖了分布内、中等偏移、大偏移等多种泛化难度，消融研究验证了 LRBS 在线探索和自适应组件的必要性。对比基准包含改进类和构造类两大阵营，且都基于公开检查点，客观公正。表3的消融实验尤其有力地表明对大规模泛化，LRBS 的探索在微调中不可或缺。整体实验设计严谨，可重复性强。

### 6. 论文的主要结论与发现

- LRBS 能在 TSP100 上取得 0.014% 的最优性间隙，超越所有改进类基线（Costa 0.065%, A-DACT 0.101%），并接近构造类方法水平。
- 泛化能力大幅提升：在 TSP200 上，LRBS（0.633%）显著优于 Costa（8.717%）和 DACT（34%+），且接近 EAS（0.455%）；在 TSP500 上，LRBS 的 4.633% 远低于 Costa 的 8.717%，甚至优于 SGBS+EAS（9.963%）。在 TSP1000 上，虽然 LRBS 单独使用（20.740%）不及 BS（14.536%），但结合在线自适应后降至 11.889%，超越 Meta-SAGE（12.038%）和 EAS（32.869%）。
- 离线微调（LRBS+FT）在 TSP500/1000 上达到最佳（3.463% 和 11.483%），验证了少量同分布数据即可大幅提升泛化性能。
- 在 PDTSP 和 PDTSPL 上，LRBS 同样大幅领先 N2S-A（Ma 等 2022），尤其在 PDTSPL500 上，结合在线自适应可将间隙从 N2S-A 的 >150% 降至约 10%。计算时间随问题规模增长更平缓。
- 消融实验表明：对于大规模泛化，LRBS 的探索在微调中起关键作用（TSP1000 上，无 LRBS FT 间隙 81.63%，有 LRBS 降至 11.48%）。

### 7. 优点

- **方法创新**：首次将有限展开束搜索引入改进启发式，通过缩短有效视界解决改进 MDP 的探索难题，无需复杂反向传播。
- **计算效率高**：相比构造式方法，LRBS 在更大规模上 runtime 增长更温和（如 TSP500 仅需 1.9 小时，而 EAS 需 4.3 小时）。
- **灵活可扩展**：搜索策略可与 EAS 等自适应方法无缝结合，支持离线/在线两种模式；框架可移植到其他组合优化问题（仅需定义合适的算子）。
- **实验充分**：在多个 TSP 变体、5 种规模下全面验证，消融实验清晰证明了 LRBS 中搜索与自适应各自的价值。
- **开源**：提供代码链接，确保可复现。

### 8. 不足与局限

- **极端规模泛化仍然困难**：TSP1000 上，纯 LRBS 表现差于简单的 Beam Search，说明策略失效时搜索本身可能退化，仍需自适应辅助。
- **依赖于已有预训练模型**：所有实验均基于 Costa 和 Ma 等人的公开模型，未证明 LRBS 能从零开始训练策略；若初始策略过差，效果可能受限。
- **自适应仍需额外计算**：在线自适应在每实例上都需要梯度更新，虽不大但增加了 runtime；离线微调需要目标分布样本，在严格未知分布下可能降低实用性。
- **问题类型相对局限**：仅在 TSP 及两种取送货变体上测试，未在其他经典组合优化问题（如 CVRP、JSP）上验证。
- **参数敏感性**：α、β、n_s 等需根据规模调整，文章虽做敏感性分析但在附录中，实际使用前可能需手动调参。

（完）
