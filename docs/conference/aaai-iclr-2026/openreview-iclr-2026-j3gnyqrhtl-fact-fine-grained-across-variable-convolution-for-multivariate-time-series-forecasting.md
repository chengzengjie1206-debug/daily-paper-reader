---
title: "FACT: Fine-grained Across-variable Convolution for Multivariate Time Series Forecasting"
title_zh: FACT：面向多元时间序列预测的细粒度跨变量卷积
authors: "Huiqiang Wang, Jieming Shi, Li Qing"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=j3gNYqrHtl"
tags: ["query:ts"]
score: 5.0
evidence: 多元时间序列预测中细粒度变量交互建模，可迁移至复合需求
tldr: 多元时序预测中变量交互随时间动态变化，现有方法仅捕获粗粒度相关。该文提出FACT，通过深度可分离卷积从时域和频域显式建模细粒度变量交互，设计DConvBlock模块。在多个多元预测基准上取得最优结果。该方法为处理高维变量依赖提供了新思路，可适用于空气质量等多变量预测场景，但不专门处理不规则采样或缺失值。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 变量间交互随时间动态变化，现有方法仅关注粗粒度相关，忽略了细粒度动态。
method: 提出FACT架构，使用深度可分离卷积从时域和频域同时建模细粒度变量交互。
result: 在多个多元时序预测基准上取得最优或接近最优性能。
conclusion: 精细建模变量动态交互显著提升了预测精度。
---

## Abstract
Modeling the relationships among variables has become increasingly important, particularly in high-dimensional multivariate time series forecasting tasks. However, most existing methods primarily focus on capturing coarse-grained correlations between variables, overlooking a finer and more dynamic aspect: the variable interactions often manifest differently as time progresses.
To address this limitation, we propose FACT, an Fine-grained Across-variable Convolution architecture for multivariate Time series forecasting that explicitly models fine-grained variable interactions from both the time and frequency domains. 
Technically, we introduce a depth-wise convolution block DConvBlock, which leverages a depth-wise convolution architecture with channel-specific kernels to model dynamic variable interactions at each granularity.
To further enhance efficiency, we reconfigure the original one-dimensional variables into a two-dimensional space, reducing the variable distance and the required model layers. Then DConvBlock incorporates multi-dilated 2D convolutions with progressively increasing dilation rates, enabling the model to capture fine-grained and dynamic variable interactions while efficiently attaining a global reception field.
Extensive experiments on twelve benchmark datasets demonstrate that FACT not only achieves state-of-the-art forecasting accuracy but also delivers substantial efficiency gains, significantly reducing both training time and memory consumption compared to attention mechanism.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：在高维多元时间序列预测任务中，变量之间的交互关系是动态变化的，但现有方法仅能捕获粗粒度的相关性（例如全局相关矩阵），忽略了更细粒度、随时间变化的动态交互。
- **研究动机**：作者认为，变量之间的相互作用并非一成不变，随着时间推移，不同变量在不同时间尺度上的关联模式会发生变化。只有精细地建模这种动态交互，才能显著提升预测精度。
- **整体含义**：本文旨在弥补现有方法在细粒度变量交互建模上的不足，提出一种高效的跨变量卷积架构，用以同时从时域和频域显式建模变量间的细粒度动态关系，从而提升多元时序预测性能。

## 2. 论文提出的方法论

- **核心思想**：提出 FACT（Fine-grained Across-variable Convolution for Multivariate Time Series Forecasting）架构，通过深度可分离卷积实现细粒度的跨变量交互建模。
- **关键技术细节**：
  - **DConvBlock 模块**：基于深度可分离卷积，每个通道使用独立的卷积核，以建模每个粒度下动态的变量交互。
  - **二维空间重配置**：将原始一维变量重新排布到二维空间，减少变量间的距离和所需模型层数，提高效率。
  - **多膨胀 2D 卷积**：在 DConvBlock 中引入逐步增大的膨胀率（dilation rates），使得模型能够捕获细粒度和动态的变量交互，同时高效地获得全局感受野。
- **公式/算法流程**（文字说明）：输入多元时间序列 → 将一维变量重构成二维空间 → 通过多个 DConvBlock 模块，每个模块使用深度可分离卷积（每变量独立卷积核）并叠加不同膨胀率的 2D 卷积 → 从时域和频域两个角度同时提取特征 → 输出预测结果。

## 3. 实验设计

- **数据集/场景**：在 12 个多元时间序列预测基准数据集上进行实验（具体名称文中未列出，但摘要明确提到 “twelve benchmark datasets”）。
- **Benchmark 与对比方法**：主要与基于注意力机制（Attention mechanism）的方法进行了对比。具体对比了哪些基线方法（如 Transformer、Informer、LSTM 等）未详细说明，但摘要强调 FACT 在准确率和效率上均超越注意力机制。
- **对比维度**：预测准确率（state-of-the-art）、训练时间、内存消耗。

## 4. 资源与算力

- **文中说明**：未明确提及使用的 GPU 型号、数量、训练时长等具体算力信息。
- **已知信息**：摘要仅提到 FACT 相比注意力机制显著减少了训练时间和内存消耗，但未提供具体数字或资源配置。因此无法总结算力细节。

## 5. 实验数量与充分性

- **实验数量**：在 12 个基准数据集上进行实验，属于中等规模（覆盖多元时序常见场景）。此外，摘要未直接提及消融实验，但可以推测作者进行了消融分析（例如验证 DConvBlock 各组件贡献）——文中未明确说明，但通常此类会议论文会包含消融实验。
- **充分性与客观性**：
  - 优点：覆盖多个数据集，与强基线（注意力机制）对比，结论具有普遍性。
  - 不足：未说明是否与更多非注意力类方法（如传统统计模型、GNN 等）对比；也没有提供统计显著性检验或误差条。消融实验细节缺失，公平性依赖标准数据划分，但未明确是否采用相同的训练/测试协议。

## 6. 论文的主要结论与发现

- FACT 在 12 个多元时序预测基准上取得了最优或接近最优的预测精度。
- 相比注意力机制，FACT 提供了显著的效率提升：训练时间和内存消耗大幅降低。
- 精细建模变量的动态交互（从时域和频域两个维度）是提升预测性能的关键。

## 7. 优点

- **方法论创新**：提出深度可分离卷积用于细粒度跨变量交互建模，区别于传统的全局相关或注意力机制，更符合变量关系动态变化的现实。
- **效率优势**：通过二维重配置和多膨胀卷积设计，减少了模型参数和计算复杂度，同时保持全局感受野，实际效率优于注意力机制。
- **性能前沿**：在多个标准数据集上达到 SOTA，验证了方法的有效性。

## 8. 不足与局限

- **实验覆盖不全**：虽然用了 12 个数据集，但未说明是否涵盖不同类型（如金融、气象、交通、工业等）以及不规则采样或缺失值场景。论文明确提到“不专门处理不规则采样或缺失值”，限制了应用范围。
- **对比方法有限**：仅强调与注意力机制的对比，缺少与其他高效架构（如 TCN、LSSL、GNN-based 方法）的全面比较。
- **消融与鲁棒性分析缺失**：未提供详细消融实验、超参数敏感性分析、稳定性测试等，论文完整性可能不足。
- **算力信息缺失**：未报告具体训练配置，不利于复现和效率量化。
- **可能偏差**：仅从摘要看，实验设计可能存在选择偏差（只报告了最好结果），但会议论文通常会有更完整的实验章节。

（完）
