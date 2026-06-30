---
title: "TW-CRL: Time-Weighted Contrastive Reward Learning for Efficient Inverse Reinforcement Learning"
title_zh: TW-CRL：高效逆强化学习的时间加权对比奖励学习
authors: "Yuxuan Li, Yicheng Gao, Ning Yang, Stephen Xia"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39499/43460"
tags: ["query:mt"]
score: 9.0
evidence: 使用时间加权对比奖励学习的逆强化学习，从演示中高效学习
tldr: TW-CRL针对强化学习中稀疏奖励和隐蔽陷阱状态问题，提出一种逆强化学习框架。它利用成功和失败的演示，通过时间加权对比学习提取关键状态，学习稠密奖励函数，从而在具有陷阱状态的稀疏奖励任务中显著提高学习效率，为RL在复杂环境下的应用提供了新方法。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39499/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1830, \"height\": 616, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39499/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 883, \"height\": 711, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39499/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1822, \"height\": 847, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39499/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 875, \"height\": 1260, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39499/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 866, \"height\": 1260, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39499/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1843, \"height\": 419, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39499/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 880, \"height\": 301, \"label\": \"Table\"}]"
motivation: 稀疏奖励和陷阱状态导致强化学习效率低下，现有方法缺乏从失败演示中利用时间信息的能力。
method: 提出TW-CRL，结合时间和对比学习从演示中学习稠密奖励函数。
result: 在GridWorld和Minigrid等具备陷阱状态的任务中，TW-CRL学习速度更快且成功率更高。
conclusion: 通过捕捉时间关键状态，TW-CRL有效引导智能体规避陷阱，加速策略学习。
---

## Abstract
Episodic tasks in Reinforcement Learning (RL) often pose challenges due to sparse reward signals and high-dimensional state spaces, which hinder efficient learning. Additionally, these tasks often feature hidden “trap states”—irreversible failures that prevent task completion but do not provide explicit negative rewards to guide agents away from repeated errors. To address these issues, we propose Time-Weighted Contrastive Reward Learning (TW-CRL), an Inverse Reinforcement Learning (IRL) framework that leverages both successful and failed demonstrations. By incorporating temporal information, TW-CRL learns a dense reward function that identifies critical states associated with success or failure. This approach not only enables agents to avoid trap states but also encourages meaningful exploration beyond simple imitation of expert trajectories. Empirical evaluations on navigation tasks and robotic manipulation benchmarks demonstrate that TW-CRL surpasses state-of-the-art methods, achieving improved efficiency and robustness.

---

## 论文详细总结（自动生成）

# 论文总结：TW-CRL: Time-Weighted Contrastive Reward Learning for Efficient Inverse Reinforcement Learning

## 1. 核心问题与整体含义
- 在分幕式（episodic）强化学习任务中，稀疏奖励信号和高维状态空间使得高效学习非常困难。
- 这类任务常存在隐藏的“陷阱状态”（trap states）：一旦进入，即使不给出显式负奖励，也会导致任务不可逆失败，而智能体缺乏负面反馈来避免重蹈覆辙。
- 现有的逆强化学习（IRL）方法主要基于成功的专家示范进行模仿，往往忽略失败经历，限制了探索能力，且无法有效识别和规避陷阱状态。
- 论文提出 **时间加权对比奖励学习（TW‑CRL）**，旨在从成功和失败的示范中同时提取时间信息，学习一个既能识别目标态又能预警陷阱态的稠密奖励函数，从而提高学习效率、避开陷阱并鼓励超越简单模仿的有意义探索。

## 2. 方法论
### 2.1 陷阱状态的形式化定义
- 在分幕式马尔可夫决策过程（Episodic MDP）中，将陷阱状态集 \(S_{\text{trap}}\) 定义为：一旦进入，无论采取什么动作，下一状态仍属于陷阱集，且目标状态不属于陷阱集。  
- 实践中，将失败轨迹的终点视为“策略依赖”的陷阱状态代理，用于构造负反馈。

### 2.2 时间加权函数
- 将智能体的动态建模为吸收马尔可夫链：成功演示中目标态为吸收态，失败演示中陷阱态为吸收态。
- 推导出在每个时间步 \(t\) 下，状态属于目标态（成功）或陷阱态（失败）的条件概率，并共享同一个函数形式：
  \[
  w(t) = 1 - \left(1 - \frac{e^{\alpha t} - 1}{e^{\alpha T} - 1}\right)^t
  \]
  其中 \(\alpha > 0\) 为超参数，\(T\) 为轨迹长度。
- 该函数为轨迹后期状态赋予更高权重，以反映其对最终结果的关键影响。

### 2.3 对比奖励学习
- 为每个状态构造估计奖励：成功演示中 \( r_{\text{est}}(s_t) = w(t) \)，失败演示中 \( r_{\text{est}}(s_t) = -w(t) \)。
- 使用均方误差损失训练奖励函数 \(r_\phi\)：
  \[
  L_{\text{CRL}} = \frac{1}{N} \left[ \sum_{\tau \in D^+} \sum_{s_t \in \tau} (r_\phi(s_t) - w(t))^2 + \sum_{\tau \in D^-} \sum_{s_t \in \tau} (r_\phi(s_t) + w(t))^2 \right]
  \]
- 该损失引导奖励网络区分有利于成功的关键状态和通向失败的危险状态。

### 2.4 完整算法流程
1. 用少量成功示范初始化数据集。
2. 利用时间加权函数为所有状态分配估计奖励标签。
3. 最小化对比损失训练奖励函数 \(r_\phi\)。
4. 基于学到的奖励函数用任意RL算法（如TD3）优化策略。
5. 让智能体与环境交互，收集新的成功/失败轨迹并加入数据集。
6. 重复步骤2‑5，交替提升奖励函数和策略质量。
- 方法可扩展至无明确成功/失败终端的任务（设定回报阈值划分轨迹）以及连续任务（截断到固定步长）。

## 3. 实验设计
### 3.1 测试环境与基准
- 共使用 **8 个环境**：U‑Maze、TrapMaze‑v1、TrapMaze‑v2（含隐藏陷阱状态和随机目标位置）、HumanoidStand、Ant、MountainCarContinuous、PandaReach、PandaPush。
- 对比了 **6 种基线方法**：GAIL、AIRL、MaxEnt、MERIT、ICRL（使用次优示范）、SASR（利用失败示范）。

### 3.2 评估方式
- 衡量 **平均分幕回报**（average episodic return）及标准差（5次独立运行）。
- 在TrapMaze‑v1上可视化学习到的奖励函数，分析不同方法对陷阱态和目标的识别能力。
- 泛化性测试：改变目标初始化的网格范围（1、3、任意），检验策略是否适应未见过的目标分布。
- 消融实验：验证时间加权函数相较于常数 +1/‑1 标签的作用，以及使用成功+失败演示相较于仅用成功、仅用失败或无演示的优势。

## 4. 资源与算力
- 论文正文及已提供的内容中 **未明确说明** 所使用的GPU型号、数量、训练时长或计算开销。
- 仅在附录位置提及超参数和网络架构细节，但附录内容未包含在本次提取文本中。

## 5. 实验数量与充分性
- **主要对比实验**：8 个环境 × 6 种基线，各运行 5 次，提供均值和标准差，总计约 280 组对比数据点，评估客观且公平。
- **奖励函数可视化**：详细展示训练早、中、后期的奖励地图，直观对比各方法的行为，不止依赖数值指标。
- **泛化实验**：在TrapMaze‑v1上测试 3 种目标分布（1、3、任意），每种方法均给出平均回报，覆盖广泛。
- **消融实验**：分别隔离时间加权和对比学习两大模块，在TrapMaze‑v1和v2上验证各自贡献，分析充分。
- 总体而言，实验设计全面，从多个维度验证了方法的有效性和鲁棒性。

## 6. 主要结论与发现
- TW‑CRL 在 **所有测试环境** 中均达到或超越所有基线方法，尤其在含有陷阱状态的迷宫任务中优势显著（如U‑Maze提升41%~97%）。
- 学习的奖励函数能够 **自动识别** 隐藏陷阱状态并为目标区域分配高值，同时不惩罚早期中性状态，从而保留了探索空间。
- 智能体不仅可以模仿专家路径，还能 **发现未在示范中出现过的捷径和替代路径**，展现出更强的探索能力。
- 方法对目标位置的变化具有良好 **泛化性**，即使训练时目标仅出现在有限区域，测试时仍能适应更广的目标分布。
- 时间加权和对比式利用失败示范是提升性能的关键，两者结合效果最佳，尤其在更复杂的环境（TrapMaze‑v2）中优势明显。

## 7. 优点
- **双源信息利用**：首次在IRL框架内同时融合成功和失败示范，充分利用训练初期产生的大量失败数据。
- **时间感知建模**：通过吸收马尔可夫链推导出简洁的时间加权函数，自动突出轨迹后期的关键状态，无需手工设计奖励。
- **鼓励探索**：奖励函数不盲目复制示范，对非专家但无害的状态保持中性，赋予智能体发现新策略的可能。
- **理论清晰**：明确定义陷阱状态，并提供从概率推导到损失函数的完整数学支撑。
- **可扩展性**：方法可直接适配无明确终端的episodic任务和连续任务，拓宽适用场景。
- **代码开源**，具有良好的可复现性。

## 8. 不足与局限
- **假设限制**：陷阱状态的定义在理论上要求完全吸收，实际中依赖失败轨迹的终止态作为代理，可能漏判或误判部分陷阱区域。
- **超参数敏感性**：时间加权函数中的 \(\alpha\) 以及拓展任务中的回报阈值 \(r_\theta\) 需根据环境调节，未讨论其鲁棒性。
- **实验局限**：所有评估均在仿真环境中进行，缺少真实机器人任务验证；此外，未与若干同样利用失败示范的工作（如IRLF、SQIL等）直接比较，比较范围仍有扩展空间。
- **学习成本**：尽管方法在样本效率上优于基线，但迭代式奖励学习和策略更新仍需要多轮交互，实际训练成本未与模仿学习类方法（如GAIL）做定量比较。
- **复杂陷阱形态**：论文仅讨论了单一陷阱区域，对于环境中多个相互关联或动态变化的陷阱场景未做深入分析。

（完）
