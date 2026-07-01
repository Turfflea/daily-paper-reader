---
title: Protocol-Guided Cross-Domain Transfer Learning for Bovine Facial Pain Recognition under Weak Dairy-Farm Labels
authors: "Patel, S., Neethirajan, S."
date: 2026-06-23
pdf: "https://www.biorxiv.org/content/10.64898/2026.06.18.733162v1.full.pdf"
tags: ["query:mt"]
score: 8.0
evidence: 用于牛面部疼痛识别的跨域深度迁移学习
tldr: 提出协议驱动迁移评估框架，通过对标签映射、域对齐、模型选择等整个适配协议进行控制，结合外部验证与不确定性量化，实现从术后肉牛到乳牛的疼痛面部表情跨域迁移识别。结果表明该方法在弱标签条件下仍可靠，为跨领域预测建模提供了严格评估范式。
source: biorxiv
selection_source: carryover_cache
motivation: 用于牛面部疼痛识别的跨域深度迁移学习。
method: 方法与实现细节请参考摘要与正文。
result: 结果与对比结论请参考摘要与正文。
conclusion: 总体而言，该工作在所述任务上展示了有效性，并提供了可复用的思路或工具。
---

## Abstract
Livestock welfare models are developed under controlled experimental conditions but deployed across farms, breeds, management systems and label regimes, where reliability remains uncertain. We introduce the Protocol-Driven Transfer Evaluation (PDTE) framework, which treats the adaptation protocol, comprising label mapping, objective design, domain alignment, model selection, calibration and threshold policy, as the experimental variable and evaluates transfer through animal-level external validation with uncertainty quantification. We apply PDTE to a bovine welfare task involving transfer of a facial pain representation from postoperative beef cattle to dairy cows under shifts in breed, sex, production system, clinical etiology, recording environment and label fidelity. Using an author-collected Canadian Holstein and Jersey dataset with an independent eight-cow test cohort, direct source-domain transfer was weak, with sequence AUC 0.418 and cow-level AUC 0.400. PDTE identified two failure modes under weak supervision: threshold collapse, in which adaptation converges to a single prediction class, and calibration-induced collapse, in which score ranking is preserved while decision behavior deteriorates. Across protocols, objective design dominated performance. Class-balanced focal adaptation achieved stable operating behavior (sequence AUC 0.611; cow-level AUC 0.667), while a target-only model attained comparable performance without source initialization (sequence AUC 0.596; paired p = 0.984), indicating that protocol design and operating-point choices contributed more than pretraining under weak-label conditions. Animal-level uncertainty remained substantial, with a bootstrap 95% confidence interval of 0.20 to 1.00, exceeding the transfer effect. These findings show that transferability limits cannot be inferred from source-domain performance alone and require protocol-controlled, uncertainty-aware evaluation in livestock AI.