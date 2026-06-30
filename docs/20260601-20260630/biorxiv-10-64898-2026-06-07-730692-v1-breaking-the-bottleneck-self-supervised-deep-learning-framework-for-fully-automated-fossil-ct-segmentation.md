---
title: "Breaking the bottleneck: self-supervised deep learning framework for fully automated fossil CT segmentation"
title_zh: 打破瓶颈：用于全自动化石CT分割的自监督深度学习框架
authors: "Roy, A., Ghosh, P., Weston, F., Hartley, B., Salili-James, A., Poon, S. T. S., Maidment, S. C. R., Butler, R. J."
date: 2026-06-11
pdf: "https://www.biorxiv.org/content/10.64898/2026.06.07.730692v1.full.pdf"
tags: ["query:mt"]
score: 9.0
evidence: 自监督SimCLR对比预训练与U-Net用于无人工标注的化石CT分割
tldr: "古生物学CT扫描中，脆弱的化石与周围岩石基质难以自动区分，传统手动分割耗时（每标本≥100小时）、主观且依赖专有软件，形成严重“分割瓶颈”。提出一种新颖的自监督深度学习方案，先以SimCLR v1对比学习从无标注数据提取特征，再自动生成伪标签并由U-Net精炼，实现端到端全自动分割。在涵盖两栖类、爬行类、恐龙和早期哺乳动物的50,626张图像上验证，在留出标本上取得93.66% Dice和82.42% IoU，与近年最佳结果持平。该方法将处理时间大幅缩减至一次性训练6小时+每标本1-3分钟，且几何泛化至外部标本达到亚体素精度，为古生物学高通量CT分析开辟新路径。"
source: biorxiv
selection_source: fresh_fetch
motivation: 古生物CT化石分割依赖人工标注，耗时数百小时，且受低对比度影响，迫切需要自动化方法。
method: 基于SimCLR v1对比学习预训练，自动生成伪标签，并通过U-Net细化，实现无监督化石CT分割。
result: "在50,626张多类群CT图像上训练，留出标本Dice 93.66%、IoU 82.42%，处理时间从约100人时降至6小时训练加1-3分钟推理，外部标本亚体素精度。"
conclusion: 自监督框架突破化石CT分割瓶颈，无需手动标注即可高效分割，有望推动大规模比较解剖学研究。
---

## 摘要
在应用于科学的深度学习中，标记训练样本稀缺且前景-背景对比度低的特定领域成像数据的语义分割仍然是一个开放挑战。古生物计算机断层扫描（CT）就是这个问题的一个例证：从周围的岩石基质中数字化分离出化石骨骼是劳动密集型的（每个数据集≥100小时）、主观的，且通常依赖昂贵的专有软件，这就形成了一个“分割瓶颈”，阻碍了对CT数据集合的大规模快速处理。在此，我们提出一个自监督的端到端框架，该框架结合了SimCLR v1对比预训练、确定性伪标签生成和U-Net精化，以完全自动化地实现化石CT分割，无需手动标注。使用来自中侏罗世Kilmaluag组的50,626张CT图像，涵盖两栖动物、爬行动物、恐龙和早期哺乳动物，该框架在一个训练期间未见的留存标本上实现了93.66%的Dice系数和82.42%的交并比（IoU），与近期基于深度学习的化石CT分割研究中报告的最高Dice和IoU值相当。跨类群的泛化能力在六个完全外部标本上通过几何验证，与手动阈值化的参考标准达到了亚体素级别的网格一致性。通过消除此前限制古生物学中深度学习方法应用的标注需求，该框架将每个标本的处理时间从约100人时减少到6小时（一次UNet训练）+1-3分钟（每个标本的网格生成），这是朝着批量处理和分析CT数据以进行大规模比较和定量分析迈出的关键第一步。

## Abstract
Semantic segmentation of domain-specific imaging data where labelled training examples are scarce and foreground-background contrast is low remains an open challenge in deep learning applied to science. Palaeontological computed tomography (CT) exemplifies this problem: digitally isolating fossilised bone from surrounding rock matrix is labour-intensive ([&ge;]100 hrs/dataset), subjective, and often reliant on expensive proprietary software, creating a "segmentation bottleneck" that prevents large-scale and rapid processing of CT data collections. Here we present a self-supervised, end-to-end framework combining SimCLR v1 contrastive pretraining with deterministic pseudo-label generation and U-Net refinement to fully automate fossil CT segmentation without manual annotation. Using 50,626 CT images from the Middle Jurassic Kilmaluag Formation spanning amphibians, reptiles, dinosaurs, and early mammals, the framework achieved a Dice coefficient of 93.66% and IoU of 82.42% on a held-out specimen not seen during training, comparable to the highest Dice and IoU values reported in recent Deep Learning-based fossil CT segmentation studies. Cross-taxon generalisation was validated geometrically on six fully external specimens, achieving sub-voxel mesh agreement with manually thresholded references. By eliminating the annotation requirement that has limited prior deep learning approaches in palaeontology, this framework reduces per-specimen processing from [~]100 person-hours to 6 hrs (one-time UNet training) +1-3 minutes (mesh generation per specimen), an essential first step towards batch processing and analysis of CT data for large-scale comparative and quantitative analyses.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **核心问题**：古生物CT扫描中，化石骨骼与围岩基质常因矿物交代导致X射线衰减系数几乎相同，形成极低的对比度；传统手动分割（阈值法、半自动工具）极度耗时（每个标本≥100人时）、主观、依赖昂贵专有软件，且训练标签复用性差，构成阻碍大规模定量研究的“分割瓶颈”。
- **整体含义**：论文旨在打破这一瓶颈，提出**无需任何手动标注的全自动化石CT分割框架**，使研究者从繁重的机械性描画中解放出来，专注于下游解剖、系统发育和功能分析，从而推动古生物学CT数据的批量化、高通量处理。

### 2. 方法论：核心思想、关键技术细节与算法流程
- **总体架构**：自监督、端到端的三阶段框架——SimCLR v1对比预训练 → 确定性规则伪标签生成 → 双流知识融合的U-Net精炼。
- **SimCLR v1对比预训练**：
  - 在无标签的39,037张CT切片上训练ResNet-50编码器。
  - 对每张切片施加领域特异的随机数据增强（随机裁剪/缩放、翻转、旋转±15°、高斯模糊、亮度/对比度抖动），生成正样本对。
  - 通过NT-Xent损失最大化正样本对在投影空间的一致性，迫使编码器学习对化石纹理、埋藏噪声、束硬化等结构特征的表示，而非仅依赖像素强度。
- **确定性规则伪标签生成**：
  - 使用固定参数的经典图像处理流水线（百分位归一化、高斯/中值模糊、Otsu阈值种子识别、区域生长、顶帽变换、腐蚀膨胀、连通分量滤波）生成粗粒度二值掩码。
  - 提供可复现的“空间先验”，明确骨骼大致位置，但不要求精确边界。
- **双流知识融合U-Net**：
  - 编码器部分用SimCLR预训练的ResNet-50权重初始化，将对比学到的纹理判别特征注入模型。
  - 同时输入粗掩码作为空间约束，U-Net通过复合损失函数（加权交叉熵+ Dice损失）精炼输出高精度分割掩码。
  - 训练时仅使用两个标本的粗掩码，第三个同属不同个体标本作为留存测试集，验证泛化性。
- **三维网格生成与几何验证**：
  - 将二维精炼掩码堆叠，通过区域生长、强度滤波、形态学清理后，使用Marching Cubes提取水密三角网格。
  - 与商业软件Avizo的手动阈值化网格进行Point Pair Registration → Iterative Closest Point (ICP) → Cloud-to-Cloud (C2C) 签署距离比较，评估几何一致性（尺度固定为1.0，部分标本ICP使用80%重叠以消除阈值网格中伪影的干扰）。

### 3. 实验设计：数据集、基准与对比方法
- **数据集**：来自苏格兰中侏罗世Kilmaluag组的多类群脊椎动物CT数据，总量50,626张2D切片，涵盖两栖类、爬行类、恐龙和早期哺乳动物，是目前已发表化石CT分割最大训练集的5倍以上。
- **数据集划分**：
  - SimCLR预训练：39,037张（19个数据集）；
  - SimCLR验证：3,765张（2个数据集）；
  - U-Net分区：7,824张（3个数据集），其中2个用于训练（配粗掩码），1个同属不同个体标本作为内部测试集。
  - 外部验证：6个完全独立、未参与任何训练的外部标本（2个蝾螈、2个_Marmorerpeton wakei_、1个早期哺乳形类、1个未描述柱牙兽），用于几何泛化检验。
- **评估基准**：
  - 内部测试：Dice系数和交并比（IoU）在留存标本的切片级别上评价。
  - 外部测试：以Avizo中的二值阈值分割网格为参考，通过C2C平均距离、标准差及Hausdorff距离进行网格级别几何比较，并以体素尺度解读偏差。
- **对比方法**：论文未直接与其它深度学习分割模型进行同数据集定量对比，但引用了近年文献报道的最高Dice/IoU值（如Yu et al. 2022, Edie et al. 2023, Knutsen & Konovalov 2024等）作为性能参照，并在讨论中指出其在相近甚至更优水平。

### 4. 资源与算力
- **硬件**：单张NVIDIA A100 GPU（峰值GPU内存占用约3.26 GB），运行于伯明翰大学BlueBEAR HPC集群。
- **训练时长**：
  - SimCLR预训练：约37–38小时（250个epoch，batch size 64）；
  - U-Net训练：约6小时10分钟（500个epoch）；
  - **总计一次性训练投入**：~43–44小时。
- **推理速度**：每个标本生成三维网格仅需1–3分钟。

### 5. 实验数量与充分性
- **实验数量**：
  - 1组大规模对比预训练实验 + 1组U-Net精炼实验。
  - 外部6个标本的几何验证，涵盖不同分类群和保存状态。
  - 另有SimCLR表示质量定性分析（UMAP特征可视化 + Grad‑CAM热图）作为自查。
- **充分性与客观性**：
  - **优势**：训练数据量庞大且分类多样，内部留存测试脱离训练个体，外部验证标本完全独立；网格比较采用尺度固定、部分标本修剪重叠的严格配准，区分了伪影与真实偏差。
  - **潜在局限**：未设计消融实验（如去掉SimCLR、只用规则掩码、只用监督方式）来量化各模块贡献；未与现有其它深度学习分割模型在同一测试集上直接对比；外部验证仅网格级，缺乏切片级Dice/IoU量化；无多中心或不同CT扫描条件的外部压力测试。因此实验数量中等，但设计上已尽力保证公平与泛化评估。

### 6. 主要结论与发现
- 提出的自监督框架在无任何手动标注的情况下，在留存内部标本上达到**93.66% Dice、82.42% IoU**，与近年有监督化石分割的最佳结果相当。
- 跨类群几何验证显示，对于保存较好的标本，AI网格与手动参考网格的C2C平均距离仅**0.63–0.75个体素**，达到亚体素一致性；甚至在某些标本上比二值阈值法边界更紧密（负C2C均数），并能正确排除阈值法无法识别的环形伪影。
- 框架将每个标本的处理时间从~100人时降至一次性训练~44 GPU小时 + 每标本1–3分钟推理，使古生物CT数据的大规模自动处理成为可能。

### 7. 优点（方法或实验设计的亮点）
- **完全无需标注**：通过对比学习利用无标签数据，结合确定性伪标签，彻底规避了古生物CT稀缺标注的瓶颈。
- **大型多类群数据集**：利用50,626张切片训练，明显超过以往任何研究，增强了模型的形态与埋藏多样性覆盖。
- **端到端自动化**：从原始CT到水密网格的全流水线自动化，无需人工干预，且代码开源、仅依赖Python，提升了可复现性和可及性。
- **严格的几何验证**：采用固定尺度ICP和裁剪/非裁剪C2C分析，细致区分了配准误差、伪影和真实几何偏差，并分三级队列解读结果，论证扎实。
- **效率跃升**：训练后推理极快，且模型可跨同组化石复用，克服了“训练一次丢一次”的旧范式。

### 8. 不足与局限
- **二维切片处理**：当前U-Net逐切片独立预测，缺乏片间三维一致性约束，可能对斜向薄层结构产生不连续，而作者也承认训练全3D网络受限于GPU内存和标注缺失。
- **泛化范围有限**：模型仅在Kilmaluag组碳酸盐岩化石上训练和测试，尚未在其他地层、岩性（如硅质、磷质）、无脊椎动物化石上验证，跨沉积环境泛化能力未知。
- **伪标签依赖**：粗掩码生成算法的性能受参数制约，虽经验性调优，但在极端低对比度或高度碎裂标本上可能提供不佳的空间先验，影响U-Net精炼效果。
- **对比基准单一**：外部网格比较仅与二值阈值法（最基础自动化方法）对比，未与商业软件中更高级的半自动工具或已发表深度学习分割模型进行同一标本的横向量化比较。
- **缺少切片级外部验证指标**：外部标本仅报告了网格输出的平均Dice/IoU，未给出切片级像素精度，难以直接与文献中切片级Dice比较绝对性能。
- **无消融分析**：未能量化SimCLR预训练、知识融合、复合损失函数等组件各自的增益，难以指导未来优化。

（完）
