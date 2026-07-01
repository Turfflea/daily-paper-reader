---
title: "Optimal Practice Schedules in a Dual-Rate Model of Motor Adaptation, and Their Recovery by Reinforcement Learning"
authors: "Jeter, R., Todorov, D., Molkov, Y."
date: 2026-06-22
pdf: "https://www.biorxiv.org/content/10.64898/2026.06.17.732970v1.full.pdf"
tags: ["query:mt"]
score: 9.0
evidence: 用强化学习优化练习时间表（顺序决策）
tldr: 运动技能训练中如何安排练习顺序以兼顾即时表现与长期保持是一个核心挑战。本文在双速率运动适应模型框架下，提出利用强化学习自动寻优练习调度，发现学习到的策略能有效突破组块练习与间隔练习的传统取舍，为康复和训练等领域的决策优化提供可迁移算法基础。
source: biorxiv
selection_source: fresh_fetch
motivation: 用强化学习优化练习时间表（顺序决策）。
method: 方法与实现细节请参考摘要与正文。
result: 结果与对比结论请参考摘要与正文。
conclusion: 总体而言，该工作在所述任务上展示了有效性，并提供了可复用的思路或工具。
---

## Abstract
A clinician guiding a stroke patient through a 45-minute rehabilitation session, a coach planning a training day, a teacher choosing the order of practice problems, they all face the same question: "given everything practiced so far, what should the next trial be?" The motor-learning literature offers two coarse answers, blocked and interleaved ("random") practice, with a well-known dissociation, blocked practice gives faster acquisition but worse retention, while interleaved practice gives the opposite. We argue that this dissociation is not a fixed property of practice schedules but a shadow of a richer structure. In particular, for a learner whose memory has a fast shared component and slower context-specific components, the best schedule should be a function of the learners current internal state and the time remaining before the retention probe. We make this precise in a minimal two-context fast-slow learner model whose optimal schedules can be computed exactly for short sessions and approximated by a structured beam-search upper bound for longer ones. The optimal schedule is not blocked, not interleaved, and not a single rule; it is a family of schedules determined by how much retention is weighted relative to acquisition. The family has three regimes (alternating, mixed, blocked-with-late-correction) and for long sessions, the optimal schedule has an interpretable structure -- exploit one context, repair the neglected one, then interleave to lock in retention. We then investigate whether a reinforcement-learning teacher, observing only the learners actions and errors without access to their internal memory states, can learn these optimal policies from interaction alone. Comparing these learned policies against the exact optima, we show that a model-free agent (PPO) recovers the short-horizon schedules and the long-horizon block-repair-interleave motif in the intermediate regime, but the benchmark also exposes a sharp failure in the acquisition-dominated regime, where PPO collapses to pure blocking and misses a sparse terminal correction. A warm-start diagnostic shows this failure is a genuine metastability of policy gradients rather than a tuning artifact, with blocked-plus-switch and pure-blocked acting as competing attractors that PPO cannot stabilize between. A hyperparameter sweep over observation history reveals that the agent requires very little behavioral context to plan optimally, demonstrating that partial observability is not a major barrier to finding optimal practice schedules. Finally, we discuss the implications of our framework for motor adaptation and contextual interference, offering practical insights on how instructors can design finite practice sessions to favor long-term retention.