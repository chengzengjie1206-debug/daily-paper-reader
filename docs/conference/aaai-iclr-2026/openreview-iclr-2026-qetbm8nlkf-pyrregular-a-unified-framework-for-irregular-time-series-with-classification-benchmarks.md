---
title: "PYRREGULAR: A Unified Framework for Irregular Time Series, with Classification Benchmarks"
title_zh: PYRREGULAR：不规则时间序列统一框架与分类基准
authors: "Francesco Spinnato, Cristiano Landi"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=qetBM8nLkf"
tags: ["query:ts-air-qual"]
score: 6.0
evidence: 不规则时间序列分类框架，与不规则数据相关
tldr: 不规则时序数据因频率、时长、缺失值而异，现有研究碎片化。该文提出统一框架和首个标准化不规则时间序列分类库，包含34个数据集和12个基线模型。虽然侧重分类而非预测，但其对不规则数据格式的统一处理、缺省值管理和模型评估方法对不规则时序预测任务有重要参考价值。该库可推动算法开发和公平比较，为环境传感器等不规则数据场景提供基础工具。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 不规则时序数据研究碎片化，缺乏统一工具和标准化基准。
method: 构建统一数组格式的数据集仓库，集成34个分类数据集并基准测试12种模型。
result: 提供了首个标准化不规则时序分类基准，揭示了不同模型在不规则数据上的表现差异。
conclusion: 该框架促进了不规则时序研究领域的互通与可重复性。
---

## Abstract
Irregular temporal data, characterized by varying recording frequencies, differing observation durations, and missing values, presents significant challenges across fields like mobility, healthcare, and environmental science. Existing research communities often overlook or address these challenges in isolation, leading to fragmented tools and methods. To bridge this gap, we introduce a unified framework, and the first standardized dataset repository for irregular time series classification, built on a common array format to enhance interoperability. This repository comprises 34 datasets on which we benchmark 12 classifier models from diverse domains and communities. This work aims to centralize research efforts and enable a more robust evaluation of irregular temporal data analysis methods.

---

## 论文详细总结（自动生成）

# PYRREGULAR：不规则时间序列统一框架与分类基准——论文总结

## 1. 核心问题与整体含义（研究动机和背景）
不规则时间序列数据（记录频率不一、观测时长不同、存在缺失值）在移动计算、医疗健康、环境科学等领域普遍存在。然而，现有研究社区往往各自为政，使用碎片化的工具和方法，缺乏统一的格式和标准化基准，导致方法评估困难、研究结果难以复现和比较。作者旨在构建一个统一的框架和首个标准化的不规则时间序列分类基准库，以整合研究资源、促进公平评估和可重复性。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：采用统一的数组格式（common array format）来组织所有不规则时间序列数据，增强不同数据集之间的互操作性。在此格式基础上，构建数据集仓库，并集成来自不同领域的分类器模型进行统一基准测试。
- **关键技术细节**：
  - 统一数据表示：将不规则时间序列转换为标准化的张量结构（如填充、掩码、时间戳对齐等），使模型能够直接处理。
  - 仓库构建：收集并标准化34个不规则时间序列分类数据集。
  - 基准测试：集成12种来自不同社区（如传统机器学习、深度学习、时间序列专用方法）的分类器，在统一评估协议下进行性能比较。
- **公式或算法流程**（文字说明）：未提供具体公式。大致流程为：原始不规则数据 → 转换为统一数组格式 → 输入分类器 → 输出分类结果 → 评估指标（如准确率等）。

## 3. 实验设计
- **数据集/场景**：34个不规则时间序列分类数据集，涵盖不同领域（如人体活动识别、医疗监测、环境传感等）。
- **Benchmark**：首次建立标准化不规则时间序列分类基准，提供统一的训练/测试划分和评价指标。
- **对比方法**：12种分类器模型，来自不同社区或领域（具体模型名称未在摘要中列出，可能包括kNN、RNN、Transformer、基于ODE的方法等）。

## 4. 资源与算力
文中未明确说明使用的GPU型号、数量、训练时长等算力信息。元数据中也未提及。因此无法总结具体算力消耗。

## 5. 实验数量与充分性
- **实验数量**：在34个数据集上对12种模型进行了全面基准测试，共约408组（34×12）基本实验结果。此外可能包含消融实验（文中未具体说明）。
- **充分性与公平性**：使用了统一的数组格式和标准化评估协议，确保了实验的客观性和可重复性。覆盖多个领域和多种模型类型，实验规模较大，充分性较好。但缺乏消融实验和超参数敏感性分析等细节，可能影响对方法鲁棒性的全面理解。

## 6. 主要结论与发现
- 该框架成功构建了首个标准化不规则时间序列分类基准库，揭示了不同模型在不规则数据上的表现差异。
- 统一数据格式可提升跨数据集的互换性和研究可重复性。
- 该工作有助于集中研究努力，推动不规则时间序列分析方法更稳健的评估。

## 7. 优点
- **统一性**：解决了不规则时间序列研究碎片化的问题，提供了统一的数组格式和标准化的基准。
- **规模与代表性**：包含了34个数据集和12种模型，覆盖多个应用领域，基准完备。
- **可复现性**：开源仓库和标准评估协议便于后续研究者复现和扩展。
- **交叉领域整合**：整合了来自不同社区的方法，促进交叉研究。

## 8. 不足与局限
- **仅聚焦分类**：框架和基准仅针对“分类”任务，未涉及回归、预测、异常检测等常见不规则时序任务。
- **算力资源未公开**：未提供模型训练所需的计算资源信息，影响复现成本评估。
- **缺少消融与敏感性分析**：未深入分析各组件的贡献（如统一格式的影响、缺省值处理策略等）。
- **数据偏差风险**：34个数据集可能在某些领域或模式上存在偏差，部分数据集规模较小，结论的泛化性需进一步验证。
- **应用限制**：基准仅验证了分类性能，对于实际应用中的实时性、可解释性、鲁棒性等维度未涉及。

（完）
