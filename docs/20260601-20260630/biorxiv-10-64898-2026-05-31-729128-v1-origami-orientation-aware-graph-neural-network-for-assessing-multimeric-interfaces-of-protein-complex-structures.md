---
title: "ORIGAMI: Orientation-Aware Graph Neural Network for Assessing Multimeric Interfaces of Protein Complex Structures"
title_zh: ORIGAMI：方位感知图神经网络用于评估蛋白质复合物结构的多聚体界面
authors: "Wang, X., Bhattacharya, D."
date: 2026-06-03
pdf: "https://www.biorxiv.org/content/10.64898/2026.05.31.729128v1.full.pdf"
tags: ["query:mt"]
score: 9.0
evidence: 方向感知的GNN利用标量和向量特征评估蛋白质多聚体界面。
tldr: 准确评估计算预测的多聚体蛋白质复合物结构质量是计算结构生物学的关键挑战，现有基于图神经网络的评估方法仅利用标量信息，忽略了蛋白质界面上残基之间天然存在的三维方向几何特征。本文提出的ORIGAMI模型是一种方向感知图神经网络，通过融合标量和3D矢量节点表示，在维持SO(3)-等变性的同时执行对称感知几何操作，精细捕获界面上的残基取向关系，从而预测局部距离差异测试（iLDDT）分数。在CASP多轮挑战及其扩展接口级评估中，ORIGAMI显著优于非等变和等变图神经网络基线，CASP16接口评估中表现尤为突出，并展示出从无叠加iLDDT到叠加DockQ的高保真跨度量泛化能力。该工作不仅提供了开源高性能评估工具，也为蛋白质界面方向信息建模开辟了新路径。
source: biorxiv
selection_source: fresh_fetch
motivation: 现有图神经网络评估方法忽略蛋白质界面残基间的三维方向特征，制约了多聚体结构质量评估的可靠性。
method: 提出方向感知图神经网络ORIGAMI，结合标量与3D矢量节点，在SO(3)等变框架下捕获残基间方向关系，预测界面局部距离差异测试得分。
result: 在CASP多轮基准和CASP16接口评估中，ORIGAMI性能全面超越对比方法，并成功从iLDDT泛化到DockQ度量。
conclusion: 开源的ORIGAMI为蛋白质界面评估树立了新基准，证实了方向信息的关键作用，推动了相关领域发展。
---

## 摘要
基于深度学习的蛋白质结构预测方法已引发计算结构生物学的范式转变，但可靠地评估计算预测的多聚体结构质量仍具挑战。近期方法已证实运用图神经网络评估蛋白质复合物多聚体界面的优势，但忽视了三维蛋白质构象空间中自然存在的几何方位特征，且仅作用于标量权重。我们提出了 ORIGAMI，一种方位感知图神经网络，用于评估蛋白质复合物结构的多聚体界面，它利用标量与三维向量节点表示执行对称性感知的几何运算，同时通过捕捉跨蛋白质-蛋白质界面残基间的细粒度方位关系来保持 SO(3) 等变性，从而估计界面局部距离差异检验（iLDDT）分数。在多个轮次的关键结构预测评估（CASP）挑战靶标上测试，ORIGAMI 在多个界面质量评估基准上取得了优越性能，尤其在扩展的 CASP16 界面级评估以及与非等变和等变图神经网络基线的受控对比中增益显著。它还通过高保真地重现基于叠加的 DockQ 分数，展示了强大的跨度量泛化能力，尽管其仅被训练用于估计无叠加的 iLDDT 分数。ORIGAMI 可于 https://github.com/Bhattacharya-Lab/ORIGAMI 免费获取。

## Abstract
Deep learning-based protein structure prediction methods have led to a paradigm-shift in computational structural biology, yet reliably assessing the quality of computationally predicted multimeric structures remains challenging. Recent methods have demonstrated benefits of employing graph neural networks for assessing multimeric interfaces of protein complexes, but ignore geometric orientational features naturally occurring in 3-dimensional protein conformational space and act only on scalar weights. We present ORIGAMI, an orientation-aware graph neural network for assessing multimeric interfaces of protein complex structures that leverages both scalar and 3D vector node representations to perform symmetry-aware geometric operations while maintaining SO(3)-equivariance by capturing fine-grained orientational relationships between residues across protein-protein interfaces to estimate the interface Local Distance Difference Test (iLDDT) score. Tested on targets from multiple rounds of Critical Assessment of Structure Prediction (CASP) challenges, ORIGAMI achieves superior performance across multiple interface quality assessment benchmarks, with particularly strong gains in the expanded CASP16 interface-level evaluation and in controlled comparisons against both non-equivariant and equivariant graph neural network baselines. It also demonstrates robust cross-metric generalization by reproducing superposition-based DockQ scores with high fidelity, despite being trained only to estimate the superposition-free iLDDT score. ORIGAMI is freely available at https://github.com/Bhattacharya-Lab/ORIGAMI.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：尽管基于深度学习的蛋白质结构预测（如AlphaFold3）取得了革命性进展，但可靠地评估计算预测的多聚体蛋白质复合物结构质量仍是一大挑战。现有基于图神经网络（GNN）的界面质量评估方法仅使用标量特征，完全忽视了残基间在三维空间中天然存在的**方位几何特征**（如侧链取向、主链框架对齐等），限制了对界面结合特异性和稳定性的精确建模。
- **整体含义**：该研究旨在解决“如何利用三维方向信息提升多聚体界面质量评估的准确性”这一关键缺口，提出了一个方位感知的等变图神经网络 ORIGAMI，通过捕捉跨界面残基的细粒度取向关系，实现了更准确、更稳健的无叠加界面质量预测，并为蛋白质界面评估树立了新基准。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：将网络参数从标量拓展为**三维向量（方向权重）**，使网络能学习特定几何构型（如对齐、正交、扭曲）上的方向滤波器，并在**局部坐标框架**下执行消息传递以维持严格的 SE(3) 等变性。
- **关键技术细节**：
    - **图构建**：抽取界面残基（任何原子与其他链原子距离 <24 Å），为每个残基连接空间上最近的 k=40 个邻居构成 K-NN 图。节点和边均采用**标量-向量双模态特征**（节点标量33维、向量3×3维；边标量34维、向量1×3维）。
    - **定向权重感知器 (DWP)**：由三部分构成：
        - **定向线性模块**：包含四种几何操作——点积滤波（测量取向对齐）、叉积滤波（捕捉正交和旋转关系）、线性向量变换、标量到向量的提升（将标量映射至方向空间）。
        - **非线性模块**：标量用 ReLU，向量使用幅度感知门控函数 `v ← v · σ(||v||₂)` 保持方向但调节幅度。
        - **标量-向量交互模块**：进行双向耦合，融合化学信息与几何模式。
    - **等变消息传递**：所有运算在由残基 N、Cα、C 原子构建的局部正交标架中进行，确保全局刚体变换下的严格等变性。
    - **训练目标**：以无叠加的 iLDDT 为真实标签，采用 Huber 损失的**单结构回归**加**成对排序损失**的多目标优化。

### 3. 实验设计：使用了哪些数据集/场景，它的 benchmark 是什么，对比了哪些方法
- **数据集**：
    - **Voro-CASP 数据集**：由 VoroIFGNN 生成的模型与 CASP13/14 结构混合，含 1541 个靶标、20703 个模型，划分为训练/验证/测试集（无靶标重叠）。
    - **CASP15 数据集**：24 个二聚体靶标，共 5264 个模型，来自前 10 个预测器和 3 个基线。
    - **CASP16 数据集**：39 个多聚体靶标（含二聚体和高阶组装），60314 个模型-界面实例；另有 12 个二聚体靶标用于与 CASP16 单模型 EMA 组对比。还单独测试了 AlphaFold2/3 生成的模型。
    - **动态验证**：1BRS 复合物的 300 ns 分子动力学轨迹。
- **Benchmark 与指标**：以 iLDDT（无叠加）和 DockQ（需叠加）为评价标准，全局评估使用 Pearson (r)、Spearman (ρ)、MAE；另使用 Top-N 命中率（iLDDT≥0.236）和 ROC-AUC 评估模型选择能力。
- **对比方法**：
    - 非等变/等变基线：VoroIF-GNN、DProQA、DeepRank-GNN-ESM、SE(3)-Transformer、EGNN、Equiformer。
    - CASP15/16 顶级单模型 EMA 预测器：如 VifChartreuseJaune、AF unmasked、APOLLO、GuijunLab-PAthreader 等。

### 4. 资源与算力
- **明确给出**：训练使用 **8 块 NVIDIA A100 GPU（80 GB 显存）**，采用 Adam 优化器（学习率 10⁻⁵），dropout 0.15，共训练 **200 个 epoch**。文中强调尽管使用了向量加权等变层，架构仍保持较高计算效率。

### 5. 实验数量与充分性
- **实验数量**：约进行了 **7 大组实验**，包括：Voro-CASP 测试集上的消融研究（架构、消息传递方案、k 取值）；CASP16 多聚体全局与单靶标/单界面分析；AlphaFold2/3 模型泛化测试；与 CASP16 五大 EMA 预测器的对比及 ROC 分析；CASP15 数据集命中率对比；分子动力学轨迹界面动态评估。消融实验对比了 4 种整体架构、5 种消息传递方案以及 4 种 k 近邻策略。
- **充分性与公平性**：实验覆盖多个独立基准、新旧 CASP 轮次、不同模型来源（VoroIFGNN、AlphaFold），且所有内部对比均使用相同的数据划分和评估协议，对比方法包含当前最优和代表性基线，整体设计**充分且公平**。

### 6. 论文的主要结论与发现
- ORIGAMI 在 CASP16 界面级评估中性能全面领先，无论是全局相关性还是每靶标/界面分析均优于基线。
- 尽管仅使用无叠加 iLDDT 训练，ORIGAMI 仍能高保真地再现需要叠加的 DockQ 分数，展现出跨度量泛化能力。
- 在 CASP16 与顶级单模型 EMA 组对比中，ORIGAMI 的 Pearson 和 MAE 最优，ROC-AUC 最高（iLDDT: 0.930; DockQ: 0.925）。
- 在 CASP15 的 Top-5 模型选择命中率上达到最高（0.917），证明其优秀的排序能力。
- 对 1BRS 分子动力学轨迹的分析表明，ORIGAMI 能敏感地捕捉到界面亚态转变，与接触保持率变化一致，而全局折叠相似性指标无法区分。
- 消融实验证实，方向权重感知器和固定 k=40 的近邻图构造是性能的关键贡献因素。

### 7. 优点：方法或实验设计上有哪些亮点
- **方法创新性**：首次将**方向向量权重**和叉积等几何滤波显式引入多聚体界面质量评估，彻底超越了传统标量 GNN 和一般等变网络的能力。
- **严格的几何等变性**：在局部标架中进行所有运算，保证了 SE(3) 等变性，使预测不受全局平移和旋转影响，这一点在生物大分子结构中至关重要。
- **无叠加目标训练**：使用 iLDDT 作为训练标签，绕开了传统叠加指标对界面评估的固有偏差，且意外地实现了对叠加指标的强泛化，具有实际应用价值。
- **评估全面且严谨**：不仅包含大规模静态预测评估，还纳入分子动力学动态界面分析，验证了模型对构象变化的敏感性；多维度对比了非等变与等变基线，强化了结论的可靠性。
- **开源可复现**：提供了完整代码和数据集，便于社区使用和后续研究。

### 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等
- **数据集来源偏差**：训练集和消融测试集基于 VoroIFGNN 生成的模型和较早的 CASP 轮次，这些模型的分布可能与最新 AlphaFold3 预测有差异，尽管 AlphaFold 泛化测试结果良好，但仍可能影响模型在某些类型复合物上的极限性能。
- **对比评估的细微差距**：在 CASP16 二聚体评估中，ORIGAMI 的 Spearman 相关系数（0.760/0.693）略低于 AF unmasked（0.805/0.802），说明在排序一致性上并非在所有情况下都绝对第一。
- **图构建的静态参数**：所使用的 24 Å 界面截止距离和 k=40 的近邻参数虽然经过消融优化，但对于尺寸极大或接触模式极其不规则的复合体，可能不是最优选择。
- **界面动态评估的局限性**：动力学验证仅基于单个经典复合体（1BRS），其结论的普适性尚需更多元、更大规模的动态体系验证。
- **未涵盖复杂化学环境**：当前模型未显式纳入水分子、金属离子、配体等异源介质，这些因素在真实界面结合中可能至关重要；也未处理长程变构效应。

（完）
