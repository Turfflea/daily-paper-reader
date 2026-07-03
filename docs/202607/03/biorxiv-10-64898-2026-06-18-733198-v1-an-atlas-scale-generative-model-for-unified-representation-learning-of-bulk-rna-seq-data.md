---
title: An atlas-scale generative model for unified representation learning of bulk RNA-seq data
authors: "Pande, A., Uyar, B., Akalin, A."
date: 2026-06-24
pdf: "https://www.biorxiv.org/content/10.64898/2026.06.18.733198v1.full.pdf"
tags: ["query:mt"]
score: 8.0
evidence: 用于大规模RNA-seq数据统一表征学习的监督VAE，实现组织分类
tldr: "针对公共 bulk RNA-seq 数据异质性强、缺乏统一表征的问题，在十一万余样本上训练监督变分自编码器，以组织类别为监督信号学习组织感知的嵌入。模型组织分类准确率达94.9%，为大规模转录组数据提供了可复用的基础表征。"
source: biorxiv
selection_source: carryover_cache
motivation: 用于大规模RNA-seq数据统一表征学习的监督VAE，实现组织分类。
method: 方法与实现细节请参考摘要与正文。
result: 结果与对比结论请参考摘要与正文。
conclusion: 总体而言，该工作在所述任务上展示了有效性，并提供了可复用的思路或工具。
---

## Abstract
Public bulk RNA-seq repositories contain hundreds of thousands of samples, creating opportunities for large-scale representation learning, but integration across studies remains challenging because of heterogeneous annotations, experimental protocols, and technical variation. While pre-trained foundation models are now widely available for single-cell RNA-seq, comparable resources for bulk RNA-seq remain scarce, motivating a model that learns a unified, tissue-aware representation directly from bulk data. We trained a supervised variational autoencoder (VAE) on a compendium of 118,263 bulk RNA-seq samples that we assembled from TCGA, GTEx, and ARCHS4 and mapped to 42 tissue categories. The model classifies tissue of origin at 94.9% balanced accuracy (weighted F1 96.2%) and compresses 16,115 genes into a 121-dimensional latent space. Tissue identity is the primary organizing axis of the latent space, while source effects remain secondary. To assess the impact of data volume, we constructed training sets at three different scales (38K, 75K, and 118K samples). Our results demonstrated that reconstruction fidelity improved incrementally with each expansion of the dataset, but with diminishing returns. We validated the model on an independent cohort of 734 paediatric tumour samples from TARGET, achieving 84.6% agreement with the expected tissue of origin. The trained model and code are available at GitHub (https://github.com/BIMSBbioinfo/flexynesis_tissue_vae_manuscript) with an interactive web application.