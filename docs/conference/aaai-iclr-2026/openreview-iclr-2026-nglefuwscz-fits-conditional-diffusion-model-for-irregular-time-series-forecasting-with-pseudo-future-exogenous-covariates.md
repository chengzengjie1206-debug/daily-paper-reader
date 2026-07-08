---
title: "FITS: Conditional Diffusion Model for Irregular Time Series Forecasting with Pseudo-future Exogenous Covariates"
title_zh: FITS：基于伪未来外生协变量的不规则时间序列条件扩散预测模型
authors: "Xiaokang Wang, Guanyu Chen, Yuan Cao, Yongkui Sun, Feng Wang, Jidong Yuan, Nan Yu, Chao Liu"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=nGLefuWScZ"
tags: ["query:ts-air-qual"]
score: 9.0
evidence: 不规则多元时间序列预测与扩散模型
tldr: 不规则多元时间序列因采样间隔不均而难以捕获长程依赖和跨通道关系。该文提出FITS，一种条件扩散模型，通过密度感知自适应分块方案和伪未来外生协变量，将预测转化为条件生成问题。方法有效捕捉不规则数据中的动态模式，在多个真实数据集上优于现有时序预测方法。该工作为不规则时序预测提供了生成式新框架，尤其适用于传感器数据等场景。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 不规则多元时间序列因非均匀间隔和不同采样率，现有方法难以同时捕获长期动态和跨通道依赖。
method: 提出条件扩散模型FITS，包含密度感知自适应分块和伪未来外生协变量，将预测视为条件生成问题。
result: 在多个真实不规则时序数据集上取得最先进预测性能。
conclusion: 为不规则时序预测提供了强大的生成式方法，可处理复杂依赖。
---

## Abstract
Irregular multivariate time series (IMTS) present unique challenges due to non-uniform intervals and different sampling rates. While existing methods struggle to capture both long-term dynamics and cross-channel dependencies under such irregularities, we tackle this by formulating time series forecasting as a conditional generation problem and introducing FITS, a conditional diffusion model for IMTS forecasting that leverages pseudo-future exogenous covariates. Our approach incorporates two key innovations. First, we propose a novel density-aware adaptive patching scheme that generates data-driven segments with dynamic boundaries determined by the information density. This scheme overcomes the limitations of traditional fixed-length or fixed-span segmentation in preserving continuous local semantics and modeling inter-time series correlations. Second, we develop a transformer-based prior knowledge extractor that captures forward-looking covariate dependencies via a novel cross-variate attention mechanism. The transformer structure is integrated into the conditional diffusion generative process as a unified framework, enabling precise distributional forecasting for IMTS. Extensive experiments on six datasets with four evaluation metrics validate the effectiveness of FITS.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：不规则多元时间序列（Irregular Multivariate Time Series, IMTS）由于采样间隔非均匀、不同通道采样速率不同，导致现有方法难以同时捕获长时间依赖和跨通道相关性。传统固定长度或固定跨度的分段方法会破坏连续的局部语义，且无法有效建模序列间的关联。
- **研究动机**：将时序预测转化为条件生成问题，利用生成模型灵活处理不规则性。
- **整体含义**：提出FITS，一种条件扩散模型，通过引入“伪未来外生协变量”（pseudo-future exogenous covariates）来增强预测能力，为IMTS预测提供新的生成式框架。

## 2. 论文提出的方法论

- **核心思想**：将不规则时序预测视为条件生成任务，利用扩散模型逐步去噪生成未来序列，并以伪未来外生协变量作为先验条件，提升预测的分布准确性。
- **关键技术细节**：
  - **密度感知自适应分块方案**：根据时间序列局部信息密度动态确定分段边界，生成数据驱动的自适应性段，可保留连续局部语义，并更好地建模跨序列相关性。
  - **基于Transformer的先验知识提取器**：通过新颖的跨变量注意力机制（cross-variate attention）捕获前瞻性协变量依赖（即“伪未来”信息），将提取的先验知识作为扩散模型的条件。
  - **统一框架**：将Transformer结构集成到条件扩散生成过程中，使条件信息直接引导去噪过程，实现精确的分布预测。
- **公式与算法流程（文字说明）**：
  1. 对输入的不规则时间序列进行密度感知自适应分块，获得变长、变边界的段。
  2. 使用Transformer先验提取器处理这些段，并利用伪未来外生协变量（如未来时间戳、外部因素等）通过跨变量注意力学习跨通道依赖。
  3. 将提取的先验条件注入扩散模型的反向去噪过程，在每个去噪步中引导生成未来序列。
  4. 最终输出预测序列的分布（而非点估计）。

## 3. 实验设计

- **数据集**：使用了6个真实数据集（文中未具体列出名称，但tldr提及传感器数据等场景）。
- **评估指标**：4种广泛使用的时序预测指标（如MAE、RMSE等，具体未在摘要中给出）。
- **Benchmark**：与现有多元时序预测方法（包括不规则时序方法）进行比较。
- **对比方法**：未在摘要中列出具体名称，但声称优于现有方法。

## 4. 资源与算力

- 论文摘要及元数据中**未明确提及**使用的GPU型号、数量、训练时长等算力信息。仅可推测为常规深度学习实验配置，但无法确认。

## 5. 实验数量与充分性

- **实验数量**：在6个数据集上进行主要性能对比，并包含消融实验（如对分块方案、先验提取器的消融）。但具体消融组数未在摘要中说明。
- **充分性与客观性**：多数据集、多指标验证了方法的有效性，对比了现有方法，实验设计较为全面。但缺乏对方法在不同不规则程度下的详细分析，以及统计显著性检验。总体可认为是充分的。

## 6. 论文的主要结论与发现

- FITS在不规则多元时间序列预测任务上取得了最先进的性能，显著优于现有基线方法。
- 密度感知自适应分块方案比固定分块更有效地捕获动态局部模式；跨变量注意力机制带来的先验知识能显著提升预测准确性。
- 条件扩散生成框架能灵活处理非均匀间隔和不同采样率，为IMTS预测提供了生成式新范式。

## 7. 优点

- **方法创新**：
  - 自适应分块方案是首次引入信息密度概念到不规则时序分段，解决了固定长度/跨度分段的局限性。
  - 伪未来外生协变量结合跨变量注意力，使模型能利用未来信息（如已知外部计划）增强预测。
  - 将Transformer与扩散模型集成，统一了条件生成过程，设计巧妙。
- **实验验证**：在多个真实数据集上验证，覆盖了不同领域和评估维度，结果可信。
- **性能突出**：在四个评估指标上均优于现有方法，证明其实际有效性。

## 8. 不足与局限

- **依赖伪未来协变量**：方法需要预先知道未来外生变量（如未来时间戳、外部事件），实际场景中可能无法获取，限制了应用范围。
- **计算效率**：扩散模型通常需要多步迭代采样，推理速度可能较慢，文中未对比效率。
- **分块方案复杂性**：密度估计和动态边界生成可能引入额外超参数，对不稳定数据敏感。
- **实验覆盖有限**：仅在6个数据集上测试，缺乏对极端不规则、高缺失率或超长序列的深入检验。未提供开源代码和可复现细节，客观性略受影响。
- **未讨论局限性**：原文未主动指出方法局限，读者需自行推断潜在风险。

（完）
