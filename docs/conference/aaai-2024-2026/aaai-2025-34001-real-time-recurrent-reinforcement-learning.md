---
title: Real-Time Recurrent Reinforcement Learning
title_zh: 实时循环强化学习
authors: "Julian Lemmel, Radu Grosu"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/34001/36156"
tags: ["query:mt"]
score: 7.0
evidence: 循环强化学习解决部分可观测序列决策问题
tldr: 针对部分可观测马尔可夫决策过程中的序列决策问题，提出实时循环强化学习框架。该算法结合元学习架构、资格迹时间差分学习和在线自动微分，在部分可观测任务中表现出色，为生物启发的决策优化提供了计算模型。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34001/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 821, \"height\": 323, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34001/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 863, \"height\": 499, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34001/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1824, \"height\": 538, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34001/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1808, \"height\": 409, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34001/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1820, \"height\": 718, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34001/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1830, \"height\": 272, \"label\": \"Figure\"}]"
motivation: 部分可观测环境下的强化学习任务具有挑战性，需要记忆与推理。
method: 结合元强化学习架构、资格迹时间差分学习和在线循环网络梯度计算。
result: 在多种部分可观测强化学习任务中成功学习有效策略。
conclusion: RTRRL展示了循环网络与强化学习结合处理部分可观测决策的潜力。
---

## Abstract
We introduce a biologically plausible RL framework for solving tasks in partially observable Markov decision processes (POMDPs). 
The proposed algorithm combines three integral parts: (1) A Meta-RL architecture, resembling the mammalian basal ganglia; (2) A biologically plausible reinforcement learning algorithm, exploiting temporal difference learning and eligibility traces to train the policy and the value-function; (3) An online automatic differentiation algorithm for computing the gradients with respect to parameters of a shared recurrent network backbone. Our experimental results show that the method is capable of solving a diverse set of partially observable reinforcement learning tasks. The algorithm we call real-time recurrent reinforcement learning (RTRRL) serves as a model of learning in biological neural networks, mimicking reward pathways in the basal ganglia.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：如何在部分可观测马尔可夫决策过程中，以生物学上合理的方式训练循环神经网络智能体，使其能够在线、实时地学习有效策略。
- **研究动机**：
    - **生物学合理性缺失**：主流的基于反向传播通过时间（BPTT）的RNN训练方法及现代强化学习算法被认为在生物学上不成立。BPTT依赖权重传输、独立的前向与反向阶段以及存储长序列的精确激活，这些都与大脑的运行机制不符。
    - **在线学习需求**：许多生物体需要从连续的经验流中在线学习，而不是像蒙特卡洛方法那样在收集完整轨迹后再进行更新。这要求算法能够实时计算梯度并更新网络。
- **整体含义**：本文旨在提出一个统一的计算框架，模拟哺乳动物基底神经节的奖励通路，证明结合生物学合理的循环网络梯度计算方法和在线强化学习算法，可以有效解决部分可观测的序列决策问题。

### 2. 论文提出的方法论
RTRRL框架由三个核心组件构成：
- **元强化学习循环架构**：
    - **网络结构**：模拟基底神经节的功能，使用一个共享的循环神经网络（RNN）作为主干，该网络接收当前的`观察`、`上一动作`和`奖励`作为输入，输出一个隐状态向量。
    - **Actor-Critic头**：在隐状态的基础上，通过两个独立的线性函数（分别对应Actor和Critic）计算动作概率分布和状态价值估计。
- **生物学合理的强化学习算法**：
    - **TD(λ)学习**：利用时间差分误差（TD-error）作为奖励预测误差（RPE）信号，同时更新Actor（策略）和Critic（价值函数）。
    - **资格迹**：采用后向视角的资格迹来分配延迟奖励的贡献，解决了在线学习中奖励信号稀疏和延迟的问题。
- **在线自动微分算法**：
    - **实时递归学习及其近似**：为替代BPTT，使用RTRL或其生物学合理变体RFLO在线计算RNN参数的梯度。RTRL核心是维护一个参数对隐状态的雅可比矩阵迹，通过递归公式 `ˆJ_{t+1} = ˆJ_t (I + ∇_{h_t} f(x_t, h_t)) + ∇_θ f(x_t, h_t)` 进行更新。
    - **随机反馈局部在线学习**：
        - **简化梯度计算**：利用连续时间RNN的神经微分方程，简化RTRL的更新规则，如公式 `ˆJ_{W, t+1} ≈ (1 - τ⁻¹) ˆJ_{W, t} + τ⁻¹ φ'(W ξ_t)ᵀ ξ_t` 所示。
        - **反馈对齐**：避免权重传输，使用固定的随机反馈矩阵 `B` 而非前向权重的转置来传播误差，参数更新形式为 `∆W_t = ˆJ_{W, t} B ε_t`。
    - **高效实现（LRU）**：针对线性循环单元（LRU）的对角循环权重矩阵，RTRL的复杂度可大幅降低，实现高效在线学习。

### 3. 实验设计
- **任务场景**：
    - **标准强化学习基准**：使用了 `gymnax` 和 `popgym` 库中的任务，这些任务被特别处理为部分可观测（如遮蔽部分观测值）或本身就是POMDP。任务涵盖离散和连续动作空间，例如 `StatelessCartPole`、`UmbrellaChain`、`RepeatPrevious`、`MetaMaze` 等。
    - **记忆长度测试**：通过 `MemoryChain` 环境（类似T迷宫）评估模型记忆信息的能力，任务难度随需要记忆的步长呈指数级增加。
    - **物理模拟环境**：在 `brax` 物理仿真平台的任务上进行测试，通过遮蔽一半观测值来构建POMDP场景。
- **对比方法**：
    - **PPO + BPTT**：使用相同的RNN模型（CT-RNN或LRU），但采用BPTT进行训练。这是实验的核心对比基线。
    - **线性函数近似的TD(λ)** ：不使用RNN，仅用线性函数近似价值，用于验证循环网络处理部分可观测问题的必要性。
- **RTRRL自身变体**：
    - **网络类型**：比较了使用CT-RNN和LRU作为循环主干的效果。
    - **梯度计算方法**：对比了RTRL和其近似RFLO。
    - **反馈机制**：对比了使用标准反向传播和使用随机反馈对齐的效果。

### 4. 资源与算力
- 论文未明确说明实验所使用的具体GPU型号、数量及详细训练总时长。文中仅在致谢部分提及部分计算在维也纳科学集群（VSC）上完成。

### 5. 实验数量与充分性
- **实验次数**：
    - **主实验结果**：图3展示了在9个不同任务上各进行5次独立运行的结果箱线图。
    - **消融实验**：进行了三组消融研究，分别针对：
        1.  **反馈对齐**：比较使用和未使用随机反馈矩阵的影响。
        2.  **元强化学习架构**：比较是否将上一动作和奖励作为输入的影响。
        3.  **熵正则化系数**：调整策略熵的权重，分析其对性能和稳定性的影响。图4（右）展示了此结果。
    - **记忆长度实验**：在 `MemoryChain` 环境上，针对不同记忆长度（指数级增长），每种梯度计算方法（BPTT, RFLO, RTRL, LocalMSE）进行了10次运行，结果如图4（左）所示。
    - **物理仿真实验**：在4个 `brax` 任务上，对每个环境进行了至少10小时的超参数调优，并选取了最佳结果进行展示（图6）。
- **充分性评估**：实验设计较全面，考虑了多种任务类型、关键组件的消融对比以及对学习算法核心能力（如长时记忆）的专项测试。对比基线包括了强大的现代方法（PPO），且运行多次重复实验展示了结果的统计分布，整体上较为充分且客观。

### 6. 论文的主要结论与发现
- **RTRRL的有效性**：提出的RTRRL算法能够成功解决一系列部分可观测的强化学习任务。
- **LRU版本性能卓越**：采用LRU作为循环主干并结合RTRL的版本（RTRRL-LRU）在绝大多数任务上取得了最高的中位奖励，一致性优于使用BPTT训练的PPO。
- **生物学合理性不影响一定性能**：完全生物学合理的版本（CT-RNN + RFLO + 反馈对齐）在多数任务上也能取得与其他方法可比的结果，甚至在某些带噪声环境（如 `NoisyStatelessCartPole`）中表现更优。这表明，满足生物学约束的近似梯度计算在许多情况下足以支撑有效的在线学习。
- **架构组件的作用**：消融实验证明，元学习架构（输入历史动作和奖励）和合适的熵正则化系数对某些任务的性能有显著影响。

### 7. 优点
- **新颖的统一框架**：首次将在线强化学习（TD(λ)）、元学习RNN架构和生物学合理的在线梯度计算（RFLO/RTRL）整合到一个统一的、功能强大的学习系统中。
- **强生物学可解释性**：框架的每个部分都紧密对应神经科学发现，如基底神经节的Actor-Critic结构、多巴胺作为RPE信号、以及使用反馈对齐和局部学习规则，为大脑学习机制提供了有力的计算模型。
- **完全在线学习**：算法使用 batch size 为1，从连续经验流中学习，无需经验回放或完整轨迹，更贴近现实世界和生物体学习场景。
- **方法对比全面**：不仅与强大的PPO基线对比，还深入对比了框架内不同模块（网络类型、梯度算法、反馈方式）的组合效果，清晰地揭示了各组件的作用。

### 8. 不足与局限
- **高方差问题**：由于使用1的批量大小进行在线学习，算法自然存在较高的方差，这从实验结果图中的箱线图范围可以观察到。
- **超参数敏感**：论文指出，该方法尤其是在处理连续动作任务时，需要进行细心的超参数调优。
- **理论分析不足**：论文以实证结果为主，缺乏对算法收敛性、稳定性或适用边界的理论分析。对为何RFLO在某些特定任务（如 `RepeatPrevious`）上效果很差的原因没有给出明确解释。
- **任务规模有限**：实验主要在中小规模的标准强化学习基准上进行，尚未验证其在更复杂、高维、长序列依赖的大型任务（如Atari游戏）上的扩展性和表现。

（完）
