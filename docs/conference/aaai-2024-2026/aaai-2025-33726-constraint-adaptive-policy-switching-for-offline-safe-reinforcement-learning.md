---
title: Constraint-Adaptive Policy Switching for Offline Safe Reinforcement Learning
title_zh: 面向离线安全强化学习的约束自适应策略切换方法
authors: "Yassine Chemingui, Aryan Deshwal, Honghao Wei, Alan Fern, Jana Doppa"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/33726/35881"
tags: ["query:mt"]
score: 8.0
evidence: 离线安全强化学习，通过约束自适应策略切换实现安全序贯决策
tldr: 针对离线安全强化学习中部署阶段安全约束变化而无需重新训练的挑战，提出约束自适应策略切换框架（CAPS）。该框架在训练时利用离线数据学习共享表示下的多策略以涵盖不同奖励-代价权衡，测试时在状态层面动态选择满足当前约束且最大化未来奖励的策略，从而在不换练的情况下适应不同安全需求，为安全敏感的序贯决策优化提供了灵活方案。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33726/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 871, \"height\": 377, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33726/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1843, \"height\": 1431, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33726/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 882, \"height\": 328, \"label\": \"Table\"}]"
motivation: 离线安全强化学习中安全约束在部署时变化而无法重新训练。
method: 训练时学习多个带共享表示的策略以优化奖励-代价权衡，测试时动态切换满足约束的最大化奖励策略。
result: 在多个离线安全RL基准上，CAPS在不同约束水平下均表现出优于现有方法的适应性和安全性。
conclusion: 策略切换机制使离线RL能灵活应对部署时的安全约束变化，拓展了安全强化学习的实用性。
---

## Abstract
Offline safe reinforcement learning (OSRL) involves learning a decision-making policy to maximize rewards from a fixed batch of training data to satisfy pre-defined safety constraints. However, adapting to varying safety constraints during deployment without retraining remains an under-explored challenge. To address this challenge, we introduce constraint-adaptive policy switching (CAPS), a wrapper framework around existing offline RL algorithms. During training, CAPS uses offline data to learn multiple policies with a shared representation that optimize different reward and cost trade-offs. During testing, CAPS switches between those policies by selecting at each state the policy that maximizes future rewards among those that satisfy the current cost constraint. Our experiments on 38 tasks from the DSRL benchmark demonstrate that CAPS consistently outperforms existing methods, establishing a strong wrapper-based baseline for OSRL.

---

## 论文详细总结（自动生成）

好的，以下是根据您提供的论文内容生成的详细中文总结。

### 1. 论文的核心问题与整体含义
- **研究背景与动机**：离线安全强化学习旨在从固定的历史数据中学习能最大化奖励且满足安全约束的策略。然而，现有方法大多假设安全约束阈值在训练时已知且在部署时保持不变。
- **核心问题**：在许多现实应用中，安全约束会在部署阶段因场景或环境变化而变化，而重新训练策略成本高昂且不切实际。如何让离线训练的智能体在不重新训练的情况下，灵活适应部署时动态变化的安全约束，是一个尚未被充分探索的挑战。
- **整体含义**：本文提出了一种名为约束自适应策略切换的通用算法框架，作为一种“包装器”，可轻松集成到现有离线强化学习算法之上，以解决上述部署约束可变的问题。

### 2. 论文提出的方法论
- **核心思想**：将问题分解为训练和测试两个阶段。训练时，学习一组覆盖不同奖励-代价权衡的多样化策略；测试时，在每一个状态下，从满足当前代价约束的策略中动态选择能最大化未来奖励的动作，从而实现自适应决策。
- **关键技术细节**：
    - **训练阶段（策略与价值函数学习）**：
        1.  利用离线强化学习算法，仅使用奖励数据训练一个**奖励最大化策略** 和对应的**奖励Q函数**。
        2.  类似地，将代价数据视为奖励，训练一个**代价最小化策略** 和对应的**代价Q函数**。
        3.  通过线性标量化奖励Q函数和代价Q函数（即），快速提取出一系列具有不同代价-奖励权衡的**中间策略**，而无需为每个中间策略运行完整的离线强化学习训练流程。
        4.  所有个策略共享一个神经网络主体，仅有输出头不同，以促进知识迁移和训练效率。
- **测试阶段（约束自适应决策）**：
    - 采用两步走策略：
        1.  **过滤**：对于当前状态，使用训练好的代价Q函数和已累积代价，筛选出所有策略建议的动作中，满足“代价Q值 + 已累积代价 ≤ 当前约束阈值”这一安全条件的**可行动作子集**。
        2.  **选择**：从可行的动作子集中，选择奖励Q值最大的动作执行。若无可行动作，则退而求其次，选择代价Q值最小的动作。
    - 该决策过程可形式化表示为：
        - 可行动作集
        - 最优动作
- **理论分析**：论文在代价最优Q函数被完美估计的假设下，证明了当一个策略是“-可容许的”时，其期望总代价的上界为，其中为MDP的最优代价变异系数，为剩余时间步长。这为本文的安全决策机制提供了理论保障。

### 3. 实验设计
- **基准与环境**：使用了来自数据集和基准框架的**38个顺序决策任务**，涵盖三个不同复杂度的环境套件：、和。
- **对比方法**：
    1.  **BC**：行为克隆基线。
    2.  **BEAR-Lag**：结合BEAR离线强化学习算法与拉格朗日法处理约束。
    3.  **CPQ**：约束惩罚的Q学习。
    4.  **COptiDICE**：基于分布校正估计的离线约束强化学习方法。
    5.  **CDT**：约束决策变换器，文中唯一能在测试时适应不同约束的基线方法。
- **本文方法变体**：使用两种不同的基础离线强化学习算法实现了CAPS，即**CAPS(IQL)** 和**CAPS(SAC+BC)**，并测试了不同策略数量 的配置。
- **评估指标与流程**：使用归一化回报和归一化代价作为指标。安全是首要指标，目标是归一化代价小于1。所有方法在3种不同的目标代价阈值配置、3个随机种子、每个种子20个评估回合下进行测试，最终报告平均归一化奖励和代价。

### 4. 资源与算力
- 论文中未明确提及实验所用的GPU型号、数量以及具体训练时长。

### 5. 实验数量与充分性
- **实验数量**：实验规模很大，主要体现在：
    - **38个不同任务**：覆盖了多种迷宫、运动和驾驶场景，保证了任务多样性。
    - **多种代价阈值配置**：测试了6组不同的代价限制组合，充分验证了方法在变约束下的适应性。
    - **丰富的消融研究**：包括策略数量的消融实验、共享与独立策略网络的对比、代价限制影响的专门分析，以及与离策略评估方法的对比。
- **充分性与公平性**：
    - **充分性**：实验设计全面，从主对比、消融到敏感性分析，系统地验证了方法的有效性及其各组件的作用。
    - **公平性**：对比方法涵盖了领域内最新且代表不同技术路线的基线；对于除外的所有基线，均明确为其每个代价阈值单独训练了模型；使用了官方基准的标准化评估协议，确保了结果的公正性和可比性。

### 6. 论文的主要结论与发现
- **性能优越**：在38个任务中，两种方法在满足安全约束的同时，显著超越了所有基线方法。例如，在34/38个任务中实现了安全行为，而第二好的基线仅满足19/38个任务。在安全的基础上，还在18个任务中获得了最高奖励。
- **策略切换有效**：仅使用两个策略的简单配置就极具竞争力，能有效管理变化的代价约束，这是一种强大的极简基线。
- **共享表示有益**：与传统独立训练策略相比，共享策略网络能促使代价策略不那么保守、奖励策略更注重安全，从而实现更均衡的性能。
- **灵活性突出**：在各类代价限制配置下，均展现出优于基线的灵活性，能很好适应不同安全要求，且无需重新训练。

### 7. 优点
- **方法论创新**：提出了一种新颖的“包装器”框架，将适应变约束的问题巧妙地分解为训练时的策略集学习和测试时的动态动作切换，易于实现和集成。
- **实验充分扎实**：在大量、多样化的基准任务上进行了评估，并与多种强基线进行了全面对比，通过详尽的消融实验证实了各设计选择的有效性。
- **实用性强**：框架无需为新的约束阈值重新训练，极大降低了部署成本，并能充分利用不断进步的离线强化学习基础算法来提升性能。
- **理论支撑**：对安全决策过程提供了有限条件下的理论安全保证分析。

### 8. 不足与局限
- **价值函数估计依赖**：方法和理论保证都高度依赖代价Q函数的准确估计。尽管论文声称使用离线强化学习训练的价值函数比离策略评估更可靠，但估计误差在复杂环境中依然存在，可能影响决策的准确性和安全性。
- **策略覆盖范围有限**：仅使用2个极端策略在面对高度非线性的帕累托前沿时，可能存在覆盖盲区。中间策略的生成依赖于线性标量化，可能无法探索到所有最优的代价-奖励权衡点。
- **资源开销未明确**：未提供具体的计算资源需求，可能影响其他研究者复现和评估其资源效率。
- **理论假设严格**：理论分析要求完美估计代价Q函数以及MDP满足“最优代价变异有界”的假设，这在现实随机环境中可能不成立。

（完）
