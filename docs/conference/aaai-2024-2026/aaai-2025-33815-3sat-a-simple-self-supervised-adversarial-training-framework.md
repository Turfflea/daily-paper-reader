---
title: "3SAT: A Simple Self-Supervised Adversarial Training Framework"
title_zh: "3SAT: 一种简单的自监督对抗训练框架"
authors: "Jiang Fang, Haonan He, Jiyan Sun, Jiadong Fu, Zhaorui Guo, Yinlong Liu, Wei Ma"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/33815/35970"
tags: ["query:mt"]
score: 10.0
evidence: 自监督对抗训练框架，提升自监督模型的鲁棒性
tldr: 针对自监督对抗训练鲁棒性落后于监督对抗训练的问题，提出3SAT框架，通过联合优化自监督学习和对抗训练，弥合性能差距；实验表明，该方法显著提升了自监督模型在对抗攻击下的安全性，为自监督模型的安全部署提供了有效鲁棒训练范式。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33815/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 712, \"height\": 590, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33815/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 805, \"height\": 605, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33815/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 826, \"height\": 383, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33815/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 865, \"height\": 380, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33815/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 858, \"height\": 374, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33815/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 854, \"height\": 394, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33815/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1116, \"height\": 355, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33815/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1122, \"height\": 392, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33815/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 835, \"height\": 109, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33815/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 882, \"height\": 288, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33815/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 876, \"height\": 141, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33815/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 549, \"height\": 140, \"label\": \"Table\"}]"
motivation: 自监督对抗训练的鲁棒性仍落后于监督对抗训练，尽管自监督学习模型性能已追平甚至超越。
method: 提出简单的自监督对抗训练框架3SAT，通过联合优化解决自监督学习与对抗训练的冲突。
result: 提升了自监督模型在对抗攻击下的鲁棒性，缩小与监督方法的差距。
conclusion: 3SAT为自监督模型的安全部署提供了有效的鲁棒训练范式。
---

## Abstract
The combination of self-supervised learning and adversarial training (AT) can significantly improve the adversarial robustness of self-supervised models. However, the robustness of self-supervised adversarial training (self-AT) still lags behind that of state-of-the-art (SOTA) supervised AT (sup-AT), even though the performance of current self-supervised learning models has already matched or even surpassed that of SOTA supervised learning models. This issue raises concerns about the secure application of self-supervised learning models.
 The inclusion of adversarial training turns self-AT into a challenging joint optimization problem, and recent studies have shown that the data augmentation methods necessary for constructing positive pairs in self-supervised learning negatively impact the robustness improvement in self-AT. Inspired by this, we propose 3SAT, a simple self-supervised adversarial training framework. 3SAT conducts adversarial training on original, unaugmented samples, reducing the difficulty of optimizing the adversarial training subproblem and fundamentally eliminating the negative impact of data augmentation on robustness improvement. Additionally, 3SAT introduces a dynamic training objective scheduling strategy to address the issue of model training collapse during the joint optimization process when using original samples directly. 3SAT is not only structurally simple and computationally efficient, reducing  self-AT training time by half, but it also improves the SOTA self-AT robustness accuracy by 16.19\% and standard accuracy by 11.41\% under Auto-Attack on the CIFAR-10 dataset. Even more impressively, 3SAT surpasses the SOTA sup-AT method in robust accuracy by a significant margin of 11.25\%. This marks the first time that self-AT has outperformed SOTA sup-AT in robustness, indicating that self-AT is a superior method for improving model robustness.

---

## 论文详细总结（自动生成）

### 1. 论文核心问题与整体含义
- **核心问题**：尽管自监督学习（SSL）在标准任务上的性能已追平甚至超越监督学习，但当前的**自监督对抗训练（self-AT）** 在对抗鲁棒性上仍明显落后于**监督对抗训练（sup-AT）**，且数据增强操作对鲁棒性提升存在负面影响，这严重限制了自监督模型在安全敏感场景中的实际部署。
- **整体含义**：论文旨在**缩小乃至逆转 self-AT 与 sup-AT 之间的鲁棒性差距**，提出一种简单、高效的自监督对抗训练框架，既大幅提升鲁棒性，又几乎不牺牲标准表示能力，从而为自监督模型的安全应用提供更优的训练范式。

### 2. 方法论
- **核心思想**：直接在**原始（未经增强）样本**上执行对抗训练，从根本上消除数据增强对鲁棒性提升的干扰，并降低对抗训练子问题的优化难度。
- **框架结构 (3SAT)**：
  - **左分支（SSL）**：基于 BYOL 风格的自监督学习，利用增强后的正样本对 $(\tilde{x}, \tilde{x}^+)$ 最小化 MSE 损失 $\ell_{\text{SSL}}^{\text{MSE}}$。
  - **右分支（AT）**：以原始样本 $x$ 与其对抗样本 $x+\delta$ 构成正样本对，最小化它们的 MSE 损失 $\ell_{\text{AT}}^{\text{MSE}}$，其中 $\delta$ 通过 PGD 攻击最大化该 MSE 损失得到（公式 6）。
  - **联合优化目标**（公式 5）：
    \[
    \min_{g,h}\ \mathbb{E}_{x}\big[\ell_{\text{SSL}}^{\text{MSE}} + s(t)\cdot \ell_{\text{AT}}^{\text{MSE}}\big]
    \]
    其中 $s(t)$ 是动态调度函数，$t$ 为当前训练轮次。
- **动态训练目标调度 (DTOS)**：
  - 为避免直接联合优化时因 AT 损失过小而导致的**训练崩塌**，引入线性热身调度：
    \[
    s(t) = \max\left(0,\ \frac{t - W}{K - W}\right)
    \]
    $W$ 为纯 SSL 预热轮数，$K$ 为总轮数；预热后逐渐增加 AT 权重。
  - 该调度可视为一种**解耦合策略**，让编码器先获得良好表示，再逐步提升鲁棒性。
- **简化设计**：**弃用双 BN 路径**，减少参数冗余，在提升鲁棒性的同时保持高表示能力；且每步只需生成一个对抗样本（以往方法需两个），计算量减半。

### 3. 实验设计
- **数据集与评估协议**：
  - 预训练与评估数据集：**CIFAR‑10、CIFAR‑100、STL‑10**。
  - 鲁棒性评测：基于 **Auto-Attack (AA)** 的鲁棒准确率，以及标准准确率 (SA)。
  - 三种微调评估方式：标准线性微调 (SLF)、对抗线性微调 (ALF)、对抗全微调 (AFF)。
- **基准对比方法**：
  - 自监督对抗训练方法：RoCL、ACL、AdvCL、TAROSS、DynACL、DynACL‑AIR。
  - 监督对抗训练 SOTA：来自 RobustBench 的 **sup-AT**（ResNet‑18 网络）。
- **关键实验设置**：
  - Backbone：**ResNet‑18**，基于 BYOL（solo‑learn 实现），批量大小 256，预训练 1000 轮。
  - 攻击生成：$\ell_\infty$ PGD‑5 步（预训练阶段），评估用 Auto‑Attack。
  - 不同预热轮数的影响：CIFAR‑100 上测试 $W=0,200,400,600,800$。
- **额外实验**：
  - 训练速度对比（RTX3090，小时）。
  - 表示可视化（UMAP）、损失景观对比（DynACL++ vs 3SAT）。
  - 双 BN 路径的消融（自然路径与对抗路径的精度/鲁棒性）。
  - 半监督场景（1% / 10% 标签）的鲁棒性。

### 4. 资源与算力
- **硬件**：单张 **NVIDIA RTX 3090** GPU。
- **训练耗时**：3SAT 预训练仅需 **14.28 小时**，而 DynACL 需 29.4 小时，ACL 需 32.7 小时，AdvACL 高达 105.0 小时。训练效率大幅提升。
- **时间复杂度分析**：因减少一次对抗样本生成，3SAT 单步复杂度约为 $O((4+2K)\cdot M)$，而 DynACL 约为 $O((4+4K)\cdot M)$，理论加速近一倍。

### 5. 实验数量与充分性
- 论文进行了**多维度、较充分的实验验证**：
  - 覆盖 3 个不同规模的数据集，兼顾低分辨率与中分辨率。
  - 采用了 3 种微调评估方式，全面衡量表示质量与鲁棒性。
  - 对比了 6 种自监督对抗训练基线 + 监督对抗 SOTA。
  - 完成训练速度、可视化、损失景观、预热策略、原始/增强样本、双 BN 路径等**多项消融与分析实验**。
  - 额外评估了半监督条件下的标签效率。
- 实验设计**公平对比**：统一网络结构（ResNet‑18），相同 PGD 超参，且引入公开基准（RobustBench）的 sup‑AT 结果。
- 不足：并未在更大规模数据集（如 ImageNet）或更现代架构（ViT/Transformer）上验证，但现有套数足以支撑其核心主张。

### 6. 主要结论与发现
- **鲁棒性大幅刷新 SOTA**：在 CIFAR‑10 上，3SAT 在 AFF 下 AA 达 **66.79%**，较先前最佳 self‑AT 方法提升 16.19%，SA 达 93.55%，提升 11.41%。
- **首次超越监督对抗训练**：AA 超过 sup‑AT SOTA 方法 **11.25%**，证明 self‑AT 可以比 sup‑AT 更有效地提升鲁棒性。
- **几乎无表示能力损失**：3SAT 的标准准确率（>93%）接近无 AT 的自监督模型，解决了以往 self‑AT 严重损害标准精度的问题。
- **训练高效**：训练耗时减半，仅需 14.28 小时，计算更环保。
- **预热调度至关重要**：DTOS 避免了直接使用原始样本时的训练崩塌，合理设置预热（如 CIFAR‑100 上 W=200）可实现最优的精度–鲁棒性权衡。

### 7. 优点
- **方法简洁有效**：仅用原始样本 + 动态权重调度，框架清晰，无需复杂组件。
- **消除数据增强副作用**：从根本上解耦增强与对抗训练，相比 DynACL 等手动调参更优雅。
- **性能突破**：首次实现 self‑AT 鲁棒性超越 sup‑AT，且标准准确率几乎无损。
- **计算效率显著**：减半的训练时间有利于快速迭代和环保。
- **标签高效**：仅用 1% 标签的 3SAT 鲁棒性已超过使用全部标签的先前 SOTA，凸显潜力。

### 8. 不足与局限
- **实验规模有限**：仅在 CIFAR‑10/100 和 STL‑10 上验证，未在 ImageNet 等大型数据集上测试，扩展性有待证明。
- **架构单一**：仅使用 ResNet‑18，未探讨该框架在 Vision Transformer 或其它现代骨干网络上的表现。
- **攻击类型局限**：预训练和评估主要基于 $\ell_\infty$ PGD 与 Auto‑Attack，对 $\ell_2$、$\ell_1$ 或其他攻击（如黑盒迁移攻击）的泛化性未深入分析。
- **调度函数依赖超参**：需为不同数据集选择预热轮数 $W$，虽然论文指出模型对 $W$ 不敏感，但额外调参仍不可避免。
- **基线对比范围**：监督对比 SOTA 取自 RobustBench，可能未穷尽所有对抗训练技巧（如额外数据、大模型蒸馏等），但已足够支撑主要结论；自监督方法仅基于 BYOL，未探讨 SimCLR、MoCo 等变体。
- **无理论收敛保证**：动态调度虽属经验有效，但缺乏优化理论层面的严格支撑。

（完）
