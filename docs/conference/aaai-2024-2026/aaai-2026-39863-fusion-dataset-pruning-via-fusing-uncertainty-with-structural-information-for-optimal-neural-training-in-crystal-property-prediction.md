---
title: "FUSION: Dataset Pruning via Fusing Uncertainty with Structural Information for Optimal Neural Training in Crystal Property Prediction"
title_zh: FUSION：通过融合不确定性与结构信息进行最优神经训练的晶体性质预测数据集剪枝
authors: "Xiean Wang, Pin Chen, Liqin Tan, Yutong Lu, Qingsong Zou"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39863/43824"
tags: ["query:mt"]
score: 8.0
evidence: 使用深度学习和数据集剪枝进行晶体性质预测，是非管理领域预测建模的实例
tldr: 大规模材料数据库为机器学习加速发现提供机遇，但更多数据未必带来更好模型。本文提出FUSION方法，融合不确定性量化和晶体结构指纹分析，将数据集剪枝建模为离散优化问题，在三个基准上一致优于基线，展示了智能数据选择对材料预测的作用，可迁移至管理科学中的数据优化建模。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39863/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1819, \"height\": 742, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39863/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1832, \"height\": 596, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39863/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1839, \"height\": 631, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39863/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 869, \"height\": 781, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39863/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 845, \"height\": 264, \"label\": \"Table\"}]"
motivation: 材料数据增多并不总能提升模型性能，需要有效的数据剪枝策略提升训练效率与预测精度。
method: 提出离线剪枝策略FUSION，结合不确定性量化与几何指纹晶体结构分析。
result: 在三个晶体性质预测数据集上，FUSION显著优于随机剪枝和基于不确定性的剪枝。
conclusion: 智能数据剪枝可提升材料预测模型训练效果，思路可推广至管理领域的样本选择与优化。
---

## Abstract
The rapid expansion of materials databases offers unprecedented opportunities for accelerating materials discovery via machine learning. However, the widespread assumption that larger datasets inherently produce better models does not hold in practice. We propose FUSION (Fusing Uncertainty with Structural Information for Optimal Neural training), an offline dataset pruning strategy that synergistically combines uncertainty quantification with crystallographic structure analysis via geometric fingerprinting, framing dataset pruning as a discrete optimization problem. Through evaluation across 3 benchmark datasets, FUSION consistently outperforms baselines, including random pruning, uncertainty sampling, weighting factor pruning, diversity sampling, and active learning. It demonstrates robust transferability across 11 diverse architectures, outperforming random pruning by 1.91–13.65% across different datasets, with an average improvement of 6.36%. Moreover, our analysis suggests that different models exhibit varying robustness characteristics when faced with pruned training data, highlighting the importance of model selection tailored to dataset composition. We identify optimal pruning points where removing just 0–8% of training data improves model performance, yielding gains up to 12.67% in specific model–dataset combinations. These results establish a new paradigm for materials informatics that prioritizes data quality over quantity, offering a pathway toward more efficient and sustainable machine learning workflows in computational materials science.

---

## 论文详细总结（自动生成）

# 论文总结：FUSION——融合不确定性与结构信息的最佳晶体性质预测数据集剪枝

## 1. 研究动机与核心问题
- **核心问题**：大规模材料数据库的迅速增长为机器学习驱动的材料发现创造了条件，但“更多数据必然带来更好模型”的假设在实践中并不成立。数据冗余、近重复结构以及低质量样本会阻碍模型训练效率和泛化能力。
- **整体含义**：论文旨在改变“数据数量优先”的传统观念，提出一种智能的数据集剪枝策略，通过选择对模型训练最有益的子集，实现在减少数据量的同时提升甚至优化模型预测性能，为材料信息学建立“数据质量优于数量”的新范式。

## 2. 方法论：FUSION 的核心思想与技术细节
- **总体框架**：FUSION 是一种离线数据集剪枝方法，将剪枝问题形式化为离散优化：寻找一个最大可移除子集，使得移除后模型性能下降不超过给定阈值 ω。
- **不确定性量化**：
  - 使用深度证据回归（DER）同时估计数据驱动的不确定性（aleatoric）和模型驱动的不确定性（epistemic）。
  - 对每个晶体结构 `xi`，从模型输出参数化的正态-逆伽马（NIG）分布，组合成总不确定性 `U(xi) = sigmoid( (U_ale + ε·U_epi) )`，通过 ε 调节两种不确定性的权重。
- **几何结构指纹与加权因子**：
  - 采用平滑原子位置重叠（SOAP）描述符提取旋转、平移不变的晶体结构指纹 `F(xi)`。
  - 定义加权因子 `W(xi) = exp(ς · min_{j≠i} d(xi,xj))`，其中 `d(·,·)` 为余弦距离。该因子对与其它结构高度相似的样本赋予更大权值，惩罚冗余。
- **影响分数与优化**：
  - 样本影响评分 `I(xi) = U(xi) / W(xi)`，低不确定性且高结构冗余的样本得分低，优先被移除。
  - 将剪枝转化为受约束的组合优化问题：最大化移除样本数，同时保持移除样本的平均影响分数 ≤ ω。采用**模拟退火**算法求解。
- **迭代工作流**：整个流程包含七阶段闭环（数据集划分、代理模型训练、几何特征分析、影响评分、离散优化、剪枝与模型训练评估），迭代调整 ω，直至找到使模型性能最优的剪枝点。

## 3. 实验设计
- **数据集**：来自 Matbench 的三个晶体性质预测任务：
  - **Dielectric**：4764 个样本，预测折射率（光学性质）。
  - **JDFT2D**：636 个样本，预测二维层状材料的剥离能（力学性质）。
  - **Perovskites**：18928 个样本，预测钙钛矿形成能（热力学性质）。
- **数据划分**：固定 70% 训练、15% 验证、15% 测试；测试集在所有方法中保持一致。
- **基准对比方法**：
  - 随机剪枝、基于 SOAP 的多样性采样（最远点采样）、基于集成的主动学习、仅使用结构信息的加权因子剪枝、仅使用不确定性的不确定性采样。
- **评估指标**：平均绝对误差（MAE）。进行 5 次不同随机种子实验并报告均值与统计检验。
- **跨模型与分布外（OOD）评估**：
  - 在 11 种不同架构上（图神经网络、Transformer 等）验证剪枝策略的迁移性。
  - 采用基于聚类的留一簇交叉验证模拟分布外泛化场景。

## 4. 资源与算力
- 论文**未明确说明**所使用的 GPU 型号、数量或具体训练时长。因此无法从文中推断计算资源需求和整体训练开销。

## 5. 实验数量与充分性
- **实验规模**：
  - 覆盖 3 个数据集、5 种基线+所提方法、4 种剪枝比例（20%, 40%, 60%, 80%）。
  - 在 11 种模型架构上进行跨模型一致性测试。
  - 额外进行消融实验（单独不确定性、单独加权因子）、分布外实验，以及以 0.5% 为步长搜索最优剪枝比的精细实验。
- **充分性与公平性**：
  - 实验设计较全面，多维度验证了方法的有效性和鲁棒性。
  - 采用固定测试集和多次随机重复，配合统计显著性检验，保证了比较的公平性。
  - 消融研究清晰展示了结合不确定性与结构信息的必要性。

## 6. 主要结论与发现
- **一致的性能优势**：FUSION 在所有修剪比下均优于其他基准，高剪枝比例时优势更显著（如 JDFT2D 在 80% 剪枝下比多样性采样 MAE 降低 12.7%）。
- **最优剪枝点现象**：首次在材料科学中系统性量化了“最优剪枝点”——仅移除 0–8% 的训练样本即可使模型性能提升（最高 12.67%），说明少量高质量的剪枝即能去除噪声与冗余。
- **跨模型迁移与敏感性**：FUSION 剪枝策略可迁移至 11 种不同神经网络结构，相对随机剪枝平均提升 6.36%。不同模型对剪枝数据的鲁棒性不同，揭示了模型选择应结合数据集组成。
- **OOD 挑战**：在分布外场景下，各方法间的性能差距缩小，表明当面临新的化学空间时，数据选择策略的优势被根本性的外推困难削弱。

## 7. 优点与亮点
- **创新性融合**：率先将模型认知（不确定性）与领域知识（晶体结构指纹）有机结合，用于材料数据的精细剪枝。
- **优化建模**：将剪枝抽象为离散优化问题，并用模拟退火求解，提供了严谨的数学框架。
- **发现最优剪枝点**：实验观察到“少即是多”的反直觉现象，验证了智能数据选取的价值。
- **完善的实验体系**：包含多数据集、多基线、跨架构迁移、分布外测试和统计检验，结果可信。
- **实用性**：提供端到端训练管线，方法可与现有材料预测模型无缝集成。

## 8. 不足与局限
- **代理模型依赖**：不确定性通过 ALIGNN + DER 获取，方法和结果可能对该特定代理模型存在偏好；未探索其他不确定性估计算法的影响。
- **分布外性能增益有限**：在 OOD 条件下 FUSION 的优势大幅缩小，方法的普适性在真正未知的材料空间中尚需验证。
- **计算开销未讨论**：模拟退火优化虽可解耦训练，但迭代流程和剪枝比例搜索可能引入不可忽视的计算成本，文中缺乏时间复杂度或实际运行时间分析。
- **超参数敏感性**：ω、ε、ς 等对性能有直接影响，但缺少系统性的超参数敏感性分析。
- **应用覆盖范围**：仅在三种材料性质上验证，扩展至更多样化的材料体系（如多元素合金、复杂缺陷结构）时的有效性有待考证。

（完）
