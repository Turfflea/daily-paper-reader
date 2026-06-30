---
title: "CALF: Aligning LLMs for Time Series Forecasting via Cross-modal Fine-Tuning"
title_zh: "CALF: 通过跨模态微调对齐大语言模型用于时间序列预测"
authors: "Peiyuan Liu, Hang Guo, Tao Dai, Naiqi Li, Jigang Bao, Xudong Ren, Yong Jiang, Shu-Tao Xia"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/34082/36237"
tags: ["query:mt"]
score: 10.0
evidence: 跨模态微调大语言模型用于多变量时间序列预测
tldr: 针对现有LLM时间序列预测方法忽略文本与时序token分布差异的问题，提出CALF框架，通过跨模态对齐与微调，提升多变量时序预测性能；实验证明其在有限数据下显著优于单一模态方法，展示了跨模态对齐在LLM时间序列预测中的关键作用。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34082/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1729, \"height\": 441, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34082/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1799, \"height\": 991, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34082/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 835, \"height\": 543, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34082/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 841, \"height\": 544, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34082/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 845, \"height\": 318, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34082/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1766, \"height\": 438, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34082/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1769, \"height\": 489, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34082/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 858, \"height\": 373, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34082/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 856, \"height\": 368, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34082/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 861, \"height\": 226, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34082/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 857, \"height\": 216, \"label\": \"Table\"}]"
motivation: 现有LLM方法忽略了文本token与时序token之间的分布差异，导致次优预测。
method: 提出CALF框架，通过减少文本和时序模态间的分布差异，对LLM进行跨模态微调。
result: 在多个多变量时间序列数据集上取得优越预测性能，尤其在数据有限时。
conclusion: CALF展示了跨模态对齐在LLM时间序列预测中的关键作用，为先进预测提供了新范式。
---

## Abstract
Deep learning (e.g., Transformer) has been widely and successfully used in multivariate time series forecasting (MTSF). Unlike existing methods that focus on training models from a single modal of time series input, large language models (LLMs) based MTSF methods with cross-modal text and time series input have recently shown great superiority, especially with limited temporal data. However, current LLM-based MTSF methods usually focus on adapting and fine-tuning LLMs, while neglecting the distribution discrepancy between textual and temporal input tokens, thus leading to sub-optimal performance. To address this issue, we propose a novel Cross-Modal LLM Fine-Tuning (CALF) framework for MTSF by reducing the distribution discrepancy between textual and temporal data, which mainly consists of the temporal target branch with temporal input and the textual source branch with aligned textual input. To reduce the distribution discrepancy, we develop the cross-modal match module to first align cross-modal input distributions. Additionally, to minimize the modality distribution gap in both feature and output spaces, feature regularization loss is developed to align the intermediate features between the two branches for better weight updates, while output consistency loss is introduced to allow the output representations of both branches to correspond effectively. Thanks to the modality alignment, CALF establishes state-of-the-art performance for both long-term and short-term forecasting tasks with low computational complexity, and exhibits favorable few-shot and zero-shot abilities similar to that in LLMs.

---

## 论文详细总结（自动生成）

好的，以下是对论文《CALF: Aligning LLMs for Time Series Forecasting via Cross-modal Fine-Tuning》的结构化、深入的中文总结。

### 1. 论文的核心问题与整体含义

*   **研究背景与动机**：多变量时间序列预测任务已因深度学习（尤其是Transformer）取得长足进步，但传统方法受限于单一时序模态的训练数据，易出现过拟合。近期，利用大语言模型的强大上下文建模能力进行时序预测成为一个新兴且有前景的方向。
*   **核心问题识别**：现有基于LLM的时序预测方法（如GPT4TS、TimeLLM）主要聚焦于将时序数据适配并微调至LLM，但**忽视了一个核心矛盾**：文本模态的预训练token与时序模态的输入token之间存在显著的**分布差异**。这种差异导致跨模态信息无法有效对齐，限制了LLM强大泛化能力的发挥，从而造成性能次优。
*   **整体含义**：本文旨在解决这一根本性的模态分布差异问题，提出一个名为 **CALF** 的跨模态LLM微调框架，通过对齐文本和时序两种模态，全面提升LLM在时间序列预测上的性能、泛化能力和计算效率。

### 2. 论文提出的方法论

CALF框架的核心思想是构建一个**双分支结构**，并通过多层次的跨模态对齐技术来减少分布差异。框架由一个**文本源分支** 和一个**时序目标分支** 组成，两个分支共享相同的预训练LLM权重。

*   **核心组件与技术细节**：
    1.  **跨模态匹配模块 (Cross-Modal Match Module)**：
        *   **目标**：对齐两个模态的输入分布。
        *   **流程**：
            *   首先将时序输入`I`通过嵌入层和多头自注意力机制处理，得到**投影后的时序token `X_time`**。
            *   为了解决直接对LLM巨大的词表进行交叉注意力计算成本过高的问题，提出**主成分词嵌入提取策略**：利用PCA对预训练的词嵌入矩阵`D`进行离线降维，得到**主成分词嵌入 `D̂`**，用少量聚类中心代表大量词汇。
            *   以`X_time`为Query，以`D̂`为Key和Value，进行多头交叉注意力计算，生成与文本分布对齐的**文本token `X_text`**。
    2.  **特征正则化损失 (Feature Regularization Loss, \( L_{feature} \))**：
        *   **目标**：对齐两个分支在**特征空间**的中间层输出，从而更有效地指导时序分支的权重更新。
        *   **公式**：\( L_{feature} = \sum_{l=1}^{L} \gamma^{(L-l)} \text{sim}(\phi_{text}^l(F_{text}^l), \phi_{time}^l(F_{time}^l)) \)
        *   其中，\( F_{text}^l \) 和 \( F_{time}^l \) 分别是第`l`层Transformer块的输出特征，`sim(·,·)`是相似性度量函数（如L1损失），`φ`是可训练的投影层，用于将两个模态的特征映射到共享空间，`γ`是控制不同层损失权重的超参数。
    3.  **输出一致性损失 (Output Consistency Loss, \( L_{output} \))**：
        *   **目标**：对齐两个分支在**输出空间**的最终表示，确保语义上下文一致性。
        *   **公式**：\( L_{output} = \text{sim}(Y_{text}, Y_{time}) \)
        *   其中，\( Y_{text} \)和\( Y_{time} \)分别是两个分支的最终输出。
*   **训练与推理**：
    *   **总损失函数**：\( L_{total} = L_{sup} + \lambda_1 L_{feature} + \lambda_2 L_{output} \)，其中 \( L_{sup} \) 是监督损失。
    *   **参数高效微调**：在时序目标分支中引入LoRA并微调位置编码权重，以防止灾难性遗忘并提高训练效率。
    *   **推理**：仅使用时序目标分支的输出作为最终预测结果。

### 3. 实验设计

*   **数据集与场景**：
    *   **长期预测**：在7个主流数据集上进行，包括ETT (4个子集)、Weather、ECL、Traffic。
    *   **短期预测**：在M4数据集上进行。
    *   **少样本/零样本学习**：在4个ETT数据集上评估模型的泛化能力。
*   **Benchmark与对比方法**：对比了四类代表性基线模型：
    1.  **LLM-based**：TimeLLM, GPT4TS。
    2.  **Transformer-based**：PatchTST, iTransformer, Crossformer, ETSformer, FEDformer, Autoformer。
    3.  **CNN-based**：TCN, MICN, TimesNet。
    4.  **MLP-based**：DLinear, TiDE。
    *   短期预测还额外对比了N-HiTS和N-BEATS。
*   **评估指标**：
    *   长期预测：均方误差和平均绝对误差。
    *   短期预测：对称平均绝对百分比误差、平均绝对比例误差和总体加权平均。
*   **公平性说明**：对于LLM-based方法，论文作者使用了其官方代码并设置了相同的实验配置（输入长度、GPT2层数等）进行复现，以确保比较的公平性。

### 4. 资源与算力

*   **说明**：文中**未明确提及**所使用GPU的具体型号、数量以及总训练时长。不过，文中提到CALF的**计算复杂度低**，并在效率分析中给出了与其他LLM方法在多个数据集上的推理时间对比，结果显示CALF具有显著的速度优势。

### 5. 实验数量与充分性

*   **实验数量**：论文进行了大量、多维度的实验，可以认为是充分的。
    *   至少包含7个数据集的长期预测、1个数据集的短期预测、4个数据集的少样本和4个数据集的零样本学习实验。
    *   对照了超过10种基线方法。
    *   进行了多组消融实验，包括不同损失函数的作用、PCA主成分数量的影响，以及可解释性分析。
    *   进行了效率分析。
*   **充分性、客观性与公平性**：
    *   **充分性**：实验覆盖了长期、短期、少样本、零样本等多种任务场景，并辅以消融和效率分析，论证全面。
    *   **客观性与公平性**：直接使用官方代码复现主要LLM基线模型，并统一实验配置，最大程度保证了对比的公平性。评估指标均为领域内标准指标。

### 6. 论文的主要结论与发现

*   **性能卓越**：CALF在长期和短期预测任务上均达到了最先进的性能，在多个数据集上显著优于现有LLM-based和Transformer-based模型。
*   **泛化能力强**：得益于跨模态对齐，CALF展现出与LLM类似的、优越的少样本和零样本泛化能力，证明模态对齐对知识迁移至关重要。
*   **计算高效**：通过主成分词嵌入策略和参数高效微调，CALF在取得领先性能的同时，保持了低计算复杂度，推理速度远超其他LLM方法。
*   **对齐有效性**：可视化分析表明，CALF的跨模态匹配模块能有效将时序序列与描述时序特征的文本词嵌入对齐，验证了该模块设计的可解释性与有效性。

### 7. 优点

*   **问题洞察深刻**：准确指出了现有LLM时序预测方法的根本缺陷——模态分布差异，为后续研究提供了明确方向。
*   **方法论创新且系统**：提出的双分支多级对齐框架具有高度的系统性和完整性，从输入、中间特征到输出三个层面进行了全面的分布对齐，思想新颖。
*   **技术设计巧妙**：离线PCA提取主成分词嵌入的策略，巧妙解决了直接交叉注意力计算成本过高的难题，平衡了效果与效率。
*   **实验设置扎实**：不仅对比全面，而且特别注重与LLM基线的公平复现对比，实验结论极具说服力。

### 8. 不足与局限

*   **算力成本细节缺失**：论文未报告模型训练的具体硬件资源和时间开销，使读者难以精确评估其在资源有限环境下的使用门槛。
*   **骨干网络单一**：实验仅基于GPT-2（6层）作为骨干LLM，未探索将该框架应用于其他更先进或架构不同的LLM（如LLaMA、BERT等）时的效果，通用性有待进一步验证。
*   **PCA信息损失**：虽然消融实验表明性能对`d`不敏感，但PCA降维本质上会带来信息损失，可能限制了模型对齐词嵌入中更精细语义的能力。更大的`d`值导致的信息冗余和学习困难也暗示了该方法存在一个效果平衡点。
*   **缺乏对非平稳性的深入讨论**：论文未明确讨论其方法在面对强烈非平稳时间序列时的鲁棒性及表现，而这在现实应用中是一个常见挑战。

（完）
