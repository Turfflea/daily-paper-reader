---
title: Scalable Multi-Objective and Meta Reinforcement Learning via Gradient Estimation
title_zh: 基于梯度估计的可扩展多目标与元强化学习
authors: "Zhenshuo Zhang, Minxuan Duan, Youran Ye, Hongyang R. Zhang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40092/44053"
tags: ["query:mt"]
score: 8.0
evidence: 多目标强化学习和元强化学习用于序列决策与优化
tldr: 针对多目标强化学习中目标数量增加导致学习效率低下的问题，提出基于目标分组的两阶段方法。该方法首先将众多目标划分为少量相关组，再通过元学习训练共享策略，最后利用梯度估计快速适应各目标组，显著提升了多目标策略优化的效率。实验在控制任务中展现出优越的性能和扩展性，为管理科学中的资源分配和调度优化提供了可迁移的强化学习框架。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40092/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1791, \"height\": 440, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40092/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1825, \"height\": 617, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40092/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 878, \"height\": 189, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40092/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 886, \"height\": 609, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40092/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1677, \"height\": 364, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40092/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 875, \"height\": 202, \"label\": \"Table\"}]"
motivation: 随着目标数量增长，为所有目标学习单一策略次优，需高效优化多目标。
method: 两阶段过程：元训练学习多目标元策略，微调时通过梯度估计将元策略适应到随机任务组。
result: 在多个控制任务中实现了高效多目标优化，性能优于单策略方法。
conclusion: 提出的方法可扩展地解决多目标强化学习，适用于机器人、控制等复杂决策场景。
---

## Abstract
We study the problem of efficiently estimating policies that simultaneously optimize multiple objectives in reinforcement learning (RL). Given n objectives (or tasks), we seek the optimal partition of these objectives into k groups, which is much smaller than n, where each group comprises related objectives that can be trained together. This problem arises in applications such as robotics, control, and preference optimization in language models, where learning a single policy for all n objectives is suboptimal as n grows. We introduce a two-stage procedure — meta-training followed by fine-tuning — to address this problem. We first learn a meta-policy for all objectives using multitask learning. Then, we adapt the meta-policy to multiple randomly sampled subsets of objectives. The adaptation step leverages a first-order approximation property of well-trained policy networks, which is empirically verified to be accurate within a 2% error margin across various RL environments. The resulting algorithm, PolicyGradEx, efficiently estimates an aggregate task-affinity score matrix given a policy evaluation algorithm. Based on the estimated affinity score matrix, we cluster the n objectives into k groups by maximizing the intra-cluster affinity scores. Experiments on three robotic control and the Meta-World benchmarks demonstrate that our approach outperforms state-of-the-art baselines by 16% on average, while delivering up to 26 times faster speedup relative to performing full training to obtain the clusters. Ablation studies validate each component of our approach. For instance, compared with random grouping and gradient-similarity-based grouping, our loss-based clustering yields an improvement of 19%. Finally, we analyze the generalization error of policy networks by measuring the Hessian trace of the loss surface, which gives non-vacuous measures relative to the observed generalization errors.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
*   **核心问题**：在多目标强化学习（multi‑objective RL）中，当优化目标（任务）数量 \(n\) 很大时，为所有目标训练一个单一的共享策略往往会因任务间冲突（负迁移）而效果不佳。如何高效地将 \(n\) 个目标自动划分为 \(k\) 个互有正迁移的群组（\(k \ll n\)），使得每个群组内的任务可以共同训练，从而提升总体性能与可扩展性？
*   **研究动机**：机器人控制、语言模型偏好优化等场景天然涉及大量相关但可能冲突的奖励目标，穷举搜索所有分组在计算上不可行（子集数量指数增长）。现有基于梯度相似度的启发式方法未能精确刻画多策略联合训练时的交互效应，因此需要一种能快速估计任意任务子集联合训练效果的方法。
*   **整体含义**：通过提出一种基于梯度估计的替代模型，能够在不进行完整训练的情况下预测策略在任意任务子集上的微调性能，进而构建任务亲和度矩阵并做聚类分组，最终实现可扩展的多目标与元强化学习。

### 2. 论文提出的方法论
*   **核心思想**：两阶段框架——**元训练 + 替代建模与聚类**。
    *   第一阶段，在所有 \(n\) 个任务上多任务训练得到一个元策略参数 \(\theta^\star\)。
    *   第二阶段，利用 \(\theta^\star\) 处的一阶泰勒展开构造一个高效的替代模型，用于估计策略在任何子集上微调后的性能，进而构造任务亲和度矩阵，最后通过凸聚类将 \(n\) 个任务划分为 \(k\) 个群组。
*   **关键技术细节**
    *   **一阶替代模型**：对 PPO 的目标函数 \(J(\theta)\) 在 \(\theta^\star\) 处做一阶展开，将概率比 \(r_t(\theta)\) 近似为 \(1 + \log r_t(\theta)\)，并利用 \(\log \pi_\theta(a_t|s_t)\) 的一阶泰勒展开 \(g_t^\top \Delta\theta\)。最大化 \(J(\theta)\) 最终简化为最小化一个**加权逻辑回归损失**：
        \[
        \min_{\Delta\theta} \sum_{t} w_t \cdot \log\left(1 + \exp\left(-y_t (g_t^\top \Delta\theta)\right)\right)
        \]
        其中 \(g_t = \nabla_\theta \log \pi_\theta(a_t|s_t)|_{\theta^\star}\)，权重 \(w_t = |\hat{A}_t|\)，伪标签 \(y_t = \text{sign}(\hat{A}_t)\)。这使得策略自适应问题转化为一个在预计算特征（梯度向量）上的线性分类任务。
    *   **随机投影降维**：为降低高维梯度向量 \(p\) 的计算量，使用高斯随机投影矩阵 \(P \in \mathbb{R}^{p \times d}\) 将梯度投影到低维空间 (\(d = 400\))，再求解 \(d\) 维的加权逻辑回归，最后将解映射回原始参数空间，实现了显著加速。
    *   **任务亲和度矩阵与聚类**：对 \(m\) 个随机采样的任务子集 \(S_j\)，用上述替代模型快速估计其损失 \(\hat{f}(S_j)\)。定义任务对 \((T_i, T_j)\) 的亲和度 \(U_{i,j}\) 为所有包含该对的子集估计损失的平均值。然后求解一个最大化簇内亲和度的**凸松弛聚类问题**，将 \(n\) 个任务分为 \(k\) 组。
    *   **算法流程**：算法1 (PolicyGradEx) 完成单次子集性能估计；算法2 通过反复调用 PolicyGradEx 构建亲和度矩阵并执行聚类。

### 3. 实验设计
*   **数据集与环境**
    *   **Meta‑World 基准**：MT10，包含 10 个多样化的机器人操作任务（共享状态/动作空间，但奖励函数和动力学不同）。
    *   **经典控制环境**：CartPole、Highway、LunarLander。每个环境通过改变物理参数（如杆长、重力、交通密度）生成 10 个源任务，构成多任务集合。
*   **对比方法**
    *   **多目标 RL 基线**：单一多任务策略、Soft Modularization、PaCo、CARE。
    *   **分组基线**：随机分组、基于梯度余弦相似度的分组。
    *   **元学习器**：MAML（用于控制环境中的元强化学习测评）。
*   **评估指标**：Meta‑World 采用平均成功率；控制任务采用在 50 个未见目标任务上经 200 步自适应后的平均奖励；同时测量归一化互信息（NMI）和 FLOPs 加速比。

### 4. 资源与算力
*   论文明确指出实验使用 PyTorch 框架，运行于配备 **Intel Xeon E5‑2623 CPU** 和单块 **NVIDIA Quadro RTX 6000 GPU** 的 Ubuntu 服务器。
*   未提供具体的训练总时长或 GPU 小时数，但通过 FLOPs 对比给出了相对加速比：PolicyGradEx 的替代估计相较于完整训练最高实现 **26× 的 FLOPs 减少**。

### 5. 实验数量与充分性
*   **实验组数**：包含 **5 组以上主要实验**：
    *   替代模型精度验证（多环境中不同距离阈值下的误差测量，表1）。
    *   损失函数估计质量验证（NMI 与加速比，表3）。
    *   多对象 RL 性能对比（4 种环境 × 约 7 种方法，表2）。
    *   元强化学习性能对比（控制环境适应后的奖励）。
    *   泛化误差分析（Hessian 迹与误差趋势，图2）。
    *   消融研究（聚类数 \(k\)、投影维度 \(d\) 的敏感性分析）。
*   **充分性与客观性**：实验设计较为全面，覆盖了离散/连续控制、操作类任务，对比了主流多任务和分组基线，并包含必要的消融验证。所有实验汇报均值和标准差，在同一硬件上复现，对比公平。但任务总数 \(n\) 仅设为 10，更大规模（如 \(n > 50\)）的验证相对不足。

### 6. 论文的主要结论与发现
*   PolicyGradEx 的替代模型能以 **\(<2\%\) 的相对误差**精确近似策略微调后的损失，且与真实分组结果的 NMI 超过 0.73，同时实现 **最多 26× 的计算加速**。
*   基于该替代模型的任务聚类在多目标 RL 中平均成功率高出现有最佳基线 **19%**，在元 RL 场景中提升 **13%**。
*   与随机分组或梯度相似性分组相比，基于损失估计的亲和度分组分别带来 **62% 和 35% 的性能提升**，表明其能有效避免负迁移。
*   策略网络的 Hessian 迹与实测泛化误差在变化趋势和量级上高度吻合，可作为解释多任务泛化行为的分析工具。

### 7. 优点
*   **创新性强**：首次将数据归因中的替代建模思想引入多目标 RL 任务分组，利用一阶逻辑回归高效模拟策略自适应，替代昂贵的全训练搜索。
*   **高效可扩展**：通过随机投影和凸聚类，将任务分组问题的复杂度从指数级或 \(O(n^2)\) 次训练降低到只需一次元训练加 \(m\) 次低维分类，实用性高。
*   **理论结合实践**：不仅给出算法，还辅以泛化误差的 Hessian 界分析，为理解多任务动态提供了新视角。
*   **实验扎实**：覆盖多种环境和基线，消融清晰，公开代码，可复现性强。

### 8. 不足与局限
*   **一阶近似的局限**：替代模型的准确度依赖微调后参数与初始点的距离，当自适应幅度较大（参数变化 \(>5\%\)）时，误差可能上升到 10% 左右，可能会影响任务差异很大的场景。
*   **任务规模有限**：所有实验的任务总数 \(n=10\)，虽然 m 个子集可以扩展，但更大规模（如 \(n=50, 100\)）下的聚类效果和矩阵填充的稳定性尚未验证。
*   **环境较简单**：控制环境相对经典，未在更复杂的视觉输入或高维连续控制（如 Humanoid）下测试，方法的通用性有待进一步检验。
*   **依赖初始元策略**：亲和度估计的质量完全依赖于元策略 \(\theta^\star\) 的收敛性，若初始多任务训练本身效果不佳（如任务极度冲突），后续聚类可能受影响。
*   **元学习器固定**：实验中仅用 MAML 作为元 RL 适配器，未探究与其他先进快速适应方法（如基于上下文的元学习）结合的效果。

（完）
