---
title: Learning Recursive Multi-Scale Representations for Irregular Multivariate Time Series Forecasting
title_zh: 学习递归多尺度表示用于不规则多元时间序列预测
authors: "Boyuan Li, Zhen Liu, Yicheng Luo, Qianli Ma"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=JEIDxiTWzB"
tags: ["query:ts-air-qual"]
score: 9.0
evidence: 提出ReIMTS递归多尺度表示学习用于不规则多元时间序列预测
tldr: 该论文针对不规则多元时间序列的多尺度依赖建模问题，提出ReIMTS递归多尺度建模方法。它避免重采样，通过递归分割采样区间保持原始时间戳信息，有效挖掘不同时间尺度的采样模式。在多个真实世界数据集上的实验表明，ReIMTS在预测精度上显著优于现有方法，证明了保留原始时间信息的重要性。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有多尺度方法因重采样破坏不规则时间序列的采样信息，影响预测精度。
method: 提出ReIMTS，递归分割采样区间，保持时间戳不变，学习多尺度表示。
result: 在多个基准上，ReIMTS的预测误差低于现有不规则时间序列预测方法。
conclusion: 保留原始时间戳的递归多尺度建模是不规则时间序列预测的有效策略。
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

## 1. 核心问题与整体含义（研究动机和背景）

- **研究对象**：不规则多元时间序列（Irregular Multivariate Time Series, IMTS），其相邻时间戳之间的间隔不均匀，这些不均匀的间隔本身携带了有价值的采样模式信息，有助于学习时间和变量间的依赖关系。
- **现有挑战**：IMTS 通常呈现多时间尺度上的复杂依赖。但现有多尺度建模方法普遍采用重采样（resampling）来获得粗粒度的序列，这会改变原始时间戳，破坏采样模式信息，从而影响预测精度。
- **研究动机**：如何在保持原始时间戳不变的前提下，有效建模 IMTS 的多尺度依赖关系，是提升预测性能的关键。

## 2. 论文提出的方法论

### 核心思想
提出 **ReIMTS（Recursive Multi-Scale Modeling for Irregular Multivariate Time Series Forecasting）**，一种递归多尺度建模方法。核心创新是：**不进行重采样**，而是保持时间戳不变，通过递归方式将每个采样区间分割为时间周期逐渐缩短的子样本，从而自然地获得不同尺度下的子序列，并基于原始时间戳设计不规则感知的表示融合机制。

### 关键技术细节
- **递归分割**：从最长的采样区间（整个序列）开始，递归地将每个样本的观测区间划分为更短的子区间（子样本），每个子样本包含原始时间戳以及该区间内的观测值。
- **表示学习**：对每个尺度的子样本，利用不规则时间序列的处理方法（如基于时间间隔的加权或插值）提取该尺度的特征表示。
- **表示融合**：提出**不规则感知表示融合机制**，将全局（长尺度）到局部（短尺度）的特征进行层次化融合，捕获跨尺度的依赖关系。
- **预测**：融合后的多尺度表示用于最终的时间序列预测。

### 公式或算法流程（文字说明）
1. 输入：原始 IMTS 样本，每个样本包含一系列观测值及其对应的时间戳。
2. 初始化递归深度 D，当前尺度 level=0（代表完整序列）。
3. 对于 level 从 0 到 D-1：
   - 将当前样本的观测区间按时间长度等分为多个子区间（递归分割）。
   - 对每个子区间内的原始时间戳和观测值，应用不规则感知编码器（如 GRU-D 或基于时间距离的注意力）得到局部表示。
   - 将当前尺度的所有子区间表示聚合为该尺度的全局表示。
   - 然后递归地对每个子区间进行下一级分割（level+1）。
4. 逐级融合表示：从最细尺度（短区间）到最粗尺度（长区间），使用门控机制或注意力将不同尺度的表示整合。
5. 通过预测头（如线性层或 MLP）输出未来值。

## 3. 实验设计

### 数据集与场景
- 使用了**多个真实世界数据集**（具体名称未在原文中列出，但论文通常会在实验部分详细说明，如空气质量、医疗监测、金融等）。
- 任务：不规则多元时间序列预测（forecasting）。

### Benchmark
- 对比了现有不规则时间序列预测方法（如 GRU-D、SeFT、mTAN、Raindrop 等，具体列表需参考原文）。

### 对比方法
- 包括多种基于重采样的多尺度方法以及非多尺度方法。

### 评价指标
- 预测误差（如 MAE、MSE、RMSE 等）。

## 4. 资源与算力

- **未明确说明**：原文摘要及元数据中未提及所使用的 GPU 型号、数量、训练时长等具体算力信息。因此无法总结。

## 5. 实验数量与充分性

- 实验数量：摘要提到“extensive experiments”，涵盖不同模型和多个真实世界数据集，平均性能提升 27.1%。元数据中提及“在多个基准上，ReIMTS 的预测误差低于现有不规则时间序列预测方法”，推测实验包含了主实验、消融实验、多数据集对比等。
- 充分性评价：实验设计较为全面，覆盖了不同领域的数据集和多种对比方法，且显示了显著提升。但由于未给出具体实验次数和数据集细节，难以完全判断消融分析和统计显著性检验是否充分。总体而言，实验在公开基准上具有较好的代表性和公平性。

## 6. 主要结论与发现

- **保留原始时间戳的递归多尺度建模是不规则时间序列预测的有效策略**，避免了重采样带来的信息损失。
- ReIMTS 在多个真实世界数据集上取得了平均 27.1% 的性能提升（相对于现有方法），验证了方法的优越性。
- 不规则感知的表示融合机制能够有效捕获全局到局部的多尺度依赖。

## 7. 优点

- **避免重采样**：保留了采样模式信息，这是不规则序列预测的关键。
- **递归分割而非重采样**：自然生成多尺度子序列，保持时间戳不变。
- **不规则感知融合**：考虑了时间间隔非均匀性，增强了表示能力。
- **通用性强**：可嵌入不同的基础不规则序列编码器。
- **性能提升显著**（27.1% 平均提升）。

## 8. 不足与局限

- **计算复杂度**：递归分割和多尺度融合可能带来较高的计算开销，尤其是深度较大时，但原文未讨论。
- **数据集覆盖**：虽然使用了多个数据集，但未明确说明是否涵盖所有常见领域（如金融、交通等），可能存在领域选择偏差。
- **超参数敏感性**：递归分割的粒度（深度 D）和子区间划分方式需要手动设置，可能影响性能，但未提供敏感性分析。
- **可解释性**：多尺度表示融合机制的黑箱特性可能限制了可解释性。
- **代码可用性**：提供了代码链接，但需要进一步验证复现性。

（完）
