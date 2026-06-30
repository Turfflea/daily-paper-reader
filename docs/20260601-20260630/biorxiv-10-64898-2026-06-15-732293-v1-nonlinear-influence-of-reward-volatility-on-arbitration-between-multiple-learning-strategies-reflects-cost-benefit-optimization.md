---
title: Nonlinear influence of reward volatility on arbitration between multiple learning strategies reflects cost-benefit optimization
title_zh: 奖赏波动性对多种学习策略间仲裁的非线性影响反映成本效益优化
authors: "Yamada, T., Samejima, K."
date: 2026-06-19
pdf: "https://www.biorxiv.org/content/10.64898/2026.06.15.732293v1.full.pdf"
tags: ["query:mt"]
score: 10.0
evidence: 研究无模型与基于模型强化学习的仲裁。
tldr: 在复杂多变的环境中，人类需要在模型免费（习惯性）和模型基（目标导向）两种强化学习系统间动态权衡。奖励波动性（volatility）作为环境变化的速度与频率指标，如何调节此权衡尚存争议。本研究通过两个修正的两步决策任务，系统操纵奖励波动性与时间压力，结合行为分析、计算模拟和分层贝叶斯模型拟合，发现奖励波动性以倒U型非线性影响策略仲裁：中等波动性下模型基策略使用最多，但仅出现在已习得任务转移结构的个体中；时间压力促发模型免费策略。模拟表明中等波动性下模型基策略相对优势最大。结论支持成本-效益优化假说，即人类在动态环境中基于收益与认知成本灵活分配决策控制，研究结果强调了个体知识状态与环境统计的交互作用，这对理解适应性决策和精神疾病中的学习异常具有重要意义。
source: biorxiv
selection_source: fresh_fetch
motivation: 奖励波动性对模型免费与模型基强化学习策略的调节作用及机制尚不明确，尤其可能存在非线性关系。
method: 采用两个修正的两步决策任务，系统性操纵奖励波动性和时间压力，结合计算建模、模拟和分层贝叶斯模型拟合分析行为数据。
result: 奖励波动性对模型基策略使用呈倒U型影响，中等波动性下最高，且仅见于已习得任务结构者；时间压力促进模型免费策略；模拟证实中等波动性下模型基优势最大。
conclusion: 人类在动态环境中根据成本-效益优化灵活调整模型基与模型免费策略的平衡，且受任务知识和时间压力调制。
---

## 摘要
动作选择涉及两个系统：无模型的强化学习策略依赖于动作-结果配对的经验，而基于模型的强化学习策略则通过利用环境不变结构的模型进行推理来实现更灵活的行为。尽管环境变化需要更高的行为灵活性，但波动性这一捕捉环境变化速度或频率的高阶统计量如何系统性地调节这些策略仍不清楚。我们采用两个修改后的两步决策任务，考察了奖赏波动性对无模型和基于模型的强化学习策略之间仲裁的影响。在实验1中，被试完成具有不同奖赏波动性和时间压力的任务。在实验2中，我们在更广的范围内系统性地操纵奖赏波动性，以评估波动性与学习策略之间的关系。行为数据通过模型无关的单试次和多试次回溯分析、强化学习模拟以及分层贝叶斯模型拟合进行分析。在两个实验中，奖赏波动性对无模型和基于模型的强化学习策略之间的仲裁均呈现倒U形的非线性效应，即在中等奖赏波动性下，基于模型的学习策略被最强烈地驱动。这些调节效应仅出现在已习得任务中转移结构的个体中，而未习得转移结构的个体无论奖赏波动性如何都依赖无模型学习策略。强化学习模拟揭示，基于模型的学习策略相对于无模型学习策略的优势在中等奖赏波动性时达到峰值。此外，增加的时间压力将行为转向无模型学习策略。这些结果表明，人类在不确定和动态环境中并不总是使用基于模型的强化学习策略，即使他们知晓任务结构，这支持了成本效益优化。

作者摘要：通过仔细考虑未来后果来灵活引导行为的能力是人类智能和理性的一个显著特性的基础。然而，是什么驱动了这个深思熟虑的系统？在本研究中，我们利用具有不确定结构和变化奖赏的决策任务，探讨了促进深思熟虑行为与习惯性行为的因素。我们发现，自发习得任务中隐藏转移结构的被试利用这一知识来引导深思熟虑行为。相反，未习得该结构的被试主要依赖习惯性策略，即重复先前得到奖赏的动作。在习得结构的被试中，深思熟虑行为的程度随奖赏波动性（即奖赏随时间变化的速度）呈非线性变化。我们还观察到，限制决策时间减少了深思熟虑行为，并促进了习惯性反应。这些发现表明，在不确定和动态环境中，深思熟虑控制根据成本效益优化进行适应性调节。我们的结果有助于理解人类如何根据环境条件灵活调整其行为控制系统。

## Abstract
Action selection involves two systems: a model-free reinforcement learning strategy, which relies on experience with action-outcome pairs, and a model-based reinforcement learning strategy, which enables more flexible behavior via inference using a model of the invariant environmental structure. Although environmental change requires more flexible behavior, the ability of volatility, a higher-order statistic that captures how rapidly or frequently the environment changes, to systematically modulate these strategies remains unclear. We examined the effects of reward volatility on arbitration between model-free and model-based reinforcement learning strategies using two modified two-step decision tasks. In Experiment 1, participants performed tasks with different levels of reward volatility and time pressure. In Experiment 2, we systematically manipulated reward volatility across a broader range to assess the relationship between volatility and learning strategy. Behavioral data were analyzed using model-agnostic one-trial and multitrial back analyses, reinforcement learning simulations, and hierarchical Bayesian model fitting. Across experiments, reward volatility exerted an inverse U-shaped nonlinear effect on the arbitration between model-free and model-based reinforcement learning strategies, as the model-based learning strategy was strongly driven at intermediate levels of reward volatility. These modulation effects were observed only in individuals who had learned the transition structure in the task, whereas those who had not learned the transition structure relied on the model-free learning strategy regardless of reward volatility. Reinforcement learning simulations revealed that the relative advantage of the model-based learning strategy over the model-free learning strategy peaked at intermediate levels of reward volatility. Additionally, increased time pressure shifted behavior toward the model-free learning strategy. These results demonstrated that, humans do not always use the model-based reinforcement learning strategy in uncertain and dynamic environments, even when they are aware of the task structure, supporting cost-benefit optimization.

Author SummaryThe ability to flexibly guide behavior by carefully considering future consequences is fundamental to a prominent property of human intelligence and rationality. However, what drives this deliberative system? In this study, we investigated the factors that promote deliberative versus habitual behavior using decision-making tasks with uncertain structures and changing rewards. We found that participants who spontaneously learned the hidden transition structure in the task used this knowledge to guide deliberative behavior. Conversely, participants who did not learn the structure relied primarily on habitual strategies, repeating actions that had previously been rewarded. Among participants who learned the structure, the degree of deliberative behavior changed nonlinearly with reward volatility, in which the speed at which rewards changed over time. We also observed that limiting the decision time reduced deliberative behavior and promoted habitual responding. These findings suggest that under uncertain and dynamic environments, deliberative control is adaptively regulated according to cost-benefit optimization. Our results contribute to understanding how humans flexibly adjust their behavioral control systems in response to environmental conditions.

---

## 论文详细总结（自动生成）

好的，请查收以下基于所提供论文内容的详细中文总结。

### 1. 论文的核心问题与整体含义
*   **研究背景**：行为控制依赖于两个系统：基于模型（MB）的策略（目标导向、灵活但计算成本高）和无模型（MF）的策略（习惯性、快速但僵化）。环境动态性，特别是**奖赏波动性**（环境变化的速度和频率），如何影响这两种策略之间的权衡（仲裁），是一个核心但尚未明确的问题。
*   **核心问题**：该研究旨在系统性地探究奖赏波动性对 MF 和 MB 强化学习策略仲裁的具体影响模式。作者检验了一个假说，即这种影响可能是**非线性的**：当波动性极低或极高时，MB策略的优势下降，而在中等波动性时，其相对优势达到峰值，这反映了大脑进行**成本-效益优化**的过程。
*   **整体含义**：人类的决策并非总是依赖于单一的、最优的策略，而是会根据环境统计结构（如波动性）、内部知识状态（是否理解任务结构）以及认知资源限制（如时间压力），动态且灵活地在不同策略间进行成本效益权衡。

### 2. 论文提出的方法论
*   **核心思想**：通过严格控制的实验范式，结合模型无关（model-agnostic）和模型依赖（model-based）的行为分析，以及强化学习模拟，多角度地证实并量化解剖奖赏波动性的非线性调节效应。
*   **模型无关分析方法**：
    *   **单试次回溯分析**：使用广义线性混合模型（GLMM）分析最近一次试验的**转移类型**（常见/罕见）和**奖赏大小**对当前“保持”行为概率的交互作用。MB策略表现为显著的转移-奖赏交互效应，MF策略则表现为显著的奖赏主效应。为控制低波动下纯MF策略可能模拟出MB行为的混淆，模型纳入了“上一试次是否选了最优选项”这一协变量。
    *   **多试次回溯分析**：使用GLM分析过去多个试次的转移、奖赏、选择及其交互效应，通过对多期效应进行求和，衡量行为受历史经验影响的持续长度和强度。
*   **模型依赖（计算建模）分析**：
    *   **混合强化学习模型**：采用一个经典的混合 RL 模型。智能体同时学习 MF 价值（通过Sarsa或Q-learning更新）和 MB 价值（通过基于已知转移概率的动态规划计算）。最终的行动选择概率由一个加权和决定：`Q_net = w * Q_MB + (1-w) * Q_MF`。
    *   **分层贝叶斯参数估计**：采用层次贝叶斯方法，允许模型的关键参数（学习率、逆温、资格迹、持续性和平衡权重 `w`）在不同波动性条件下独立变化，从而分离出波动性对MF/MB平衡 `w` 的特异性影响。
*   **强化学习模拟**：通过在模拟的 MF 和 MB 智能体上进行网格搜索，计算不同波动性水平下两种策略的**期望奖赏面**，并将MB策略相对于MF策略的优势量化为两个奖赏面之间的体积差，以此来定义“收益”。

### 3. 实验设计
*   **核心范式**：在两个实验中均采用了**修正的两步决策任务**。
    *   **实验 1**：操纵奖赏波动性（由奖赏量级高斯随机游走的标准差定义，分低/中/高三个水平）和时间压力（由试次间隔定义，分高/低两水平）。包含一个探索性条件（中等波动性下增加第二步可用动作数）。22名健康被试参与。
    *   **实验 2**：为检验非线性，系统性操纵更广泛的奖赏波动性（由切换奖赏概率分布的泊松分布参数 λ 定义，共4个水平，包括极高和极低）。53名健康被试参与。任务结构要求独立学习两个状态的价值。
*   **对比方法与基准**：
    *   **关键分组对比**：根据事后口头报告，将每个实验的被试分为**知晓组**（已习得任务转移结构）和**不知晓组**（未习得结构），作为核心的个体差异变量。
    *   **条件内对比**：比较不同波动性条件或时间压力条件下的行为指标及模型参数（特别是平衡参数 `w`）。
    *   **方法与数据对比**：将模型无关分析的结果与纯MF和纯MB策略的模拟数据（图3、图8）进行对比。将实验估计的效应量变化映射到由RL模拟构建的参数变化空间中，以分离 `w` 的变化。
*   **数据集**：实验1有22名被试数据（18名“知晓”），实验2有48名有效被试数据（25名“知晓”）。每名被试在4种条件下各完成120次试次。分析中排除了被试可能还未习得转移结构的初始试验（实验1前20试次，实验2前40试次）。

### 4. 资源与算力
*   论文的**计算方法**主要为强化学习模拟和层次贝叶斯模型的马尔可夫链蒙特卡洛（MCMC）参数估计。
*   文中**未明确提及**所使用的具体GPU型号或数量，也未报告模型训练的精确时长。其计算需求侧重于统计推断，而非大规模深度学习训练。

### 5. 实验数量与充分性
*   **实验数量**：开展了**两个核心行为实验**。实验1作为初步探索，实验2作为预注册的、系统性更强的验证和扩展。
*   **分析层面**：针对每个实验，均进行了**多层面的分析**：
    *   模型无关的**单试次和多试次回溯分析**。
    *   **计算模型拟合并比较**了多种模型（全参数可变模型、固定参数模型等）的拟合优度（WAIC）。
    *   **参数恢复分析**以验证模型估计的可靠性。
    *   **强化学习模拟**（收益模拟和参数变化到效应量的映射模拟）。
*   **充分性与客观公平性**：
    *   **充分性**：实验设计严谨，通过实验2的预注册、拉丁方设计平衡顺序效应、排除初始试次、多分析方法交叉验证等手段，对核心假设的检验是充分且有力的。
    *   **客观公平性**：分析是客观公平的。例如，在模型拟合中，尽管“全参数可变”模型并非拟合最优，但作者选择其为最终分析模型，以允许各波动性条件下的所有参数自由变化，从而**避免其他参数（如学习率）的变化混淆对核心参数 `w` 的估计**。作者也承认了单维度 MF/MB 混合模型可能存在的分类错误等局限。

### 6. 论文的主要结论与发现
*   **倒U形非线性关系**：奖赏波动性对MF/MB仲裁的影响呈倒U形。在**中等波动性**下，MB策略的参与度最高。这是本研究的核心发现。
*   **知识状态的调节作用**：上述非线性调节效应**仅存在于“知晓组”**（已掌握任务转移结构的个体）被试中。在“不知晓组”中，行为主要由MF策略驱动，且与波动性无关。
*   **成本-效益优化证据**：时间压力（代表高认知成本）促使行为转向MF策略，而MB策略在收益最高（中等波动性）时使用最多，共同支持了仲裁过程遵循成本-效益优化的理论。
*   **计算模拟的支持**：RL模拟证实，MB相对于MF的收益确实在中等波动性时达到峰值，与行为发现一致。

### 7. 优点
*   **假说驱动的系统性操纵**：本研究明确针对波动性的非线性效应这一未被充分探索的假说，并通过两个互补的实验（特别是预注册的实验2）系统性地操纵了广泛的波动性水平。
*   **多层面的分析技术**：巧妙地结合了模型无关分析（控制混淆因素）、计算模型拟合（分离参数）和RL模拟（验证收益），从多个角度共同得出结论，增强了结论的稳健性。
*   **精细的实验控制**：通过区分“知晓”与“不知晓”组、排除学习初期的试验、使用拉丁方设计最小化序列效应，展示了很高的实验控制标准。
*   **清晰的理论贡献**：研究结果有力地支持和细化了“成本-效益优化”理论，明确指出任务结构知识是波动性调节MB策略的前提，推动了该领域的认知。

### 8. 不足与局限
*   **学习策略维度的简化**：将复杂的策略简化为单一的 MF-MB 连续谱，可能忽略了其他策略（如继承表征SR），或MF策略中基于特定状态表征的复杂形式，这些可能被误识别为MB行为。
*   **模型估计偏差的潜在风险**：尽管模型选择有其理由，但使用非最优拟合的“全参数可变”模型，可能导致 `w` 参数估计不够精确。混合模型本身也可能因模型设定错误而产生偏差。
*   **实验1的样本量**：实验1的“不知晓组”样本量较小（N=4），限制了针对该群体的结论效力。
*   **生态效度与应用限制**：实验室的抽象决策任务与真实世界的复杂决策仍有差距。结论向临床或其他应用领域的推广需要进一步验证。

（完）
