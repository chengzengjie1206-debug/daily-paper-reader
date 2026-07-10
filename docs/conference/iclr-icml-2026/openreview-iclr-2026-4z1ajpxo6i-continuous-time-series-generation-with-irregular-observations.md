---
title: Continuous Time Series Generation with Irregular Observations
title_zh: 基于不规则观测的连续时间序列生成
authors: "Xu Zhang, Junwei Deng, Chang Xu, Hao Li, Jiang Bian"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=4z1AjpXo6i"
tags: ["query:ts-air-qual"]
score: 9.0
evidence: 不规则观测时间序列生成，采用MOE-NCDE
tldr: 现有时间序列生成方法假定数据规则采样，无法处理真实世界中的不规则稀疏观测。本文提出MN-TSG，基于专家混合神经控制微分方程（MOE-NCDE）框架，能够学习动态时序模式并生成连续、高分辨率的不规则时间序列。实验表明其有效克服了NCDE的动态模式学习困难，为不规则时序预测提供生成式支持。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 大多时间序列生成方法假设规则采样，与真实不规则数据不符。
method: 提出MN-TSG，结合MOE和NCDE处理不规则观测并实现连续生成。
result: 在不规则时间序列生成任务上优于现有NCDE方法。
conclusion: MN-TSG为不规则时序生成提供了有效框架，可支撑下游预测任务。
---

## Abstract
Time series generation (TSG) contributes to diverse fields (e.g., healthcare), but most methods assume regularly sampled data and fixed outputs—mismatched with real-world settings where observations are irregular and sparse. 
This mismatch is especially problematic in domains such as clinical monitoring, where irregularly recorded data must support downstream tasks with continuous and high-resolution data.
Neural Controlled Differential Equations (NCDEs) show significant potential in handling irregular time series, but still face challenges in learning dynamic temporal patterns and continuous TSG.
To address this, we propose MN-TSG, a framework that explores MOE (Mixture of Experts)-NCDE and integrates it with existing TSG models for irregular or continuous TSG tasks. 
The key designs of MOE-NCDE are the dynamic functions with mixture of experts and the decoupled design to better optimize the MOE dynamics. 
Further, we employ the existing TSG model to learn the joint distribution of the mixture of experts and the time series. In this way, the model can not only generate new samples but also produce suitable experts for them to enable MOE-NCDE for refined continuous TSG tasks. 
We have validated the effectiveness of our method on ten public and synthetic datasets, outperforming advanced TSG baselines in both irregular-to-regular and irregular-to-continuous generation tasks.

---

## 论文详细总结（自动生成）

# 论文中文总结：Continuous Time Series Generation with Irregular Observations（基于不规则观测的连续时间序列生成）

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：现有时间序列生成（TSG）方法多数假设数据为规则采样，且输出为固定分辨率，这与真实世界（如临床监测）中普遍存在的**不规则、稀疏观测**不符，导致生成效果差。
- **背景**：不规则时间序列在下游任务（如连续、高分辨率数据生成）中需求迫切，但现有基于神经控制微分方程（NCDE）的模型在学习动态时序模式和连续生成方面仍存在困难。
- **目标**：提出一种无需规则采样的连续时间序列生成框架，支持**不规则→规则**和**不规则→连续**两种生成任务。

## 2. 方法论
- **核心思想**：将**混合专家（Mixture of Experts, MOE）** 与**神经控制微分方程（NCDE）** 结合，提出 MOE-NCDE，并集成到已有 TSG 模型中，形成 MN-TSG 框架。
- **关键技术细节**：
  - **动态函数**：使用多个专家（expert）组成动态函数，每个专家负责不同时间区域的模式学习，增强对不规则动态的建模能力。
  - **解耦设计**：为更好地优化 MOE 动态，模型将专家分配与连续生成过程解耦，降低优化难度。
  - **联合分布学习**：利用现有 TSG 模型（如扩散模型或 GAN）学习**混合专家分配与时间序列的联合分布**，使生成新样本时能自动为其分配合适专家，进而通过 MOE-NCDE 生成连续、高分辨率的时序数据。
- **算法流程**（文字说明）：
  1. 使用已有 TSG 模型（如 TimeGAN、DiffTime）预训练，学习时序数据的分布。
  2. 在该分布中加入混合专家（MOE）的条件变量，训练**条件生成器**，使其输出同时包含时序样本和对应的专家权重。
  3. 将专家权重喂入 MOE-NCDE 控制器，NCDE 根据控制路径（控制微分方程）连续演化。
  4. 最终输出任意时间点上的插值或外推值，实现连续生成。

## 3. 实验设计
- **使用数据集**：**10 个公开数据集和合成数据集**（具体名称未在摘要中列出，但涵盖医疗、空气质量等不规则时序场景）。
- **Benchmark 任务**：
  - 不规则→规则生成（将不规则观测映射到规则网格）。
  - 不规则→连续生成（生成任意时间点的连续值）。
- **对比方法**：先进的 TSG 基线（如 NCDE 变体、TimeGAN、DiffTime 等）。MN-TSG 在两种任务上均超越基线。

## 4. 资源与算力
- **文中未明确说明**：未报告使用的 GPU 型号、数量、训练时长等算力资源。需注意这一信息缺失，可能影响可复现性。

## 5. 实验数量与充分性
- **数量**：共 10 组数据集，涵盖多种不规则模式，实验量较大。
- **充分性**：包含不规则→规则和不规则→连续两类任务，对比了先进基线；但摘要未提及**消融实验**（如是否对 MOE 数量、解耦设计进行单独验证），也未报告统计显著性测试。实验设计相对充分，但透明度可进一步提升。

## 6. 主要结论与发现
- MN-TSG 框架成功克服了 NCDE 在不规则时间序列动态模式学习上的困难。
- 在 10 个数据集上，MN-TSG 在不规则→规则和不规则→连续生成任务中均显著优于现有方法（如 NCDE 基线）。
- 该框架提供了一种有效途径，可将不规则、稀疏观测转化为连续、高分辨率数据，为下游预测任务（如临床决策）提供生成式支持。

## 7. 优点
- **方法新颖**：首次将 MOE 与 NCDE 结合，解决 NCDE 在复杂动态模式下的学习瓶颈。
- **灵活通用**：可适配现有 TSG 模型（如扩散模型、GAN），不依赖特定生成架构。
- **输出连续**：能生成任意时间点的连续值，优于传统离散输出方法。
- **实验扎实**：在 10 个数据集上验证，覆盖多种不规则场景，结果有竞争优势。

## 8. 不足与局限
- **算力与实现细节缺失**：未说明训练成本，可能限制可复现性和实用评估。
- **实验分析不够深入**：缺乏对 MOE 专家数量、解耦策略的消融研究；未报告运行时间或模型复杂度。
- **应用局限性**：主要针对生成任务，未直接验证生成数据在下游预测任务上的增益（摘要提到“支撑下游预测”但未给出具体下游实验）。
- **被拒稿风险**：该论文标记为 ICLR-2026-Rejected-Public，说明审稿人可能对方法有效性、对比公平性或理论创新性存疑。
- **专家分配可解释性**：MOE 的专家选择机制缺乏可视化或可解释性分析。

（完）
