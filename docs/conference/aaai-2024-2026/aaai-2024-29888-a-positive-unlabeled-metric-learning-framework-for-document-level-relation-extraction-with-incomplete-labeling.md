---
title: A Positive-Unlabeled Metric Learning Framework for Document-Level Relation Extraction with Incomplete Labeling
title_zh: 面向不完整标注的文档级关系抽取的正无标记度量学习框架
authors: "Ye Wang, Huazheng Pan, Tao Zhang, Wen Wu, Wenxin Hu"
date: 2024-03-25
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/29888/31550"
tags: ["query:mt"]
score: 8.0
evidence: 面向不完整标注的文档级关系抽取的正无标记度量学习
tldr: P3M将文档级关系抽取建模为不完整标注下的度量学习问题，提出正增强和正混合的正无标记度量学习框架。它通过拉近实体对与对应关系嵌入、推远与其他关系，有效缓解了标注不全问题，在DocRED和Re-DocRED数据集上取得了优于现有方法的效果，为弱监督信息抽取提供了新思路。
source: AAAI-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29888/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1597, \"height\": 613, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29888/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 883, \"height\": 895, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29888/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1114, \"height\": 271, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29888/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1671, \"height\": 936, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29888/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 817, \"height\": 537, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29888/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 760, \"height\": 501, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29888/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 838, \"height\": 314, \"label\": \"Table\"}]"
motivation: 文档级关系抽取中普遍存在标注不完整问题，现有正负无偏学习方法仍有提升空间。
method: 提出P3M框架，将关系抽取形式化为度量学习，结合正数据增强和混合策略。
result: 在文档级关系抽取基准上，P3M显著优于基线，尤其在标注不全场景下优势明显。
conclusion: 通过度量学习方法适配正无标记场景，有效提高了弱监督关系抽取的鲁棒性。
---

## Abstract
The goal of document-level relation extraction (RE) is to identify relations between entities that span multiple sentences. Recently, incomplete labeling in document-level RE has received increasing attention, and some studies have used methods such as positive-unlabeled learning to tackle this issue, but there is still a lot of room for improvement. Motivated by this, we propose a positive-augmentation and positive-mixup positive-unlabeled metric learning framework (P3M). Specifically, we formulate document-level RE as a metric learning problem. We aim to pull the distance closer between entity pair embedding and their corresponding relation embedding, while pushing it farther away from the none-class relation embedding. Additionally, we adapt the positive-unlabeled learning to this loss objective. In order to improve the generalizability of the model, we use dropout to augment positive samples and propose a positive-none-class mixup method. Extensive experiments show that P3M improves the F1 score by approximately 4-10 points in document-level RE with incomplete labeling, and achieves state-of-the-art results in fully labeled scenarios. Furthermore, P3M has also demonstrated robustness to prior estimation bias in incomplete labeled scenarios.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义
- **研究背景**：文档级关系抽取旨在识别跨句子的实体间关系。现有数据集（如DocRED）存在严重的标注不完整问题，即大量真实关系被遗漏（假阴性），导致监督学习性能骤降。
- **核心挑战**：传统的正‑无标记（PU）学习方法在面对标注偏差和样本分布不完整时，模型泛化性不足；且已有的PU基线（如SSR‑PU）仍有较大提升空间。
- **整体含义**：本文提出将文档级关系抽取转化为一个**正‑无标记度量学习**任务，通过对正样本进行增强和混合，大幅提升模型在不完整标注乃至完全标注场景下的性能，同时显著提高对先验估计偏差的鲁棒性。

## 2. 方法论
- **整体框架**：**P3M（Positive‑augmentation and Positive‑mixup Positive‑Unlabeled Metric Learning）**。
- **核心思想**：
  - 为每个预定义关系以及“无关系”类（none‑class）分别学习一个代理嵌入（anchor embedding）。
  - 训练时，将实体对的嵌入拉向对应的关系嵌入，推远“无关系”嵌入；采用**SoftMax‑norm 损失**作为度量学习目标。
  - 将该目标适配到PU学习范式，得到正‑无标记度量学习损失（PUM）。
- **关键技术细节**：
  1. **正‑无标记度量学习（PM）**：利用已标注正样本与未标注样本，结合先验概率估计和非负风险估计（Non‑Negative Risk Estimator）推导最终损失函数（式5），并引入类别权重缓解类不平衡。
  2. **基于Dropout的正样本增强（P2M）**：仅对已标注正样本施加dropout噪声，生成增强嵌入$\mathbf{x}'$，扩展正样本分布，同时保持未标注数据的先验不变，损失函数见式7。实验表明仅增强正样本优于增强全量样本。
  3. **正‑无关系类混合（P3M）**：采用mixup思想，将正样本嵌入与“无关系”类代理嵌入进行插值（式9），生成更多样的伪样本，进一步强化模型泛化。损失函数见式11。
  - 最终总损失为 PM 损失与 mixup 损失的加权和（式12），由超参数$\nu$控制强度。
- **推理方式**：对于任意实体对，若其嵌入与关系$i$的相似度高于“无关系”嵌入，则认为关系$i$成立。

## 3. 实验设计
- **数据集与场景**：
  - **DocRED**：原始不完全标注训练集（3,053篇文档，96种关系） + 完全标注的Re‑DocRED测试集。
  - **DocRED ext**：极端不完全标注的训练集（平均每文档仅5.4个关系），评估极端标签缺失下的性能。
  - **ChemDisGene**：生物医学领域远程监督训练集（不完全标注，76,942篇，14种关系） + 人工重新标注的全关系测试集。
  - 完全标注设定：在Re‑DocRED训练集上进行全监督训练，测试模型在无缺失标签情况下的表现。
- **对比方法**：
  - 传统全监督模型：BiLSTM, GAIN, DocuNET, ATLOP。
  - PU学习基线：SSR‑PU（Wang et al. 2022）。
  - 自身变体：PM (无增强)、P2M(all)（增强所有样本）、P2M（仅增强正样本）、P3M(ori)（对未标注样本进行mixup）、P3M（对none‑class嵌入进行mixup）。
- **编码器/预训练模型**：BERT$_{\text{Base}}$、RoBERTa$_{\text{Large}}$（DocRED）、PubMedBERT（ChemDisGene）。
- **评估指标**：微平均F1、Ign F1（排除训练集共享关系）、精确率、召回率。

## 4. 资源与算力
- **硬件**：所有实验均在 **1块 Tesla A100 或 Tesla V100 GPU** 上完成。
- **训练配置**：DocRED 批量大小4，ChemDisGene 批量大小8，学习率3e-5（DocRED）或2e-5（ChemDisGene），训练10个epoch，均使用线性预热+线性衰减。论文未明确给出单次训练时长，但可推断所需算力适中，实验规模适合常见学术环境。

## 5. 实验数量与充分性
- **实验组数**：
  - 在 DocRED 和 DocRED ext 上比较了 BERT$_{\text{Base}}$ 和 RoBERTa$_{\text{Large}}$ 两种设置下 **6种方法变体 + 基线** 共8组结果。
  - 在 ChemDisGene 上进行了类似的 **6种变体对比** 及与多个基线比较。
  - 在**完全标注设定**下，与 SOTA 模型（ATLOP, DocuNET, KD‑DocRE, SSR‑PU）进行了对比。
  - 对关键超参数（$\lambda$、dropout率、$\alpha$、$\nu$）进行了**独立变化实验**（每个参数5‑6个取值），并展示了 F1 变化曲线。
  - 进行了**先验估计鲁棒性实验**，从$\pi_i = \pi_{\text{labeled},i}$ 到 $5\pi_{\text{labeled},i}$ 共5组不同估计。
- **实验充分性与公平性**：
  - 实验覆盖了不同程度的不完整标注、不同领域、不同预训练模型，**对比丰富**。
  - **消融设计合理**：逐步从PM→P2M(all)→P2M→P3M(ori)→P3M，清晰展示各模块贡献。
  - 采用**官方标准评估协议**，未使用开发集或测试集调参，最终结果取5次不同随机种子的平均值，**公平性强**。
  - 超参数分析详细且可视化，支持方法选择的合理性。

## 6. 主要结论与发现
- P3M 在 DocRED 不完全标注训练集上，F1 相比 SSR‑PU 在 BERT$_{\text{Base}}$ 和 RoBERTa$_{\text{Large}}$ 下分别提升约 **4.9 和 4.8 个百分点**。
- 在极端不完全标注场景（DocRED ext）下，F1 提升更显著，分别为 **9.8 和 10.1 个百分点**。
- 即使使用完全标注数据，P3M 也取得了 state‑of‑the‑art 的结果（RoBERTa$_{\text{Large}}$ 下 F1 80.02）。
- 仅增强正样本的 dropout 策略优于全量样本增强；用 none‑class 关系嵌入作为伪负样本的 mixup 比直接混合未标注样本更有效。
- 方法对**先验估计偏差不敏感**：即使设为$\pi_i = \pi_{\text{labeled},i}$（即假设无未标注正样本），模型性能依然稳健，利于实际应用。

## 7. 优点
- **问题适配巧妙**：将度量学习与PU学习结合，引入“无关系”代理嵌入，使得正负样本分离更直觉化，且易于适配 mixup 等增强技术。
- **模块设计有据可依**：基于 PU 学习的理论推导，确保损失函数的无偏性；增强策略仅作用于正样本，避免了未标注数据中的噪声放大。
- **实验扎实全面**：涵盖多种标签缺失程度、不同领域、不同预训练模型，消融实验和超参数分析详尽，结论可信。
- **鲁棒性强**：对先验估计的宽容度极大降低了对精确先验的依赖，有利于部署到真实弱监督场景。

## 8. 不足与局限
- **基础模型依赖性**：部分误差仍来源于编码器自身的分类能力，若底层模型效果不佳，精确率会受影响。
- **超参数仍需微调**：$\nu$、dropout 率等与标签缺失程度相关，文中虽有敏感性分析，但未给出自动选择策略。
- **mixup 策略相对简单**：正样本与“无关系”类的线性插值未考虑关系间的语义相似性，可能引入边界模糊。
- **实验报告存在细节缺失**：未提供具体的训练耗时、内存占用等信息，不易评估实际工程成本。
- **领域覆盖有限**：仅测试了通用域（维基百科）和生物医学域，是否适用于其他领域（如法律、金融）的文档级 RE 有待验证。

（完）
