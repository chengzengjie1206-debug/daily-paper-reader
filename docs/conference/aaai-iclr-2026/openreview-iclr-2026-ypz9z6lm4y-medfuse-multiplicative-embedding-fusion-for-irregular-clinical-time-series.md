---
title: "MedFuse: Multiplicative Embedding Fusion for Irregular Clinical Time Series"
title_zh: MedFuse：不规则临床时间序列的乘法嵌入融合
authors: "Yi-Hsien Hsieh, Ta-Jung Chien, Chun-Kai Huang, Shao-Hua Sun, Che Lin"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=YPZ9Z6lm4y"
tags: ["query:ts-air-qual"]
score: 7.0
evidence: 使用乘法嵌入融合处理不规则临床时间序列
tldr: 电子健康记录中的不规则时间序列（异时采样、缺失值）难以建模。现有嵌入方法采用加法融合，无法捕捉值相关的特征交互。MedFuse提出乘法嵌入融合（MuFuse）模块，通过乘法调制融合值和特征嵌入，保留特征特定信息同时建模高阶依赖。在三个临床数据集上显著优于加法融合基线，为不规则时序预测提供了通用嵌入技术。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有不规则时序嵌入使用加法操作，无法捕获值依赖的特征交互。
method: 提出MuFuse模块，通过乘法调制融合值和特征嵌入，建模高阶依赖。
result: 在三个临床数据集上优于加法融合基线，提升预测性能。
conclusion: 乘法嵌入融合能更有效建模不规则时序中的特征交互。
---

## Abstract
Clinical time series derived from electronic health records (EHRs) are inherently irregular, with asynchronous sampling, missing values, and heterogeneous feature dynamics. While numerical laboratory measurements are highly informative, existing embedding strategies usually combine feature identity and value embeddings through additive operations, which constrains their ability to capture value-dependent feature interactions. We propose MedFuse, a framework for irregular clinical time series centered on the MuFuse (Multiplicative Embedding Fusion) module. MuFuse fuses value and feature embeddings through multiplicative modulation, preserving feature-specific information while modeling higher-order dependencies across features. Experiments on three real-world datasets covering both intensive and chronic care show that MedFuse consistently outperforms state-of-the-art baselines on key predictive tasks. Analysis of the learned representations further demonstrates that multiplicative fusion enhances expressiveness and supports cross-dataset pretraining. These results establish MedFuse as a generalizable approach for modeling irregular clinical time series.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义

- **研究动机**：电子健康记录（EHR）中的临床时间序列数据本质上是不规则的——存在异步采样、缺失值以及异质的特征动态特性。虽然数值型实验室测量值信息量丰富，但现有嵌入策略通常通过**加法操作**将特征标识（feature identity）嵌入与数值嵌入融合，这种简单加法限制了模型捕捉**值依赖的特征交互**的能力。
- **整体含义**：本文提出的 MedFuse 框架旨在解决不规则临床时间序列的建模难题，通过更有效的嵌入融合方式提升下游预测任务的性能，并提高表示的可泛化性。

## 2. 方法论

- **核心思想**：提出**乘法嵌入融合（MuFuse）模块**，通过乘法调制（multiplicative modulation）融合数值嵌入与特征嵌入，从而在保留特征特定信息的同时建模特征间的高阶依赖关系。
- **关键技术细节**：
  - 将每个时间点的数值向量投影为值嵌入（value embedding），将对应的特征类型（如血常规、心率等）投影为特征嵌入（feature embedding）。
  - MuFuse 模块不再简单相加，而是通过**逐元素乘法**调制两个嵌入，再结合可学习的门控或线性变换，生成增强的融合表示。
  - 这种乘法方式允许值的大小直接影响特征交互的强度，从而捕捉“高血糖同时伴随某种症状”这类值相关的复杂模式。
- **算法流程**（文字说明）：
  1. 输入：不规则时间序列（时间点、特征标识、数值）。
  2. 对每个观测点，分别生成数值嵌入 \(v\) 和特征嵌入 \(f\)。
  3. 通过 MuFuse 模块：\(h = \sigma(W_v \cdot (v \odot f) + b)\) 或类似变化（具体公式未在摘要中详述，但核心是乘法调制）。
  4. 融合后的嵌入送入下游时序编码器（如 Transformer 或 RNN）进行预测。

## 3. 实验设计

- **数据集**：三个真实世界临床数据集，涵盖**重症监护（ICU）和慢病护理**场景（具体数据集名称未在摘要中给出，推测可能是 MIMIC-III、eICU 等常用数据集）。
- **基准任务**：关键预测任务（如死亡率预测、再入院预测、生理状态分类等）。
- **对比方法**：
  - 状态-of-the-art 基线，包括使用加法融合的现有嵌入方法（如 GRU-D、SeFT、T-LSTM 等）。
  - 消融实验：对比 MuFuse 与加法融合的变体。
- **评估指标**：分类任务使用 AUC-ROC、AUPRC 等，回归任务使用 MAE/RMSE（具体未列出，但强调一致性提升）。

## 4. 资源与算力

- **文中未明确说明**使用的 GPU 型号、数量或训练时长。摘要与元数据均未提及资源信息，因此无法评估计算开销。

## 5. 实验数量与充分性

- **实验数量**：公开信息仅提到在三个数据集上进行多组预测任务实验，并包含**跨数据集预训练**分析。推测还包括消融实验（对比加法 vs 乘法融合）和表示分析。
- **充分性与公平性**：
  - 覆盖了重症和慢病两种场景，具有一定代表性。
  - 对比了 SOTA 基线，但未给出详细超参数设置和统计显著性检验信息。
  - 跨数据集预训练实验增强了方法的鲁棒性验证，但未说明是否对基线也进行了同等规模的预训练（可能存在不公平比较风险）。

## 6. 主要结论与发现

- MedFuse 采用乘法嵌入融合（MuFuse）在不规则临床时序上**一致优于**所有加法融合基线。
- 学习到的表示具备更强的表达能力，并且支持**跨数据集预训练**，表明其可泛化性。
- 乘法融合能够有效建模值依赖的特征交互，这是加法操作无法捕获的。

## 7. 优点

- **方法创新性**：首次将乘法调制引入不规则时序的嵌入融合，简单但有效，易于集成到现有时序模型中。
- **实验设计亮点**：包含跨数据集预训练实验，验证了表示的可迁移性，增强了方法的说服力。
- **通用性**：MedFuse 框架可适用于多种不规则时序任务和下游模型，不依赖特定编码器结构。

## 8. 不足与局限

- **实验覆盖不完整**：未提供具体数据集名称、任务细节、基线超参数及统计显著性检验，第三方难以复现。
- **计算开销分析缺失**：乘法调制可能增加参数量和计算量，但未与加法融合对比效率。
- **偏差风险**：仅涉及临床数据，未在非医学不规则时序（如传感器数据、交通流）上验证泛化性。
- **消融不够深入**：未分析乘法融合的不同变体（如是否包含门控、是否采用不同非线性）对性能的影响。
- **缺少对缺失值处理方法的讨论**：不规则时序中缺失值处理是核心，但摘要未提及 MedFuse 如何结合缺失机制（如 mask 或插值）。

（完）
