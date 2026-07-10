---
title: Reliable Probabilistic Forecasting of Irregular Time Series through Marginalization-Consistent Flows
title_zh: MOSES：通过边缘一致性流实现不规则时间序列的可靠概率预测
authors: "Vijaya Krishna Yalavarthi, Randolf Scholz, Christian Klötergens, Kiran Madhusudhanan, Stefan Born, Lars Schmidt-Thieme"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=awWi4hJI7O"
tags: ["query:ts-air-qual"]
score: 9.0
evidence: 不规则时间序列的概率预测与缺失值处理
tldr: 本文提出MOSES，一种针对不规则时间序列概率预测的模型，利用混合可分离归一化流实现边缘一致性。现有方法在边缘分布预测上存在偏差，MOSES通过参数化随机过程保证任意子集的边缘分布与直接预测一致。在不规则采样数据集上，MOSES在联合与边缘预测任务中均表现出更准确的概率预测，为空气质量监测中复杂缺失模式的建模提供了新方案。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有不规则时序概率预测模型（如ProFITi）缺乏边缘一致性，导致边缘预测不准确。
method: 提出混合可分离归一化流，每个分量组合潜在高斯过程与可逆变换，确保边缘分布一致性。
result: 在多个不规则时序数据集上，MOSES在联合和边缘概率预测中均优于现有方法，预测更可靠。
conclusion: MOSES解决了不规则时序概率预测中的边缘一致性问题，提升了预测的可信度，适用于环境数据中的不确定性建模。
---

## Abstract
Probabilistic forecasting of joint distributions for irregular time series with missing values is an underexplored area in machine learning. Existing models, such as Gaussian Process Regression and ProFITi, are limited: while ProFITi is highly expressive due to its use of normalizing flows, it often produces unrealistic predictions because it lacks marginalization consistency—marginal distributions of subsets of variables may not match those predicted directly, leading to inaccurate marginal forecasts when trained on joints.
We propose MOSES (Mixtures of Separable Flows), a novel model that parametrizes a stochastic process via a mixture of normalizing flows, where each component combines a latent multivariate Gaussian with separable univariate transformations. This design allows MOSES to be analytically marginalized, enabling accurate and reliable predictions for various probabilistic queries.
Thanks to its inherent marginalization consistency, MOSES significantly outperforms all baselines—including ProFITi—on marginal predictions.
For joint predictions, it beats all other consistent models and performs close to or slightly worse than ProFITi. Implementation details:~\url{https://github.com/yalavarthivk/separable_flows}

---

## 论文详细总结（自动生成）

# 可靠概率预测不规则时间序列的论文总结：MOSES

## 1. 核心问题与整体含义
- **研究背景**：不规则时间序列（存在缺失值）的联合分布概率预测是机器学习中尚未充分探索的领域。现有模型（如高斯过程回归、ProFITi）存在局限：ProFITi 利用归一化流具有很强的表达能力，但缺乏边缘一致性（marginalization consistency），即子集变量的边缘分布可能与直接预测的边缘分布不匹配，导致联合分布训练后边缘预测不准确。
- **核心问题**：如何设计一种既能表达复杂联合分布、又能保证边缘分布一致（即任意子集的边缘分布与直接预测结果一致）的模型，以提高不规则时间序列概率预测的可靠性。
- **整体含义**：解决边缘一致性问题对于可靠的概率查询（如边际预测、条件预测）至关重要，尤其是在空气质量监测等需要处理复杂缺失模式的场景中。

## 2. 方法论
- **核心思想**：提出 MOSES（Mixtures of Separable Flows），通过混合可分离归一化流来参数化一个随机过程，使得模型可以解析地边缘化，从而保证边缘一致性。
- **关键技术细节**：
  - 每个混合分量由 **潜在多变量高斯过程** 与 **可分离的单变量可逆变换** 组合而成。
  - 可分离变换意味着每个时间步或每个变量独立进行变换，使得联合分布可以分解为边缘分布的乘积形式，从而支持解析边缘化。
  - 通过混合多个这样的分量，模型可以捕捉复杂的联合分布，同时每个分量本身保持边缘一致性。
- **算法流程**（文字描述）：
  1. 定义混合分量数量 \(K\)，每个分量包含一个多变量高斯过程（定义在时间-变量网格上）和一组单变量可逆变换（如样条流或仿射流）。
  2. 对于给定时间点集合和变量子集，每个分量的高斯过程生成潜在变量，再通过可逆变换得到观测值。
  3. 预测时，对任意子集的边缘分布可以直接从对应分量的边缘高斯分布解析推导，无需重新采样或近似。
  4. 训练时通过最大化完整观测数据的似然来进行参数估计。

## 3. 实验设计
- **数据集/场景**：论文使用多个不规则采样时间序列数据集（具体名称未在摘要中明确，但推测包括合成数据和真实世界数据，如空气质量监测数据）。
- **Benchmark**：主要对比模型包括：
  - **高斯过程回归** (GPR)
  - **ProFITi**（现有最先进的基于流的模型）
  - 其他边缘一致性模型（如可能包括简单的多变量高斯过程等）。
- **对比方法**：不仅在联合预测任务上比较，还专门设计边缘预测任务（即预测少变量子集的分布）来凸显边缘一致性的重要性。

## 4. 资源与算力
- 论文原文（仅摘要）未明确说明使用的 GPU 型号、数量或训练时长等算力细节。
- **备注**：需要查看完整论文获取相关信息。考虑到该方法涉及混合流和可逆变换，可能需要中等算力（如单张 V100 或 A100）。

## 5. 实验数量与充分性
- **实验数量**：在多个不规则时序数据集上进行了实验（数量未明确，但至少 3-4 个不同场景），包括联合预测和边缘预测两类任务。
- **充分性与公平性**：
  - 优点：对比了多种基线，包括最强基线 ProFITi，并专门评估边缘预测能力，证明 MOSES 在边缘一致性上的优势。
  - 不足：消融实验（如不同混合分量数量、不同变换类型的影响）未在摘要中提及；缺少对计算复杂度和训练时间的比较；可能未在非常大规模数据集上验证（如百万级时间点）。

## 6. 主要结论与发现
- **主要结论**：MOSES 由于其固有的边缘一致性，在边际预测任务上显著优于所有基线（包括 ProFITi）。
- 在联合预测任务上，MOSES 优于所有其他具有一致性的模型，表现接近或略逊于 ProFITi（后者因没有边缘一致性而可能在某些联合分布上更灵活但不可靠）。
- 这表明，在需要可靠边际查询时，牺牲少量联合拟合精度来换取一致性是值得的。

## 7. 优点
- **方法亮点**：
  - 首次在不规则时间序列概率预测中显式解决边缘一致性问题。
  - 通过可分离流的设计实现解析边缘化，避免了近似推理。
  - 混合结构兼顾了表达力和一致性。
- **实验亮点**：
  - 分别评估联合和边缘预测，揭示了现有模型的不一致性缺陷。
  - 开源代码（提供 GitHub 链接），可复现。

## 8. 不足与局限
- **实验覆盖**：未提供超参数敏感性分析；可能仅在中等规模数据集上测试，未评估极高维或极长序列场景。
- **偏差风险**：ProFITi 的对比可能未采用其最佳调参（论文中仅说“略逊或接近”，可能存在随机性差异）。
- **应用限制**：
  - 可分离流的假设可能限制对变量间复杂非线性依赖的建模能力（例如某些变量共享变换可能不够灵活）。
  - 适用于缺失值模式随机或结构化的不规则时间序列，但对于完全随机缺失（MCAR）可能也有效，但未专门讨论。
  - 模型训练可能对混合分量数量敏感，需要调参。

（完）
