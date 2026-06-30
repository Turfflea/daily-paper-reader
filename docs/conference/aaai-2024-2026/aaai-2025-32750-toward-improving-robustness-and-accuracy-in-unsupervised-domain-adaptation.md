---
title: Toward Improving Robustness and Accuracy in Unsupervised Domain Adaptation
title_zh: 面向无监督域自适应的鲁棒性与准确性提升
authors: "Aishwarya Soni, Tanima Dutta"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/32750/34905"
tags: ["query:mt"]
score: 7.0
evidence: 自训练结合伪标签细化实现鲁棒无监督域自适应
tldr: 无监督域自适应中缺乏目标域标签，伪标签噪声导致对抗鲁棒性和准确性下降。本文提出CAM+SPLR，一种自训练方法，包含一致性注意力映射与自伪标签细化，通过可靠伪标签和注意力对齐提升性能。实验表明，该方法在多个UDA基准上同时提升了干净和对抗样本下的性能，为鲁棒域自适应提供了有效解决方案。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32750/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 850, \"height\": 322, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32750/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 889, \"height\": 627, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32750/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1556, \"height\": 967, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32750/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 622, \"height\": 809, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32750/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 654, \"height\": 691, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32750/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 825, \"height\": 284, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32750/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1634, \"height\": 391, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32750/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1851, \"height\": 391, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32750/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 890, \"height\": 389, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32750/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 895, \"height\": 283, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32750/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 880, \"height\": 323, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32750/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 803, \"height\": 285, \"label\": \"Table\"}]"
motivation: 无监督域自适应中缺乏目标域标签，伪标签噪声导致对抗鲁棒性和准确性下降。
method: 提出自训练范式下的CAM+SPLR，包含一致性注意力映射与自伪标签细化。
result: 在多个UDA基准上同时提升了干净和对抗样本下的性能。
conclusion: 通过可靠伪标签和注意力对齐，实现了更鲁棒且准确的无监督域自适应。
---

## Abstract
Adversarial robustness in the context of Unsupervised Domain Adaptation (UDA) is particularly challenging due to the lack of labels in the target domain. Pseudo labels are often
used to make adversarial robust models but compromise robustness and accuracy, falling short of the performance due to noise and inaccuracies in these pseudo labels. The main challenges in achieving robustness and accuracy include ensuring reliable pseudo labels and developing effective training methods that bring alignment between clean and adversarial examples of target data. To address these challenges, we propose a novel training method within the self-training paradigm Consistent Attention Mapping with Self Pseudo Label Refinement (CAM+SPLR). It begins with the pre-training of the UDA model, resulting in a UDA pre-trained model, which is initialized into two separate models: the Anchor model and the TargetNet model. The Anchor model encourages the attention maps of clean images and their adversarial counterparts to be similar, while the TargetNet model simultaneously performs self-training using  Adversarial target data and refining the pseudo labels. CAM+SPLR improves both semantically relevant key features and pseudo-labels through a two-step stochastic gradient descent process during training. We conducted extensive experiments on benchmark datasets, including OfficeHome, PACS, and VisDA, demonstrating significant improvements in both robustness and accuracy. Our method achieves an average accuracy improvement of 6% and 8.1% and an average robustness improvement of 10.2% and 4.9%, compared to state-of-the-art methods on the PACS and VisDA datasets.

---

## 论文详细总结（自动生成）

### 1. 研究动机与核心问题
- **背景**：无监督域适应（UDA）在无标签目标域上的泛化取得了成功，但对抗鲁棒性研究严重滞后。现有的对抗训练依赖真实标签，而 UDA 场景下只能依赖伪标签，这会引入噪声和不准确。
- **核心挑战**：
  - 伪标签噪声导致模型在目标域对抗样本上过拟合，泛化能力下降。
  - 对抗扰动和错误伪标签会使模型关注语义无关的特征（注意力偏移），损害干净样本和对抗样本的性能。
- **目标**：同时提升 UDA 模型的对抗鲁棒性与标准准确率，通过可靠伪标签以及对齐干净与对抗样本的注意力机制来解决上述问题。

### 2. 方法论
- **整体框架**：基于自训练的 **CAM+SPLR**（Consistent Attention Mapping with Self Pseudo Label Refinement）。
- **两阶段流程**：
  1. 预训练一个 UDA 基线模型（本文采用 DANN）。
  2. 初始化两个独立模型：
     - **Anchor 模型**：冻结参数，仅处理干净目标图像，生成注意力图作为监督。
     - **TargetNet 模型**：进行自训练，处理对抗样本，同时优化注意力对齐和伪标签。
- **关键技术**：
  - **一致性注意力映射（CAM）**：惩罚干净图像与对抗图像在指定特征层的注意力图之间的 L2 距离，促使模型关注语义关键区域。
    - 注意力损失：\( L_{\text{ATT}} = \sum_{z \in S} \left\| \frac{P_z^{\text{ADV}}}{\|P_z^{\text{ADV}}\|_2} - \frac{P_z^{\text{CLEAN}}}{\|P_z^{\text{CLEAN}}\|_2} \right\|_2 \)
  - **自伪标签细化（SPLR）**：在一个 epoch 内执行两步随机梯度下降：
    - **第一步 SGD**：优化组合损失 \( L_1 = \text{CE}(\hat{p}_t, y_t) + \lambda_1 L_{\text{ATT}} \)，更新 TargetNet 参数得到 \(\theta'_M\)。
    - **第二步 SGD**：利用更新后的模型重新预测，优化 \( L_2 = L_{\text{UDA}} + \lambda_2 L_{\text{SSL}} \)，进一步更新参数。
      - \( L_{\text{UDA}} \) 包括源域交叉熵和无监督一致性损失（干净与对抗预测一致）。
      - \( L_{\text{SSL}} = \Delta_{\text{CE}} \cdot \text{CE}(\hat{p}_t, \hat{y}_t) \)，其中 \(\Delta_{\text{CE}}\) 是第一步更新前后在源域上的交叉熵变化，用于调整伪标签的权重。
- **算法本质**：通过注意力对齐约束增强特征鲁棒性，同时利用第二步梯度反馈动态修正伪标签质量，无需额外模型或复杂组件。

### 3. 实验设计
- **数据集**：
  - OfficeHome（4 域，65 类）
  - PACS（4 域，7 类）
  - VisDA（2 域，12 类）
- **基线对比**：
  - **UDA 基线**：DANN（主要）、DAN、JAN。
  - **UDA + 对抗训练变体**：UDA+AT、UDA+Trades、UDA+Mart。
  - **SOTA 方法**：ARTUDA、SRoUDA、DART。
- **攻击设置**：白盒 PGD20，扰动半径 \( \epsilon = 2/255 \) 和 \( 8/255 \)（VisDA/PACS 部分实验），评估 FGSM、PGD10、CW∞ 等多种攻击。
- **评估指标**：干净准确率与对抗鲁棒性（PGD20 下分类正确率）。

### 4. 资源与算力
- 论文未明确说明 GPU 型号、数量及训练时长。
- 部分实现细节：使用 ResNet-50 骨干，批量大小为 32，Adam 优化器，学习率 0.001，CAM+SPLR 共迭代 25K 次。其余算力信息缺失。

### 5. 实验数量与充分性
- **实验丰富度**：
  - 多个源-目标域配对实验（OfficeHome 12 对、PACS 12 对、VisDA 2 向）。
  - 不同攻击强度（\( \epsilon = 2/255 \)、\( 8/255 \)）和多攻击类型。
  - 消融实验：注意力特征层级（低、中、高、所有层）对比；CAM 与 SPLR 独立组件贡献。
  - UDA 基线替换实验（DAN、JAN、DANN）。
  - 攻击迭代次数和扰动大小对鲁棒性的影响。
- **充分性评价**：实验覆盖了主流的域适应基准，多维度对比公平，消融充分，具备较强说服力。但缺少在更强的自适应攻击（如 AutoAttack）下的评估，计算资源信息也未报告。

### 6. 主要结论与发现
- **性能提升**：
  - 在 PACS 和 VisDA 上，平均准确率分别提升 6% 和 8.1%，平均鲁棒性提升 10.2% 和 4.9%（相比 SOTA）。
  - 在 OfficeHome 上鲁棒性平均提升 5.2%，准确率提升 0.9%。
- **关键发现**：
  - 联合使用所有特征层注意力图（All Levels）比仅用高层图性能更好（约 2-3% 的提升）。
  - CAM 与 SPLR 单独使用均有贡献，两者协同可大幅超越标准对抗训练的基线（10–14% 提升）。
  - 方法对不同的 UDA 基线具有普适性，且在不同攻击强度下保持领先。

### 7. 优点
- **创新性**：首次将注意力一致性约束与伪标签逐步细化结合在 UDA 自训练框架中，解决伪标签噪声与注意力偏移的双重问题。
- **有效性**：在多个数据集上同时显著提升鲁棒性和准确率，超越现有 SOTA。
- **简洁性**：无需额外模型，仅通过两步 SGD 实现联合优化，易于与现有 UDA 方法集成。
- **可解释性**：通过注意力图可视化清晰展示了方法对语义特征的聚焦能力。

### 8. 不足与局限
- **攻击评估局限**：仅考虑 L∞ 范数的 PGD 攻击，未验证在 L2、L1 或更复杂的自适应攻击（如 AutoAttack）下的表现。
- **伪标签阈值依赖**：训练中使用置信度阈值筛选伪标签，但未详细讨论其敏感性或自适应调整策略。
- **计算开销**：双模型架构 + 两步 SGD 可能会导致训练时间增加，但论文未提供实际训练成本数据。
- **评估域适应种类有限**：仅在分类任务上验证，未推广至目标检测、分割等更复杂的 UDA 场景。
- **UDA 基线影响**：性能高度依赖预训练 UDA 模型的质量（实验中 DANN 基线本身鲁棒性极低），可能导致初始伪标签质量波动。

（完）
