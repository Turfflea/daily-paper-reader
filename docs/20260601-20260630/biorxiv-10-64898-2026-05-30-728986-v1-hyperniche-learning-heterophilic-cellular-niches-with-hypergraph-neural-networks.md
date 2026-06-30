---
title: "HyperNiche: Learning Heterophilic Cellular Niches with Hypergraph Neural Networks"
title_zh: "HyperNiche: 利用超图神经网络学习异配性细胞生态位"
authors: "Mahmud, M. I., Banerjee, T."
date: 2026-06-03
pdf: "https://www.biorxiv.org/content/10.64898/2026.05.30.728986v1.full.pdf"
tags: ["query:mt"]
score: 6.0
evidence: 利用超图神经网络建模空间转录组中的高阶细胞关系，扩展图方法于关系数据
tldr: 针对空间转录组学中传统图方法仅依赖成对相似性、难以捕获异质性多细胞生态位的问题，HyperNiche提出基于超图的高阶关系建模框架。通过兼容性驱动的锚点中心超边构建和节点角色解耦，模型整合空间几何信息，能够同时学习同质性和异质性细胞关系。在乳腺癌和肺癌Xenium数据上，该框架在聚类指标（ARI、NMI）和生物学解释性上超越现有方法，并产生特征多样性更高的超边，证明其捕获异质细胞生态位的能力。该工作突显了高阶关系建模对解析肿瘤微环境复杂空间组织的重要性。
source: biorxiv
selection_source: fresh_fetch
motivation: 传统图方法仅建模成对相似性，倾向于产生同质簇，无法捕获肿瘤微环境中跨细胞类型的异质性多细胞生态位。
method: 提出HyperNiche超图框架，通过锚点嵌入和成员嵌入解耦，结合空间几何的兼容性驱动机制动态构建锚点中心超边，学习高阶细胞关系。
result: 在乳腺癌和肺癌空间转录组数据上，HyperNiche在聚类性能（ARI、NMI）和生物学解释性上显著优于基线，且超边内特征多样性更高。
conclusion: HyperNiche通过超图高阶关系建模有效捕获异质细胞生态位，提升了空间组织解析力，为肿瘤微环境研究提供了新工具。
---

## 摘要
我们提出 HyperNiche，一个基于超图的框架，用于从空间转录组学数据中建模高阶异质性细胞生态位。不同于依赖成对相似性且倾向于产生同质聚类的传统基于图的方法，HyperNiche 通过兼容性驱动机制学习以锚点为中心的超边，该机制捕获细胞间的同配性和异配性关系。通过将节点角色解耦为锚点和成员表示，并将空间几何信息整合到超边构建中，该模型能够发现跨越多种细胞类型的多细胞生态位。我们在来自乳腺癌和肺癌组织微阵列的高通量 Xenium 空间转录组学数据集上评估 HyperNiche，展示了在聚类性能（ARI、NMI）和生物学可解释性方面相对于最先进的基于图的基线的改进。进一步分析表明，HyperNiche 产生的超边具有显著更高的边内特征多样性，这表明与基于相似性的模型相比，其捕获异质性细胞生态位的能力增强。这些结果凸显了高阶关系建模对于理解复杂空间组织组织和肿瘤微环境的重要性。

## Abstract
We propose HyperNiche, a hypergraph-based framework for modeling higher-order, heterogeneous cellular niches from spatial transcriptomics data. Unlike conventional graph-based methods that rely on pairwise similarity and tend to produce homogeneous clusters, HyperNiche learns anchor-centered hyperedges through a compatibility-driven mechanism that captures both homophilic and heterophilic relationships among cells. By decoupling node roles into anchor and member representations and integrating spatial geometry into hyperedge construction, the model enables the discovery of multicellular niches that span diverse cell types. We evaluate HyperNiche on high-plex Xenium spatial transcriptomics datasets from breast and lung cancer tissue microarrays, demonstrating improvements over state-of-the-art graph-based baselines in clustering performance (ARI, NMI) and biological interpretability. Further analysis shows that HyperNiche produces hyperedges with significantly higher intra-edge feature diversity, indicating an enhanced ability to capture heterogeneous cellular niches compared to similarity-based models. These results highlight the importance of higher-order relational modeling for understanding complex spatial tissue organization and tumor microenvironments.