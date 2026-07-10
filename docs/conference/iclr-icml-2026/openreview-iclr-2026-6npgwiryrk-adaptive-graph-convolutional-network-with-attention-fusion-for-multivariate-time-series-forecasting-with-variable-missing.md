---
title: Adaptive Graph Convolutional Network with Attention Fusion for Multivariate Time Series Forecasting with Variable Missing
title_zh: 自适应图卷积网络与注意力融合用于变量缺失的多变量时间序列预测
authors: "Wenfeng Zhou, Bin Chen, Xiaoyun Xia, Binbin Guo, Jiacheng Chen, Guojiang Shen, Xiangjie Kong"
date: 2025-09-16
pdf: "https://openreview.net/pdf?id=6NPGwIRyrk"
tags: ["query:ts-air-qual"]
score: 7.0
evidence: 变量缺失下的多变量时间序列预测
tldr: 多变量时间序列中变量缺失严重影响预测性能，现有方法存在误差累积或依赖局部相关性等问题。VMPredictor提出端到端框架，通过自适应图卷积和注意力融合有效建模变量间关系。在交通、天气等数据集上，该方法在变量缺失场景下显著优于现有方法。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 变量缺失使预测模型难以捕获变量间内在关系，现有方法鲁棒性不足。
method: 提出VMPredictor，结合自适应图卷积和注意力融合建模时空依赖。
result: 在多个缺失率场景下预测精度优于现有方法。
conclusion: VMPredictor为变量缺失的多变量时间序列预测提供了鲁棒解决方案。
---

## Abstract
Multivariate time series forecasting (MTSF) plays a vital role in diverse applications such as traffic prediction, weather research, and energy management. However, missing subset variable forecastinh has emerged as a critical challenge in MTSF due to factors such as sensor failures and maintenance. Variable incompleteness severely hinders the ability of forecasting models to capture intrinsic inter-variable relationships. Existing approaches either suffer from severe error accumulation, lack flexible mechanisms for handling missing data, or overly rely on local spatiotemporal correlations. To address these limitations, we propose VMPredictor, a novel end-to-end framework that effectively models spatiotemporal dependencies among incomplete variables for accurate forecasting. VMPredictor incorporates two key components: (1) the Adaptive Missing Filling and Enhancement Layer , which introduces learnable embeddings to adaptively fill missing positions and dynamically refine incomplete representations during training; and (2) the Spatiotemporal Dependency Mining Layer, built upon a Dynamic Graph Convolution Layer-Normalized Gated Recurrent Unit, where dynamic graph convolution adaptively reconstructs spatial correlations and replaces all fully connected layers in GRU to capture synchronized spatiotemporal dependencies. Together, these innovations endow VMPredictor with robust missing-data handling and precise spatiotemporal relation modeling. Extensive experiments on five real-world datasets under varying missing rates demonstrate the superiority of our approach over existing baselines. Codes can be found at https://anonymous.4open.science/r/Code-A216/.

---

## 论文详细总结（自动生成）

好的，根据您提供的论文元数据和摘要内容，以下是针对论文《Adaptive Graph Convolutional Network with Attention Fusion for Multivariate Time Series Forecasting with Variable Missing》的详细中文总结。

---

## 1. 论文的核心问题与整体含义

- **研究动机**：多变量时间序列预测（MTSF）在交通、气象、能源管理等领域至关重要。然而，由于传感器故障、维护等原因，常常出现“子集变量缺失”（即部分变量在部分时间点缺失）的问题，严重影响模型捕捉变量间内在依赖关系的能力。
- **核心挑战**：现有方法面对变量缺失时存在三大不足：
  - 误差累积严重；
  - 缺乏灵活处理缺失数据的机制；
  - 过度依赖局部时空相关性，泛化能力弱。
- **研究目标**：提出一种端到端框架，在变量缺失场景下仍能有效建模时空依赖，实现高精度预测。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **框架名称**：VMPredictor（端到端架构）
- **核心思想**：通过自适应填充缺失位置 + 动态图卷积捕获时空依赖，避免传统填充或插值带来的误差累积。
- **两大关键组件**：

  - **自适应缺失填充与增强层（Adaptive Missing Filling and Enhancement Layer）**：
    - 引入可学习的嵌入向量（learnable embeddings），在训练过程中自适应地填补缺失位置。
    - 动态细化不完整的表示，使得模型能够根据上下文自动学习缺失值的合理替代，而非使用固定插值。

  - **时空依赖挖掘层（Spatiotemporal Dependency Mining Layer）**：
    - 基于**动态图卷积层归一化门控循环单元（Dynamic Graph Convolution Layer-Normalized Gated Recurrent Unit）**。
    - 动态图卷积：自适应地重构变量间的空间相关性（图结构可学习），而非依赖预设邻接矩阵。
    - 替换GRU中所有全连接层为图卷积操作，从而同时捕捉同步的时空依赖关系（即时间步与变量间的联合模式）。

- **算法流程**（文字描述）：
  1. 输入：缺失的多变量时间序列数据。
  2. 通过自适应缺失填充与增强层，对缺失位置进行可学习嵌入填充，得到增强后的完整表示。
  3. 将增强后的表示送入由动态图卷积驱动的GRU模块，在每个时间步利用动态图卷积更新隐藏状态，同时建模变量间空间关联和时序演变。
  4. 输出：未来时间步的预测值。

## 3. 实验设计

- **数据集**：5个真实世界数据集，涵盖不同领域（交通、天气等，具体名称未在摘要中列出，但推测包含交通流量、气象站数据等）。
- **场景**：在**多种缺失率**下进行验证（如10%、20%、50%等），模拟不同程度的变量缺失。
- **基线方法**（Baseline）：与现有主流多变量时间序列预测方法对比（具体方法名未列出，但提及“优于现有baselines”）。
- **评估指标**：未明确给出，通常为MAE、RMSE、MAPE等。

## 4. 资源与算力

- **未明确说明**：论文摘要和元数据中未提及使用的GPU型号、数量、训练时长等算力信息。因此无法总结算力消耗。

## 5. 实验数量与充分性

- **实验数量**：在5个不同数据集、多种缺失率下进行了对比实验，此外应包含消融实验（验证两个组件各自的有效性）和超参数分析。
- **充分性判断**：
  - 覆盖多个领域和多种缺失率，场景较为多样。
  - 但缺少对基线方法具体名称和结果的详细披露（摘要未提供），实验设计是否公平难以完全判断。根据摘要描述“demonstrate the superiority over existing baselines”，推测对比实验合理。
  - 若论文完整版包含更多消融、敏感度分析，则实验充分性较好；仅从摘要看，实验规模适中。

## 6. 论文的主要结论与发现

- VMPredictor在变量缺失的多变量时间序列预测任务上显著优于现有方法。
- 自适应填充层能有效避免传统填充的误差累积，动态图卷积GRU能灵活捕获变量间随时间变化的空间依赖。
- 该框架提供了一种鲁棒的解决方案，尤其适用于传感器故障等实际场景中的缺失数据问题。

## 7. 优点：方法或实验设计上的亮点

- **方法创新**：
  - 将可学习嵌入用于缺失填充，而非固定插值，增强了模型对缺失模式的适应性。
  - 用动态图卷积替换GRU的全连接层，实现时空同步建模，避免了分离式建模的信息损失。
- **实验设计亮点**：
  - 多数据集、多缺失率验证，体现了方法的泛化能力。
  - 代码公开（匿名仓库），有利于可复现性。

## 8. 不足与局限

- **实验覆盖**：未说明具体数据集名称、缺失率范围、基线方法细节，可能影响读者对结果全面性的判断。
- **偏差风险**：仅基于5个数据集，且未包含合成缺失 vs 真实缺失的对比，可能存在数据偏差。
- **应用限制**：该方法假设缺失模式是随机的或可学习的，若缺失非随机（如系统性缺失），性能可能下降；此外，动态图卷积的计算复杂度可能较高，缺乏效率分析。
- **资源未报告**：缺失训练时间和硬件要求，不利于实际部署评估。

---

（完）
