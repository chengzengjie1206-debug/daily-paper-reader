---
title: Learning Recursive Multi-Scale Representations for Irregular Multivariate Time Series Forecasting
title_zh: 学习递归多尺度表示用于不规则多变量时间序列预测
authors: "Boyuan Li, Zhen Liu, Yicheng Luo, Qianli Ma"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=JEIDxiTWzB"
tags: ["query:ts-air-qual"]
score: 9.0
evidence: 递归多尺度不规则多变量时间序列预测
tldr: 不规则多变量时间序列在多时间尺度上存在依赖，但现有重采样方法破坏原始采样模式。本文提出ReIMTS，通过递归保持时间戳不变，逐级拆分采样间隔以构建多尺度表示。在多个真实数据集上，ReIMTS在预测误差上显著低于重采样基线，且更好地保留了不规则信息。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有不规则时序多尺度方法采用重采样，破坏原始时间戳和采样模式信息。
method: 提出递归多尺度建模，不重采样而直接拆分采样间隔，逐层学习多尺度表示。
result: 在多个真实世界数据集上，ReIMTS的预测误差低于最先进的不规则时序方法。
conclusion: 保留原始时间戳的多尺度方法能更有效利用不规则时序中的信息。
---

## Abstract
Irregular Multivariate Time Series (IMTS) are characterized by uneven intervals between consecutive timestamps, which carry sampling pattern information valuable and informative for learning temporal and variable dependencies.
In addition, IMTS often exhibit diverse dependencies across multiple time scales.
However, many existing multi-scale IMTS methods use resampling to obtain the coarse series, which can alter the original timestamps and disrupt the sampling pattern information.
To address the challenge, we propose ReIMTS, a **Re**cursive multi-scale modeling approach for **I**rregular **M**ultivariate **T**ime **S**eries forecasting.
Instead of resampling, ReIMTS keeps timestamps unchanged and recursively splits each sample into subsamples with progressively shorter time periods.
Based on the original sampling timestamps in these long-to-short subsamples, an irregularity-aware representation fusion mechanism is proposed to capture global-to-local dependencies for accurate forecasting.
Extensive experiments demonstrate an average performance improvement of 27.1\% in the forecasting task across different models and real-world datasets.
Our code is available at [https://github.com/Ladbaby/PyOmniTS](https://github.com/Ladbaby/PyOmniTS).

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机与背景）

- **问题**：不规则多变量时间序列（IMTS）具有不均匀的时间间隔，这些间隔携带了有价值的采样模式信息，且IMTS往往在不同时间尺度上表现出多样的依赖关系。现有的大多数多尺度IMTS方法使用**重采样**来获得粗粒度序列，但重采样会改变原始时间戳，破坏采样模式信息，从而影响预测性能。
- **动机**：提出一种**不依赖重采样**的多尺度建模方法，保留原始时间戳，充分利用不规则时序中的全局-局部依赖，以提升预测精度。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：递归多尺度建模。保持时间戳不变，递归地将每个样本拆分为具有**逐渐缩短时间跨度**的子样本（即逐级划分采样间隔），从而自然构建从全局到局部的多尺度表示。
- **关键技术**：
  - **不规则感知表示融合机制**：基于原始采样时间戳，在长到短的子样本上融合多尺度特征，捕获全局到局部的依赖关系。
  - **递归拆分**：不改变原始时间戳，仅按时间跨度划分，避免了重采样带来的信息损失。
- **算法流程**（文字描述）：
  1. 输入原始不规则多变量时间序列，保持所有时间戳不变。
  2. 递归地将每个采样间隔拆分为更短的子间隔，生成一组时间跨度递减的子样本。
  3. 对每一级子样本，利用原始时间戳计算不规则感知表示。
  4. 通过融合机制聚合从全局（长时间跨度）到局部（短时间跨度）的表示，用于最终预测。

## 3. 实验设计

- **数据集与场景**：使用了多个真实世界数据集（具体名称未在摘要中列出，但元数据提到“多个真实世界数据集”），任务为**不规则多变量时间序列预测**。
- **基准方法**：对比了最先进的**不规则时序方法**，以及通过重采样获得的多尺度基线方法。
- **性能指标**：预测误差（未具体说明指标，如MAE/RMSE等），平均性能提升**27.1%**（跨不同模型和数据集）。

## 4. 资源与算力

- 论文元数据和摘要中**未明确说明**训练所使用的GPU型号、数量或训练时长，也未提及算力资源消耗信息。因此无法总结具体算力。

## 5. 实验数量与充分性

- **实验数量**：未明确列出具体实验组数，但从“不同模型和数据集”以及“平均27.1%提升”看，至少包含了多个数据集和多个基线方法的对比实验。
- **充分性与公平性**：作者声称性能提升是跨不同模型和数据集平均得出的，表明进行了较全面的比较。但未提供消融实验细节或统计显著性检验，也未说明是否进行了超参数调优的公平对齐。总体而言，实验覆盖了多种场景，但公开细节有限，可认为**较为充分但不够完全公开**。

## 6. 主要结论与发现

- 保留原始时间戳的递归多尺度方法（ReIMTS）能**更有效地利用不规则时序中的采样模式信息**，获得比重采样方法显著更低的预测误差。
- 在多种现实数据集上，ReIMTS平均性能提升27.1%，证明了方法的有效性和通用性。

## 7. 优点

- **方法创新性强**：首次提出递归拆分而非重采样，彻底避免了采样模式信息破坏。
- **保留原始时间戳**：更好地保留了不规则时序的固有信息，符合实际问题物理意义。
- **不规则感知融合机制**：有效捕获多尺度依赖，提升预测精度。
- **性能提升显著**：平均27.1%的相对改进，展示出显著优势。

## 8. 不足与局限

- **实验细节不透明**：未列出具体数据集名称、基线方法列表、指标定义，不利于重复和深入分析。
- **资源信息缺失**：未提供代码运行环境、算力需求，可能影响可复现性和实践部署评估。
- **可能存在的偏差风险**：仅报告了平均提升，未给出方差或最差情况表现，可能存在选择性报告。
- **应用限制**：递归拆分可能增加计算复杂度，尤其对于长时间序列或高采样频率数据；未讨论该方法在极稀疏或不规则程度极端场景下的鲁棒性。
- **消融实验缺失**：未验证各组件（递归拆分 vs. 融合机制）的独立贡献。

（完）
