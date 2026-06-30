---
title: Scaling SMILES-Based Chemical Language Models for Therapeutic Peptide Engineering
title_zh: 面向治疗性肽工程扩展基于SMILES的化学语言模型
authors: "Feller, A. L., Secor, M., Swanson, S., Wilke, C. O., Deibler, K."
date: 2026-06-05
pdf: "https://www.biorxiv.org/content/10.64898/2026.01.06.697994v4.full.pdf"
tags: ["query:mt"]
score: 6.0
evidence: 使用深度化学语言模型进行药物发现中的肽工程
tldr: 治疗性肽占据药物发现中间地带，兼具高特异性和化学多样性，但现有计算模型无法有效处理其复杂结构：蛋白模型限于天然氨基酸，化学模型难处理长序列聚合物。研究者提出 PeptideCLM-2，一种基于超亿分子训练的化学语言模型套件，能原生表征复杂肽化学。在膜扩散、生物功能、半衰期等预测任务上，该模型性能显著超越先前方法，为治疗性肽工程提供了强大的新工具。
source: biorxiv
selection_source: fresh_fetch
motivation: 治疗性肽在计算模型中存在盲点，现有基础模型无法同时兼顾其化学多样性与类聚合物结构，亟需专用方法。
method: 基于超1亿分子训练化学语言模型 PeptideCLM-2，原生表征复杂肽化学，扩展机器学习工具包。
result: 在膜扩散、生物功能、半衰期等开发终点预测上，PeptideCLM-2 性能显著优于先前方法。
conclusion: PeptideCLM-2 弥补了治疗性肽的计算缺口，为肽类候选药物优化提供了有效手段。
---

## 摘要
治疗性肽在药物发现中占据独特的中间地带，兼具蛋白质相互作用的高特异性和小分子的化学多样性，然而它们目前处于计算盲区。现有基础模型无法有效处理它们：蛋白质模型仅限于天然氨基酸，而化学模型难以处理大型、类聚合物的序列。这种脱节迫使该领域依赖静态化学描述符，这些描述符无法捕捉细微的化学细节，或依赖为特定数据集定制设计的复杂多嵌入流程。为弥合这一差距，我们提出了 PeptideCLM-2，一套在超过 1 亿个分子上训练的化学语言模型，能够原生地表示复杂肽化学。这种建模方法扩展了治疗性肽机器学习模型的可用工具库。基准测试结果显示，在预测开发终点（包括膜扩散、生物功能和半衰期）方面，其性能优于先前的方法。

## Abstract
Therapeutic peptides occupy a unique middle ground in drug discovery, offering the high specificity of protein interactions with the chemical diversity of small molecules, yet they currently fall in a computational blind spot. Existing foundation models cannot handle them effectively: protein models are restricted to natural amino acids, while chemical models struggle to process large, polymer-like sequences. This disconnect has forced the field to rely on static chemical descriptors that fail to capture subtle chemical details or on complex multi-embedding pipelines that are custom tailored to specific datasets. To bridge this gap, we present PeptideCLM-2, a suite of chemical language models trained on over 100 million molecules to natively represent complex peptide chemistry. This modeling approach expands the available toolkit of machine learning models for therapeutic peptides. Benchmarking results show strong performance versus prior methods for predicting development endpoints including membrane diffusion, biological function, and half life.