---
title: Inverse reinforcement learning reveals action-oriented value signals in naturalistic decision making
title_zh: 逆强化学习揭示自然决策中面向行动的价值信号
authors: "Lee, S. H., Chung, C., Oh, M.-h., Ahn, W.-Y."
date: 2026-06-29
pdf: "https://www.biorxiv.org/content/10.64898/2026.06.24.733779v1.full.pdf"
tags: ["query:mt"]
score: 9.0
evidence: 应用逆向强化学习从驾驶任务中的序列决策推断价值，符合用于序列决策和优化的强化学习。
tldr: 自然决策环境下，目标导向行为的价值计算是认知神经科学的挑战。标准计算模型难以刻画实时行为。本研究采用逆强化学习（IRL）从fMRI驾驶任务行为中推断逐刻奖励轨迹。结果显示，IRL奖励轨迹最稳定地与背侧纹状体等奖励脑区活动相关，也涉及认知控制与感觉运动网络。这表明IRL奖励可作为基于行为、时间分辨的行动导向估值信号，为复杂决策研究提供新框架。
source: biorxiv
selection_source: fresh_fetch
motivation: 自然决策中的价值计算难以用标准模型刻画，需要适合实时行为的神经计算方法。
method: 采用逆强化学习（IRL）从fMRI驾驶任务的行为中推断逐刻奖励轨迹，并分析其与脑活动的关联。
result: IRL奖励轨迹最稳定地与背侧纹状体等奖励脑区活动相关，也广泛涉及认知控制与感觉运动区域。
conclusion: IRL奖励可作为基于行为的、时间分辨的行动导向估值信号，为自然决策神经计算研究提供新框架。
---

## 摘要
认知神经科学的一个主要挑战是如何解释在复杂和自然环境中目标导向行为的价值是如何计算的。标准的决策计算模型在受控的、基于试次的范式中取得了巨大成功，但往往不适用于自然范式中实时展开的行为。逆强化学习（IRL）提供了一种从自然环境中观察到的行为中推断潜在评价状态的方法，但其神经可解释性在很大程度上仍是未知的。在这里，我们研究了在fMRI扫描期间执行的实时驾驶任务中，由IRL推导出的逐刻奖励轨迹是否映射到大脑中的价值信号。IRL推导出的奖励轨迹与背侧纹状体的活动关联最为稳健，这一脑区通常与价值引导的行动选择相关。它们还显示出与支持其他过程的分布式脑区的关联，包括认知控制和感觉运动处理。这种模式表明，IRL奖励捕捉了以奖励回路为中心的分布式神经活动，可能反映了评估如何与其他过程相互作用。总之，这些发现表明，IRL奖励为自然决策过程中面向行动的评估提供了一个以行为为基础、时间解析的代理指标。

## Abstract
A major challenge for cognitive neuroscience is to explain how value of a goal-directed behavior is computed in complex and naturalistic environments. Standard computational models of decision making have been highly successful in controlled, trial-based paradigms, but they are often ill-suited to real-time behavior unfolding in naturalistic paradigms. Inverse reinforcement learning (IRL) offers a way to infer latent evaluative state from observed behavior in naturalistic environments, but its neural interpretability remains largely unknown. Here, we investigated whether moment-to-moment reward trajectories derived from IRL map onto value signals in the brain during a real-time driving task performed during fMRI scanning. IRL-derived reward trajectories were most robustly associated with activity in the dorsal striatum, a region often linked to value-guided action selection. They also showed associations with distributed regions supporting additional processes, including cognitive control and sensorimotor processing. This pattern suggests that IRL reward captures distributed neural activity centered on the reward circuitry, potentially reflecting how valuation interacts with other processes. Together, these findings suggest that IRL reward provides a behaviorally grounded, temporally resolved proxy for action-oriented valuation during naturalistic decision making.

---

## 论文详细总结（自动生成）

# 论文总结：逆强化学习揭示自然决策中面向行动的价值信号

## 1. 论文的核心问题与整体含义
- **研究动机**：认知神经科学面临的核心挑战是，人类在复杂、自然的日常环境中如何实时计算目标导向行为的价值。传统计算模型主要在严格控制的试次式范式中成功，难以直接适用于自然任务里连续、实时展开的行为。
- **整体含义**：本文探索**逆强化学习（IRL）** 能否填补这一空白——即从自然行为的观测数据中推断出潜在的、逐刻变化的“奖励轨迹”，并检验这种轨迹是否具有真实的神经基础（即能否映射到大脑的价值回路）。若成功，IRL奖励将成为一个基于行为、时间解析的“面向行动的价值信号”代理，为自然决策的神经计算研究提供新框架。

## 2. 方法论
- **核心思想**：不预设固定的奖励函数，而是**从观测行为逆向推导主体内部的潜在奖励评估序列**。具体做法是从被试在执行自然任务（这里是驾驶）时的决策序列中，利用IRL推测出每一时刻的“奖励轨迹”。
- **技术路线**（基于摘要推断）：
    - **输入**：自然驾驶任务中的实时操作行为（如转向、速度控制等），以及可能的任务环境状态。
    - **模型**：采用逆强化学习算法，假设行为是由一个潜在回报函数驱动的最优或近似最优策略产生，反推出逐刻（moment-to-moment）的奖励值。
    - **神经验证**：将IRL推导出的奖励时间序列作为回归量，与fMRI血氧水平依赖（BOLD）信号进行全脑相关分析，观察其能否预测奖励相关脑区的活动。
- **未在摘要中说明**：具体IRL算法类型（如最大熵IRL、贝叶斯IRL等）、状态空间定义、策略表示等细节。

## 3. 实验设计
- **数据集 / 场景**：
    - **任务**：被试在fMRI扫描期间执行实时驾驶任务（real-time driving task），构成自然、连续决策环境。
    - **数据模态**：行为数据（驾驶操作序列）+ 全脑fMRI BOLD信号。
- **比较基准**：
    - 摘要未明确列出对比的 baseline 模型。但从结论“IRL奖励轨迹与纹状体活动关联最为稳健”可推断，研究很可能**将IRL奖励轨迹与其它候选价值模型**（如基于任务客观成功的奖励、简单行为指标、或传统强化学习模型的价值信号）进行了对比，以验证IRL推断的神经可解释性是否更优。
- **对比思路**：通过脑区激活模式的特异性与稳定性，判断哪种计算价值信号最能捕捉真实的神经评估过程。

## 4. 资源与算力
- **文中未明确说明**：提供的摘要和元数据中没有任何关于GPU型号、数量、训练时长等算力资源的说明。IRL算法的计算量通常取决于状态-动作空间的规模和轨迹长度，在驾驶任务中可能可控，但确切消耗未知。

## 5. 实验数量与充分性
- **实验组数估计**：本文至少包含核心的IRL推导与fMRI关联分析，并很可能包含一系列控制分析或对比分析（例如，对比IRL奖励在不同脑网络中的表现，检验其与运动、认知控制区域的交互）。但由于信息有限，无法准确估计消融实验、不同参数设置或不同数据集的数量。
- **充分性与公平性评价**：
    - **客观性**：使用数据驱动的IRL从行为中推导价值，再与全脑BOLD做统计关联，流程相对客观。
    - **充分性可能受限**：摘要仅报告了与背侧纹状体的稳健关联，以及广泛分布网络的关联。要确认结果的可靠性，需要更详细的统计检验（如交叉验证、被试间一致性）、稳健性检验（如不同IRL超参数）以及对混淆因素的控制。这些在摘要中未呈现。
    - **公平性**：若对比了不同价值模型，需确认对比条件是否公平（若对比，可能使用相同的fMRI回归模型框架）。

## 6. 主要结论与发现
- **核心发现**：由IRL推导的逐刻奖励轨迹**最稳健地映射到背侧纹状体（dorsal striatum）的活动**，该脑区是价值引导行动选择的经典区域。
- **广泛关联**：IRL奖励还显示出与**认知控制网络、感觉运动处理区域的活动相关**，表明价值计算与这些过程在自然行为中动态交织。
- **理论贡献**：这些结果表明，IRL奖励可作为一个**基于真实行为、时间解析度高、面向行动的价值代理信号**，能够有效捕捉自然决策中围绕奖励回路的分布式神经活动，为研究复杂动态环境下的决策神经机制提供了新的有效工具。

## 7. 优点
- **方法创新**：将逆强化学习引入自然决策的认知神经科学研究，解决了传统模型难以处理连续、实时行为的难题。
- **时间解析力**：得到的是逐刻价值信号，远超传统试次平均模型的时间分辨率。
- **神经验证**：不只是算法应用，而是用fMRI数据验证了IRL奖励的神经可解释性，建立了行为推断与脑活动的直接桥梁。
- **生态效度**：采用自然驾驶任务，更贴近真实决策情境，增强了结论的普适性。

## 8. 不足与局限
- **任务特异性**：仅在一种自然任务（驾驶）上验证，能否推广到其他复杂决策情境（如社会互动、战略博弈）存疑。
- **模型细节缺失**：摘要未提供IRL的具体实现、关键假设和超参数，可复现性和清晰度受影响。
- **因果性不明**：相关性分析只能说明IRL奖励与脑区活动共变，无法证明该信号是大脑实际计算的价值表征，也无法排除反向或第三方驱动。
- **对比基线未知**：未明确与其他候选模型（如简单动作价值、规则基奖励）的对比结果，难以评判IRL带来的增益究竟多大。
- **样本量与统计力**：未报告被试数量，可能影响结果的稳健性。

（完）
