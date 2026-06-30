---
title: Self-supervised Internal Learning Enhances Isotropic Resolution for Three-dimensional Fluorescence Microscopy
title_zh: 自监督内部学习增强三维荧光显微镜的各向同性分辨率
authors: "Wei, M., Xu, P., Liu, J., Li, X., Feng, X., Zhu, J., Dong, R., Ran, H., Zhu, W., Han, Y., Li, Y., Guo, M., Liu, H."
date: 2026-06-08
pdf: "https://www.biorxiv.org/content/10.64898/2026.06.04.717237v1.full.pdf"
tags: ["query:mt"]
score: 9.0
evidence: 自监督内部学习用于显微镜图像恢复
tldr: 三维荧光显微镜常因轴向采样稀疏导致各向异性分辨率，阻碍生物结构精细解析。DeepIso提出自监督内学习策略，结合监督预训练，从单个体积数据自适应估计轴向退化函数，无需点扩散函数建模或横向-轴向等价假设，恢复丢失的高频信息并改善细长结构连续性。在合成数据和共聚焦、光片、结构光照明显微镜实验数据上验证，其各向同性恢复效果在视觉质量和量化指标上均显著优于现有计算方法，且可有效支撑后续分割与跟踪等三维定量分析。
source: biorxiv
selection_source: fresh_fetch
motivation: 三维荧光显微镜轴向信息模糊、各向异性，现有光学和计算恢复方法需精确PSF或苛刻校准，难以广泛应用。
method: DeepIso结合监督预训练与自监督内学习，从单个体积自适应估计轴向退化，无需PSF建模。
result: 在合成与多种显微镜实验数据上，DeepIso在视觉质量和量化指标上显著优于现有方法，有效恢复轴向高频，提升结构连续性。
conclusion: DeepIso为三维荧光显微镜提供了一种无需PSF、适用多模态的各向同性增强工具，利于下游定量分析。
---

## 摘要
三维荧光显微镜通常呈现各向异性分辨率，因为轴向信息采样不足且比横向信息更模糊，这使精细三维结构的定量解释复杂化。尽管已探索了光学校正和计算复原方法，但许多方法需要严苛的系统校准，或依赖于精确的点扩散函数模型和假设，而这些难以在所有样本和成像方式中满足。本文提出DeepIso，一种自监督各向同性复原框架，将监督预训练与内部学习推断阶段相结合，直接从所测体积估计退化。无需显式指定点扩散函数或强制侧向-轴向结构等效，DeepIso恢复了轴向频率成分并改善了细长结构的连续性，同时保留精细特征，在视觉检查和定量指标上均优于现有计算方法。该方法在合成基准和实验数据集上得到验证，展示了在共聚焦、光片和三维结构照明显微镜中的各向同性增强，从而支持下游体积分析，包括分割和追踪。

## Abstract
Three-dimensional fluorescence microscopy often exhibits anisotropic resolution because axial information is poorly sampled and more blurred than lateral information, which complicates quantitative interpretation of fine 3D structures. Although optical remedies and computational restoration have been explored, many approaches require demanding system calibration or rely on accurate PSF models and assumptions that are difficult to satisfy across all samples and modalities. Here we present DeepIso, a self-supervised isotropy restoration framework that couples supervised pretraining with an internal-learning inference stage to estimate degradation directly from the measured volume. Without explicit PSF specification or enforced lateral-axial structural equivalence, DeepIso recovers axial frequency content and improves the continuity of elongated structures while retaining fine features, with superior performance over existing computational approaches in terms of both visual inspection and quantitative metrics. The method is validated on synthetic benchmarks and experimental datasets, demonstrating isotropy enhancement across confocal, light-sheet, and 3D structured illumination microscopy, thereby supporting downstream volumetric analysis including segmentation and tracking.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
三维荧光显微镜（如共聚焦、光片、结构照明显微镜）在成像时，因光学衍射限制和轴向采样间隔较大，通常导致**轴向分辨率远低于横向分辨率**（即各向异性分辨率）。这种各向异性严重阻碍了对精细三维生物结构（如微管、线粒体、细胞膜）的精确分割、追踪和定量分析。现有解决方案可分为两类：
*   **硬件方法**：如多视角成像、4Pi-SIM等，虽能实现近各向同性分辨率，但通常需要复杂的光学系统、严苛的校准和较高的计算成本。
*   **计算方法**：如三维反卷积、基于深度学习的复原方法（如 CARE, CycleGAN, SelfNet 等）。但这些方法要么依赖精确且难以获取的**点扩散函数（PSF）模型**，要么基于**侧向与轴向视图结构相似**的不合理假设，容易产生伪影或纹理幻觉。

该研究的核心问题是：**如何在不依赖精确PSF和侧向-轴向结构相似假设的前提下，从单次各向异性采集的体积数据中，通过计算复原出高保真的各向同性三维结构**。

### 2. 方法论：DeepIso框架
DeepIso 是一个**两阶段自监督内部学习框架**，其核心思想是将监督预训练与无监督的测试时自适应相结合，直接从待处理数据学习退化过程。

*   **第一阶段：监督预训练**
    *   **目标**：使主干网络（IsoNet）学习从各向异性到各向同性的通用映射，并让辅助的编码器-解码器对（IsoEnc/IsoDec）学习各向异性图像在隐空间的紧凑表征。
    *   **关键技术细节**：
        1.  **合成数据生成**：从高质量的横向切片出发，通过一系列不同方向和尺度的一维高斯模糊核（模拟轴向退化）和像素尺寸对齐（模拟采样差异），生成伪各向异性输入图像。
        2.  **IsoNet训练**：以合成各向同性图像为真值，训练IsoNet（基于RCAN架构），最小化重构损失。
        3.  **IsoEnc/IsoDec训练**：IsoEnc将伪各向异性图像编码为1024维隐向量；IsoDec则以该隐向量和对应的各向同性图像为输入，重建各向异性图像，并通过最小化自重建损失来正则化训练。此设计为后续的测试时适应提供了**隐空间一致性约束**，防止网络产生不受约束的幻觉。

*   **第二阶段：自适应推理**
    *   **目标**：将预训练模型在线适配到特定的、未知真实退化的实验数据上，无需任何外部监督。
    *   **流程**：
        1.  对原始轴向图像进行像素对齐（下采样再上采样，以匹配横向像素尺寸），得到“对齐轴向数据”。
        2.  将对齐轴向数据输入IsoNet，得到初始的“各向同性估计”。
        3.  **关键自适应步骤**：冻结阶段一训练好的IsoEnc和IsoDec。将对齐轴向数据（作为“各向异性真相”）和各向同性估计（作为“高分辨率参考”）分别输入IsoEnc和IsoDec，生成“潜在各向异性输出”。通过最小化“潜在各向异性输出”与“对齐轴向数据”（真实观测）之间的一致性损失，**仅迭代更新IsoNet的参数**。

### 3. 实验设计与对比方法
论文通过合成数据和多种实验数据验证DeepIso的有效性，并进行了全面的量化对比。

*   **数据集与场景**：
    *   **合成数据**：包含点、线、空心球、空心圆柱等几何基元的模拟体积，其侧向与轴向视图结构故意不同，用于定量评估。
    *   **实验数据**：
        1.  **超分辨率3D-SIM数据**：固定的U2OS细胞（微管、线粒体外膜）和细菌膜，以硬件增强的4B-SIM或标准3D-SIM作为输入/真值。
        2.  **光片显微镜数据**：活U2OS细胞（线粒体）和斑马鱼胚胎（细胞膜），以单视图SPIM作为输入，双视图融合的diSPIM作为近各向同性真值。
        3.  **共聚焦显微镜数据**：小鼠Thy1神经元，展示在常规衍射受限系统上的泛化能力。
        4.  **下游任务**：对斑马鱼胚胎进行6.25小时的长时间成像，以验证其在细胞分割和动态追踪中的效果。

*   **对比方法**：与多种主流方法进行公平对比，所有方法均在相同数据集上，按作者推荐的设置重新训练。
    *   **监督方法**：CARE（内容感知图像复原）。
    *   **无监督/弱监督方法**：CycleGAN， SelfNet， SSAI-3D。

*   **评估指标**：
    *   峰值信噪比 (PSNR)
    *   结构相似性指数 (SSIM)
    *   均方根误差 (RMSE)
    *   归一化互相关 (NCC)
    *   傅里叶频谱分析（评估轴向频率支持范围）

### 4. 资源与算力
论文明确提到了训练和推理所需的硬件和时间成本：
*   **硬件**：所有实验均在单个 **NVIDIA GeForce RTX 4080 SUPER** (16GB显存) GPU上进行。
*   **训练时长**：
    *   以共聚焦小鼠神经元数据为例，IsoNet的预训练耗时约**4.8小时**，IsoEnc/IsoDec的预训练耗时约**6.0小时**。由于两者结构独立，可以并行训练以缩短总耗时。
    *   第二阶段自适应推理收敛迅速，约**12,000次迭代，耗时13分钟**。
*   **推理过程**：训练和自适应完成后，模型可以直接用于处理新数据，采用与CARE等方法相同的分块重建策略，无需额外优化。

### 5. 实验数量与充分性
实验设计充分且客观，覆盖了广泛的成像模态和生物样本，并对方法进行了严格验证。
*   **多维度数据集**：实验涉及4种不同的显微镜模态（合成、共聚焦、光片、3D-SIM）和多种生物样本（细菌、细胞、组织器官），总数至少为**5个独立的主要实验 set**。
*   **公平对比**：与**4种**主流深度学习方法进行了量化对比，所有方法均使用官方代码和推荐设置进行重训练。
*   **多指标量化**：采用PSNR、SSIM、RMSE、NCC等多种互补指标进行评估，并提供了具有误差棒的量化结果（例如，合成数据N=31张图，微管数据N=26个ROI等），确保了统计可靠性。
*   **定性证据详实**：通过大量的放大视图、线强度剖面图和傅里叶频谱图，直观且深入地展示了各方法在恢复结构细节、对比度和频率成分上的差异。

### 6. 主要结论与发现
*   DeepIso成功地在**无需精确PSF估计和侧向-轴向结构相似假设**的情况下，实现了从各向异性数据中恢复各向同性体积。
*   在所有测试的模态和对比方法中，DeepIso在**视觉质量和所有量化指标（PSNR, SSIM, RMSE, NCC）上均达到了最优或领先水平**。
*   DeepIso能有效**扩展轴向的频率支持范围**，恢复丢失的轴向高频信息，改善细长结构的连续性，并抑制了其他方法（特别是GAN-based方法）中常见的伪影和纹理幻觉。
*   该方法不仅适用于静态结构成像，还能为**长时间动态成像提供高质量恢复**，直接促进下游的细胞分割和追踪任务，其效果可与硬件双视图融合方法相媲美。

### 7. 优点
*   **免PSF和假设的自由度**：框架的核心优势在于不依赖难以获取的PSF和不合理的结构相似性假设，提高了方法的普适性和准确性。
*   **新颖的自监督机制**：通过“预训练+自适应”的两阶段设计和**冻结的编码-解码器提供的隐空间一致性约束**，巧妙地将外部知识迁移到内部学习，有效防止了无监督学习中的幻觉问题。
*   **强大的泛化能力**：已在从普通共聚焦到超分辨SIM的多种成像模态和多种生物样本上得到验证，展现出跨模态、跨样本的鲁棒性。
*   **下游任务友好**：论文论证了该方法不仅是一个图像美化工具，其复原结果可直接提升分割、追踪等定量生物学分析的精度。

### 8. 不足与局限
*   **样本特异的适应性需求**：虽然跨模态泛化能力强，但面对结构差异巨大的不同生物样本时，仍推荐进行特定数据集的训练或自适应，一个通用模型可能无法在所有场景下达到最佳性能。
*   **计算成本相对较高**：虽然推断速度快，但预训练和自适应阶段仍需数小时的计算时间，这可能限制了其在计算资源有限环境下的即时应用。
*   **各向异性处理的局限性**：当前设计主要针对**轴向分辨率低于横向**的常见情况。论文指出，若要处理横向也存在各向异性的更复杂场景，需要对网络架构进行重新设计，这被留作未来的工作。
*   **复杂样本的挑战**：对于存在严重光散射、折射率不均或光学像差的复杂样本，其复原效果仍有待验证，未来可结合物理模型先验或多视图融合策略进一步提升。

（完）
