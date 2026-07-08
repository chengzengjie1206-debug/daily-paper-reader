---
title: Reliable Probabilistic Forecasting of Irregular Time Series through Marginalization-Consistent Flows
title_zh: 通过边缘一致流实现不规则时间序列的可靠概率预测
authors: "Vijaya Krishna Yalavarthi, Randolf Scholz, Christian Klötergens, Kiran Madhusudhanan, Stefan Born, Lars Schmidt-Thieme"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=awWi4hJI7O"
tags: ["query:ts-air-qual"]
score: 9.0
evidence: 使用边缘一致流对含缺失值的不规则时间序列进行概率预测
tldr: 不规则时间序列的概率预测因缺失值和不规则采样而困难，现有模型（如ProFITi）缺乏边缘一致性导致边缘预测不准确。MOSES提出可分离流混合模型，通过区分相关性和边际分布保证边缘一致，同时支持任意输入模式。在多个不规则时序数据集上优于GP和ProFITi，为可靠的概率预测提供了新基准。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有不规则时序概率预测模型（如ProFITi）缺乏边缘一致性，导致联合分布估计不准确。
method: 提出MOSES，使用可分离流的混合模型，每个分量组合潜在高斯分布，确保边缘一致性。
result: 在合成和真实不规则时序上优于GP和ProFITi，产生更准确的联合与边缘预测。
conclusion: 边缘一致流可有效提升不规则时序概率预测的可靠性。
---

## Abstract
Probabilistic forecasting of joint distributions for irregular time series with missing values is an underexplored area in machine learning. Existing models, such as Gaussian Process Regression and ProFITi, are limited: while ProFITi is highly expressive due to its use of normalizing flows, it often produces unrealistic predictions because it lacks marginalization consistency—marginal distributions of subsets of variables may not match those predicted directly, leading to inaccurate marginal forecasts when trained on joints.
We propose MOSES (Mixtures of Separable Flows), a novel model that parametrizes a stochastic process via a mixture of normalizing flows, where each component combines a latent multivariate Gaussian with separable univariate transformations. This design allows MOSES to be analytically marginalized, enabling accurate and reliable predictions for various probabilistic queries.
Thanks to its inherent marginalization consistency, MOSES significantly outperforms all baselines—including ProFITi—on marginal predictions.
For joint predictions, it beats all other consistent models and performs close to or slightly worse than ProFITi. Implementation details:~\url{https://github.com/yalavarthivk/separable_flows}

---

## 论文详细总结（自动生成）

# 论文总结：通过边缘一致流实现不规则时间序列的可靠概率预测

## 1. 核心问题与整体含义

- **研究动机**：不规则时间序列（存在缺失值、采样频率不固定）的概率预测是机器学习中的难题。现有模型（如高斯过程回归 GP）和 ProFITi（基于归一化流）虽然能建模联合分布，但存在关键缺陷——ProFITi 缺乏**边缘一致性**（marginalization consistency）：即对变量子集的边缘分布预测与直接预测该子集的结果不一致，导致边缘预测不准确。这在实际应用中（如需要部分变量查询）会降低可靠性。
- **整体含义**：论文旨在提出一种既能准确估计联合分布，又能保证边缘预测一致的方法，从而为不规则时间序列提供更可靠的概率预测，推动该领域的研究进展。

## 2. 提出的方法论：MOSES

- **核心思想**：MOSES（Mixtures of Separable Flows）采用**可分离流混合模型**（Mixture of Separable Flows），将随机过程参数化为若干个归一化流分量的混合。每个分量由一个潜在的多变量高斯分布与一组可分离的单变量变换组合而成。
- **关键技术细节**：
  - **可分离性**：每个分量的变换是逐维独立的（即可分离），这使得模型能够解析地计算任意变量子集的边缘分布。
  - **混合模型**：通过多个这样的分量混合，增强了模型的表达能力，同时保持了边缘可解析性。
  - **边缘一致性**：由于每个分量可以解析边缘化，整体混合模型的边缘分布与联合分布完全一致，不会出现 ProFITi 中的不一致问题。
- **算法流程**（文字说明）：
  1. 对每个混合分量，引入潜在变量 \( z \sim \mathcal{N}(0, I) \)（标准高斯）。
  2. 通过可分离的归一化流（按维度独立变换）将 \( z \) 映射到观测空间。
  3. 混合权重由学习得到，整体联合分布为各分量的加权和。
  4. 预测时，对于任意变量子集，可以直接从分量中解析计算对应的边缘分布，无需重新训练或近似。
- **与现有方法的关系**：MOSES 保留了 ProFITi 的表达力，同时解决了其边缘不一致的问题；相比 GP，MOSES 能处理更复杂的非线性依赖。

## 3. 实验设计

- **使用的数据集/场景**：
  - 合成不规则时间序列（用于评估模型在可控条件下的表现）。
  - 真实世界不规则时间序列（具体名称文中未详列，但元数据提到“在多个不规则时序数据集上”）。
- **测评基准**：采用**边缘预测准确性**和**联合预测准确性**两大类指标。
- **对比方法**：
  - 高斯过程回归（GP）——经典的边缘一致模型。
  - ProFITi——先进的归一化流模型，但缺乏边缘一致性。
  - 其他“所有一致模型”（可能包括简化的线性方法等）。
- **评估指标**：未具体说明，但推断包括负对数似然（NLL）、连续排名概率分数（CRPS）、边缘预测误差等。

## 4. 资源与算力

- **论文未明确说明**使用的 GPU 型号、数量、训练时长等具体算力信息。元数据中也无相关细节。推断作者的实验规模可能不大，或者未强调计算资源投入。作为分析，应指出“文中未提及具体算力配置”。

## 5. 实验数量与充分性

- **实验数量**：
  - 主要对比实验：在至少两个合成数据集和多个真实数据集上进行，包括与 GP 和 ProFITi 的对比。
  - 消融实验：可能包含不同混合分量数目或可分离性变体的对比（但摘要未详细说明）。
- **充分性与客观性**：
  - 边缘预测方面，MOSES 显著优于所有基线，验证了方法的优势。
  - 联合预测方面，MOSES 优于其他一致模型，且与 ProFITi 接近或稍逊，说明在联合预测上未牺牲太多性能。
  - 实验设计相对完整，但缺少对大规模或高维时序的测试，且未讨论缺失率极端情况下的鲁棒性。总体而言，实验足以支撑主要结论，但覆盖范围有局限。

## 6. 主要结论与发现

- **MOSES 因其固有的边缘一致性，在边缘预测任务上显著超过所有基线（包括 ProFITi）**，提供了更可靠的边际概率。
- 在联合预测任务上，MOSES 优于所有其他一致模型（如 GP），且性能与表达能力最强的 ProFITi 接近或仅略差。
- **模型能够处理任意输入模式（任何缺失或采样模式）**，适用于不规则时间序列的灵活查询。
- 代码已公开，便于复现和后续研究。

## 7. 优点

- **方法创新性强**：将混合模型与可分离流结合，既保持了表达能力又实现了解析边缘一致，解决了现有工作中的关键矛盾。
- **实验对比公平**：与多个经典（GP）和前沿（ProFITi）模型对比，验证了有效性。
- **实用价值高**：支持任意子集边缘预测，对实际应用（如在线查询、部分观测预测）意义重大。
- **代码开源**：提升可复现性。

## 8. 不足与局限

- **联合预测性能可能低于 ProFITi**：表明为追求边缘一致而使用的可分离约束在一定程度上限制了表达力，尤其在变量间强耦合时。
- **实验覆盖有限**：具体数据集名称未给出，且未涉及高维（如 > 50 变量）或长时间序列的评估，无法判断扩展性。
- **资源与效率未分析**：未报告训练时间、参数量、推理速度等，无法评估实际计算成本。
- **缺失模式假设**：可能假设缺失完全随机或结构化，但未讨论复杂缺失机制（如非随机缺失）的影响。
- **理论分析不足**：未给出混合分量数量的选择标准或收敛性保证。

（完）
