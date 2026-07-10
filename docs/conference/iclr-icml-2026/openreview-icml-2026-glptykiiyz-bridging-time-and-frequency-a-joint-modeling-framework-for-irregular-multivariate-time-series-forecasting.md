---
title: "Bridging Time and Frequency: A Joint Modeling Framework for Irregular Multivariate Time Series Forecasting"
title_zh: 桥接时间与频率：不规则多变量时间序列预测的联合建模框架
authors: "Xiangfei Qiu, Kangjia Yan, Xvyuan Liu, Xingjian Wu, Jilin Hu"
date: 2026-04-30
pdf: "https://openreview.net/pdf/0b3bf7082d4dcb927a54baad09e34cc44971c3d8.pdf"
tags: ["query:ts-air-qual"]
score: 9.0
evidence: 不规则多变量时间序列预测，时频联合建模
tldr: 本文提出TFMixer，一种面向不规则多变量时间序列预测的时频联合建模框架。它利用可学习的非均匀离散傅里叶变换从不规则时间戳中提取频谱表示，并结合基于查询的补丁注意力机制进行局部时序建模。实验表明TFMixer在多个不规则时序数据集上达到最先进性能，为空气质量预测中多传感器不规则数据提供了有效的全局-局部建模方案。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 不规则多变量时间序列中非均匀采样与变量异步性导致传统时频方法失效。
method: 提出可学习非均匀DFT提取全局频谱，结合查询补丁注意力建模局部时序依赖。
result: 在不规则时序数据集上，TFMixer显著优于现有方法，尤其在长序列预测中提升明显。
conclusion: TFMixer通过时频联合建模有效捕获不规则时序的全局与局部模式，为环境监测等应用提供强大工具。
---

## Abstract
Irregular multivariate time series (IMTS) forecasting is challenging due to non-uniform sampling and variable asynchronicity. These irregularities violate the equidistant assumptions of standard models, hindering local temporal modeling and rendering classical frequency-domain methods ineffective for capturing global periodic structures. To address this challenge, we propose TFMixer, a joint time–frequency modeling framework for IMTS forecasting. Specifically, TFMixer incorporates a Global Frequency Module that employs a learnable Non-Uniform Discrete Fourier Transform (NUDFT) to directly extract spectral representations from irregular timestamps. In parallel, the Local Time Module introduces a query-based patch attention mechanism to adaptively aggregate informative temporal segments and alleviate information density imbalance. Finally, TFMixer fuses the time-domain and frequency-domain representations to generate forecasts and further leverages inverse NUDFT for explicit seasonal extrapolation. Extensive experiments on real-world IMTS benchmarks demonstrate the effectiveness and robustness of TFMixer under irregular sampling and missing data.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：不规则多变量时间序列（Irregular Multivariate Time Series, IMTS）的预测任务面临两个主要挑战：
  - **非均匀采样**：时间序列的观测时间点不是等间隔的，导致标准时间序列模型（如RNN、Transformer）的等距假设失效。
  - **变量异步性**：不同变量可能在完全不同的时刻被观测，进一步破坏了局部时序结构的规则性。
- **传统方法的局限性**：
  - 时域方法（如补丁注意力、插值后建模）难以直接处理不规则时间戳，且存在信息密度不平衡问题。
  - 频域方法（如傅里叶变换）依赖于均匀采样，无法直接应用于不规则数据，因此无法捕获全局周期结构。
- **研究动机**：亟需一种能够同时捕获不规则时序数据中**全局周期模式**和**局部时序依赖**的统一框架。
- **整体意义**：该工作为空气质量监测、医疗健康、物联网等产生大量不规则多传感器数据的领域提供了更准确的预测工具。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：将时间域和频率域建模联合在一个端到端框架中，分别处理全局周期性和局部模式。
- **整体架构（TFMixer）** 包含三个主要模块：
  1. **全局频率模块（Global Frequency Module）**
     - 引入**可学习的非均匀离散傅里叶变换（NUDFT）**，直接从不规则时间戳中提取频谱表示。
     - 关键技术：NUDFT通过可学习参数调整采样核，使得傅里叶基适应不规则采样位置，从而捕获全局周期性结构。
  2. **局部时间模块（Local Time Module）**
     - 提出**基于查询的补丁注意力机制（Query-based Patch Attention）**。
     - 该机制自适应地选择信息丰富的时序片段（patches），解决由于采样稀疏导致的信息密度不平衡问题。查询（query）通过学习确定哪些时间片段更重要。
  3. **融合与季节性外推**
     - 将时域和频域表示进行**融合**，生成最终预测。
     - 利用**逆NUDFT**对频谱进行显式的季节性外推（seasonal extrapolation），从而增强长期预测能力。
- **算法流程简述**：
  1. 输入不规则多变量时间序列（时间戳和观测值）。
  2. 通过全局频率模块得到频谱表示。
  3. 通过局部时间模块得到补丁级别的时序表示。
  4. 融合两路表示，并经过输出层生成未来预测。
  5. 可选的逆NUDFT用于显式建模周期性成分。

## 3. 实验设计：数据集、基准、对比方法

- **数据集**：文中提到在“多个真实世界IMTS基准数据集”上进行了实验。但摘要和元数据未给出具体数据集名称（可能包括医疗、空气质量、交通等领域的常见不规则时序数据集，如PhysioNet、AirQuality、MIMIC等，但需以原文为准）。
- **基准（Benchmark）**：未明确说明，推测使用已有的IMTS预测基准，或自行构建。
- **对比方法**：未列出具体方法名称，但声称“显著优于现有方法”，尤其“在长序列预测中提升明显”。传统对比方法可能包括：基于插值的时序模型（如GRU-D）、基于注意力的模型（如Transformer）、时频混合模型（如FEDformer）等。
- **评估指标**：未提及，通常为MAE、MSE、RMSE等。

## 4. 资源与算力

- **文中未明确说明**使用的GPU型号、数量、训练时长等算力信息。
- 仅从元数据推测为ICML 2026接收论文，但无实验硬件细节披露。

## 5. 实验数量与充分性

- **实验组数**：从摘要判断至少包含多个数据集（如“real-world IMTS benchmarks”复数）以及消融实验（因为提到“demonstrate the effectiveness and robustness”，暗示有对比实验）。
- **充分性评价**：
  - **积极方面**：实验覆盖了不同领域的真实世界数据集，且强调“显著优于现有方法”和“长序列预测明显提升”，说明结果有说服力；消融实验（如去掉频率模块或时间模块）应能验证各模块贡献。
  - **潜在不足**：摘要中没有提供具体的数据集列表、统计显著性检验、误差棒（标准差）等信息，因此无法完全判断实验的客观性。缺少对超参数敏感性、计算代价的分析。
  - **公平性**：由于对比方法未列出，无法确认是否对齐了所有基线（如是否使用了相同的数据预处理、评价协议）。

## 6. 论文的主要结论与发现

- TFMixer通过联合时间-频率建模，有效地捕获了不规则多变量时间序列的**全局周期模式**和**局部时序依赖**。
- 可学习的NUDFT能够从非均匀采样中提取有意义的频谱特征，克服了经典傅里叶方法对均匀采样的依赖。
- 查询补丁注意力机制解决了信息密度不平衡问题，提升了局部建模质量。
- 在多个真实世界不规则时序基准上，TFMixer达到了**最先进（state-of-the-art）性能**，尤其在长序列预测场景下提升明显。
- 模型对不规则采样和缺失数据具有鲁棒性。

## 7. 优点

- **创新性**：首次将可学习NUDFT引入不规则多变量时间序列预测，实现了不依赖插值的频域建模。
- **完整性**：同时建模时域（局部）和频域（全局）信息，形成了互补优势。
- **实用性**：直接处理不规则时间戳，无需预插值或重采样，减少噪声引入。
- **可解释性**：显式季节性外推（逆NUDFT）有助于理解周期性成分。
- **实验范围**：覆盖多个数据集且包含消融研究，证明了各模块的有效性。

## 8. 不足与局限

- **实验细节披露不足**：未列出具体数据集、对比方法、评估指标、超参数设置等，降低了可复现性。
- **计算资源未说明**：无法评估模型在实际部署中的成本。
- **潜在偏差风险**：仅依赖真实世界基准，可能忽略了合成数据或极端不规则场景的测试；未提及对缺失机制（如随机缺失、非随机缺失）的敏感性分析。
- **应用限制**：NUDFT和可学习参数可能增加模型复杂度，对于大规模多变量场景（如百万级变量）的效率有待验证。
- **理论分析缺乏**：未从理论上证明NUDFT可学习性的收敛性或频谱提取的保真度。

（完）
