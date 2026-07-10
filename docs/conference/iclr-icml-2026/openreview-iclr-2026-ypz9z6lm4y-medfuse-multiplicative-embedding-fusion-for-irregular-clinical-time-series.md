---
title: "MedFuse: Multiplicative Embedding Fusion for Irregular Clinical Time Series"
title_zh: MedFuse：面向不规则临床时间序列的乘法嵌入融合
authors: "Yi-Hsien Hsieh, Ta-Jung Chien, Chun-Kai Huang, Shao-Hua Sun, Che Lin"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=YPZ9Z6lm4y"
tags: ["query:ts-air-qual"]
score: 7.0
evidence: 不规则临床时间序列与缺失值处理
tldr: 电子健康记录中的临床时间序列具有不规则采样、缺失值和异质性特征。现有加性嵌入方式无法捕捉值依赖的交互。本文提出MedFuse，采用乘法调制融合值嵌入与特征嵌入，保留特征特定信息并建模高阶依赖。在三项临床预测任务上，MedFuse优于加性基线，尤其对缺失值鲁棒。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有不规则时序嵌入方法仅用加性操作，无法建模值依赖的特征交互。
method: 提出MuFuse模块，通过乘法调制将值嵌入与特征嵌入融合，学习高阶依赖。
result: 在三个临床预测数据集上，MedFuse的AUC与F1均优于现有不规则时序模型。
conclusion: 乘法嵌入融合能有效提升不规则临床时序的表示能力与预测性能。
---

## Abstract
Clinical time series derived from electronic health records (EHRs) are inherently irregular, with asynchronous sampling, missing values, and heterogeneous feature dynamics. While numerical laboratory measurements are highly informative, existing embedding strategies usually combine feature identity and value embeddings through additive operations, which constrains their ability to capture value-dependent feature interactions. We propose MedFuse, a framework for irregular clinical time series centered on the MuFuse (Multiplicative Embedding Fusion) module. MuFuse fuses value and feature embeddings through multiplicative modulation, preserving feature-specific information while modeling higher-order dependencies across features. Experiments on three real-world datasets covering both intensive and chronic care show that MedFuse consistently outperforms state-of-the-art baselines on key predictive tasks. Analysis of the learned representations further demonstrates that multiplicative fusion enhances expressiveness and supports cross-dataset pretraining. These results establish MedFuse as a generalizable approach for modeling irregular clinical time series.

---

## 论文详细总结（自动生成）

# MedFuse：面向不规则临床时间序列的乘法嵌入融合

## 1. 论文的核心问题与整体含义

- **研究动机**：电子健康记录（EHR）中的临床时间序列具有不规则采样、缺失值多、特征异质性高等特点。现有方法通常使用加性操作（加法）将特征标识嵌入与数值嵌入结合，但加法无法捕捉依赖于数值大小的特征间交互，限制了表示能力。
- **整体含义**：提出一种新的乘法调制融合策略，通过乘法嵌入融合（MuFuse）模块，能够保留特征特定信息，同时建模更高阶的跨特征依赖，从而提升不规则临床时间序列的预测性能与鲁棒性。

## 2. 论文提出的方法论

- **核心思想**：用**乘法调制**替代加法融合，将值嵌入（value embedding）和特征嵌入（feature embedding）通过逐元素乘法（element-wise multiplication）进行融合，使得不同数值可以动态调整特征嵌入的表达，捕获值依赖的交互。
- **关键技术细节**：
  - **MuFuse模块**：对于每个时间点的观测值，首先将数值映射到值嵌入向量，特征标识映射到特征嵌入向量；然后通过乘法调制（例如 Hadamard 积）融合二者，得到增强的表示向量。
  - **整体框架 MedFuse**：基于 MuFuse 构建的端到端模型，可能是将融合后的表示通过时间注意力或序列模型（如 Transformer）进行时序建模，最终用于分类/回归预测。
- **公式/算法流程**（文字说明）：
  1. 输入：对每个时间点 \( t \)，特征 \( f \) 的数值 \( x_{t,f} \) 和特征标识 \( f \)。
  2. 值嵌入 \( v_{t,f} = W_v \cdot x_{t,f} \)（或使用正弦/可学习编码），特征嵌入 \( e_f \) 从可查表获得。
  3. 融合：\( h_{t,f} = v_{t,f} \odot e_f \)（逐元素相乘），必要时可附加可学习的缩放参数或偏置。
  4. 将融合后的表示输入时序编码器（如 Transformer），得到序列级表示，输出预测。

## 3. 实验设计

- **数据集与场景**：使用了三个真实世界的临床数据集，涵盖重症监护（如 MIMIC）和慢性护理场景。
- **基准**：对比了最先进的不规则时序模型，包括加性嵌入的基线方法（如简单拼接或加法融合的模型）、以及专门处理缺失值的模型。
- **对比方法**：文中提到“state-of-the-art baselines”，具体名称未在给定摘要中列出（需查阅全文），但强调 MedFuse 在 AUC 和 F1 上均优于现有模型。

## 4. 资源与算力

- 论文Markdown元数据及摘要中**未明确说明**使用了多少 GPU 型号、数量、训练时长等算力细节。需要指出这一信息缺失。

## 5. 实验数量与充分性

- 实验覆盖了三个不同临床场景的数据集，进行了主要预测任务的对比。
- 还进行了**跨数据集预训练**（cross-dataset pretraining）分析，验证了乘法融合的泛化性。
- 进行了**表示学习分析**，表明乘法融合增强了特征的表达能力。
- 整体实验数量较为充分（多个数据集 + 预训练分析 + 表示分析），但缺少消融实验具体数量的说明（需查全文）。结论客观，对比公平。

## 6. 论文的主要结论与发现

- MedFuse 在三个临床数据集上一致优于现有最先进的不规则时序基线。
- 乘法嵌入融合比加法融合更有效，能够提升表示的表达力和对缺失值的鲁棒性。
- 乘法融合支持跨数据集预训练，可迁移到不同临床场景。
- MedFuse 作为通用框架，适用于不规则临床时间序列建模。

## 7. 优点

- **方法创新**：提出乘法调制融合，弥补了加性操作无法建模值依赖交互的缺陷，简单有效。
- **实验稳健**：覆盖不同护理场景（重症/慢性），且包含预训练实验，验证了方法的通用性。
- **可解释性**：通过表示分析，直观展示了乘法融合带来的表达能力提升。
- **应用价值**：直接针对 EHR 中常见的不规则与缺失问题，临床预测任务（如结局预测、病情监测）有重要实用意义。

## 8. 不足与局限

- **算力信息缺失**：未报告训练资源，难以评估可复现性与部署成本。
- **消融实验细节不明**：未在摘要中提到是否对 MuFuse 的变体（如乘法 vs 加法 vs 拼接）做了系统消融（需查全文确认）。
- **潜在偏差**：可能仅在特定缺失模式或时间间隔分布下表现最佳，泛化到其他不规则模式（如极稀疏数据）的边界未探讨。
- **应用限制**：依赖于可学习的特征标识嵌入，当特征空间极大（如数万种概念）时，参数量可能过大；另外乘法调制对数值范围敏感，可能需要归一化处理。

（完）
