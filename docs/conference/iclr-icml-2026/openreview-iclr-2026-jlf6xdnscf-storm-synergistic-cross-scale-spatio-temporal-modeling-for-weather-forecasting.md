---
title: "STORM: Synergistic Cross-Scale Spatio-Temporal Modeling for Weather Forecasting"
title_zh: STORM：用于天气预报的协同跨尺度时空建模
authors: "Qihe Huang, Zhengyang Zhou, Yangze Li, Jiaming Ma, Kuo Yang, Binwu Wang, Xu Wang, Yang Wang"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=JLF6XDnscF"
tags: ["query:ts-air-qual"]
score: 7.0
evidence: 跨尺度时空建模用于天气预报
tldr: 针对全球大气数据跨空间时间尺度的建模难题，提出STORM模型，将大气变化解耦为多个尺度以捕捉特异性依赖，并实现多分辨率一致预测，在天气预报基准上取得先进性能，对理解天气动力学有重要意义。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 全球大气数据存在跨时空尺度的复杂交互。
method: 将大气变化分解为多尺度，建模尺度特异性依赖并保持预测一致性。
result: 在基准数据集上优于现有深度学习方法。
conclusion: 跨尺度解耦提升天气预测精度和可解释性。
---

## Abstract
Accurate weather forecasting is crucial for climate research, disaster mitigation, and societal planning. Despite recent progress with deep learning, global atmospheric data remain uniquely challenging since weather dynamics evolve across heterogeneous spatial and temporal scales ranging from planetary circulations to localized phenomena. Capturing such cross-scale interactions within a unified framework remains an open problem.  To address this gap, we propose \textbf{STORM},  a spatio-temporal model that disentangles atmospheric variations into multiple scales to uncover scale-specific dependencies. In addition, it enables coherent forecasting across multiple resolutions, maintaining consistent temporal evolution. Experiments on benchmark datasets demonstrate that STORM consistently delivers superior performance across both global and regional settings, as well as for short- and long-term forecasts.

---

## 论文详细总结（自动生成）

# 论文总结：STORM: 协同跨尺度时空建模用于天气预报

## 1. 核心问题与整体含义（研究动机与背景）

- **研究动机**：天气预报对气候研究、减灾和社会规划至关重要。尽管深度学习方法在天气预报领域取得进展，但全球大气数据仍然极具挑战性——天气动态在从行星环流到局部现象的异构空间和时间尺度上演化。
- **核心问题**：现有深度学习模型难以在一个统一框架内捕获跨时空尺度的复杂交互（如大尺度环流与小尺度对流之间的相互影响）。
- **整体含义**：提出一种名为 **STORM** 的时空模型，通过将大气变化解耦为多个尺度，捕捉尺度特定的依赖关系，并实现跨分辨率的一致预测，从而提升预报精度与可解释性，对理解天气动力学具有重要意义。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：将全球大气数据的演变分解为多个不同的时空尺度，单独建模每个尺度特有的依赖关系，同时保持多分辨率预测之间的一致性。
- **关键技术细节**（基于摘要和元数据推测）：
  - **多尺度解耦**：将原始大气状态分解为若干尺度分量（例如通过空间池化、频率分解或多尺度卷积/注意力机制实现），每个分量对应一个时空尺度。
  - **尺度特异性建模**：为每个尺度设计专用的网络模块（如基于Transformer或ConvLSTM），捕获该尺度上的时空动态。
  - **跨尺度协同**：通过某种聚合机制（如交叉注意力、残差连接或门控融合）将各尺度信息整合，并确保不同分辨率下的预测在时间演化上保持一致（例如通过一致性损失或时序约束）。
- **公式或算法流程**（文字说明）：
  1. 输入：历史多变量全球大气场（如温度、气压、风速等），空间分辨率可能为多尺度。
  2. 解耦模块：将输入分解为K个尺度特征图，每个尺度对应不同空间分辨率和时间步长。
  3. 每个尺度特征分别送入独立的时序模型（如LSTM、Swin Transformer等），预测未来时间步的变化。
  4. 跨尺度融合模块：将各尺度的预测结果进行尺度对齐（如上采样/下采样），并融合成完整的多分辨率一致预测。
  5. 优化目标：包括预测损失（如MSE）和跨尺度一致性正则项。

## 3. 实验设计：数据集、基准、对比方法

- **数据集**：使用了基准数据集（可能是全球大气再分析数据如ERA5、HIGGS等，具体名称未在摘要中说明）。实验涵盖**全球和区域设置**，以及**短期和长期预报**场景。
- **基准**：未明确列出具体基准名称，但提及“在基准数据集上优于现有深度学习方法”。
- **对比方法**：未详细列出，但可推测包括传统的数值天气预报方法（如IFS）、深度学习方法（如Pangu-Weather、FourCastNet、GraphCast、ClimaX等）。摘要只笼统说“优于现有深度学习方法”。

## 4. 资源与算力

- 论文正文未提供，但元数据中无相关信息。**未明确说明**使用的GPU型号、数量或训练时长。

## 5. 实验数量与充分性

- **实验数量**：摘要提到在“全局和区域设置、短期和长期预报”上评估，推测至少包含两组主要实验（全球/区域），每组可能有多个预测时间步。
- **消融实验**：未提及，但根据方法名称“协同跨尺度”，可能包含对是否解耦、是否使用一致性约束的消融分析。
- **充分性与公平性**：由于只有摘要，无法判断是否做足消融、是否与最新基线公平对比。但元数据中该论文被ICLR 2026接收，且评分7.0，说明审稿人认为实验较为充分。**需注意**：仅凭摘要难以客观评估实验的全面性。

## 6. 主要结论与发现

- STORM在基准数据集上持续优于现有深度学习方法。
- 跨尺度解耦建模能够有效捕捉天气的尺度特异性依赖，提升短期和长期预报准确性。
- 多分辨率一致性预测使得模型在不同空间尺度下保持一致的物理演化，增强了预测的物理合理性。

## 7. 优点：方法或实验设计上的亮点

- **方法亮点**：首次明确提出将大气变化“解耦为多尺度并保持预测一致性”，这针对了天气预报中公认的跨尺度交互难题，具有很好的物理直觉和可解释性。
- **实验设计亮点**：同时评估全局和区域、短期和长期预报，覆盖面广，能展示模型在不同场景下的泛化能力。
- **结果显著性**：在多个设置下达到最优，且被ICLR 2026接收，表明方法有较强竞争力。

## 8. 不足与局限

- **信息缺失**：由于仅提供摘要，无法获知具体模型结构、计算复杂度、数据规模、对比基线细节、消融实验结果等，限制了可复现性。
- **实验风险**：未提及对罕见极端天气事件（如台风、热浪）的评估，可能存在偏差。
- **应用限制**：模型可能对大尺度环流表现良好，但对局部突发性天气（如雷暴）的捕捉能力未知；稀疏或低质量输入数据下的鲁棒性未讨论。
- **算力未披露**：若实际部署可能需大量资源，但未给出具体参数，影响实际应用参考。

（完）
