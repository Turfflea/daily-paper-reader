---
title: Improve Robustness of Reinforcement Learning against Observation Perturbations via l∞ Lipschitz Policy Networks
title_zh: 通过l∞ Lipschitz策略网络提升强化学习对观测扰动的鲁棒性
authors: "Buqing Nie, Jingtian Ji, Yangqing Fu, Yue Gao"
date: 2024-03-25
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/29360/30567"
tags: ["query:mt"]
score: 9.0
evidence: 提出具有l∞ Lipschitz策略网络的鲁棒深度强化学习方法
tldr: 针对深度强化学习智能体易受观测扰动影响的脆弱性问题，本文提出SortRL方法，从网络架构角度增强鲁棒性。该方法设计了一种具有全局l∞ Lipschitz连续性的新型策略网络，并提供了便捷的鲁棒性增强手段。实验证明，SortRL能有效提升策略对观测扰动的鲁棒性，为强化学习在实际应用中的可靠部署提供了重要支撑。
source: AAAI-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29360/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 863, \"height\": 665, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29360/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 870, \"height\": 398, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29360/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 865, \"height\": 451, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29360/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1792, \"height\": 1336, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29360/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1662, \"height\": 587, \"label\": \"Table\"}]"
motivation: 深度强化学习在序列决策任务中表现优异，但易受观测扰动影响，限制了实际部署的可靠性。
method: 提出名为SortRL的鲁棒强化学习方法，采用具有全局l∞ Lipschitz连续性的新型策略网络架构。
result: 实验表明，所提方法能有效提升强化学习策略对观测扰动的鲁棒性。
conclusion: SortRL通过改进策略网络架构显著增强了深度强化学习的鲁棒性，有助于推动其在真实世界中的应用。
---

## Abstract
Deep Reinforcement Learning (DRL) has achieved remarkable advances in sequential decision tasks. However, recent works have revealed that DRL agents are susceptible to slight perturbations in observations. This vulnerability raises concerns regarding the effectiveness and robustness of deploying such agents in real-world applications. In this work, we propose a novel robust reinforcement learning method called SortRL, which improves the robustness of DRL policies against observation perturbations from the perspective of the network architecture. We employ a novel architecture for the policy network that incorporates global $l_\infty$ Lipschitz continuity and provide a convenient method to enhance policy robustness based on the output margin. Besides, a training framework is designed for SortRL, which solves given tasks while maintaining robustness against $l_\infty$ bounded perturbations on the observations. Several experiments are conducted to evaluate the effectiveness of our method, including classic control tasks and video games. The results demonstrate that SortRL achieves state-of-the-art robustness performance against different perturbation strength.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：深度强化学习（DRL）智能体在观测（状态）受到微小扰动时决策会变得不稳定，甚至完全失效，这严重阻碍了 DRL 在真实场景（如自动驾驶、机器人操控）的可靠部署。
- **整体含义**：本文旨在从**策略网络架构**的角度直接为策略赋予内在的鲁棒性，提出一种名为 SortRL 的方法，使策略本身具备全局 $l_\infty$ Lipschitz 连续性，从而无需显式攻击即可保证在观测扰动下不改变决策。

### 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：
  - 将鲁棒策略学习转化为求解约束优化问题：在最大化期望回报的同时，要求策略在状态 $s$ 处的“鲁棒半径” $R(\pi,s)$ 不低于扰动界 $\epsilon$。
  - 利用 Lipschitz 神经网络（SortNet）作为策略网络 $g_\pi$，并证明该网络是 $l_\infty$ 1‑Lipschitz 的；进而推导出鲁棒半径的下界为输出 margin 的一半（即 $R(\pi,s) \ge \frac{1}{2} \big(\max_{a}g_\pi^a(s) - \max_{a\neq a^*}g_\pi^a(s)\big)$），将鲁棒性要求转化成对输出 margin 的简单约束。
- **关键技术细节**：
  - **策略网络结构**：采用 M 层全连接 SortNet，每层进行排序聚合与加权，保证整个网络满足 $l_\infty$ 1‑Lipschitz 连续性。
  - **将问题转化为 margin 最大化**：得益于 Lipschitz 性质，只要 margin 不小于 $2\epsilon$，就能保证对任意 $\ell_\infty$ 半径 $\epsilon$ 内的扰动决策不变。
  - **训练框架（Policy Distillation + Hinge loss）**：
    1. 先用标准 DRL 算法训练一个教师策略 $\pi_T$，并收集干净状态‑动作数据集 $\mathcal{D}$；
    2. 用 SortNet 构建学生策略 $\pi_S$，通过最小化组合损失 $L_{\pi_S} = \lambda L_{CE}(g_\pi(s), a^*) + L_{Rob}(g_\pi(s), \theta, a^*)$ 进行训练。其中 $L_{CE}$ 为交叉熵模仿损失，$L_{Rob}$ 为基于 hinge loss 的鲁棒性损失，仅在 margin 不足且决策与教师一致时才激活。
    3. 训练过程中 $\lambda$ 逐步衰减，从最初注重模仿教师，逐渐过渡到优先保证鲁棒性，实现最优性与鲁棒性的平衡。
- **数学支撑**：
  - Theorem 1 给出最坏对手下性能损失被 KL 散度界的证明，从而将鲁棒问题转化为最小化 KL 散度。
  - 利用 Lipschitz 连续性将 KL 散度界与鲁棒半径关联，避免显式寻找最差扰动。

### 3. 实验设计：数据集 / 场景、benchmark 及对比方法
- **实验场景与数据集**：
  - **经典控制任务**：CartPole, Acrobot, MountainCar, LunarLander。教师策略由 PPO 训练，数据集 5 万条。
  - **视频游戏**：Atari 环境（Freeway, RoadRunner, Pong, BankHeist）和 ProcGen 环境（Jumper, Coinrun）。教师策略分别由 DQN 和 PPO 训练，数据集 10 万条。
  - **强扰动场景**：在 BankHeist（$\epsilon$ 至 15/255）和 Freeway（$\epsilon$ 至 20/255）下进行测试。
- **扰动与攻击设置**：
  - 攻击者：PGD（10 或 30 步）、RI‑FGSM、RI‑FGSM‑Multi、RI‑FGSM‑Multi‑T。
  - 扰动强度：0/255 至 5/255，以及更高的 10/255–20/255。
- **对比方法**：
  - 标准 DRL：DQN、A3C、PPO。
  - 鲁棒方法：RS‑DQN、SA‑DQN、WocaR‑DQN、RADIAL（‑DQN 及 ‑A3C）、ATLA 等；强扰动实验还包括 BCL‑RADIAL、BCL‑MOS‑AT。
- **评测指标**：
  - 不同扰动强度下的 episod reward。
  - 动作认证率（ACR）：在整个 rollout 中决策不受任意 $\ell_\infty$ 攻击影响的比例。

### 4. 资源与算力
- 文中明确声明：所有 SortRL 策略均使用 **AdamW 优化器**在 **单块 NVIDIA RTX 3090 GPU** 上训练。
- 未报告具体的训练时长（小时或天数），仅给出了教师策略采样数据集的大小（经典控制 50k 条，视频游戏 100k 条）。因此**训练时间成本未能完全量化**。

### 5. 实验数量与充分性
- **实验组数**：
  - 4 个经典控制任务 × 多种扰动强度（共约 4 组完整曲线）。
  - 6 个视频游戏（4 Atari +2 ProcGen）× 3–4 种扰动强度，加上与多个 baseline 的横向对比（表1、表2）。
  - 2 个游戏的强扰动消融（图3），对比多种攻击和鲁棒方法。
  - 统一使用 PGD 和多种攻击进行鲁棒性评估。
- **充分性与客观性**：
  - 实验覆盖了低维连续/离散控制和高维图像输入，环境多样，扰动设置从弱到强，对比方法包含近年来主流的正则化、对抗训练及认证式鲁棒 RL 方法。
  - 评价指标兼顾名义性能和鲁棒性能（episode reward 与 ACR），并引入标准化得分跨任务聚合，分析较为全面。
  - 未发现单独的结构性消融实验（如去掉 SortNet 对比、不同 $\lambda$ 或 $\theta$ 的影响），但训练框架的核心组分均有所体现。
  - 总体实验设计比较严谨，**具备良好的可比性和充分性**。

### 6. 论文的主要结论与发现
- SortRL 在经典控制和视频游戏任务上均取得了**最先进的鲁棒性**，尤其在强扰动（$\epsilon \ge 5/255$）下优势明显。
- 相比现有方法，SortRL 在维持高鲁棒性的同时**对名义性能的牺牲极小**（例如 Atari 任务在 $\epsilon = 5/255$ 时仅下降 <1.7%），说明 Lipschitz 架构与 margin 优化能更好地平衡最优性与鲁棒性。
- 动作认证率普遍 >99.6%，证实了理论下界的实际有效性。
- 在某些任务（如 Freeway, Jumper, Coinrun）出现鲁棒训练后名义性能反超标准 DRL 的现象，暗示平滑策略有时能发现更优轨迹。

### 7. 优点：方法或实验设计上的亮点
- **架构层级保证鲁棒性**：首次从网络结构入手直接赋予策略 $l_\infty$ Lipschitz 性质，避免了繁琐的对抗训练或在线攻击生成，计算友好。
- **鲁棒性可通过输出 margin 简便度量与优化**：无需复杂的认证过程，只需输出值差异即可得到鲁棒半径下界并指导训练。
- **训练框架清晰有效**：Policy Distillation 配合 decay 的 $\lambda$ 策略，解耦了任务解决与鲁棒性要求，易于实现且稳定。
- **实验评估扎实**：覆盖多个域、多种攻击器、不同扰动强度，与大量主流 baseline 对比，证据充分。

### 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等
- **仅适用于离散动作空间**：论文明确聚焦于离散动作，未涉及连续控制（如 MuJoCo 等），限制了在机器人运动控制等典型领域的直接应用。
- **依赖教师策略与离线数据集**：需要先训一个高性能教师并采样，若教师本身存在偏差或缺乏最优性，可能影响学生最终表现；与在线端到端训练相比增加了步骤。
- **网络架构约束**：采用全连接 SortNet，未给出卷积版本或高维图像输入的扩展方式，实验中 Atari 任务可能使用了某种预处理，但结构灵活性有限。
- **训练成本不透明**：虽提及用单块 3090 GPU，但未报告各任务完整训练时间，难以评估实际计算开销。
- **攻击模型限定**：对抗扰动仅考虑 $l_\infty$ 有界观测扰动，对于其他类型扰动（如自然噪声、风格变化、对抗性模糊等）未检验泛化能力。
- **超参数敏感性未分析**：$\lambda$ 衰减策略、hinge threshold $\theta$ 等关键超参数的选择方法和影响缺乏消融实验或详细讨论。
- **强扰动下性能仍有下降**：在 $\epsilon > 5/255$ 时回报有所衰减（如图3），尽管优于对比方法，但绝对指标仍有提升空间。

（完）
