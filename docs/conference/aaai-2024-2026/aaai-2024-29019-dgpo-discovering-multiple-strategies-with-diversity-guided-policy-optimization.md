---
title: "DGPO: Discovering Multiple Strategies with Diversity-Guided Policy Optimization"
title_zh: DGPO：多样性引导策略优化发现多种策略
authors: "Wentse Chen, Shiyu Huang, Yuan Chiang, Tim Pearce, Wei-Wei Tu, Ting Chen, Jun Zhu"
date: 2024-03-25
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/29019/29933"
tags: ["query:mt"]
score: 7.0
evidence: 通过多样性引导优化学习多种策略，增强序列决策鲁棒性
tldr: 针对传统强化学习仅追求单一最优策略的局限，提出多样性引导策略优化(DGPO)算法。通过信息论多样性内在奖励，在单次训练中用共享网络发现多种解策略。实验表明，DGPO能有效产生多样且高奖励的策略，提升策略鲁棒性和互动性。
source: AAAI-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29019/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 798, \"height\": 337, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29019/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1694, \"height\": 709, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29019/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1781, \"height\": 506, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29019/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1790, \"height\": 477, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29019/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 489, \"height\": 293, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29019/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 795, \"height\": 791, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29019/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 804, \"height\": 461, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29019/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 631, \"height\": 904, \"label\": \"Figure\"}]"
motivation: 单一策略难以应对意外扰动或提供多样交互，需要学习多种解策略。
method: 在策略优化中加入信息论多样性内在奖励，交替约束策略多样性和外在奖励进行训练。
result: 在多个环境中生成多种有效策略，并提高策略鲁棒性。
conclusion: DGPO为多策略发现提供了有效方法，适用于需要策略多样性的场景。
---

## Abstract
Most reinforcement learning algorithms seek a single optimal strategy that solves a given task. However, it can often be valuable to learn a diverse set of solutions, for instance, to make an agent's interaction with users more engaging, or improve the robustness of a policy to an unexpected perturbance. We propose Diversity-Guided Policy Optimization (DGPO), an on-policy algorithm that discovers multiple strategies for solving a given task. Unlike prior work, it achieves this with a shared policy network trained over a single run. Specifically, we design an intrinsic reward based on an information-theoretic diversity objective. Our final objective alternately constraints on the diversity of the strategies and on the extrinsic reward. We solve the constrained optimization problem by casting it as a probabilistic inference task and use policy iteration to maximize the derived lower bound. Experimental results show that our method efficiently discovers diverse strategies in a wide variety of reinforcement learning tasks. Compared to baseline methods, DGPO achieves comparable rewards, while discovering more diverse strategies, and often with better sample efficiency.

---

## 论文详细总结（自动生成）

好的，根据您提供的论文内容，以下是关于《DGPO: Discovering Multiple Strategies with Diversity-Guided Policy Optimization》的结构化中文总结。

### 1. 论文的核心问题与整体含义

*   **研究动机与背景**：
    *   传统的强化学习（RL）算法通常致力于寻找解决任务的单一最优策略。
    *   然而，在许多实际场景中，学习一组多样化的解决方案具有重要价值，例如：
        *   **提升鲁棒性**：防止策略对单一解“过拟合”，使其能更好地应对环境中的意外扰动。
        *   **增强交互性与竞争力**：在对话系统或竞技博弈中，多样化的策略能使交互更具吸引力，或避免被对手轻易预测。
*   **核心问题**：
    *   如何设计一种算法，能够在单次训练过程中，使用一个共享的策略网络，高效地发现一组既能完成任务（高外部奖励）又具备行为多样性的高质量策略集合。
*   **整体含义**：
    *   本文提出了多样性引导策略优化（DGPO）算法，旨在解决上述问题。它通过在训练中交替优化策略的外部奖励和内在的多样性奖励，实现了在保证任务性能的同时，自动发现多种不同的解决方案。

### 2. 论文提出的方法论

*   **核心思想**：
    *   将寻找多样化策略的问题形式化为两个交替的约束优化问题，并通过概率图模型推理来求解。
    *   **第一阶段（多样性约束优化）**：在保证策略多样性不低于某个阈值的情况下，最大化外部奖励。
    *   **第二阶段（外部奖励约束优化）**：在确保外部奖励达到预设目标值的情况下，最大化策略的多样性。
*   **关键技术细节**：
    *   **策略表征**：使用一个以隐变量 `z` 为条件的策略网络 `π(a|s, z)`。每个回合开始时，从一个分类分布中采样一个 `z`，策略根据这个 `z` 产生一条特定的行为轨迹。
    *   **多样性度量（Diversity Score）**：设计了一种严格的成对多样性度量 `DIV(πθ)`。它不是衡量每个策略与平均行为分布的差异，而是通过计算**每个策略与距离它最近的其他策略**在状态访问分布上的KL散度来衡量，确保任意两个策略之间都有足够的差异。
    *   **内在奖励（Intrinsic Reward）**：为了最大化上述多样性度量，论文推导出一个基于信息论的内在奖励下界。该内在奖励 `rin_t` 通过一个学得的判别器 `qϕ(z|s)` 来计算，它衡量了基于下一状态 `s_{t+1}` 区分当前隐变量 `z` 的难易程度。
    *   **统一优化框架**：DGPO通过指示函数，巧妙地统一了两个阶段的训练。
        *   总奖励 `rtotal` 由外部奖励 `r` 和内在奖励 `rin` 通过一个掩码机制组合而成。
        *   当多样性不足时（`J_Div < δ`），算法专注于优化内在奖励以提升多样性。
        *   当性能不足时（`J < R_target`），算法专注于优化外部奖励。
        *   当性能达标后，算法再次专注于优化内在奖励，以在确保性能的前提下进一步扩大多样性。
        *   该算法是基于PPO的on-policy算法，包含一个隐变量条件策略网络、两个分别预测外部和内在价值函数的评论家网络，以及一个区分隐变量的判别器网络。

### 3. 实验设计

*   **实验场景与数据集**：
    *   **多智能体粒子环境（MPE）**：包括 `Spread (easy)` 和 `Spread (hard)` 两个场景，存在多条到达目标点或协同覆盖目标点的优解路径。
    *   **Atari游戏**：包括 `Pong` 和 `Boxing`，用于测试在图像观测任务中的表现。
    *   **星际争霸多智能体挑战赛（SMAC）**：包括 `2s vs. 1sc` 和 `3m` 两个地图，需要复杂的微操策略。
*   **基准方法**：
    *   **MAPPO**：适用于多智能体的标准PPO算法，仅追求单一最优解。
    *   **DIAYN**：一种无监督技能发现算法，通过最大化状态与技能的互信息来学习多样化技能（实验中结合了外部奖励）。
    *   **SMERL**：在外部奖励高于阈值时，最大化外部奖励与内在奖励的加权组合。
    *   **RSPO**：一种需要多阶段训练的算法，通过切换外部和内在奖励来迭代式地发现新策略。
*   **评估指标**：
    *   **性能**：回合奖励或胜率。
    *   **多样性得分**：基于策略行为嵌入（如智能体位置轨迹）的成对距离自定义指标。

### 4. 资源与算力

*   **文中明确提及的硬件配置**：
    *   所有实验均在一台配置如下参数的机器上完成：
        *   **内存**：128 GB RAM
        *   **CPU**：1个32核CPU
        *   **GPU**：1块 GeForce RTX 3090 GPU
*   **未明确说明的信息**：论文未提及每一组实验具体的训练时长（如小时或天数）。

### 5. 实验数量与充分性

*   **实验组数量**：论文在3大类（MPE, Atari, SMAC）共6个具体任务上，将DGPO与4种基准方法进行了性能与多样性的对比。此外，还针对DGPO的关键组件进行了消融实验。
*   **实验充分性与公平性**：
    *   **充分性**：实验覆盖了多智能体、单智能体、图像输入等多种任务类型，通过性能和多样性两个维度的散点图、策略发现速度图以及行为可视化（热力图、轨迹图）进行了全面评估。消融实验验证了各部分设计的必要性。
    *   **公平性与客观性**：
        *   **随机种子**：MPE、Atari、SMAC的实验结果分别基于5个随机种子取平均，确保了结果的统计可靠性。
        *   **超参数**：实验尽量对所有方法使用相同的超参数以进行公平比较。对于RSPO，则使用了其开源实现。
        *   **潜在偏差**：自定义的多样性评分指标可能更有利于DGPO，但论文同时也提供了可视化结果作为定性证据。

### 6. 论文的主要结论与发现

*   DGPO能够在一个共享网络和单次训练流程中，有效地发现一组多样化且高质量的策略。
*   相比基准方法，DGPO在**获得可比较的（或更高的）外部奖励的同时，能发现更多样化的策略**，且通常具有**更好的采样效率**。
*   简单的将内在与外在奖励直接结合的方法（如 DIAYN）不足以发现所有优解，而DGPO采用的交替约束优化框架是使其成功的关键。
*   在MPE任务中，DGPO是能与RSPO一同发现所有优解的方法，但DGPO的**收敛速度远快于RSPO**（在Spread (easy)和Spread (hard)上分别有1.7倍和15倍的加速）。

### 7. 优点

*   **方法论创新**：
    *   提出了一个新颖的、更严格的**成对多样性度量**，避免了策略只在整体上看似不同，但个体间可能相似的弱点。
    *   巧妙地将多策略发现问题**构建为两个交替的约束优化问题**，并通过概率图模型框架优雅地解决了该问题。
*   **算法效率高**：
    *   与需要多阶段或多网络的前序工作（如RSPO）相比，DGPO使用**共享网络和单次端到端训练**，大大提升了样本效率和实用性。
*   **实验设计扎实**：
    *   实验覆盖领域广，包含多/单智能体、向量/图像输入，对比基线丰富，且包含了定量指标、收敛性分析和可视化结果，论证令人信服。

### 8. 不足与局限

*   **超参数的数量与敏感性**：DGPO框架引入了多个关键超参数，如策略数量 `nz`、多样性阈值 `δ` 和性能目标 `R_target`。这些参数的选择可能对最终效果有显著影响，但论文未深入探讨其敏感性或提供调参指南。
*   **多样性阈值的设定依赖**：`δ` 和 `R_target` 的设定可能需要根据具体任务进行调整，其自适应能力有待进一步研究。
*   **计算成本**：虽然文中未与其他方法对比训练时间/计算量，但DGPO内部维护了多个网络（策略、两个评论家、一个判别器），相比基础PPO，其单步更新的计算成本显著增加。
*   **应用场景的局限性**：该方法主要应用于多解存在的任务。对于那些最优策略唯一或解空间极窄的任务，DGPO的多样性驱动机制可能无意义，甚至引入噪声干扰训练。

（完）
