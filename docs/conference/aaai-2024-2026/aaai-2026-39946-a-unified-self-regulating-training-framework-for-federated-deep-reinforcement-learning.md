---
title: A Unified Self-Regulating Training Framework for Federated Deep Reinforcement Learning
title_zh: 一种面向联邦深度强化学习的统一自调节训练框架
authors: "Meng Xu, Xinhong Chen, Zhongying Chen, Guanyi Zhao, Yang Jin, Jianping Wang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39946/43907"
tags: ["query:mt"]
score: 9.0
evidence: 面向联邦深度强化学习的自调节训练框架，处理动态状态转移
tldr: 针对联邦深度强化学习中动态环境导致策略偏差的问题，提出统一的自调节训练框架，通过动态稀疏化训练使模型适应变化；实验表明全局策略性能显著提升，为联邦深度强化学习在动态场景下的应用提供了有效解决方案。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39946/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1576, \"height\": 514, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39946/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1750, \"height\": 747, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39946/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 841, \"height\": 500, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39946/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 769, \"height\": 262, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39946/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 861, \"height\": 264, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39946/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 823, \"height\": 436, \"label\": \"Table\"}]"
motivation: 现有联邦深度强化学习方法在静态环境有效，但实际场景中动态状态转移导致策略偏差。
method: 提出通用的自调节训练框架，结合稀疏训练方法，动态稀疏化和调整模型参数以适应环境变化。
result: 在多个DRL环境中，该方法提升了客户端和全局策略的性能与鲁棒性。
conclusion: 该框架为联邦深度强化学习在动态场景下的应用提供了有效的解决方案。
---

## Abstract
Federated Deep Reinforcement Learning (FDRL) aims to enable distributed collaborative training of multiple DRL models while preserving privacy. Existing FDRL methods function in static client environments, but real-world scenarios often involve dynamic state transitions, such as noise, which render static model topologies inadequate and result in biased policy loss. This degrades client performance and leads to suboptimal global policies. To address this challenge, we develop a generic solution, referred to as the self-regulating training framework, which can be seamlessly integrated into existing FDRL approaches to address dynamic state transitions. Specifically, we propose a Sparse Training (ST) method that dynamically sparsifies and adjusts the topology of each model during training to maximize model performance and reduce model complexity. Additionally, we introduce an auxiliary model to adaptively regulate the policy loss of client models, mitigating loss bias and facilitating updates that yield improved returns. Experimental results demonstrate that our method enhances six state-of-the-art (SOTA) FDRL approaches across nine tasks in terms of return.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **研究背景**：联邦深度强化学习（FDRL）允许多个智能体在保护数据隐私的前提下协作训练 DRL 模型，但现有方法均假设客户端环境是**静态**的。
- **核心问题**：真实场景中常出现**动态状态转移**（如噪声、环境参数时变），导致预先固定的模型拓扑不再适用，并引发**策略损失偏差**，使客户端性能下降，全局模型聚合后形成“负反馈循环”，最终产生次优策略。
- **整体含义**：该工作提出一种**通用的自调节训练框架**，可无缝集成到现有 FDRL 方法中，通过动态调整模型稀疏性与策略损失，使 FDRL 系统能够适应动态环境，从而提升全局策略的回报。

### 2. 论文提出的方法论
#### 2.1 核心思想
在现有 FDRL 框架中引入两个即插即用的模块：
- **稀疏训练（Sparse Training, ST）**：在训练过程中动态稀疏化并调整每个局部模型的连接拓扑，以提升性能并降低模型复杂度。
- **辅助模型（Auxiliary Model）**：自适应地调节各客户端策略损失，缓解因环境动态造成的损失偏差，引导策略向更高回报方向更新。

#### 2.2 关键技术细节
- **ST 的动态拓扑调整**
  - 采用“**丢弃-生长（drop and grow）**”机制：每次更新后，按绝对值重要性移除固定数量（\(N_d\)）的连接，并在原无连接处随机新生等量（\(N_g = N_d\)）连接；新连接初始化为 0。
  - 全局聚合后，因局部模型拓扑各异导致全局模型稀疏度下降，故对全局模型进行**剪枝**，保留绝对值最大的前 \(k\) 个连接，使稀疏度与局部模型一致。
- **辅助模型与策略损失调节**
  - 辅助模型为一个三层稀疏 MLP，输入为演员网络提取的特征、状态和动作，输出辅助损失 \(L_a(\theta^\mu)\)。
  - 辅助模型总损失由两部分组成：
    - **TD 损失 \(L_1\)**：保证辅助模型自身的收敛。
    - **Q 值提升损失 \(L_2\)**：最大化辅助更新前后 Q 值的差值（\(J_{aux2} - J_{aux1}\)），并用 tanh 函数压缩范围。
  - 演员在标准策略梯度更新后，额外使用辅助损失进行梯度更新，使得策略朝更高的 Q 值（长期回报）方向优化。

#### 2.3 算法流程（文字描述）
1. 初始化稀疏模型（演员、双评论家、辅助模型），使用 Erdős-Rényi 随机图设定初始稀疏连接。
2. 每个客户端循环：
   - 从本地回放缓冲区采样常规和辅助小批样本。
   - 更新评论家（TD 误差最小化）。
   - 更新演员：先标准策略梯度，再计算辅助损失并更新演员。
   - 更新辅助模型（\(L_1 + L_2\)）。
   - 动态调整稀疏拓扑（丢弃、生长连接）。
   - 软更新目标网络。
3. 达到通信轮次后，上传局部模型至服务器进行聚合，服务器剪枝全局模型以维持与客户端相同的稀疏度，再下发至客户端。

### 3. 实验设计
#### 3.1 数据集/场景
- **MuJoCo 连续控制任务**（5个）：HalfCheetah-v2, Hopper-v2, Walker2d-v2, Ant-v2, Humanoid-v2。
  - 动态环境实现：为每个客户端的观测添加随机噪声 \(\psi \mathcal{N}(0,1)\)，噪声级别设 0、1、3，级别越高动态性越强。
- **Gym 经典控制任务**（4个）：CartPole, Pendulum, BipedalWalkerHardcore, LunarLander。
  - 动态环境实现：为每个客户端设定不同的物理参数（如杆长、重力、障碍物概率等），参数在 ±50% 范围内变动。

#### 3.2 基准与对比方法
- 六个 SOTA FDRL 方法：**SCCD, FRD, FedKL, QAvg, FRL, AFedPG**。
- 评价指标：**最终回报**（最后 10 轮通信的平均回报）和回合回报曲线。

### 4. 资源与算力
- **硬件**：Intel Core i7-9700 CPU，64GB RAM，单张 **NVIDIA RTX 2080 GPU**。
- **软件**：Torch 1.2.0 + Gym 0.16.0 + mujoco-py 1.50.0.1。
- **算力细节**：论文未明确给出单次实验或全部实验的总训练时长，也未提及 GPU 使用数量（仅提及型号）。**缺少训练时间/算力总量信息**。

### 5. 实验数量与充分性
- **实验组数**：
  - MuJoCo 任务：3 种基线方法 × 5 个任务 × 3 个噪声级别 = **45 组**（每组 5 个随机种子）。
  - Gym 任务：3 种基线方法 × 4 个任务 × 1 个扰动级别 = **12 组**。
  - 消融实验：2 种基线方法 × 2 个任务 × (仅 ST、仅辅助模型、完整框架) = 约 **8 组**。
  - 超参数敏感性：2 种基线方法 × 2 个任务 × 3 个稀疏度 = **12 组**。
  - 客户端数量变化：2 种基线方法 × 2 个任务 × 3 种数量 = **12 组**。
- **充分性与客观性**：实验规模较大，覆盖连续和离散动作空间、多类基线方法、多种动态程度，并包含消融、敏感性、可伸缩性分析；采用多随机种子并报告均值和标准差，对比公平。结果证明所提框架在绝大多数情况下均有正向提升。

### 6. 论文的主要结论与发现
- 所提出的自调节框架能够**即插即用**地集成到现有 FDRL 方法中，有效缓解动态环境导致的性能退化。
- 稀疏训练和辅助模型**协同工作**，既能提升模型在动态环境下的表现，又能降低计算、存储和通信成本。
- 在多个动态任务上，集成后的方法相比原始基线获得**一致的回报提升**，尤其在高动态扰动下优势明显。

### 7. 优点
- **通用性强**：作为框架而非单一算法，可与多种现有 FDRL 方法（如 QAvg, SCCD, FedKL 等）结合。
- **轻量高效**：动态稀疏化在提升性能的同时减少了模型参数量，降低资源开销，适用于边缘设备。
- **理论支撑**：提供了复杂度分析和理论解释，说明为何本方法优于原始 FDRL 方法。
- **实验全面**：涵盖多种任务、多个基线、消融与超参数分析，验证充分。

### 8. 不足与局限
- **任务类型有限**：主要在 MuJoCo 和 Gym 控制任务上验证，未涉及更复杂的部分可观测或离散高维场景（如 Atari 等）。
- **模型架构单一**：方法描述基于 MLP 网络，对 CNN、Transformer 等其他架构的适用性未讨论。
- **动态环境假设简化**：仅通过加性噪声或参数随机化模拟动态变化，未涉及非平稳环境中的结构性变化。
- **资源报告不完整**：未提供训练耗时、能耗等实际部署资源开销的量化数据。
- **稀疏度敏感性**：尽管 50% 稀疏度表现最优，但更高稀疏度的性能退化明显，未见对稀疏度自适应调整策略。

（完）
