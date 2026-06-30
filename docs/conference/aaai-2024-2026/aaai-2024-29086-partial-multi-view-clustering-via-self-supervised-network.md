---
title: Partial Multi-View Clustering via Self-Supervised Network
title_zh: 基于自监督网络的部分多视图聚类
authors: "Wei Feng, Guoshuai Sheng, Qianqian Wang, Quanxue Gao, Zhiqiang Tao, Bo Dong"
date: 2024-03-25
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/29086/30054"
tags: ["query:mt"]
score: 9.0
evidence: 采用自监督对比学习实现无标签的部分多视图聚类
tldr: 针对部分多视图数据缺失问题，提出一种基于自监督网络的聚类方法（PVC-SSN）。通过对比学习引导模型学习判别性和一致性子空间表示，充分利用了不同视图间的相关性，并在无监督条件下挖掘数据内在的判别特征，从而在视图不完整的情况下仍能获得高质量的聚类结果，为现实应用中的多视图数据分析提供了有效工具。
source: AAAI-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29086/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 831, \"height\": 358, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29086/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1543, \"height\": 767, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29086/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 876, \"height\": 839, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29086/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 864, \"height\": 805, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-29086/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 875, \"height\": 337, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29086/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1833, \"height\": 564, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29086/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1832, \"height\": 652, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29086/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1819, \"height\": 606, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29086/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 897, \"height\": 150, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-29086/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 830, \"height\": 172, \"label\": \"Table\"}]"
motivation: 现有部分多视图聚类方法未能充分挖掘视图间相关性且忽略数据自身判别特征。
method: 利用对比学习获取判别一致性子空间表示，并设计自监督模块进行引导。
result: 在不完整多视图数据集上取得了优于现有方法的聚类性能，证明了对视图缺失的鲁棒性。
conclusion: 自监督对比学习能有效提升部分多视图聚类的判别性和鲁棒性，具有实际应用价值。
---

## Abstract
Partial multi-view clustering is a challenging and practical research problem for data analysis in real-world applications, due to the potential data missing issue in different views. However, most existing methods have not fully explored the correlation information among various incomplete views. In addition, these existing clustering methods always ignore discovering discriminative features inside the data itself in this unsupervised task. To tackle these challenges, we propose Partial Multi-View Clustering via Self-Supervised \textbf{N}etwork (PVC-SSN) in this paper. 
Specifically, we employ contrastive learning to obtain a more discriminative and consistent subspace representation, which is guided by a self-supervised module. Self-supervised learning can exploit effective cluster information through the data itself to guide the learning process of clustering tasks. Thus, it can pull together embedding features from the same cluster and push apart these from different clusters. Extensive experiments on several benchmark datasets show that the proposed PVC-SCN method outperforms several state-of-the-art clustering methods.

---

## 论文详细总结（自动生成）

好的，以下是基于给定论文内容生成的结构化、深入、客观的中文总结。

### **1. 论文的核心问题与整体含义**

*   **研究背景与问题**：在现实应用中，由于传感器故障、传输丢失等原因，多视图数据经常出现某几个视图样本缺失的问题，这被称为“部分多视图数据”。现有的部分多视图聚类方法存在两个主要局限：
    1.  **视图间相关性挖掘不足**：未能充分学习差异较大的各视图之间的隐藏关联，无法获得精确的公共聚类结构。
    2.  **忽略数据的判别性特征**：作为一种无监督任务，现有方法忽略了数据本身蕴含的类别判别信息，导致聚类性能受限。
*   **整体含义与目标**：为了解决上述问题，本文提出一种新颖的**基于自监督网络的部分多视图聚类方法**。其核心目标是利用**对比学习**和**自监督信号**，从不完整的视图中学习一个更具判别性和一致性的子空间表示，从而显著提升聚类准确度。

### **2. 论文提出的方法论**

该方法的核心思想是**利用聚类任务自身产生的伪标签作为自监督信号，引导对比学习过程，从而拉近同簇内不同视图的样本特征，推远不同簇的样本特征**。

*   **网络架构**：模型主要由三个关键组件构成：
    *   **多视图对比编码器网络**：为每个视图设计一个编码器，将高维数据映射到低维子空间。然后通过一个非线性对比头将配对样本的特征进一步映射，并在此空间执行有监督的对比学习。
    *   **自表达学习层**：通过加权融合机制，将多个视图配对数据的潜在特征融合成一个公共表示，再与未配对数据拼接。基于子空间聚类的“自表达属性”（即 $Z = ZS$），学习一个反映样本间相关性的系数矩阵 $S$，并通过谱聚类从 $S$ 构建的亲和矩阵中获得聚类伪标签 $P$。
    *   **多视图解码器网络**：将自表达层重构后的表示解码回原始输入，以确保潜在子空间表示能够有效反映原始数据的结构特征。
*   **关键技术细节与公式**：
    *   **多视图对比损失 ($L_{mcl}$)**：此为方法核心。它利用上一轮迭代的聚类结果伪标签来构造正负样本对。对于来自某一视图的样本 $q_{ia}$，与它属于同一簇的其他视图样本构成正样本对 $B(q_{ia})$，其余样本构成负样本对。损失函数旨在最大化正样本对之间的相似度，最小化负样本对之间的相似度。公式如下：
        $L_{mcl} = \sum_{i=1}^v \sum_{a=1}^p \frac{-1}{|B(q_{ia})|} \sum_{B(q_{ia})} \log \frac{\exp (q_{ia} \cdot q_{jb} / \tau)}{\sum_{C(q_{ia})} \exp (q_{ia} \cdot q_{kc} / \tau)}$
    *   **自表达损失 ($L_{se}$)**：用于学习自表达系数矩阵 $S$，并约束其对角线元素为0，避免平凡解。
        $L_{se} = \frac{1}{2} ||Z - ZS||_F^2 + ||S||_F^2 \quad \text{s.t.} \quad \text{diag}(S) = 0$
    *   **重构损失 ($L_{re}$)**：计算输入数据与解码器重构数据之间的均方误差。
        $L_{re} = \sum_{i=1}^v ||X_i - \tilde{X}_i||_F^2$
    *   **总目标函数 ($L$)**：模型先在预训练阶段优化 $L_{pre} = L_{re} + L_{se}$，得到初始伪标签。然后在正式训练阶段优化总目标，联合学习。
        $L = L_{re} + \lambda_1 L_{se} + \lambda_2 L_{mcl}$
*   **算法流程**：
    1.  **预训练阶段**：使用$L_{re}$和$L_{se}$对网络进行预训练，获得初始化的聚类伪标签。
    2.  **正式训练阶段**：使用完整损失函数 $L$ 对整个网络进行端到端优化，交替更新网络参数和自表达系数矩阵 $S$。
    3.  **聚类结果输出**：循环训练至收敛后，从最终的系数矩阵 $S$ 构建亲和矩阵 $C = \frac{1}{2}(|S| + |S|^T)$，并在其上执行谱聚类，得到最终聚类结果。

### **3. 实验设计**

*   **数据集**：在3个公开基准数据集上进行了评估。
    *   **BDGP**：包含2500个样本，5个类别，有图像和文本2个视图。
    *   **MNIST**：使用了4000个手写数字样本，提取了原始图像、边缘、LBP和编码器特征作为4个视图。
    *   **HW**：包含2000个手写数字样本，10个类别，选取了FAC、FOU、KAR这3个视图。
*   **对比方法**：与8种先进方法进行了比较，包括：
    *   **单视图方法**：谱聚类（SC）。
    *   **完整多视图方法**：AMGL， RMSC， ConSC。
    *   **不完整/部分多视图方法**：IMG, GPVC, PVC-GAN, iCmSC。
*   **实验场景与评估指标**：
    *   为模拟数据缺失，对每个数据集设置了5组缺失率：0.1, 0.3, 0.5, 0.7, 0.9。
    *   使用准确率（ACC）和归一化互信息（NMI）作为评价指标，并在相同缺失率下随机缺失10次取平均值。

### **4. 资源与算力**

*   **硬件平台**：实验运行在 Ubuntu Linux 16.04 系统上，配备 **NVIDIA Titan Xp GPU** 和 **64 GB** 内存。
*   **软件与优化**：使用PyTorch工具箱（部分对比方法使用MATLAB），采用Adam优化器，学习率设置为0.0001。
*   **训练时长**：论文报告了不同深度方法在BDGP数据集上运行10000个epochs的具体时间。PVC-SSN训练时间（46232秒）低于iCmSC（66010秒）和PVC-GAN（74582秒），表明其计算复杂度合理。

### **5. 实验数量与充分性**

*   **实验组数**：实验非常充分，涵盖了3个多视图数据集、5个缺失率、9种对比方法，合计约**135组主实验**（每种方法在不同缺失率下均需测试）。
*   **消融研究**：设计了消融实验，通过移除多视图对比模块（变体名为PVC）与完整模型对比，证实了自监督对比损失在BDGP数据集上对性能的关键提升作用。
*   **可视化分析**：提供了BDGP数据集上不同方法得到的子空间特征的t-SNE可视化图，直观证明了PVC-SSN学习到的表示更清晰、更具判别性。
*   **超参数分析**：在HW和MNIST数据集上，对两个关键超参数 ($\lambda_1, \lambda_2$) 进行了大范围 $[0.01, 0.1, 1, 10, 100]$ 的敏感性分析，表明模型对参数变化具有鲁棒性。
*   **收敛性分析**：展示了在三个数据集上损失值随训练轮次下降并趋于稳定的曲线，验证了优化算法的收敛性。
*   **时间复杂性对比**：对比了不同深度模型在BDGP数据集上的运行时间，证明了效率的可比性。
*   **公平性**：实验设置公平，通过统一缺失率、多次运行取平均值、对比多种类型的方法，确保了评估的客观性。

### **6. 论文的主要结论与发现**

*   PVC-SSN在所有数据集和几乎所有缺失率下的聚类性能（ACC, NMI）均优于现有最先进方法，特别是在高缺失率场景下优势更明显。
*   自监督对比学习模块是性能提升的关键，它能有效挖掘利用数据中的类别信息，学习到更具判别性的特征表示。
*   该方法不仅能处理双视图数据，还能轻松扩展到更多视图，表现出良好的通用性。

### **7. 优点**

*   **创新性强**：将自监督对比学习巧妙地引入部分多视图聚类任务，用聚类伪标签引导正负样本对的选择，解决了无监督聚类中判别信息匮乏的痛点。
*   **方法设计合理**：网络结构（编码器-自表达层-解码器）完整，各模块各司其职。融合机制和对比损失的结合使得学到的子空间既一致又具判别性。
*   **实验详尽且扎实**：大量的定量、定性、消融、敏感性和收敛性实验，从多个维度严谨地验证了方法的有效性、鲁棒性和效率。
*   **性能优异**：在多个基准数据集上取得了一致的、显著的最高性能，证明了方法的先进性。

### **8. 不足与局限**

*   **网络结构固定的潜在限制**：文中编码器和解码器网络的结构细节未被详细讨论，可能针对特定数据集设计了各自的结构，其普适性需要更多验证。
*   **初始伪标签质量依赖**：模型性能严重依赖预训练阶段获得的初始伪标签。如果预训练得到的聚类结果很差，可能会对后续的对比学习产生误导。
*   **缺失模式假设单一**：实验仅在随机缺失模式下进行验证，未讨论在非随机缺失（如整个视图完全缺失、块缺失）等更复杂情况下的性能。
*   **应用场景限制**：作为学术研究，在更大型、更真实的工业级数据集上的验证和可扩展性讨论相对缺乏。

（完）
