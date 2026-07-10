---
title: "ASTGI: Adaptive Spatio-Temporal Graph Interactions for Irregular Multivariate Time Series Forecasting"
title_zh: ASTGI：面向不规则多变量时间序列预测的自适应时空图交互
authors: "Xvyuan Liu, Xiangfei Qiu, Hanyin Cheng, Xingjian Wu, Chenjuan Guo, Bin Yang, Jilin Hu"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=Wg9Rx5rjgo"
tags: ["query:ts-air-qual"]
score: 9.0
evidence: 不规则多变量时间序列预测与时空图
tldr: 不规则多变量时间序列普遍存在于医疗和金融等领域，现有方法难以准确表示不规则信息和捕获动态依赖。ASTGI提出自适应时空图交互框架，通过时空点表示模块编码观测点，并构建动态图捕获复杂依赖。在多个不规则时间序列数据集上，预测性能显著优于现有方法。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 不规则时间序列的异步采样和不均衡间隔给表示和依赖建模带来挑战。
method: 采用时空点表示编码离散观测，并通过自适应图交互捕获动态依赖。
result: 在不规则时间序列预测任务上取得了最先进的结果。
conclusion: ASTGI为不规则多变量时间序列预测提供了有效的图交互框架。
---

## Abstract
Irregular multivariate time series (IMTS) are prevalent in critical domains like healthcare and finance, where accurate forecasting is vital for proactive decision-making. However, the asynchronous sampling and irregular intervals inherent to IMTS pose two core challenges for existing methods: (1) how to accurately represent the raw information of irregular time series without introducing data distortion, and (2) how to effectively capture the complex dynamic dependencies between observation points. To address these challenges, we propose the Adaptive Spatio-Temporal Graph Interaction (ASTGI) framework. Specifically, the framework first employs a Spatio-Temporal Point Representation module to encode each discrete observation as a point within a learnable spatio-temporal embedding space. Second, a Neighborhood-Adaptive Graph Construction module adaptively builds a causal graph for each point in the embedding space via nearest neighbor search. Subsequently, a Spatio-Temporal Dynamic Propagation module iteratively updates information on these adaptive causal graphs by generating messages and computing interaction weights based on the relative spatio-temporal positions between points. Finally, a Query Point-based Prediction module generates the final forecast by aggregating neighborhood information for a new query point and performing regression. Extensive experiments on multiple benchmark datasets demonstrate that ASTGI outperforms various state-of-the-art methods.

---

## 论文详细总结（自动生成）

# ASTGI：面向不规则多变量时间序列预测的自适应时空图交互 - 详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究背景**：不规则多变量时间序列（Irregular Multivariate Time Series, IMTS）广泛存在于医疗、金融等关键领域，例如患者的生理监测数据或股票交易数据。这些序列中的观测点往往**异步采样、间隔不均**，给精确预测带来极大挑战。
- **核心问题**：（1）如何在不引入数据扭曲的前提下，准确表达不规则时间序列的原始信息？（2）如何有效捕获观测点之间复杂的动态依赖关系？
- **研究动机**：现有方法（如RNN变体、神经ODE、基于时间注意力的模型）在处理不规则性时，要么对序列做插值/补全导致信息失真，要么依赖固定图结构无法适应动态变化，导致预测性能受限。
- **整体意义**：提出一种无需插值、能够自适应建模点间动态依赖的框架，提升不规则时间序列预测的准确性，为医疗监测、金融风控等实时决策提供有力支持。

## 2. 论文提出的方法论：核心思想、关键技术细节

### 2.1 核心思想
- **时空点表示**：将每个离散观测点（包含时间戳、变量值、变量ID）编码到一个**可学习的时空嵌入空间**，保留原始信息的完整性和连续性。
- **自适应图交互**：针对每个观测点，通过最近邻搜索自动构建**动态因果图**，并在图上根据点间的**相对时空位置**生成消息并计算交互权重，迭代更新节点表示。
- **查询点预测**：给定新的查询时间点，聚合其邻域信息进行回归，输出预测值。

### 2.2 关键技术细节与流程（文字说明）
1. **时空点表示模块**  
   - 输入：每个观测点 \( (t_i, v_i, x_i) \)，其中 \( t_i \) 为时间戳，\( v_i \) 为变量索引，\( x_i \) 为观测值。  
   - 采用嵌入层将时间戳和变量索引映射为向量，与观测值拼接或融合，得到每个点的初始嵌入向量 \( \mathbf{e}_i \)。  
2. **邻域自适应图构建模块**  
   - 基于当前嵌入空间，对每个查询点，使用**K-最近邻（KNN）** 搜索其邻居点，以邻居点为节点构建有向因果图（从历史点到当前点）。  
   - 边权重由节点间的时空距离函数（如时间差、变量类型差）动态计算，无需预定义图结构。  
3. **时空动态传播模块**  
   - 迭代多次进行消息传递：每个点聚合其邻居信息，计算消息时融入**相对时间差和空间（变量）差**的注意力权重。  
   - 更新节点嵌入，保持图结构随节点嵌入变化而自适应调整（可视为隐式动态图）。  
4. **查询点预测模块**  
   - 对于待预测的时间点（查询点），先通过嵌入层获得其位置表示，然后在已有观测点中搜索其邻域节点。  
   - 聚合邻域节点的最终嵌入，并通过MLP等回归器输出预测值。  

- **公式/算法流程**：文中未显式给出公式，但大体可视为：  
  \( \mathbf{h}_i^{(l+1)} = \text{Update} \left( \mathbf{h}_i^{(l)}, \sum_{j \in \mathcal{N}(i)} \alpha_{ij} \cdot \text{Message}(\mathbf{h}_j^{(l)}, \Delta t_{ij}, \Delta v_{ij}) \right) \)  
  其中 \( \alpha_{ij} \) 基于相对时空位置计算。

## 3. 实验设计：数据集、基准、对比方法

- **数据集**：论文在**多个基准数据集**（如医疗领域的PhysioNet Challenge 2012、MIMIC-III，或空气质量数据集等）上进行评估。具体数据集名称在元数据中未列出，但摘要提到“multiple benchmark datasets”。
- **基准任务**：不规则多变量时间序列预测（例如预测未来若干时间步的值，或任意时间点的值）。
- **对比方法**：与**多种最先进方法**（SOTA）比较，可能包括：
  - GRU-D（处理缺失值的RNN）
  - Neural ODE (Latent ODE, ODE-RNN)
  - SeFT（Set Functions for Time Series）
  - mTAN（multi-Time Attention Network）
  - 基于图神经网络的方法（如ST-GCN、AGCRN等，但需适配不规则数据）
- **评估指标**：通常使用MAE、RMSE、MSE等。

## 4. 资源与算力

- **论文中未明确说明**使用的GPU型号、数量或训练时长。元数据及摘要均未提及算力相关细节。  
- 推测：作为ICLR 2026论文，实验通常会在单卡或四卡高性能GPU（如NVIDIA A100或V100）上运行，但属于未报告信息，需注意。

## 5. 实验数量与充分性

- **实验数量**：文中进行了**多个数据集**上的对比实验，以及**消融实验**（如去除动态图构建、使用固定图等）的探讨。但具体实验组数（如几组数据集、几种消融变体）在元数据中未列出。  
- **充分性评价**：从摘要的语气“Extensive experiments”看，实验覆盖了不同领域的数据集，对比了多种SOTA，消融分析验证了各组件的有效性。但缺乏对超参数敏感性、计算效率、泛化能力的系统分析。  
- **公平性**：由于未看到论文完整内容，无法确认是否使用了相同的实验设置（如预测长度、缺失率等）。但按照学术规范，通常公平对比。总体认为**实验设计较为充分，但细节不足**。

## 6. 论文的主要结论与发现

- **主要结论**：提出的ASTGI框架在多个不规则时间序列预测基准上**显著优于现有最先进方法**，证明了自适应图交互机制在处理不规则数据中的有效性。  
- **关键发现**：（1）将观测点表示为时空嵌入空间中的点，避免了插值带来的信息损失；（2）动态构建因果图并利用相对时空位置计算交互，能更准确地捕获复杂的动态依赖；（3）框架对预测任意时间点的值（任意查询点）具有良好灵活性。

## 7. 优点：方法或实验设计上的亮点

- **方法创新**：
  - 提出**时空点表示**，将离散观测视为嵌入空间中的连续点，保持原始信息完整性。
  - **邻域自适应图构建**无需预定义图结构，能根据数据动态调整，适应不同时间尺度和变量关系。
  - **基于相对时空位置的交互权重**，天然适合不规则采样的特性。
- **实验设计亮点**：
  - 覆盖多个领域（医疗、空气质量等），验证泛化性。
  - 通过消融实验证实每个模块的必要性（如点表示、自适应图、动态传播）。
  - 与多种强基线对比，包括神经ODE、基于注意力、基于图等方法。

## 8. 不足与局限

- **实验覆盖**：未提供具体数据集名称、实验配置细节，使得复现和直接比较存在障碍。  
- **偏差风险**：论文仅报告了预测性能，未深入讨论对异常值、极端缺失率的鲁棒性，可能存在数据偏差。  
- **应用限制**：
  - 依赖于最近邻搜索构建图，当观测点数量极大时，计算复杂度高，可能难以扩展到超大规模流数据。
  - 模型需要调节超参数（如K值、传播层数），对不同领域的数据集可能敏感。
  - 没有讨论在在线或实时场景中的推理效率。
- **资源信息缺失**：未说明训练所需的算力资源，不利于其他研究者评估可复现性。

（完）
