---
title: Conformal Crystal Graph Transformer with Robust Encoding of Periodic Invariance
title_zh: 具有周期不变性鲁棒编码的共形晶体图Transformer
authors: "Yingheng Wang, Shufeng Kong, John M. Gregoire, Carla P. Gomes"
date: 2024-03-25
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/27781/27598"
tags: ["query:mt"]
score: 8.0
evidence: 用于晶体性质预测的具备周期不变性鲁棒编码的图Transformer
tldr: 针对现有晶体图网络对周期不变性编码不够鲁棒且难以捕获三维角度信息的问题，提出共形晶体图Transformer，通过新颖的编码方式提升晶体性质预测准确性，助力材料发现。
source: AAAI-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2024-accepted/aaai-2024-27781/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1737, \"height\": 666, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-27781/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1675, \"height\": 480, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-27781/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 776, \"height\": 245, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-27781/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 858, \"height\": 325, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2024-accepted/aaai-2024-27781/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 771, \"height\": 266, \"label\": \"Table\"}]"
motivation: 现有图神经网络无法充分鲁棒地编码晶体的周期不变性和三维角度信息。
method: 设计共形晶体图Transformer，引入鲁棒编码机制以精确反映晶体的几何约束。
result: 在多个晶体性质预测任务上，所提方法优于以往图神经网络模型。
conclusion: 该框架为基于图神经网络的晶体材料设计提供了更强大的表征工具。
---

## Abstract
Machine learning techniques, especially in the realm of materials design, hold immense promise in predicting the properties of crystal materials and aiding in the discovery of novel crystals with desirable traits. However, crystals possess unique geometric constraints—namely, E(3) invariance for primitive cell and periodic invariance—which need to be accurately reflected in crystal representations. Though past research has explored various construction techniques to preserve periodic invariance in crystal representations, their robustness remains inadequate. Furthermore, effectively capturing angular information within 3D crystal structures continues to pose a significant challenge for graph-based approaches. This study introduces novel solutions to these challenges. We first present a graph construction method that robustly encodes periodic invariance and a strategy to capture angular information in neural networks without compromising efficiency. We further introduce CrystalFormer, a pioneering graph transformer architecture that emphasizes angle preservation and enhances long-range information. Through comprehensive evaluation, we verify our model's superior performance in 5 crystal prediction tasks, reaffirming the efficiency of our proposed methods.

---

## 论文详细总结（自动生成）

### 研究动机与核心问题
- **整体含义**：本文聚焦于利用图神经网络（GNN）进行晶体材料性质预测，旨在通过更鲁棒的图构建与表示学习方法，提升机器学习模型对材料属性预测的准确度，从而加速新材料发现。
- **核心问题**：
  - **周期不变性编码不鲁棒**：现有构建晶体图的方法（如基于半径的多边图、k-近邻图）在处理实际结构扰动时，可能产生不一致的图结构，破坏了晶体固有的**周期不变性**。
  - **角度信息捕获困难**：3D晶体结构中的**角度信息**对性质预测至关重要，但现有GNN要么通过构建计算成本高昂的线图来获取，要么使用了方法但未见性能提升，缺乏一个高效且能带来实际增益的角度建模方式。

### 方法论：CrystalFormer 的关键技术
CrystalFormer 的核心在于两个部分：一个鲁棒的图构建方法和一个能隐式保持角度信息的图Transformer架构。

#### 1. 鲁棒图构建方法（鲁棒编码周期不变性）
为解决近邻选择随机性导致的周期不变性破坏问题，论文提出了一种分层加权近邻选择策略，综合考虑三种物理化学性质，计算近邻原子的复合权重 \( w_{ij} \)：
- **倒数权重 \( w^r_{ij} \)**：惩罚与中心原子距离较远的近邻原子。
- **距离惩罚权重 \( w^d_{ij} \)**：引入原子共价半径，进一步对距离进行非线性惩罚。
- **电负性权重 \( w^e_{ij} \)**：偏好选择与中心原子电负性差异大的近邻原子。
这三种权重归一化后相乘，得到最终权重。随后，通过一种基于投影和AUROC的方法，确定性地选择权重最显著的部分原子作为邻居，从而消除随机性，构建出唯一且鲁棒的晶体图。此方法效率远高于Voronoi图法。

#### 2. 隐式角度信息保持策略
- **核心思想**：图结构中边的相对位置向量天然包含了角度信息。神经网络可以作为通用逼近器学习这种映射，但难以保证角度信息在网络传递过程中不丢失。
- **保持方法**：利用 **共形映射能够保角** 的特性。由于严格的共形映射不适用于神经网络，论文将其近似为**最小化狄利克雷能量的调和映射**。
- **目标函数**：在图上定义狄利克雷能量 \( E_{dir}(H) \)，该能量衡量了未直接相连的顶点对间，经过一个共享的线性映射 \( H \) 后的位置差异，其权重 \( \kappa_{ij} \) 基于以其相邻顶点为媒介的夹角（具体为夹角的余切值）来确定。通过最小化该能量，使得映射 \( H \) 能近似地保持局部角度信息。该公式可经推导转化为高效的矩阵迹运算形式。

#### 3. CrystalFormer 架构
- **输入嵌入**：原子特征通过组合周期表位置和多种物理化学性质得到。边特征则通过径向基函数处理原子间距离，或通过上述调和映射处理相对位置向量得到。
- **边感知的消息传递层**：这是模型的核心创新层。在Transformer的注意力机制中，计算Query和Key时，**不仅包含节点嵌入，还拼接了从源/目标节点到共同邻居的特定边嵌入**（即 `e_ik` 和 `e_kj`）。这使得模型能够显式利用包含角度信息的2跳邻居关系来计算注意力系数，从而增强长程信息交互。消息聚合后，采用跳跃知识网络进行特征更新。
- **交替堆叠**：网络由节点感知和边感知的消息传递层**交替堆叠**而成，以分别处理节点-边关系和边-边角度关系。
- **输出**：最终通过池化原子表示，得到晶体级别的性质预测。

### 实验设计
- **数据集**：在 **JARVIS** 材料基准（DFT-2021.8.18 3D版本，包含55,722个晶体）上进行评估，聚焦于五个关键性质预测任务：光学带隙(Bandgap OPT)、MBJ势带隙(Bandgap MBJ)、总能量(Total Energy)、形成能(Formation Energy) 和能量凸包(Ehull)。
- **对比基准（Benchmark）**：与一系列经典及先进的GNN模型进行对比，包括 **CGCNN、SchNet、MEGNET、GATGNN、ALIGNN、Matformer** 和 **PotNet**。
- **评估指标**：性能指标主要为**平均绝对误差（MAE）**；效率指标包括训练时间和推理吞吐量。

### 资源与算力
- 论文中**未明确说明**实验所使用的GPU型号、数量以及具体的总训练时长。仅在效率对比中提供了训练一个epoch的秒数(s/epoch)和推理吞吐量(samples/s)的相对值。

### 实验充分性与公正性
- **实验数量**：进行了多组实验，包括主性能对比、效率对比、图构建方法效率对比和消融实验，覆盖了性能、效率、模块有效性等多个方面，实验设计相对充分。
- **公正性与充分性**：
  - **公平比较**：明确说明了采用与现有工作相同的数据集划分，确保了比较的公平性。
  - **充分性**：主实验涵盖了5个不同性质的任务，评估了模型在不同预测目标上的泛化能力。消融研究验证了鲁棒图构建和角度保持模块这两个核心组件的各自贡献，论证较为扎实。
  - **局限**：仅在一个数据集（JARVIS）上进行测试，未能验证模型在其他常见晶体材料基准（如Materials Project）上的泛化性能。

### 主要结论与发现
- **性能优越**：CrystalFormer 在5个预测任务中的4个上取得了最优性能，尤其是在Bandgap (MBJ)任务上，MAE相对次优结果大幅降低了51.9%。
- **效率更佳**：其图构建方法比Voronoi图方法快约8倍，且能提取到更多近邻。模型训练和推理效率也优于同样使用角度信息的ALIGNN和Matformer。
- **模块有效性验证**：消融实验证明，**鲁棒图构建**和**角度保持的边感知Transformer块**均是模型取得卓越性能的关键。仅替换图构建方法即可带来显著提升。

### 优点
- **方法创新性强**：将几何中的共形映射思想近似为图上的狄利克雷能量最小化，用于神经网络的角度保持，思路新颖且有理论支撑。
- **解决实际痛点**：提出的图构建方法切实解决了k近邻图等方法的随机性和脆弱性问题，提高了模型在真实实验数据（可能存在轻微误差）下的鲁棒性。
- **架构设计巧妙**：边感知注意力机制的设计很优雅，无缝地将角度（2跳边关系）信息集成到Transformer框架中，同时避免了构建线图的高昂成本。
- **性能与效率兼顾**：在显著提升预测精度的同时，还实现了更快的预处理和模型训练/推理速度。

### 不足与局限
- **评估基准单一**：所有实验仅在JARVIS基准上进行，未在Materials Project等更大、更常用的材料数据库上进行验证，模型的泛化能力有待进一步考证。
- **算力透明度不足**：未提供详细的硬件配置和总训练时长，使得其他研究者难以精确评估和复现其资源需求。
- **物理权重参数依赖**：鲁棒图构建方法涉及多个物理参数（如原子半径、电负性）和一个需要调节的容忍参数 \( \lambda \)，其普适性可能依赖于这些参数设定的默认值或特定调整。
- **应用范围**：模型目前仅聚焦于晶体性质预测，其在更复杂的任务如晶体生成、动力学模拟上的应用潜力尚未被探讨。

（完）
