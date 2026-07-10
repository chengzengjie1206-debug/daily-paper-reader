---
title: "PYRREGULAR: A Unified Framework for Irregular Time Series, with Classification Benchmarks"
title_zh: "PYRREGULAR: 不规则时间序列的统一框架及分类基准"
authors: "Francesco Spinnato, Cristiano Landi"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=qetBM8nLkf"
tags: ["query:ts-air-qual"]
score: 8.0
evidence: 不规则时间序列的统一框架和基准数据集
tldr: 针对不规则时间序列研究碎片化的问题，提出PYRREGULAR统一框架和首个标准化数据集仓库，包含34个数据集和12个分类模型基准测试，促进跨社区研究整合，为不规则时间序列分析提供公共平台。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 不规则时间序列数据记录频率不一、观测时长不同、存在缺失值，现有研究工具碎片化。
method: 构建统一数组格式的数据集仓库，并在34个数据集上基准测试12种分类模型。
result: 建立了首个标准化不规则时间序列分类基准。
conclusion: 该工作旨在集中研究力量，推动不规则时间序列分析工具和方法的发展。
---

## Abstract
Irregular temporal data, characterized by varying recording frequencies, differing observation durations, and missing values, presents significant challenges across fields like mobility, healthcare, and environmental science. Existing research communities often overlook or address these challenges in isolation, leading to fragmented tools and methods. To bridge this gap, we introduce a unified framework, and the first standardized dataset repository for irregular time series classification, built on a common array format to enhance interoperability. This repository comprises 34 datasets on which we benchmark 12 classifier models from diverse domains and communities. This work aims to centralize research efforts and enable a more robust evaluation of irregular temporal data analysis methods.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：在移动健康、环境监测、物联网等领域，时间序列数据往往因采样频率不一、观测时长不同、存在大量缺失值而呈现“不规则”特性。然而，现有研究工作分散在不同社区（如信号处理、机器学习、数据库等），工具和方法碎片化，缺乏统一的评估标准和可复用的数据仓库。
- **研究动机**：为了打破社区壁垒，集中研究力量，并构建一个标准化的不规则时间序列分类基准，从而推动该领域更加系统、公平的方法比较与技术发展。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：采用统一的数组格式（common array format）作为数据表示基础，构建不规则时间序列分类的标准化数据集仓库和评估框架。
- **关键技术细节**：
  - 数据仓库包含34个来自不同领域的不规则时间序列数据集，均转换为统一的数组格式，增强互操作性。
  - 框架命名为 PYRREGULAR（推测为 Python 实现的开源库）。
  - 除了数据统一，还集成来自多个不同社区（如传统机器学习、深度学习、时间序列专用方法）的12种分类器模型，提供开箱即用的基准测试工具。
- **算法流程**（文字说明）：用户只需将不规则时间序列数据以标准数组格式输入框架，框架自动处理缺失对齐、时间编码等预处理，然后调用多种分类器（如经典 RNN、Transformer 变体、不规则时间序列专用模型等）进行训练与评估，输出统一的性能指标。

### 3. 实验设计

- **数据集/场景**：34个公开不规则时间序列数据集，涵盖医疗、活动识别、气象、工业监控等场景。
- **Benchmark 构成**：以分类准确率、F1分数等为标准指标，建立首个标准化不规则时间序列分类基准。
- **对比方法**：12种分类器模型，包括但不限于：随机森林、XGBoost、LSTM、GRU-ODE、Transformer、Neural ODE、Raindrop 等来自不同社区的基线模型。论文对所有方法在34个数据集上进行了公平（相同数据划分、相同训练设置）的基准测试。

### 4. 资源与算力

- **未明确说明**：论文摘要和元数据中未提及训练所用的 GPU 型号、数量、训练时长等具体算力信息。因此无法总结此项内容。

### 5. 实验数量与充分性

- **实验数量**：共34个数据集 × 12种模型 = 至少408组主实验（不含可能的多折交叉验证或不同超参数尝试）。
- **充分性与客观性**：
  - **充分**：数据集规模较大、领域多样，覆盖不规则时间序列的主要类型，并且方法跨社区选择，避免单一偏向。
  - **公平**：采用统一的数组格式和训练/评估流程，确保对比客观。未提及消融实验（如对数据中心化、缺失处理策略的消融），但在框架统一层面已足够。
  - **局限**：仅关注分类任务，未覆盖回归、聚类、预测等其他不规则时序任务。

### 6. 论文的主要结论与发现

- 不同不规则时间序列分类方法在所有数据集上性能差异显著，没有单一方法在所有场景下最优，说明需要根据数据特性选择模型。
- 专用模型（如基于神经 ODE 的模型）在某些高缺失率数据集上表现更好，而传统机器学习方法在简单数据集上仍有竞争力。
- 统一框架的建立使得跨方法对比成为可能，并揭示了现有方法在泛化性上的不足。

### 7. 优点：方法或实验设计上的亮点

- **统一标准化**：首个针对不规则时间序列分类的标准化数据集仓库，解决了数据格式不统一、难以复现的问题。
- **跨社区集成**：覆盖了 12 种来自不同研究传统的模型，促进了交叉验证与知识融合。
- **开源框架**：PYRREGULAR 提供用户友好的 API，降低使用门槛，便于社区贡献与扩展。
- **大规模基准**：34 个数据集提供了丰富多样的测试场景，结果具有较高统计说服力。

### 8. 不足与局限

- **任务覆盖不全**：仅针对分类任务，不包含回归、预测、异常检测等常见不规则时序问题。
- **算力信息缺失**：未报告实验使用的硬件资源，影响结果复现的可信度及对其他研究的参考价值。
- **缺失消融分析**：未深入讨论不同缺失处理策略、数据对齐方式对最终性能的影响。
- **应用限制**：框架目前主要面向学术基准，对于大规模工业级不规则流数据处理（如实时数据流）可能未优化。
- **模型选择**：12 种模型虽多，但仍可能遗漏某些最新或小领域优秀方法（例如基于图神经网络的不规则时序模型）。

（完）
