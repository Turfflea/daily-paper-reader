---
title: "APOSM: Pairwise preference learning improves generative small-molecule design"
title_zh: APOSM：成对偏好学习改进生成式小分子设计
authors: "Dreisler, M. W., Michael, R., Hatzakis, N. S., Boomsma, W."
date: 2026-06-10
pdf: "https://www.biorxiv.org/content/10.64898/2026.06.06.730554v1.full.pdf"
tags: ["query:mt"]
score: 9.0
evidence: 消息传递图神经网络用于分子属性预测；成对偏好学习
tldr: 小分子先导优化依赖替代模型筛选候选化合物，但测量噪声和稀疏性限制模型可靠性。APOSM利用成对比较训练替代模型，结合片段生成器、成对图神经网络和概率排序，在主动学习循环中改进候选选择。在分子优化和GPCR配体发现任务中，APOSM显著提升目标达成率和采样效率，特别在绝对分数难以校准的场景下优势突出。该工作展示了成对偏好学习为生成式分子设计提供更稳健的优化信号。
source: biorxiv
selection_source: fresh_fetch
motivation: 筛选数据噪声大且稀疏，绝对分数校准困难，传统点估计替代模型可靠性不足，亟需更稳定的排序机制。
method: 提出APOSM算法，集成基于片段的分子生成器、成对消息传递图神经网络及概率排序，通过批量主动学习进行候选优先排序。
result: 在PMO基准和GPCR配体重发现任务上，APOSM的目标达成率和采样效率均优于无指导优化、Graph-GA和点回归消融，在绝对分数难校准任务上增益最显著。
conclusion: 成对偏好学习能有效提升替代模型在噪声稀疏数据上的可靠性，APOSM为小分子设计提供了一种高效的主动学习策略。
---

## 摘要
小分子先导物优化受限于候选化合物的合成与测试成本，使得在实验测试前优先筛选化合物的替代模型成为设计过程的核心。此类替代模型的可靠性受限于筛选测量的噪声和稀疏性。我们发现，在此情境下，基于候选分子间的成对比较训练替代模型，而非基于绝对预测分数，能产生显著更可靠的活性候选选择信号。我们开发了APOSM，一种主动学习算法，它将基于片段的生成器、成对消息传递图神经网络替代模型以及概率排序集成在批次采集循环中。在实用分子优化基准测试和GPCR配体再发现任务中，相比无引导的基于片段的优化、Graph-GA遗传算法以及点式回归消融模型，APOSM提高了目标达成度和采样效率，且在绝对分数最难校准的任务上增益最大。

## Abstract
Small-molecule lead refinement is constrained by the cost of synthesizing and assaying candidates, making the surrogate models that prioritize compounds for experimental testing central to the design process. The reliability of such surrogates is limited by the noise and sparsity of screening measurements. We show that training the surrogate on pairwise comparisons between candidate molecules, rather than on absolute predicted scores, yields a substantially more reliable signal for active candidate selection in this regime. We develop APOSM, an active-learning algorithm that combines a fragment-based generator, a pairwise message-passing graph neural network surrogate, and probabilistic ranking inside a batched acquisition loop. On the Practical Molecular Optimization benchmark and a GPCR ligand rediscovery task, APOSM improves target attainment and sampling efficiency over unguided fragment-based optimization, the Graph-GA genetic algorithm, and a pointwise-regression ablation, with the largest gains on tasks where absolute scores are hardest to calibrate.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：小分子先导化合物优化过程中，候选分子的合成与生物测试成本高昂，因此需要依赖替代模型（surrogate models）在实验前对分子进行优先级排序。但替代模型的可靠性受到筛选数据**噪声大、稀疏性高**的严重制约。
- **动机**：传统替代模型多基于点回归，输出分子性质的绝对预测分数。在这种低数据质量环境下，绝对分数的校准困难，导致模型对活性分子的辨别能力不足。
- **整体含义**：本研究提出，用**成对比较（pairwise comparisons）**替代绝对分数来训练替代模型，能给出更稳健、可靠的候选优选信号，从而在稀疏噪声数据中提升分子设计的效率。

### 2. 论文提出的方法论
- **核心思想**：将分子优化建模为基于**成对偏好学习（pairwise preference learning）**的主动学习循环，通过比较候选分子对来引导生成模型，而非直接预测每个分子的绝对活性值。
- **方法名称**：APOSM（Active Pairwise Optimization of Small Molecules）。
- **关键技术细节**：
  - **分子生成器**：采用**基于片段（fragment-based）**的生成策略，能够按结构模块生成候选分子。
  - **替代模型**：**成对消息传递图神经网络（pairwise message-passing graph neural network）**，对输入的两个分子图进行联合编码和比较，输出两者相对优劣的概率。
  - **排序与采集**：在批量主动学习循环中，利用该成对模型的输出构建**概率排序（probabilistic ranking）**，作为采集函数（acquisition function）来挑选下一轮最具潜力的候选分子。
- **对比的消融模型**：文中提到与**点回归消融模型（pointwise-regression ablation）**比较，即用同样架构但预测绝对分数的模型，以证明成对偏好学习的增益。

### 3. 实验设计
- **数据集与场景**：
  - **实用分子优化基准（Practical Molecular Optimization benchmark, PMO）**：通用小分子多目标优化任务。
  - **GPCR配体再发现任务（GPCR ligand rediscovery task）**：针对G蛋白偶联受体（GPCR）的配体发现场景，属于实际药物靶点相关的任务。
- **对比方法**：
  - **无引导的基于片段优化（unguided fragment-based optimization）**：不使用任何替代模型的基线生成器。
  - **Graph-GA遗传算法**：基于图结构的遗传算法，常用于分子优化。
  - **点回归消融模型**：将APOSM中的成对学习替换为绝对值预测的版本，用于评估成对比较的贡献。
- **评估指标**：目标达成度（target attainment）和采样效率（sampling efficiency）。

### 4. 资源与算力
- 元数据及摘要中**未明确说明**所使用的GPU型号、数量、训练时长等算力细节。原始论文正文部分缺失，无法提供相应信息。

### 5. 实验数量与充分性
- 基于摘要，论文至少包含了**1个基准测试（PMO）及1个生物靶点任务（GPCR）**，并在各任务上对比了**4种方法**（无引导、Graph-GA、点回归、APOSM本身）。这构成了至少2个主实验×4组对比的设置。
- 文中明确提到**消融实验**（点式回归对比），证明实验设计考虑了内部有效性验证。
- **充分性初步判断**：从摘要来看，实验覆盖了通用优化和靶向发现两类典型场景，对比方法包含无模型基线、经典优化方法和消融版本，设计较为公平。但因缺失完整实验细节（如重复次数、统计检验、不同噪声水平的鲁棒性实验等），无法全面评估其充分性。

### 6. 论文的主要结论与发现
- 成对偏好学习在噪声稀疏数据下，相比基于绝对分数的点回归，能够显著提高替代模型选择活性候选的可靠性。
- APOSM算法在分子优化基准和GPCR配体重发现任务中，相较于对比方法，**目标达成度和采样效率均有提升**。
- 尤其在**绝对分数难以校准的任务**上，APOSM的增益最为明显，表明成对比较策略对于那些预测值本身就不准、但相对排序更易学习的场景尤为有效。
- 整体结论：成对偏好学习为生成式小分子设计提供了一个更稳健的优化信号，可有效应对真实世界中筛选数据的低质量特性。

### 7. 优点（方法或实验设计上的亮点）
- **问题切入点精准**：聚焦于现实筛选数据噪声大、稀疏导致绝对预测不可靠这一痛点，提出用相对排序替代绝对值，思想上具有创新性。
- **方法集成度高**：将片段生成、图神经网络、概率排序及批量主动学习整合为统一框架，形成端到端的优化流程。
- **验证设计严谨**：设置了点式回归消融模型，直接对比成对学习与绝对预测的差异，因果链条清晰。
- **任务覆盖面合理**：同时包含通用基准和具体药物靶点（GPCR），体现方法在标准测试与真实应用中的潜力。

### 8. 不足与局限（包括实验覆盖、偏差风险、应用限制等）
- **信息不完整**：本文仅提供摘要与元数据，完整方法细节、超参数设置、计算成本等均无从知晓，限制了对方法可复现性和实用性的深入评估。
- **实验规模未知**：虽然任务类型较丰富，但无法确定每个任务下分子数量、主动学习轮次、随机种子重复数等，难以判断结论的统计稳健性。
- **对比方法局限性**：仅与Graph-GA和点回归消融比较，未提及更多前沿生成模型（如reinforcement learning guided generation, 贝叶斯优化等），缺少更广泛的baseline对比。
- **受体结构信息未知**：GPCR任务中是否利用受体结构信息未提及，若为纯配体基设计，可能无法反映结构指导下的性能。
- **应用限制**：成对比较模型通常比点回归模型计算复杂度更高（每对需一次推理），在大规模库筛选时可能带来效率问题，论文中未对此讨论。

（完）
