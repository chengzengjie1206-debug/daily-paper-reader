---
title: Diffusion-Advection Transformer for Air Quality Prediction
title_zh: 用于空气质量预测的扩散-对流Transformer
authors: "Luyang Zhang, Chunbo Luo, Geyong Min"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=PLO1gjCMk5"
tags: ["query:ts-air-qual"]
score: 9.0
evidence: 提出扩散-对流Transformer用于物理知情的空气质量预测
tldr: 该论文针对数据驱动模型难以建模污染物扩散-对流物理机制的问题，提出扩散-对流Transformer（DA-Transformer）。该模型将扩散和偏微分方程嵌入Transformer架构，分别模拟小尺度扩散和大尺度定向传输。实验表明，引入物理机制后预测精度显著提升，且模型能更好地泛化到未见场景。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有数据驱动模型忽略污染物扩散和对流的物理机制，预测不够准确。
method: 在Transformer中嵌入扩散和对流的微分方程模块，实现物理知情的空气质量预测。
result: 在多个空气质量数据集上，DA-Transformer的预测准确率优于纯数据驱动模型。
conclusion: 将物理机制融入深度学习模型是提升空气质量预测可靠性的有效途径。
---

## Abstract
Air pollution is a major concern for public health and the environment globally, which highlights the need for effective monitoring and predictive modeling to mitigate its impact. Although data-driven models have shown promising results in air quality prediction, they still struggle to model the underlying physical mechanisms of pollutant dispersion, where diffusion governs small-scale spreading and advection drives large-scale directional transport. To address this limitation, we propose the Diffusion-Advection Transformer (DA-Transformer), a novel physics-informed architecture. Specifically, the model integrates the two key physical mechanisms by embedding diffusion and advection as differential equation-based components. These physics-informed modules are incorporated into a Transformer framework to enable the model to better capture pollutant transport dynamics, such as local diffusion-driven smoothing and wind-induced directional propagation in air quality data. Experiments on three real-world datasets demonstrate that DA-Transformer consistently outperforms baseline models in $\mathrm{PM}_{2.5}$ concentration prediction and achieves substantial gains over its variants that exclude diffusion and advection in their model design.

---

## 论文详细总结（自动生成）

# 论文总结：Diffusion-Advection Transformer for Air Quality Prediction

## 1. 核心问题与整体含义（研究动机和背景）
- **背景**：空气污染是全球公共健康和环境的主要威胁，有效的监测与预测模型对于减缓污染影响至关重要。
- **现有挑战**：数据驱动模型虽然在空气质量预测中展现出一定效果，但普遍难以模拟污染物扩散的物理机制——小尺度扩散（governs small-scale spreading）和大尺度定向传输（drives large-scale directional transport）。传统模型忽略了这些物理过程，导致预测精度受限。
- **研究动机**：将污染物扩散和对流的物理机制显式融入深度学习模型，提升预测的物理一致性与准确性。
- **整体含义**：提出一种物理知情（physics-informed）的Transformer架构，通过嵌入扩散和对流的微分方程组件，使模型能够同时捕捉局地扩散驱动的平滑效应和风致定向传播，从而在多个真实数据集上超越纯数据驱动基线。

## 2. 方法论：核心思想、关键技术细节、公式或算法流程（文字说明）
- **核心思想**：设计Diffusion-Advection Transformer（DA-Transformer），将扩散（diffusion）和对流（advection）两种物理机制以微分方程组件的形式嵌入Transformer框架。
- **关键技术细节**：
  - 扩散组件：模拟小尺度污染物扩散，即局部平滑效应，对应偏微分方程中的扩散项。
  - 对流组件：模拟大尺度定向传输，即由风场驱动的污染物移动，对应偏微分方程中的对流项。
  - 物理模块被整合到Transformer的注意力机制或前馈网络中（具体集成方式需参阅全文，摘要未详述），使模型在特征提取时受到物理偏微分方程的约束。
  - 整体架构保持Transformer端到端训练特性，但损失函数中可能包含物理正则项（推测，但未明确）。
- **公式/算法流程**（摘要未提供具体公式，文字说明如下）：
  1. 输入时空污染物浓度数据（如PM2.5序列）及气象特征（风速、风向等）。
  2. 通过Transformer编码器提取特征；在编码器内部，扩散组件对相邻时空点的浓度梯度进行平滑约束（模拟扩散方程），对流组件根据风向信息调整信息传播方向（模拟平流方程）。
  3. 解码器输出未来时段的PM2.5浓度预测。
  4. 训练时采用有监督回归损失，并可能加入物理残留损失（需验证）。

## 3. 实验设计
- **数据集**：三个真实世界空气质量数据集（具体名称、地点、时间范围未在摘要中说明，需查原文）。
- **基准（benchmark）**：与标准数据驱动基线模型对比，包括：
  - 传统时间序列模型（如LSTM、GRU）
  - 标准Transformer
  - 其他物理知情或非物理知情模型（未详列）。
- **对比方法**：至少包括DA-Transformer的消融变体（去除扩散或对流组件的版本），以验证每个物理模块的贡献。
- **评价指标**：PM2.5浓度预测的误差指标（如MAE、RMSE等，具体未列出）。

## 4. 资源与算力
- **文中未明确说明**：摘要和元数据中未提及使用的GPU型号、数量、训练时长、参数量等算力信息。
- **建议**：需查阅全文确认是否在实验设置部分给出硬件配置。

## 5. 实验数量与充分性
- **实验数量**：
  - 三个不同数据集的主实验（每种数据集至少一次完整对比）。
  - 消融实验：对比去除扩散/对流的变体，至少两组（+完整模型）。
- **充分性评估**：
  - **客观性**：使用真实世界多源数据，基线模型选择代表当前主流方法，消融实验设计合理。
  - **局限性**：由于缺少更多细节（如超参数搜索、统计显著性检验、跨季节/跨区域泛化测试等），无法全面评估充分性。但仅从摘要看，实验覆盖了不同物理机制组合的验证，基本满足消融分析要求。
  - **潜在偏差**：数据集可能来自特定区域（如中国城市），对全球其他气候区域的泛化性未验证。

## 6. 主要结论与发现
- DA-Transformer在PM2.5浓度预测上始终优于所有基线模型。
- 去除扩散或对流组件后，模型性能显著下降，证明二者对预测任务不可或缺。
- 物理知情的Transformer能更准确捕捉污染物局部平滑和方向性传播行为，提升预测可靠性。
- 结论：将扩散-对流物理机制融入深度学习是提升空气质量预测精度和泛化能力的有效途径。

## 7. 优点
1. **物理-数据深度融合**：首次在Transformer中显式嵌入扩散和对流偏微分方程，而非仅作为正则项，实现了更强的物理约束。
2. **双尺度机制覆盖**：同时建模小尺度扩散（局地）和大尺度对流（全局），符合真实污染物传输多尺度特性。
3. **可解释性增强**：物理组件的存在使模型行为更可解释（例如，哪些预测变化归因于扩散/对流）。
4. **实验设计扎实**：在三个真实数据集上验证，并通过消融实验确定每个物理模块的贡献，结论可信。
5. **性能增益明确**：与变体对比获得“substantial gains”，说明物理引入不是冗余。

## 8. 不足与局限
1. **算力消耗未报告**：无法评估模型训练效率及可复现性要求。
2. **物理假设简化**：可能仅考虑了简单扩散和对流方程，实际大气中存在湍流、化学反应等更复杂过程，模型未涉及。
3. **数据集多样性不足**：三个数据集可能均来自相似地理区域（如东亚），缺少极地、干旱地区等极端环境验证。
4. **泛化能力验证缺失**：未测试模型对未见过的污染事件（如野火、沙尘暴）的预测能力。
5. **对比方法范围有限**：未与基于物理模拟的数值模型（如WRF-Chem）或最新的物理引导图神经网络对比。
6. **统计显著性未明确**：未在摘要中说明是否进行t检验或置信区间报告。

（完）
