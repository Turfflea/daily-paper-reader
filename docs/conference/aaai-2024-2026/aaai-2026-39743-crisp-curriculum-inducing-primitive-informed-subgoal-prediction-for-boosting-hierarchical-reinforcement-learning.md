---
title: "CRISP: Curriculum-Inducing Primitive Informed Subgoal Prediction for Boosting Hierarchical Reinforcement Learning"
title_zh: CRISP：课程诱导的基元知情子目标预测以提升分层强化学习
authors: "Utsav Singh, Vinay P. Namboodiri"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39743/43704"
tags: ["query:mt"]
score: 8.0
evidence: 课程引导的基元知情子目标预测增强分层强化学习
tldr: 分层强化学习因低层基元持续更新导致高层子目标过时，引入非平稳性问题。本文提出CRISP，一种课程驱动框架，包含基元知情解析（PIP）自适应重标注示范子目标，以及逆强化正则化器引导高层策略。在多个长时域任务上，CRISP显著提高了训练稳定性和最终性能，为稳定HRL提供了有效手段。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39743/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 871, \"height\": 694, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39743/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1202, \"height\": 780, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39743/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1828, \"height\": 396, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39743/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 881, \"height\": 528, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39743/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 831, \"height\": 531, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39743/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 873, \"height\": 532, \"label\": \"Table\"}]"
motivation: 分层强化学习因低层基元持续更新导致高层子目标过时，引入非平稳性问题。
method: 提出课程驱动框架，包含基元知情解析（PIP）自适应重标注示范子目标，以及逆强化正则化器。
result: 在多个长时域任务上显著提高了训练稳定性和最终性能。
conclusion: 通过课程学习和子目标重标注，有效缓解了HRL中的非平稳性挑战。
---

## Abstract
Hierarchical reinforcement learning (HRL) leverages temporal abstraction to efficiently tackle complex long-horizon tasks. However, HRL often collapses because the low-level primitive’s continual updates make earlier sub-goals issued by the high-level policy obsolete, introducing non-stationarity that destabilizes training. We propose CRISP, a curriculum-driven framework that tackles this instability with three key ingredients: (1) primitive-informed parsing (PIP), which adaptively re-labels a handful of expert demonstrations to always generate reachable subgoals by the current low-level primitive; (2) an inverse-reinforcement-learning regularizer that steers the high-level policy toward the expert-induced subgoal distribution and stabilizes learning; and (3) a unified training loop that leverages these components to boost sample efficiency. Across six sparse-reward robotic navigation and manipulation benchmarks, CRISP improves success rates by more than 40% over strong hierarchical and flat baselines and successfully transfers to real-world tasks, demonstrating the promise of curriculum-based HRL for practical scenarios.

---

## 论文详细总结（自动生成）

好的，我将基于提供的论文内容，生成一份详细的结构化中文总结。

### 1. 论文的核心问题与整体含义

*   **核心问题**：分层强化学习通过时间抽象来分解复杂的长程任务，但常因**非平稳性**问题而失效。具体来说，高层策略为低层策略（基元）设定的子目标会随着低层策略的持续更新而过时，导致高层策略需要不断适应一个“移动的靶子”，从而破坏训练的稳定性。
*   **整体含义**：论文旨在通过引入一种**课程学习**框架来解决分层强化学习中的非平稳性问题，使子目标的难度能够动态适应低层策略的能力演化，从而稳定训练过程并提高稀疏奖励环境下的样本效率与成功率。

### 2. 论文提出的方法论

CRISP框架的核心思想是集成强化学习与模仿学习，通过三个关键组件来稳定分层训练：

*   **基元知情解析（Primitive-Informed Parsing, PIP）**
    *   **核心思想**：周期性地根据当前低层基元的能力，将少量的专家示范轨迹自适应地重新分割，生成一个由**可达子目标对**构成的数据集。
    *   **工作流程**：PIP从专家轨迹的起始状态开始，将轨迹中的每个后续状态依次作为子目标交给低层基元执行。如果基元在设定的`c`个时间步内无法到达某个状态，则将其前一个已成功到达的状态标记为一个可达子目标，并将其设为新的起点，继续解析后续轨迹。这个过程确保了生成的所有子目标都是当前低层基元有能力完成的。

*   **基于逆强化学习的正则化（IRL Regularization）**
    *   **核心思想**：利用PIP生成的可达子目标数据集，训练一个判别器来区分专家子目标和策略生成的子目标，并使用类似GAIL（生成对抗模仿学习）的IRL损失函数来引导高层策略，使其生成的子目标分布与数据集中的可达子目标分布难以区分。
    *   **公式**：高层策略的目标是最大化其强化学习目标函数与IRL正则化项的加权和，如公式(3)所示，其中`ψ`是平衡因子。低层策略也采用了类似的正则化（公式4），但其使用的是原始的专家动作数据，以确保其行为接近专家。IRL损失的优化目标如公式(1)和(2)所示，本质上是一个极小极大博弈。

*   **统一训练循环**
    *   **流程**：训练过程交替进行。每隔`p`个epoch，利用PIP重新生成一次子目标数据集。在其他时间步，使用标准强化学习算法（SAC）收集经验并更新两个层级策略，更新时同时结合任务奖励和IRL正则化项。

### 3. 实验设计

*   **基准测试平台**：在六个具有连续状态和动作空间的复杂稀疏奖励机器人任务上进行评估：
    1.  迷宫导航 (maze navigation)
    2.  拾取与放置 (pick and place)
    3.  装箱 (bin packing)
    4.  中空操作 (hollow)
    5.  绳索操作 (rope manipulation)
    6.   Franka Kitchen 复杂任务套件
*   **对比方法**：与多个强基线方法进行了比较，包括：
    *   **分层方法**：RPL (固定窗口解析), HAC, SAGA, RAPS (手工基元), HIER (分层SAC), HIER-NEG (带惩罚的分层SAC)。
    *   **平面方法**：DAC (基于判别器的模仿学习), FLAT (标准SAC策略), BC (行为克隆)。
*   **真实世界验证**：在四轴Dobot Magician机械臂上，对拾取与放置、装箱和绳索操作三个任务进行了真实世界迁移实验。

### 4. 资源与算力

*   论文**未明确提及**具体的GPU型号、数量或总训练时长。文中只说明了训练使用了Adam优化器和SAC算法，并提供了代码但未详述算力开销。

### 5. 实验数量与充分性

*   **实验数量**：实验设计较为全面。主实验覆盖了6个模拟环境与多个基线方法的全面对比；补充材料中还包含了大量消融实验，分析了关键超参数（`p`, `ψ`）、专家示范数量、解析窗口大小等对性能的影响；此外还包含了真实世界机器人实验和非平稳性指标分析。
*   **实验充分性、客观性与公平性**：
    *   **公平性**：论文明确指出，所有基线方法均基于其原始论文推荐的参数范围进行了独立的超参数网格搜索，以确保对比的公平性。
    *   **客观性**：在模拟环境中，所有结果均以五个随机种子下的平均成功率报告，并给出了标准差，增强了结果的可信度。
    *   **充分性**：系统回答了论文提出的六个核心问题（Q1-Q6），从性能提升、非平稳性缓解、课程效果可视化到真实世界迁移和设计选择影响，论证链条完整。

### 6. 论文的主要结论与发现

1.  **显著性能提升**：CRISP在所有六个模拟基准测试中，成功率比最强的分层和平面基线方法高出40%以上，并展现出更快的收敛速度和更稳定的训练过程。
2.  **有效缓解非平稳性**：通过对比高层策略提出的子目标与低层基元实际达到状态之间的距离，证明CRISP能在整个训练过程中持续生成可达的子目标，从而有效缓解了分层强化学习的非平稳性问题。
3.  **动态课程生成**：PIP和IRL正则化共同作用，生成了一种隐式课程。随着训练进行，低层基元能力提升，CRISP提出的子目标也会变得越来越复杂和遥远，自然地从简单任务过渡到复杂任务。
4.  **真实世界可迁移性**：在仿真中训练的CRISP策略能够直接迁移到真实世界的机械臂任务中，并取得显著优于基线方法的成功率，展示了其在现实场景中的应用潜力。

### 7. 优点

*   **创新性**：提出PIP方法，根据低层策略的实时能力动态解析示范数据，这是一个直观且有效的解决非平稳性问题的新角度。
*   **方法优雅**：CRISP将课程学习的思想自然地融入分层强化学习框架中，通过IRL正则化优雅地将模仿学习与强化学习结合，无需复杂的手工设计。
*   **实验扎实**：在多个稀疏奖励的复杂机器人任务上进行了详尽评估，覆盖了分层和平面基线，并进行了真实世界验证，证明其方法的鲁棒性和实用性。
*   **分析深入**：不仅展示了性能提升，还通过可视化子目标课程和分析非平稳性指标，深刻揭示了方法有效背后的运作机制。

### 8. 不足与局限

*   **对环境重置的依赖**：PIP算法在解析专家示范时，假设环境可以重置到示范中的任意状态。这在现实世界中直接应用成本高昂或不切实际，论文也承认了这一点，并讨论了一些未来改进方向。
*   **对示范数据的敏感性**：虽然论文分析了示范数量和质量的影响，但CRISP本质上仍依赖专家示范。在示范稀缺或质量不佳的领域，其效果可能会大打折扣。
*   **超参数`p`的计算开销**：PIP的周期性执行会引入一定的计算开销，尤其是在长程任务中，虽然论文声称此开销“处于边缘水平”，但未提供定量分析。
*   **泛化性局限**：真实世界实验仅在一个型号的桌面机械臂上进行，其在更复杂的移动操作或高动态环境中的泛化能力有待进一步验证。

（完）
