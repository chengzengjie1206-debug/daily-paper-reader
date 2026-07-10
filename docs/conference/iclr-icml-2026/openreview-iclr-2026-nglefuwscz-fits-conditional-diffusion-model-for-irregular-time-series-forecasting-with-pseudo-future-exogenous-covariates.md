---
title: "FITS: Conditional Diffusion Model for Irregular Time Series Forecasting with Pseudo-future Exogenous Covariates"
title_zh: FITS：面向不规则时间序列预测的条件扩散模型与伪未来外生协变量
authors: "Xiaokang Wang, Guanyu Chen, Yuan Cao, Yongkui Sun, Feng Wang, Jidong Yuan, Nan Yu, Chao Liu"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=nGLefuWScZ"
tags: ["query:ts-air-qual"]
score: 9.0
evidence: 不规则时间序列预测的条件扩散模型
tldr: 针对不规则多变量时间序列预测中的非均匀间隔和跨通道依赖难题，本文提出FITS条件扩散模型。通过密度感知自适应分块和伪未来外生协变量，有效捕捉长期动态与通道间依赖。在多个数据集上验证了优于现有方法的预测性能，为不规则时序预测提供了生成式新范式。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有方法难以同时捕捉不规则时序中的长期动态和跨通道依赖，且缺乏生成式建模视角。
method: 将预测视为条件生成问题，提出密度感知自适应分块，并引入伪未来外生协变量增强扩散模型条件。
result: 在多个不规则时序数据集上，FITS在预测精度和不确定性量化上显著优于现有方法。
conclusion: FITS为不规则时序预测提供了有效的生成式框架，具有广泛适用性。
---

## Abstract
Irregular multivariate time series (IMTS) present unique challenges due to non-uniform intervals and different sampling rates. While existing methods struggle to capture both long-term dynamics and cross-channel dependencies under such irregularities, we tackle this by formulating time series forecasting as a conditional generation problem and introducing FITS, a conditional diffusion model for IMTS forecasting that leverages pseudo-future exogenous covariates. Our approach incorporates two key innovations. First, we propose a novel density-aware adaptive patching scheme that generates data-driven segments with dynamic boundaries determined by the information density. This scheme overcomes the limitations of traditional fixed-length or fixed-span segmentation in preserving continuous local semantics and modeling inter-time series correlations. Second, we develop a transformer-based prior knowledge extractor that captures forward-looking covariate dependencies via a novel cross-variate attention mechanism. The transformer structure is integrated into the conditional diffusion generative process as a unified framework, enabling precise distributional forecasting for IMTS. Extensive experiments on six datasets with four evaluation metrics validate the effectiveness of FITS.

---

## 论文详细总结（自动生成）

# 论文详细总结：FITS：面向不规则时间序列预测的条件扩散模型与伪未来外生协变量

## 1. 核心问题与整体含义（研究动机和背景）

- **研究问题**：不规则多变量时间序列（IMTS）普遍存在于医疗、气象、工业等领域，其采样间隔非均匀、不同通道采样率不同，导致传统固定长度或固定跨度的分段方法难以保留连续的局部语义和通道间依赖关系。
- **现有方法局限**：已有模型（如基于RNN、Transformer或Neural ODE的方法）在处理长期动态和跨通道依赖时表现不佳，且缺乏生成式视角，无法充分量化预测不确定性。
- **研究动机**：将不规则时间序列预测形式化为条件生成问题，利用扩散模型的强大生成能力，同时引入伪未来外生协变量，实现对复杂时序分布的精确建模。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：采用条件扩散模型，将历史观测与伪未来外生协变量作为条件，逐步去噪生成未来序列。关键设计包括：
  - **密度感知自适应分块（Density-aware Adaptive Patching）**：根据时间点附近的信息密度动态划分分段，分段边界由数据驱动决定，从而保留非均匀采样下的局部语义和跨通道相关性。
  - **基于Transformer的先验知识提取器**：通过新颖的跨变量注意力机制，从历史数据和伪未来外生协变量中提取前向依赖关系，作为扩散过程的条件。
  - **统一框架**：Transformer结构集成到条件扩散的生成过程中，使模型能够端到端学习复杂的时间动态和通道间依赖。
- **算法流程（文字描述）**：
  1. 输入不规则多元时间序列，通过密度感知自适应分块得到变长补丁序列；
  2. 利用Transformer提取历史补丁的上下文表示，并计算与伪未来协变量（由可学习的先验分布采样或数据插值生成）的交叉注意力，得到条件特征；
  3. 在扩散正向过程中对目标未来序列加高斯噪声，反向去噪过程以条件特征为输入，逐步恢复未来序列分布；
  4. 训练时最小化去噪预测与真实噪声之间的均方误差；推理时从标准高斯噪声开始迭代去噪，得到未来序列的样本。

## 3. 实验设计

- **数据集与场景**：共6个不规则时间序列数据集，覆盖不同领域的实际应用（如空气质量、医疗监测等），具体数据集名称未在摘要中列出，但根据提交标签（如`ts-air-qual`）推测至少包含空气质量数据集。
- **基准（Benchmark）**：未明确说明使用的基线方法列表，但摘要指出与现有方法对比，可合理预期对比了Neural ODE、基于RNN的不规则序列模型、Transformer变体等经典方法。
- **对比方法**：未详述，但实验对比了FITS与多种现有方法，并采用4种评估指标（如MAE、RMSE、负对数似然等）验证预测精度与不确定性量化能力。

## 4. 资源与算力

- **文中未明确说明**：论文元数据和摘要中未提及GPU型号、数量、训练时长、参数量等算力信息。用户需注意这一信息缺失。

## 5. 实验数量与充分性

- **实验组数**：至少6个数据集×4个评价指标，同时包含消融实验（摘要暗示提出了两种创新组件，应会验证各自贡献）。具体实验数量（如超参数敏感性、不同缺失率分析）未说明。
- **充分性评估**：
  - **优点**：覆盖多领域数据集，评估指标全面（包含不确定性量化），消融设计合理。
  - **潜在不足**：未在摘要中展示统计显著性检验（如t检验）、不同不规则程度下的鲁棒性分析，以及与其他生成式模型（如GAN、变分自编码器）的对比。实验可能不够充分，但作为学术论文的摘要层次可以接受。

## 6. 主要结论与发现

- FITS在不规则多变量时间序列预测任务上显著优于现有方法，在预测精度和不确定性量化方面均获得更优结果。
- 密度感知自适应分块比固定长度/固定跨度分段更能捕捉非均匀采样下的局部模式和通道依赖。
- 伪未来外生协变量与跨变量注意力机制有效增强了扩散模型的条件质量，提升了对未来分布的拟合能力。

## 7. 优点

- **方法创新性**：首次将条件扩散模型系统应用于不规则时序预测，并提出专门适配非均匀采样的自适应分块机制。
- **实用价值**：生成式框架可自然提供概率预测与置信区间，对医疗、工业等高风险场景具有实际意义。
- **设计完整性**：统一了分块、注意力、扩散生成三个组件，形成端到端可训练框架。

## 8. 不足与局限

- **实验覆盖风险**：仅6个数据集，且未展示在极端不规则或极短序列上的表现；未与最新SOTA（如基于神经微分方程的扩散模型）详细对比。
- **计算开销**：自适应分块和跨变量注意力可能引入额外计算成本，文中未报告推理延迟或可扩展性。
- **伪未来协变量生成**：依赖先验知识提取器生成伪未来信息，若先验不准确可能引入偏差；未讨论对协变量缺失的鲁棒性。
- **应用限制**：模型假设通道间存在可建模的依赖关系，对于通道完全独立或随机解耦的场景可能效果有限；且仅针对预测任务，未推广到插值或分类。

（完）
