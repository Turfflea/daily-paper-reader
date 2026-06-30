---
title: Efficient Learning of Predictive Maps for Flexible Planning
title_zh: 面向灵活规划的预测地图高效学习
authors: "Bazarjani, A., Piray, P."
date: 2026-06-22
pdf: "https://www.biorxiv.org/content/10.64898/2026.02.11.705395v2.full.pdf"
tags: ["query:mt"]
score: 9.0
evidence: 带重要性采样的TD学习构建策略无关的预测图用于规划。
tldr: 认知地图通过可复用的预测性内部表征支持灵活行为，但后继表征的策略依赖性严重限制了规划。本文引入重要性采样的后继表征模型SR-IS，结合时序差分与重要性采样，学习策略无关的环境结构，在环境变化时可高效更新。SR-IS在规划任务中超越现有模型，并首次解释了人类重新规划中的分级偏差。该工作连接了预测地图理论与实际规划行为，为大脑灵活决策机制提供新视角。
source: biorxiv
selection_source: fresh_fetch
motivation: 现有的后继表征受策略依赖限制，难以支持灵活规划，需构建策略无关预测地图。
method: 提出SR-IS模型，融合时序差分与重要性采样，学习策略无关的预测地图并实现高效更新。
result: SR-IS在规划任务中优于现有模型，能首次解释人类重新规划中的分级偏差。
conclusion: 该工作连接预测地图理论与灵活规划行为，揭示大脑决策新机制，为理解灵活决策提供新见解。
---

## 摘要
认知地图通过提供可复用的任务结构内部表征，支持灵活行为。后继表征是一种编码预期未来状态占用的预测地图，已被提出作为大脑中计算此类地图的一种可能方式，但其对策略的依赖性严重限制了灵活规划。我们引入一种新模型——重要性采样后继表征（SR-IS），它将时序差分学习与重要性采样相结合，构建与策略无关的预测地图。SR-IS 学习环境的结构，而不受智能体当前决策策略的约束。当环境变化时，这些表征可以被高效更新，从而实现快速的行为适应。我们证明，SR-IS 在规划任务中优于现有模型，并对人类重新规划中的分级偏差提供了更好的解释，而之前的模型无法解释这一点。这项工作将预测地图理论与观察到的规划行为联系起来，并为大脑中的灵活决策提供了新见解。

## Abstract
Cognitive maps enable flexible behavior by providing reusable internal representations of task structure. The successor representation, a predictive map that encodes expected future state occupancy, has been proposed as one way such maps might be computed in the brain, but its policy dependence severely limits flexible planning. We introduce a new model, the successor representation with importance sampling (SR-IS), which combines temporal-difference learning with importance sampling to construct policy-independent predictive maps. SR-IS learns the structure of the environment without being constrained by the agents current decision policy. These representations can be efficiently updated when the environment changes, enabling rapid behavioral adaptation. We show that SR-IS outperforms existing models in planning tasks and provides a better account of the graded biases in human replanning that previous models could not explain. This work bridges theories of predictive maps with observed planning behavior and offers new insights into flexible decision-making in the brain.

---

## 论文详细总结（自动生成）

# 论文《Efficient Learning of Predictive Maps for Flexible Planning》详细总结

## 1. 论文的核心问题与整体含义
- **研究背景**：认知地图是大脑支持灵活行为的一种内部表征，它允许复用任务结构的知识。后继表征（Successor Representation, SR）作为一种编码预期未来状态占用的预测地图，被广泛用来解释大脑如何计算认知地图。
- **核心问题**：传统的后继表征严重依赖于智能体当前的行为策略（policy-dependent）。当策略改变或环境需要灵活规划时，这种依赖性成为一个关键瓶颈，限制了快速行为适应和在不同目标下的复用能力。
- **整体含义**：本文提出一种策略无关的预测地图学习方法，核心目标是使智能体在学习环境结构时不固着于某一特定策略，从而在环境变化或重新规划时能高效更新表征并支持灵活的决策。

## 2. 论文提出的方法论
- **方法名称**：重要性采样后继表征（Successor Representation with Importance Sampling, SR-IS）
- **核心思想**：将时序差分学习（Temporal-Difference Learning, TD）与重要性采样（Importance Sampling）技术相融合。通过重要性采样，模型可以在学习阶段用行为策略采样数据，却能估计目标策略下的后继表征，从而剥离学习过程对当前决策策略的依赖。
- **技术细节**（文字描述）：
  - 传统SR更新依靠与特定策略相关的状态转移，SR-IS在更新每一步时，会乘以重要性权重（目标策略与行为策略在该动作上的概率之比），从而校正策略偏移。
  - 最终得到的预测地图仅编码环境本身的转移结构，而非某个特定策略下的占用信息。
  - 当环境发生变化时，这些策略无关的表征可以通过局部更新高效调整，无需从头重新学习，实现快速行为适应。
- **公式或算法流程**：文中未提供完整公式，但根据描述，其算法流程可概括为：
  1. 智能体按行为策略与环境交互，收集状态-动作-下一状态序列。
  2. 对每步转移，计算重要性采样权重 w = π_target(a|s) / π_behavior(a|s)。
  3. 使用该权重调制的TD误差，更新SR矩阵中对应状态-状态对的预期未来占用。
  4. 重复直至收敛，得到不依赖行为策略的预测地图。

## 3. 实验设计
- **数据集/场景**：论文进行了规划任务实验和人类行为数据拟合（重新规划任务）。具体场景未在给出内容中详述，但涉及环境变化下的重新规划。
- **Benchmark与对比方法**：与现有模型进行对比，其中包括传统后继表征（策略依赖版本）及其他未明确列出的规划模型。SR-IS在规划性能上优于这些模型，并且是唯一能够解释人类重新规划中出现的分级偏差（graded biases）的模型。
- **评价维度**：规划任务表现、对人类行为偏差的解释力。

## 4. 资源与算力
- **文中提及情况**：所提供的摘要及元数据中，**没有明确说明**所使用的GPU型号、数量、训练时长以及具体算力资源。由于内容是简要摘要，无法获知计算资源细节。

## 5. 实验数量与充分性
- **实验数量**：从简短摘要可推断至少包含以下几类实验：
  - 规划任务上对比SR-IS与现有模型的性能。
  - 人类重新规划行为的分级偏差拟合实验。
  - 可能还包括表征更新效率的验证（当环境变化时高效更新）。
- **充分性与公平性分析**：摘要强调SR-IS“优于现有模型”且“首次解释”了前人无法解释的现象，暗示对比实验至少覆盖了主要竞争者。但未提供统计显著性、样本量、重复次数等细节，因此无法从已提供材料判断实验是否在统计上充分。实验看似公平，因为模型在同一任务上比较，且解释了同一种行为数据。

## 6. 论文的主要结论与发现
- SR-IS成功构建了策略无关的预测地图，解除了传统SR对行为策略的依赖。
- 在规划任务中，SR-IS表现超越现有的基于SR的模型。
- SR-IS首次为人类重新规划中观察到的分级偏差提供了计算层面的解释，而之前模型无法做到。
- 这项工作架起了预测地图理论与灵活规划行为之间的桥梁，并为理解大脑中的灵活决策机制提供了新见解。

## 7. 优点
- **方法论创新**：将重要性采样巧妙引入SR学习，解决了长期存在的策略依赖问题，思路简洁且有效。
- **理论贡献**：将预测地图（认知地图）与实际规划行为联系起来，统一了表征学习与行为决策的解释。
- **行为解释力**：成功捕捉到人类重新规划中的细微偏差（分级偏差），超越了先前的计算模型。
- **高效自适应**：模型在环境变化时可快速更新，更符合生物智能的灵活性需求。

## 8. 不足与局限
- **细节缺失**：摘要未提供实验的具体设计、数据集规模、统计检验等，难以评估结果的稳健性。
- **应用范围未知**：仅提及规划任务和人类行为拟合，可能局限于离散状态空间，尚未在大规模或连续控制任务中验证。
- **计算成本**：重要性采样可能引入高方差，文中没有讨论方差控制技术或其对收敛性的影响。
- **生物学合理性**：虽然模型旨在为大脑提供新见解，但所需的中心化SR矩阵和精确重要性权重计算在神经网络中如何实现尚不明确。
- **对比范围**：对比的基线可能有限，未与最新的基于模型的规划算法或基于深度强化学习的认知地图模型进行系统比较。

（完）
