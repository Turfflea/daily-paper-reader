---
title: "SwitchTab: Switched Autoencoders Are Effective Tabular Learners"
title_zh: SwitchTab：切换式自编码器是有效的表格学习器
authors: "Jing Wu, Suiyao Chen, Qi Zhao, Renat Sergazinov, Chen Li, Shengjie Liu, Chongchao Zhao, Tianpei Xie, Hanqing Guo, Cheng Ji, Daniel Cociorva, Hakan Brunzell"
date: 2024-03-25
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/29523/30869"
tags: ["query:mt"]
score: 9.0
evidence: 面向表格数据的自监督表示学习
tldr: 针对表格数据自监督学习中样本依赖关系不明显的问题，提出SwitchTab方法，通过非对称编码器-解码器解耦样本对中的互信息和显著特征，学习更具判别性的嵌入，提升下游分类等任务的准确率，为表格数据的自监督表示学习提供了有效新范式。
source: AAAI-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29523/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 839, \"height\": 782, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29523/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1828, \"height\": 759, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29523/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1739, \"height\": 853, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29523/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 844, \"height\": 649, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29523/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1834, \"height\": 659, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29523/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 880, \"height\": 187, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29523/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1833, \"height\": 723, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29523/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 881, \"height\": 384, \"label\": \"Table\"}]"
motivation: 现有自监督表示学习方法难以捕捉表格数据中微弱的样本间依赖关系。
method: 提出SwitchTab，使用非对称编码器-解码器框架解耦样本对中的互信息和显著特征以学习嵌入。
result: 学习到的嵌入有助于形成更好的决策边界，提升下游预测性能。
conclusion: SwitchTab为表格数据的自监督表示学习提供了有效的解决方案，有望在管理科学等领域的预测建模中应用。
---

## Abstract
Self-supervised representation learning methods have achieved significant success in computer vision and natural language processing (NLP), where data samples exhibit explicit spatial or semantic dependencies. However, applying these methods to tabular data is challenging due to the less pronounced dependencies among data samples. In this paper, we address this limitation by introducing SwitchTab, a novel self-supervised method specifically designed to capture latent dependencies in tabular data. SwitchTab leverages an asymmetric encoder-decoder framework to decouple mutual and salient features among data pairs, resulting in more representative embeddings. These embeddings, in turn, contribute to better decision boundaries and lead to improved results in downstream tasks. To validate the effectiveness of SwitchTab, we conduct extensive experiments across various domains involving tabular data. The results showcase superior performance in end-to-end prediction tasks with fine-tuning. Moreover, we demonstrate that pre-trained salient embeddings can be utilized as plug-and-play features to enhance the performance of various traditional classification methods (e.g., Logistic Regression, XGBoost, etc.). Lastly, we highlight the capability of SwitchTab to create explainable representations through visualization of decoupled mutual and salient features in the latent space.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **研究背景**：自监督表示学习在计算机视觉和自然语言处理中取得成功，但这些方法依赖数据样本间的显式空间或语义依赖，难以直接应用于表格数据。
- **核心问题**：表格数据（tabular data）的样本间依赖关系不明显，特征异构（数值、类别混合）、冗余且分布离散，导致现有自监督方法难以捕捉关键潜在特征，影响下游任务（如分类）的决策边界。
- **整体含义**：论文提出 **SwitchTab**，一种生成式自监督预训练框架，通过解耦样本对中的“互信息”（mutual）与“显著信息”（salient）学习更具判别性的嵌入，从而提升表格数据的表示学习效果。

## 2. 方法论

- **核心思想**：借鉴对比变分自编码器（cVAE）和交换自编码器（Swapping Autoencoder）的思路，将样本对的特征空间显式解耦为 **互信息**（样本间共有、可交换）和 **显著信息**（样本特有、用于区分），并通过特征交换与重构强化这种解耦。
- **关键技术细节**：
  - **特征破坏（Feature Corruption）**：对每个样本随机选取部分特征，用该特征在数据集中的均匀分布采样值替换，增强鲁棒性。
  - **非对称编码器-解码器框架**：
    - 编码器 \( f \)：将破坏后的样本编码为通用嵌入 \( z_1, z_2 \)。
    - 两个投影头 \( p_m \)（互信息投影）和 \( p_s \)（显著信息投影）：将 \( z \) 投影为互信息嵌入 \( m \) 和显著嵌入 \( s \)。
    - 解码器 \( d \)：拼接 “显著 + 互信息” 后重建原始样本。
  - **切换机制（Switching Mechanism）**：对样本对 \( (x_1, x_2) \) 进行四种重建：
    - 恢复对（recovered）：\( \tilde{x}_1 = d(s_1 \oplus m_1) \)，\( \tilde{x}_2 = d(s_2 \oplus m_2) \)。
    - 交换对（switched）：\( \hat{x}_1 = d(s_1 \oplus m_2) \)，\( \hat{x}_2 = d(s_2 \oplus m_1) \)。
  - **损失函数**：自监督版本仅使用均方误差重建损失（式1），监督版本增加分类交叉熵/回归 RMSE（式2），总损失 \( L_{\text{total}} = L_{\text{recon}} + \alpha \cdot L_{\text{cls}} \) （\( \alpha \) 默认为1）。
- **算法流程**（文字描述）：
  1. 从无标签数据中采样两个 mini-batch \( \{x_1^i\} \) 和 \( \{x_2^i\} \)。
  2. 对每个样本进行特征破坏，得到 \( \breve{x}_1^i, \breve{x}_2^i \)。
  3. 经编码器得到 \( z_1^i, z_2^i \)。
  4. 通过 \( p_m, p_s \) 得到 \( m_1^i, s_1^i \) 和 \( m_2^i, s_2^i \)。
  5. 形成恢复样本 \( \tilde{x}_1^i, \tilde{x}_2^i \) 和交换样本 \( \hat{x}_1^i, \hat{x}_2^i \)。
  6. 计算重建损失并更新所有参数（RMSProp）。
- **下游使用**：
  - **微调**：冻结编码器并添加线性层，使用全量标签微调。
  - **即插即用嵌入**：将预训练的显著嵌入 \( s \) 与原始特征 \( x \) 拼接作为新特征输入传统模型（如 XGBoost）。

## 3. 实验设计

- **数据集**：
  - 基准数据集11个（Gorishniy et al. 2021）：California Housing（CA）、Adult（AD）、Helena（HE）、Jannis（JA）、Higgs（HI）、ALOI（AL）、Epsilon（EP）、Year（YE）、Covertype（CO）、Yahoo（YA）、Microsoft（MI）。
  - 额外公开数据集7个：Bank（BK）、Blastchar（BC）、Arrhythmia（AT）、Arcene（AR）、Shoppers（SH）、Volkert（VO）、MNIST（MN）。
- **场景与任务**：分类（binary/multi-class）和回归；评估端到端微调性能，以及显著嵌入作为即插即用特征的提升。
- **对比方法**：
  - 传统模型：Logistic Regression、Random Forest、XGBoost、LightGBM、CatBoost。
  - 深度模型：MLP、ResNet、SNN、AutoInt、DCN V2、NODE、FT-Transformer、TabNet、TabTransformer。
  - 自监督表格方法：VIME、SCARF（表2中提及）、SAINT、ReConTab。
  - 消融自身变体：无切换版本、不同特征破坏率。

## 4. 资源与算力

- **硬件与训练时长**：文中**未明确说明**GPU型号、数量或具体训练时间。仅给出了训练超参数（预训练1000 epoch，batch size 128，优化器RMSProp，学习率0.0003；微调200 epoch，Adam，学习率0.001），表明实验可在常规资源下完成，但算力信息缺失。

## 5. 实验数量与充分性

- **主要实验量**：
  - 在11个基准数据集上对比约12种基线方法（表1）。
  - 在7个额外数据集上进行分类任务对比，并包含即插即用嵌入实验（表2），共涵盖约8种方法（传统模型+深度模型+自监督方法）。
  - 消融实验：切换机制的贡献（7个数据集，表3）；特征破坏率（7个数据集，多个比率0.0~0.6，表4）。
- **充分性评估**：实验覆盖多个领域、不同规模和维度（从452样本到超过百万样本），方法对比全面，且包含自监督和监督两种预训练模式。消融设计合理，验证了切换机制和适当破坏率的必要性。
- **客观公平性**：所有模型使用统一数据预处理（Min-Max缩放、缺失值填补、反向差分编码），固定优化器与关键超参数，并汇报多次随机种子平均结果，对比较为公平。

## 6. 主要结论与发现

- SwitchTab在绝大多数分类任务上取得最优或接近最优的准确率/AUC，在回归任务上与XGBoost等传统方法竞争激烈。
- 即插即用的显著嵌入能进一步提升传统模型性能（AUC绝对提升0.5%~3.5%）。
- 视觉化显示互信息嵌入在类别间高度重叠（可交换），而显著嵌入明显分离，验证了解耦的有效性。
- 切换机制是核心贡献，移除后性能明显下降；最优特征破坏率约为0.3，高维数据集可适度增大。

## 7. 优点

- **新颖的解耦视角**：首次在表格数据中显式建模样本对间的互信息与显著信息。
- **方法优雅通用**：自监督与监督均可训练，下游微调和即插即用两种使用方式，灵活性强。
- **实证扎实**：在18个数据集、多种基线、传统与深度方法上全面评估，结论可靠。
- **可解释性**：通过潜在空间可视化展示解耦效果，增强了模型透明度。
- **独立于特定架构**：编码器仅用三层Transformer，设计简洁，易于复现和扩展。

## 8. 不足与局限

- **算力信息缺失**：未报告GPU资源与训练时长，不利于评估实际部署成本。
- **超参数敏感性**：特征破坏率、投影头设计、损失权重等可能影响性能，文中未提供系统的调参指导。
- **领域局限性**：主要实验集中于公开表格基准，缺少真实工业场景（如医疗、金融）的验证。
- **对某些数据集效果一般**：如在Arrhythmia等数据集上未超越所有基线（表2中SwitchTab AUC 0.928，MLP为0.912，SAINT为0.941），反映了表格数据领域“无单一统治方法”的普遍现象。
- **回归任务优势有限**：在回归任务上传统 boosting 模型偶尔仍占优，SwitchTab的优势主要在分类。
- **数据预处理依赖**：类别编码（反向差分编码）和缺失值填补策略可能影响结果推广性。

（完）
