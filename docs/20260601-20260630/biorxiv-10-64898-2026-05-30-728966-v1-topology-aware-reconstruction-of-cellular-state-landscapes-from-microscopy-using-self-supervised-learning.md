---
title: Topology-aware reconstruction of cellular state landscapes from microscopy using self-supervised learning
title_zh: 利用自监督学习从显微镜图像中实现拓扑感知的细胞状态图谱重建
authors: "Messori, E., Taha, D. M., Fournier, L., Foix Romero, A., Uhlmann, V., Frossard, P., Vincent-Cuaz, C., Patani, R., Luisier, R."
date: 2026-06-03
pdf: "https://www.biorxiv.org/content/10.64898/2026.05.30.728966v1.full.pdf"
tags: ["query:mt"]
score: 9.0
evidence: 在显微图像上进行无标签的自监督学习。
tldr: 从静态荧光显微镜图像重建连续细胞状态景观，在密集生物培养中仍具挑战，通常依赖分割和标注。SI-SimCLR通过空间信息引导的自监督学习联合图偏最优传输，无需分割从荧光图像重建表型景观。在人iPSC星形胶质细胞中解析出与疾病和炎症相关的互连亚型，ALS细胞表型受限，形态转录组互补。该可扩展的标注自由策略为细胞状态景观重建及异质性分析提供了通用解决方案。
source: biorxiv
selection_source: fresh_fetch
motivation: 从显微镜图像重建连续细胞状态景观面临密集培养和分割标注瓶颈，缺乏无标注方法。
method: 提出SI-SimCLR，空间信息引导的自监督学习与图偏最优传输结合，从荧光图像无分割重建细胞景观。
result: 在iPSC星形胶质细胞中解析出疾病与炎症相关互连亚型，ALS细胞形态受限，形态与转录组互补。
conclusion: 该可扩展的无标注策略为细胞异质性分析和表型景观重建提供了通用方案。
---

## 摘要
细胞形态和空间组织提供了细胞状态的互补读出信息。然而，从成像数据重建连续的细胞状态图谱仍然具有挑战性，特别是在密集生物培养环境中。本文提出了SI-SimCLR，一个空间感知的自监督学习框架，能够直接从荧光显微镜图像中学习生物信息表征，无需图像分割或人工标注。结合基于图的偏最优传输框架，SI-SimCLR能够从静态成像数据重建细胞表型图谱，揭示表型亚状态的组织与连接方式。为建立并验证该框架，我们利用高内涵成像和匹配的群体转录组学数据，生成了人iPSC来源的星形胶质细胞多模态数据集。SI-SimCLR解析了与疾病和炎症状态相关的不同且相互连接的星形胶质细胞亚状态。肌萎缩侧索硬化症的星形胶质细胞占据了形态图谱的受限区域。引人注目的是，形态学和转录组学捕获了星形胶质细胞状态变异的各自不同且互补的方面。总之，我们的框架建立了一种可扩展且无需标注的策略，用于从显微镜数据重建细胞表型图谱，从而能够分析生物系统间的细胞异质性、图谱连通性和表型响应。

## Abstract
Morphology and spatial organisation provide complementary readouts of cellular state. However, reconstructing continuous cellular state landscapes from imaging data remains challenging, particularly in dense biological cultures. Here we present SI-SimCLR, a spatially informed self-supervised learning framework that learns biologically informative representations directly from fluorescence microscopy images without requiring segmentation or manual annotation. Combined with a graph-based partial optimal transport framework, SI-SimCLR enables reconstruction of cellular phenotypic landscapes from static imaging data, revealing how phenotypic substates are organised and connected. To establish and validate this framework, we generated a multimodal dataset of human iPSC-derived astrocytes using high-content imaging and matched bulk transcriptomics. SI-SimCLR resolved distinct interconnected astrocyte substates associated with disease and inflammatory states. ALS astrocytes occupied constrained regions of the morphological landscape. Strikingly, morphology and transcriptomics captured distinct and complementary aspects of astrocyte state variation.Together, our framework establishes a scalable and annotation-free strategy for reconstructing cellular phenotypic landscapes from microscopy data, enabling analysis of cellular heterogeneity, landscape connectivity and phenotypic responses across biological systems.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：在密集生物培养环境中，从显微镜图像重建连续的细胞状态景观（cellular state landscapes）极具挑战。传统方法严重依赖图像分割（segmentation）和人工标注（manual annotation），这成为高通量分析中的瓶颈。
- **研究动机**：细胞形态与空间组织能够互补地反映细胞状态，但现有技术难以直接从原始图像中无监督、无标注地重建出可解释的、拓扑感知的表型图谱。因此，亟需一种可扩展、无需标注的策略，从静态荧光显微镜图像中揭示细胞亚状态的组织、连接关系及其在疾病背景下的变化。
- **整体含义**：论文旨在打破分割与标注的依赖，提供一种通用框架，不仅捕获细胞异质性，还能展示表型景观（phenotypic landscape）的连通性，从而更全面地理解生物系统中细胞状态的连续变异。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **框架名称**：SI‑SimCLR
- **核心思想**：将空间信息引导的自监督学习（spatially informed self‑supervised learning）与图偏最优传输（graph‑based partial optimal transport）相结合，实现从原始荧光显微镜图像到连续细胞表型图谱的直接重建。
- **关键技术细节**：
  - **自监督表征学习（SI‑SimCLR 部分）**：
    - 直接从荧光显微镜图像中学习生物信息丰富的表征，**无需进行细胞分割**或人工标注。
    - 引入了空间信息引导机制（spatially informed），使学习到的表征能够编码细胞的空间组织关系，而不仅仅是单个细胞的形态特征。
  - **图偏最优传输（Graph‑based Partial Optimal Transport）**：
    - 在学习到的细胞表征基础上，构建设置最优传输问题的图结构。
    - 通过部分最优传输，从静态图像重建连续的细胞表型景观，**揭示不同表型亚状态是如何组织和相互连接的**，从而得到拓扑感知的表型图谱。

## 3. 实验设计：数据集、场景、对比方法

- **数据集**：
  - 自行构建的**多模态数据集**：使用高内涵成像（high‑content imaging）技术获取**人iPSC来源的星形胶质细胞**荧光显微镜图像，并匹配了**群体转录组学数据**（matched bulk transcriptomics）作为独立验证模态。
- **实验场景**：
  - 解析星形胶质细胞中与疾病（肌萎缩侧索硬化症，ALS）和炎症状态相关的表型亚状态。
  - 比较健康与 ALS 星形胶质细胞的形态景观占用区域。
  - 考察形态学与转录组学捕获细胞状态变异的互补程度。
- **对比方法 / Benchmark**：
  - 摘要与元数据中**未明确列出**具体比较的基线方法。从研究动机可推断，其隐含对比对象为需要先分割再提取特征的传统分析流程（如经典形态学分析、基于分割的深度学习等）。本文聚焦于展示无标注、无分割策略与多模态生物学发现的有效性。

## 4. 资源与算力

- 论文元数据和摘要中**未提及**所使用的 GPU 型号、数量、训练时长等算力相关信息，因此无法给出具体数据。

## 5. 实验数量与充分性

- **实验数量**：
  - 主要围绕一个核心的多模态数据集（iPSC星形胶质细胞）展开，但涵盖了多个分析维度：
    1. 表征学习与景观重建效果验证；
    2. 疾病关联亚状态的解析（ALS 与炎症）；
    3. ALS 细胞在形态景观中的受限区域分析；
    4. 形态学与转录组学的互补性对比。
- **充分性与客观性评估**：
  - **充分性**：从生物学问题的角度看，实验横跨健康与疾病状态、多模态验证（成像 + 转录组），并进行了表型连通性分析，探索维度较为丰富。
  - **客观公平性**：由于摘要未报告与其他无标注或传统方法的定量指标对比（如分类准确率、景观平滑性等），无法判断其是否进行了公平的客观基准测试。可能存在仅在新数据集上自证有效的风险，但需阅读全文才能确认。

## 6. 论文的主要结论与发现

- **解析了互连的亚状态**：SI‑SimCLR 成功分辨出人星形胶质细胞中与疾病和炎症密切相关的、相互连接的不同亚状态，表明表型景观具有明确的生物学意义。
- **疾病关联的景观约束**：肌萎缩侧索硬化症（ALS）的星形胶质细胞仅占据形态景观的一个**受限区域**，提示疾病状态下细胞状态空间的压缩或固定。
- **形态与转录组的互补性**：形态学特征和转录组学分别捕获了星形胶质细胞状态变异的**不同且互补的层面**，单独一种模态不足以全面刻画细胞状态。
- **框架的通用性**：该可扩展、无需标注的策略为从显微镜数据重建细胞表型景观提供了通用解决方案，可用于分析跨生物系统的细胞异质性、景观连通性和表型响应。

## 7. 优点：方法或实验设计上的亮点

- **完全无标注、无分割**：打破了传统流程对细胞分割和人工标注的强依赖，极大提升了分析的通量和普适性，尤其适用于密集培养的群体。
- **拓扑感知的景观重建**：不仅提取细胞状态，还能通过最优传输揭示亚状态之间的连续转换关系（景观连通性），超越了单纯的聚类或离散分类。
- **多模态交叉验证**：匹配的转录组数据为形态景观的生物学解释提供了独立且互补的证据，增强了发现的可靠性。
- **空间信息显式建模**：将空间组织纳入自监督学习，使表征更具生物学上下文，避免了孤立分析单个细胞造成的上下文丢失。

## 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等

- **数据集单一性**：主要结论仅基于人 iPSC 来源星形胶质细胞一种细胞类型，普适性有待在其他细胞类型、组织或临床样本中验证。
- **对比评估缺失**：未在公开 benchmarks 或与已有无标注 / 自监督方法进行定量比较，方法的相对优势难以精确评估。
- **静态图像局限**：仅使用静态荧光显微镜图像，无法捕获细胞状态的动态变化和时序信息；景观的时间连续性是否成立尚未检验。
- **算力与实施细节未公开**：缺少资源消耗和训练策略说明，影响复现性和对计算成本的实际评估。
- **偏最优传输的假设**：图偏最优传输依赖于适当的图构建和运输成本定义，若假设与生物实际不符，可能导致景观失真，但文中未讨论其鲁棒性。
- **转录组匹配为群体水平**：匹配的是群体转录组（bulk），而非单细胞转录组，因此形态‑转录互补性结论反映的是群体层面，难以解析单细胞水平的模态对应关系。

（完）
