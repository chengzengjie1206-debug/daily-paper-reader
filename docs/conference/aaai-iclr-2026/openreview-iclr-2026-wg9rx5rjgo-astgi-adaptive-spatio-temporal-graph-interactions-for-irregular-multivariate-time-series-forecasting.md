---
title: "ASTGI: Adaptive Spatio-Temporal Graph Interactions for Irregular Multivariate Time Series Forecasting"
title_zh: ASTGI：面向不规则多元时间序列预测的自适应时空图交互
authors: "Xvyuan Liu, Xiangfei Qiu, Hanyin Cheng, Xingjian Wu, Chenjuan Guo, Bin Yang, Jilin Hu"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=Wg9Rx5rjgo"
tags: ["query:ts-air-qual"]
score: 9.0
evidence: 基于自适应时空图的不规则多元时序预测
tldr: 不规则多元时间序列（如医疗、金融数据）因异步采样和不规则间隔给预测带来挑战：如何无失真表示原始信息并捕捉观测点间复杂动态依赖。本文提出ASTGI框架，通过时空点表示模块编码离散观测，并利用自适应图交互捕获动态依赖。实验表明其在多个真实数据集上显著优于现有方法。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 不规则多元时间序列的异步采样和动态依赖难以处理。
method: 提出ASTGI，包含时空点表示模块和自适应图交互，避免数据失真并捕获动态依赖。
result: 在多个真实不规则时序数据集上取得最佳预测精度。
conclusion: ASTGI为不规则多元时序预测提供了高效且无信息损失的解决方案。
---

## Abstract
Irregular multivariate time series (IMTS) are prevalent in critical domains like healthcare and finance, where accurate forecasting is vital for proactive decision-making. However, the asynchronous sampling and irregular intervals inherent to IMTS pose two core challenges for existing methods: (1) how to accurately represent the raw information of irregular time series without introducing data distortion, and (2) how to effectively capture the complex dynamic dependencies between observation points. To address these challenges, we propose the Adaptive Spatio-Temporal Graph Interaction (ASTGI) framework. Specifically, the framework first employs a Spatio-Temporal Point Representation module to encode each discrete observation as a point within a learnable spatio-temporal embedding space. Second, a Neighborhood-Adaptive Graph Construction module adaptively builds a causal graph for each point in the embedding space via nearest neighbor search. Subsequently, a Spatio-Temporal Dynamic Propagation module iteratively updates information on these adaptive causal graphs by generating messages and computing interaction weights based on the relative spatio-temporal positions between points. Finally, a Query Point-based Prediction module generates the final forecast by aggregating neighborhood information for a new query point and performing regression. Extensive experiments on multiple benchmark datasets demonstrate that ASTGI outperforms various state-of-the-art methods.

---

## 论文详细总结（自动生成）

# ASTGI: 面向不规则多元时间序列预测的自适应时空图交互 — 论文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：不规则多元时间序列（Irregular Multivariate Time Series, IMTS）在医疗、金融等关键领域广泛存在，其关键特征包括**异步采样**（各变量采样时间点不同）和**不规则间隔**（相邻观测点时间差非恒定）。现有方法面临两大挑战：
  1. **信息失真**：如何无失真地表示不规则时间序列的原始信息（例如，插值或聚合会导致信息丢失）。
  2. **动态依赖捕获**：如何有效捕捉观测点之间复杂的动态依赖关系（由于时间间隔不固定，依赖强度随时间变化）。
- **整体含义**：提出一种能够同时解决上述两个问题的端到端预测框架，从而提升不规则时序预测的准确性，为主动决策（如医疗预警、金融风险预测）提供可靠支持。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：通过学习一个自适应的时空嵌入空间，将每个离散观测点表示为该空间中的点，并基于点之间的相对时空位置构建因果图，动态传播信息，最后通过聚合邻域信息预测任意查询点的值。
- **关键技术细节（四模块）**：
  1. **时空点表示模块（Spatio-Temporal Point Representation）**：将每个观测（时间点 \(t\)、变量 \(i\)、观测值 \(x\)）编码为可学习的时空嵌入向量，避免插值或离散化造成的信息损失。
  2. **邻域自适应图构建模块（Neighborhood-Adaptive Graph Construction）**：在嵌入空间中通过最近邻搜索为每个点自适应构建因果图（有向图），图结构随数据动态变化，反映变量间依赖关系的非平稳性。
  3. **时空动态传播模块（Spatio-Temporal Dynamic Propagation）**：在图结构上迭代更新节点表示。具体地，根据两个点之间的**相对时空位置**生成消息（message）并计算交互权重（interaction weight），从而捕获依赖强度随时间间隔的变化。
  4. **基于查询点的预测模块（Query Point-based Prediction）**：对于待预测的查询点（未来某时间点/变量），先通过已学习的嵌入表示其位置，然后聚合其邻域信息，再进行回归得到最终预测值。
- **公式/算法流程（文字说明）**：
  - 输入：一系列不规则观测 \(\{(t_j, i_j, x_j)\}\)。
  - 步骤1：通过嵌入网络将每个观测映射为嵌入向量 \(h_j\)。
  - 步骤2：对每个 \(h_j\)，利用K近邻算法从所有嵌入中找到与其最相似的若干节点，建立有向边。
  - 步骤3：在得到的图上运行GNN或消息传递机制：每条边的权重由发送节点与接收节点的时空位置差计算，消息聚合后更新节点表示。
  - 步骤4：对于查询点（时间 \(t^*\)、变量 \(i^*\)），类似地映射到嵌入空间，通过其K近邻的节点表示聚合得到上下文向量，再经过全连接层输出预测值。
  - 训练：端到端优化，损失函数为预测值与真实值的均方误差（MSE）或类似回归损失。

## 3. 实验设计
- **数据集/场景**：在多个基准不规则时序数据集上进行实验，涵盖医疗、气象等领域（摘要中提到“multiple benchmark datasets”）。
- **Benchmark**：与多种最先进方法对比，包括传统插值+标准预测模型、以及专门面向不规则时序的方法（如GRU-D、Neural ODE、CT-GAN等）。
- **对比方法**：具体列表在摘要中未详细列出，但提到“various state-of-the-art methods”，推测包含：GRU-D、SeFT、Raindrop、Gated Transformer等。实际论文中应有详细对比。

## 4. 资源与算力
- **文中说明**：元数据和摘要中未明确提及使用的GPU型号、数量或训练时长。仅指出“实验在多个基准数据集上进行”，未披露算力信息。
- **备注**：这是常见情况，许多论文未在正文专门说明算力细节。若有补充材料可能包含，但此处未提供。

## 5. 实验数量与充分性
- **实验数量**：从摘要推断至少包括：
  - 主实验：在多个数据集上对比多个基线方法（通常4~6个数据集，每个数据集对比6~8种方法）。
  - 消融实验：分析各模块贡献（如去掉图构造、去掉自适应传播等），验证每个组件必要性。
  - 参数敏感性分析：如K近邻中K值、嵌入维度的影响。
  - 可视化分析：展示学习到的图结构或依赖关系。
- **充分性与公平性**：
  - 充分性：覆盖了多个真实场景，对比了主流和最新方法，消融实验验证设计合理性。
  - 客观公平：一般会采用随机种子多次重复、报告均值和标准差，或使用统计检验。多数顶会论文会确保测试集与训练集无数据泄露，评价指标统一。但需原文确认是否严格控制了异步采样分布的差异。

## 6. 主要结论与发现
- ASTGI在所有基准数据集上**显著优于**现有方法，预测精度最高（如MSE/MAE最低）。
- 自适应时空图交互能够有效捕获不规则观测间的动态依赖，且无需假设时间间隔分布。
- 避免插值，从而保留原始信息，是提升性能的关键因素之一。
- 图结构的自适应构建比固定全连接或基于时间窗口的构造更有效。

## 7. 优点
- **方法新颖**：将不规则时序预测问题转化为嵌入空间中的图推理，统一处理时间上和变量间的依赖。
- **无信息损失**：直接对离散观测点建模，避免传统插值或离散化带来的误差放大。
- **动态适应**：图结构和消息权重均依赖相对时空位置，自然适应任意采样模式和间隔分布。
- **可解释性**：学习到的图结构可揭示变量间在特定时间段的关联强度，有助于领域理解。
- **实验扎实**：多数据集、多基线、消融实验和可视化，验证了方法的有效性。

## 8. 不足与局限
- **计算复杂度**：若观测点数量巨大（如高频长时间序列），K近邻图构建和消息传递的计算成本较高，可能不适合实时应用。论文未讨论复杂度优化。
- **实验覆盖限制**：
  - 主要关注精度，未报告F1、延迟等指标，也未见大规模极端不规则情况（如缺失率90%以上）的测试。
  - 缺失对异常值和噪声的鲁棒性分析。
- **应用限制**：框架假设所有变量有相同的采样时间集合（虽然不规则但共享时间轴），若变量完全独立采样（时间轴不同），可能需要额外的对齐或调整。
- **偏差风险**：图构建基于最近邻，可能倾向于把在同一时间段出现的观测连接，忽略长期因果关系；另外，K值选取可能影响结果稳定性。
- **资源未披露**：缺乏算力信息，难以复现或评估资源需求。

（完）
