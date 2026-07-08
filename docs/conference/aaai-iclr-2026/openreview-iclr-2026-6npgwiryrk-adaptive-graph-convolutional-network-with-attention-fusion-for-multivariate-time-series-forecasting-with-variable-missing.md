---
title: Adaptive Graph Convolutional Network with Attention Fusion for Multivariate Time Series Forecasting with Variable Missing
title_zh: 基于自适应图卷积与注意力融合的多变量缺失时序预测
authors: "Wenfeng Zhou, Bin Chen, Xiaoyun Xia, Binbin Guo, Jiacheng Chen, Guojiang Shen, Xiangjie Kong"
date: 2025-09-16
pdf: "https://openreview.net/pdf?id=6NPGwIRyrk"
tags: ["query:ts"]
score: 9.0
evidence: 多变量时间序列预测中的变量缺失处理
tldr: 针对传感器故障等导致的变量缺失严重阻碍多变量时序预测的问题，本文提出VMPredictor框架。该框架通过自适应图卷积和注意力融合弹性建模缺失变量间的时空依赖，避免误差累积。实验表明在多种缺失模式下均取得稳定提升。该工作为缺失值时序预测提供了端到端解决方案。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 变量缺失导致模型无法捕捉变量间关系，现有方法存在误差累积和灵活性不足。
method: 设计自适应图卷积网络动态学习缺失变量依赖，结合注意力融合增强鲁棒性。
result: 在交通、天气等数据集上，多种缺失模式下预测性能优于现有方法。
conclusion: VMPredictor为处理变量缺失的时序预测提供了有效且灵活的框架。
---

## Abstract
Multivariate time series forecasting (MTSF) plays a vital role in diverse applications such as traffic prediction, weather research, and energy management. However, missing subset variable forecastinh has emerged as a critical challenge in MTSF due to factors such as sensor failures and maintenance. Variable incompleteness severely hinders the ability of forecasting models to capture intrinsic inter-variable relationships. Existing approaches either suffer from severe error accumulation, lack flexible mechanisms for handling missing data, or overly rely on local spatiotemporal correlations. To address these limitations, we propose VMPredictor, a novel end-to-end framework that effectively models spatiotemporal dependencies among incomplete variables for accurate forecasting. VMPredictor incorporates two key components: (1) the Adaptive Missing Filling and Enhancement Layer , which introduces learnable embeddings to adaptively fill missing positions and dynamically refine incomplete representations during training; and (2) the Spatiotemporal Dependency Mining Layer, built upon a Dynamic Graph Convolution Layer-Normalized Gated Recurrent Unit, where dynamic graph convolution adaptively reconstructs spatial correlations and replaces all fully connected layers in GRU to capture synchronized spatiotemporal dependencies. Together, these innovations endow VMPredictor with robust missing-data handling and precise spatiotemporal relation modeling. Extensive experiments on five real-world datasets under varying missing rates demonstrate the superiority of our approach over existing baselines. Codes can be found at https://anonymous.4open.science/r/Code-A216/.

---

## 论文详细总结（自动生成）

好的，由于提供的论文内容仅包含摘要和元数据，以下总结将严格基于这些有限信息展开，并在必要时指出信息的缺失。

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：多变量时间序列预测（MTSF）中，由于传感器故障、维护等原因，经常出现部分变量缺失（Variable Missing）的情况。这种变量不完整性严重阻碍了模型捕捉变量间的内在依赖关系，导致预测性能急剧下降。
- **整体含义**：现有的缺失值处理方法（如插补、丢弃）存在误差累积、灵活性不足或过度依赖局部时空关联等缺陷。本文旨在提出一种端到端框架，能够弹性地建模缺失变量间的时空依赖，从而在不引入额外误差的前提下，实现鲁棒且准确的预测。

### 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：设计一个名为 **VMPredictor** 的端到端框架，通过自适应填充与动态图卷积，同时解决缺失值处理和时空依赖建模两大挑战。
- **关键技术细节**：
    - **自适应缺失填充与增强层（Adaptive Missing Filling and Enhancement Layer）**：引入可学习的嵌入向量，在训练过程中自适应地填充缺失位置，并动态优化不完整的表示，避免固定插补带来的偏差。
    - **时空依赖挖掘层（Spatiotemporal Dependency Mining Layer）**：基于 **动态图卷积层归一化门控循环单元（Dynamic Graph Convolution Layer-Normalized Gated Recurrent Unit, DGC-LN-GRU）**。其中动态图卷积能够自适应地重构变量间的空间相关性，并替换GRU中所有的全连接层，从而同步捕获时空依赖，提升模型对动态关联的适应能力。
- **公式或算法流程**：原文未提供具体公式，仅以文字描述。流程可概括为：输入含缺失的多变量序列 → 自适应填充增强层生成完整表示 → 动态图卷积GRU模块进行时空特征提取 → 输出预测结果。

### 3. 实验设计
- **数据集**：在5个真实世界数据集上进行实验，涵盖交通、天气等场景（如交通流量、气象观测等），且在不同缺失率下验证。
- **Benchmark**：与现有基线方法（如传统插值+预测模型、端到端缺失处理模型等）进行对比，具体方法名称未列出。
- **对比方法**：摘要中仅提及“现有基线”，未明确给出对比方法列表。

### 4. 资源与算力
- 文中未提及GPU型号、数量、训练时长等算力资源信息。无法对计算效率进行评估。

### 5. 实验数量与充分性
- **实验数量**：只提到在5个数据集、多种缺失率下进行实验，具体实验组数（如消融实验、超参数分析等）未详细说明。
- **充分性评估**：从现有信息看，实验覆盖多领域数据集和不同缺失率场景，具有一定的广度；但缺少详细结果（如表格、误差指标），无法判断实验是否包含了充分的消融分析、统计显著性检验以及公平性对比（如超参数是否统一调优）。因此，实验充分性有待更多细节佐证。

### 6. 论文的主要结论与发现
- VMPredictor在所有缺失模式下均取得稳定的性能提升，优于现有方法。
- 自适应填充与动态图卷积的结合能够有效避免误差累积，并灵活应对不同缺失模式。
- 该框架为处理变量缺失的时序预测提供了一种端到端、且鲁棒的解决方案。

### 7. 优点
- **方法创新性**：将可学习填充与动态图卷积集成到GRU中，同时处理缺失和时空依赖，避免了传统两阶段方法的误差累积。
- **框架灵活性**：自适应机制使模型适用于不同缺失率和缺失模式（随机缺失、系统缺失等），无需针对每种情况单独设计插补策略。
- **实验设计**：跨多个真实场景和不同缺失率进行验证，增强了结论的泛化性。

### 8. 不足与局限
- **信息缺失严重**：由于提供的论文内容极其有限，无法评估方法的具体细节（如图卷积的构造方式、注意力融合机制的具体实现）、实验结果的量化指标、以及与最优方法的差距。这些缺失使得结论的可信度大打折扣。
- **实验覆盖有限**：未提供详细实验结果（如RMSE、MAE表格），也未说明是否在公开标准基准数据集上进行对比，难以判断公平性。
- **算力成本未报告**：缺少计算资源描述，可能影响实际部署的可行性评估。
- **缺乏理论分析**：未对自适应填充的收敛性、动态图卷积的泛化误差等提供理论支撑，方法的稳定性存疑。

（完）
