---
title: Hierarchical Reinforcement Learning with Topology-Aware Exploration Framework for Multi-path Commodity Flow Problem
title_zh: 面向多径商品流问题的拓扑感知探索的分层强化学习框架
authors: "Jingchen Jiang, Xuan Zhou, Jiayuan Li, Geng Han, Xiang Shi, Fang Deng"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40947/44908"
tags: ["query:mt"]
score: 10.0
evidence: 拓扑感知探索的分层强化学习用于网络资源分配
tldr: 现有的多径商品流问题方法依赖预生成路径，忽视实时负载状态和决策耦合，导致解质量不高。本文提出HRL-TAE，首个端到端框架，结合分层强化学习与拓扑感知探索，利用状态转移引导列表（STGL）指导状态转移。在动态网络环境下，该方法相比基线方法取得了更高的吞吐量和更低的延迟，为网络资源分配提供了高效方案。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40947/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1553, \"height\": 719, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40947/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1752, \"height\": 1358, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40947/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 871, \"height\": 912, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40947/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1660, \"height\": 806, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40947/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1479, \"height\": 837, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40947/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 701, \"height\": 635, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40947/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 859, \"height\": 453, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40947/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 807, \"height\": 575, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40947/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 863, \"height\": 344, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40947/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1455, \"height\": 904, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40947/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 872, \"height\": 900, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40947/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1429, \"height\": 508, \"label\": \"Table\"}]"
motivation: 现有的基于预生成路径的方法忽视实时负载状态和决策耦合，导致解质量不高。
method: 提出分层强化学习与拓扑感知探索，利用状态转移引导列表（STGL）指导状态转移。
result: 在动态网络环境下，相比基线方法取得了更高的吞吐量和更低的延迟。
conclusion: 首次实现端到端的动态多径商品流高质量解决方案，显著提升网络传输效率。
---

## Abstract
The multi-path commodity flow problem (MPCFP) is crucial for ensuring reliable and high-speed data transmission in communication networks. However, existing studies that employ pre-generated routing paths neglect real-time load state and the coupling among decisions, thus hindering the achievement of high-quality solutions. To overcome this, we propose Hierarchical Reinforcement Learning with Topology-Aware Exploration (HRL-TAE), which is the first fully end-to-end framework that dynamically produces high-quality solutions based on real-time network states. HRL-TAE integrates an exploration mechanism and utilizes the State Transition Guiding List (STGL) to guide state transitions, thereby transforming topology exploration into a Markov decision process. Guided by STGL, two closely coupled layers in HRL-TAE, that is, the path construct layer and the ratio allocate layer, construct multiple subpaths for each flow and allocate traffic ratios among them. Subsequently, adaptive constraint-driven masks exclude infeasible actions during decision making, thereby guaranteeing that all constraints are satisfied. We also adopt a tailored training approach to obtain accurate gradient estimates and improve training efficiency. Simulations and real-world experiments demonstrate that HRL-TAE achieves superior performance.

---

## 论文详细总结（自动生成）

## 论文核心问题与整体含义（研究动机和背景）
* **核心问题**：多径商品流问题（MPCFP）是在通信网络中，为每个数据流构造多条子路径并分配流量比例，以最小化最大链路利用率（MLU），实现负载均衡。
* **研究背景**：通信网络面临设备与用户需求爆炸式增长，急需高效的网络资源分配与流量管理策略。
* **现有方法局限**：
    * 传统方法（启发式算法、进化计算、线性规划）难以兼顾解的质量与计算效率。
    * 现有学习型方法（如TEAL、DRSIR）依赖预先生成的K最短路径（KSP）集，再使用强化学习分配流量，忽视了**实时网络负载状态**和**不同流决策之间的耦合**，导致次优解。
* **整体含义**：提出首个**全端到端**框架，能够根据实时网络状态动态生成高质量的多径路由方案，同时解决路径构造与比例分配这两个紧密耦合的子问题。

## 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
* **核心思想**：将MPCFP分解为两个耦合子任务——路径构造与比例分配，采用**分层强化学习**，并设计**拓扑感知探索机制**将拓扑探索转化为马尔可夫决策过程（MDP）。
* **关键技术细节**：
    1.  **树状探索过程（TLEP）与状态转移引导列表（STGL）**：
        *   用一个二维列表 `L = (N, B)` 记录待访问节点ID及分配给这些节点的流量。
        *   智能体每次移动到列表首节点，分层网络输出要访问的下一跳节点集合 `V` 及对应的分配比例 `R`。
        *   根据输出更新边剩余容量、标记已到达节点、更新STGL并按节点到目的地的距离排序，引导状态转移。
        *   重复直到STGL中只剩下目的节点，完成一个流的路径探索。
    2.  **分层网络结构**（路径构造层 + 比例分配层）：
        *   路径构造层：输入状态信息，输出下一跳节点选择概率。
        *   比例分配层：输入状态信息和路径构造层选中的节点，输出在这些节点间的分配比例（用正态分布权重表示）。
        *   两层结构类似，均采用编码器-解码器架构。
    3.  **自适应约束驱动掩码机制**：
        *   编码器中屏蔽已访问节点；解码器中仅允许选择当前节点的未访问邻居（路径层）或由路径层选定的节点（分配层）。
        *   编码器保留部分不可行动作以增强状态理解，解码时严格过滤，保证生成的解满足容量、流守恒等约束。
    4.  **四元组协同训练（QCT）方法**：
        *   解决分层强化学习中的信用分配问题。
        *   将训练网络与基线网络（rollout baseline）交叉组合成四种组合（如“路径-训练+分配-基线”等），分别计算奖励，利用控制变量思想构造损失函数，获得更精确的梯度估计，减少波动并提升训练效率。
        *   梯度更新基于REINFORCE算法，基线网络通过T检验确定是否更新。

## 实验设计：数据集/场景、基准方法、对比方法
* **数据集与场景**：
    *   **仿真实验**：使用来自Internet Topology Zoo的四种真实网络拓扑——Abilene（11节点）、ATT（25节点）、Geant（37节点）、DFN（58节点），每种拓扑上随机生成20个测试实例。
    *   **真实系统实验**：在基于ONOS控制器的软件定义网络（SDN）系统上进行，使用BigTao网络测试仪和核心交换机等设备，采用Waxman算法生成的H9N（9节点）和H18N（18节点）拓扑，各生成测试实例。
* **对比方法**：
    *   **TEAL**：基于KSP预生成路径，使用深度强化学习分配流量的学习型方法。
    *   **ECMP**：等成本多路径，均衡分配流量的简单规则。
    *   **DRSIR**：在SDN中利用深度强化学习从预生成路径集中选择路径和分配流量的方法。
* **消融实验**：
    *   **Abl.1**：不使用QCT，采用基本REINFORCE同时更新两层。
    *   **Abl.2**：两层交替训练（每10个epoch切换）。
    *   均在相同测试数据上比较。

## 资源与算力
*   论文**未明确说明**使用的GPU数量、训练总时长等具体算力资源。仅提及实验平台为“搭载第9代Intel Core i9处理器、NVIDIA GeForce GTX1660Ti GPU、32GB RAM的计算机”，训练使用Adam优化器，共1000个epoch，每epoch包含10个批次，批次大小128。

## 实验数量与充分性
* **实验数量较多，设计较为充分**：
    *   **仿真实验**：在4种拓扑上各进行20个测试案例，共80个独立对比实验，对比4种算法，表格给出每个案例的具体MLU和耗时。
    *   **消融实验**：同样在4种拓扑的20个案例上进行，对比3种算法变体（HRL-TAE, Abl.1, Abl.2），并展示了Geant拓扑上的训练曲线。
    *   **真实系统实验**：在2种拓扑（H9N, H18N）上各进行10个测试案例，对比4种算法。
* **客观性与公平性**：
    *   测试数据根据各拓扑连接模式随机生成，所有算法在相同实例上评估。
    *   对比算法均为该领域代表性方法（规则式ECMP、基于KSP的学习型TEAL和DRSIR）。
    *   消融实验变量单一（仅改变训练方式），能有效验证QCT的有效性。
    *   评估指标统一为最大化链路利用率（MLU）和计算时间，具有可比较性。

## 论文的主要结论与发现
*   HRL-TAE在多种真实网络拓扑和真实SDN系统中均实现了**最低的MLU**，且计算时间与对比算法相当或更优。
*   与TEAL、ECMP、DRSIR相比，HRL-TAE的路由方案分别在仿真实验中平均性能提升**7.08%、15.35%、9.74%**，在真实系统实验中平均提升**6.90%、15.80%、8.72%**。
*   消融实验证明QCT方法显著减少了学习波动，提升训练效率，且性能优于联合训练或交替训练。
*   整体表明，端到端、拓扑感知的分层RL框架能够克服预生成路径的局限性，获得更优的负载均衡方案。

## 优点：方法或实验设计上的亮点
*   **方法创新性**：首次实现MPCFP的全端到端解决方案，无需预生成路径，直接利用实时状态同时进行路径构建和比例分配，开创了新范式。
*   **拓扑感知探索机制**：设计TLEP和STGL巧妙地将非结构的路径探索问题转化为结构化的MDP，克服了RL应用于此类问题的关键障碍。
*   **硬约束满足**：通过自适应掩码在决策时即保证解满足所有约束，无需启发式修复，保证了方案的可行性。
*   **针对性的训练策略**：QCT方法通过交叉组合方式缓解了分层RL中的信用分配问题，从训练曲线上看确实有效提升了训练的稳定性和效率。
*   **实验全面扎实**：包含多种规模拓扑的仿真、深入详细的消融研究，以及在真实SDN系统上的验证，覆盖了从虚拟到物理的多层评估。

## 不足与局限：包括实验覆盖、偏差风险、应用限制等
*   **算力信息缺失**：未提供训练时间和显存占用等量化信息，难以评估实际训练成本，影响复现性评估。
*   **可扩展性待验证**：实验拓扑规模最大为58节点（DFN）和37节点（Geant），对于超大规模（如数据中心、骨干网数百节点）网络的适应性未探讨。
*   **动态变化场景有限**：实验虽有真实系统验证，但流量模式似乎是静态实例（每个案例一组固定的流需求），未验证在网络动态变化（如拓扑变化、流量实时波动）下在线自适应的能力。
*   **算法对比范围**：对比方法主要选取基于KSP的学习型方法和简单基线，缺少与最新传统优化求解器或更先进的启发式方法的直接性能对比。
*   **泛化能力**：论文训练和测试均在同一拓扑上进行（不同实例），对于跨拓扑泛化（zero-shot）的能力没有给出评估，部署到全新网络可能需要重新训练。
*   **单目标优化**：目标仅为最小化MLU，未考虑延迟、丢包率、能耗等多目标需求，可能与实际网络运营的复杂需求存在差距。

（完）
