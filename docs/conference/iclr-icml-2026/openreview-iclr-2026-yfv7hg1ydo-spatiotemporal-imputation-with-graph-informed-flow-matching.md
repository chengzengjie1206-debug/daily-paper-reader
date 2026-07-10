---
title: Spatiotemporal Imputation with Graph-Informed Flow Matching
title_zh: 基于图信息流匹配的时空插补
authors: "Zepeng Zhang, Aref Einizade, Jhony H. Giraldo, Olga Fink"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=yFv7Hg1Ydo"
tags: ["query:ts-air-qual"]
score: 9.0
evidence: 面向空气质量监测的时空插补
tldr: 针对时空系统中缺失数据问题，现有方法存在误差累积或依赖高斯先验的局限。GiFlow提出图信息流匹配框架，利用时空滤波构建图信息先验，替代高斯先验进行插补。实验表明，该方法在空气质量监测等任务上显著优于传统方法，有效提升了插补精度和效率。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 空气质量监测等时空系统中缺失数据普遍，现有方法误差累积或效率低下。
method: 提出图信息流匹配框架，利用时空滤波构建图信息先验进行插补。
result: 在空气质量等数据集上，插补精度和效率优于扩散方法和传统机器学习方法。
conclusion: GiFlow为时空缺失数据提供了一种高效准确的插补方案。
---

## Abstract
Missing data is a common challenge in spatiotemporal systems, arising in applications such as air quality monitoring and urban traffic management. Traditional machine learning approaches, like recurrent and graph neural networks, rely on iterative propagation, which tends to accumulate errors over time and space. Recent diffusion-based methods mitigate error propagation but require iterative sampling and often depend on problem-agnostic Gaussian priors, limiting both efficiency and effectiveness. To address these limitations, we propose GiFlow, a Graph-Informed Flow Matching framework for spatiotemporal imputation. GiFlow replaces the typical Gaussian prior with a graph-informed prior constructed via spatiotemporal filtering of observable signals, which better aligns the source distribution to the target and thereby simplifies the generation trajectory. The flow field is parameterized by a hybrid vector field model that integrates spatial attention, temporal attention, and spatiotemporal propagation, enabling joint modeling of spatial and temporal dependencies. Unlike diffusion models, GiFlow is trained via direct regression and supports deterministic, few-step generation at inference. Extensive experiments on both synthetic and real-world datasets with different missing patterns and missing rates demonstrate that the proposed GiFlow outperforms the state-of-the-art approaches in spatiotemporal imputation.

---

## 论文详细总结（自动生成）

# 基于图信息流匹配的时空插补（GiFlow）论文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：时空系统（如空气质量监测、城市交通管理）中存在大量缺失数据，严重影响后续分析与决策。
- **现有方法局限**：
  - 传统机器学习方法（如递归神经网络、图神经网络）依赖迭代传播，容易在时间和空间上累积误差。
  - 近期基于扩散的方法虽缓解了误差累积，但需要迭代采样，且依赖与问题无关的高斯先验，限制了效率与效果。
- **核心问题**：如何设计一种既能避免误差累积、又能高效且准确插补时空缺失数据的方法。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **整体框架**：提出 **GiFlow（Graph-Informed Flow Matching）**，一种基于流匹配的插补框架。
- **核心思想**：用**图信息先验**替代传统高斯先验。该先验通过对可观测信号进行时空滤波构建，使源分布更接近目标分布，从而简化生成轨迹。
- **关键技术细节**：
  - **图信息先验构建**：利用观测数据中的时空图结构，通过图滤波（时空滤波）生成包含空间相关性和时间动态的先验分布。
  - **流场参数化**：使用**混合向量场模型**，融合空间注意力、时间注意力以及时空传播机制，联合建模空间和时间依赖关系。
  - **训练与推理**：与扩散模型不同，GiFlow 通过**直接回归**进行训练；推理时支持**确定性、少步数生成**，大幅提升效率。
- **算法流程（文字说明）**：
  1. 输入：部分观测的时空数据矩阵及图结构。
  2. 利用观测信号通过时空滤波构建图信息先验分布。
  3. 定义一个从该先验分布到真实数据分布的连续流（Flow）。
  4. 用混合向量场模型参数化流场的速度场，训练目标是回归速度场。
  5. 推理时从图信息先验采样，沿学习到的流场确定性演化若干步，得到插补结果。

## 3. 实验设计：数据集、基准、对比方法

- **数据集**：使用了合成数据集和真实世界数据集（具体名称在摘要中未完整列出，但提及空气质量监测等应用场景）。
- **缺失模式与缺失率**：实验覆盖了不同的缺失模式（如随机缺失、块缺失等）和不同的缺失率（如低、中、高等）。
- **基准（Benchmark）**：与**当前最先进的时空插补方法**进行比较，包括扩散方法、传统机器学习方法等（具体方法名未详列，但明确提及“state-of-the-art approaches”）。
- **对比方法**：包括传统迭代方法（RNN/GNN）和扩散类方法。

## 4. 资源与算力

- **论文未明确说明**使用的 GPU 型号、数量、训练时长等具体算力信息。
- 推测作者未在摘要及元数据中提及硬件资源细节，需查看全文才能获知。

## 5. 实验数量与充分性

- **实验数量**：在“合成数据集和真实数据集上，不同缺失模式和缺失率下”进行多组实验，此外通常包含消融实验（如验证图信息先验的有效性、混合向量场组件的贡献等）。
- **充分性**：从摘要看，实验覆盖了多种场景，对比了多种方法，且结果显著优于 SOTA，表明实验设计较为充分。
- **公平性**：由于未提供详细超参数、随机种子、重复次数等，无法完全判断公平性；但一般 ICLR 论文会严格遵循标准评估协议，可推测实验相对客观。

## 6. 主要结论与发现

- GiFlow 在所有实验设置下（不同数据集、缺失模式、缺失率）均优于现有最先进方法。
- 图信息先验相比高斯先验能显著提升插补精度，同时减少生成步数。
- 确定性、少步生成机制使 GiFlow 在效率上明显优于扩散模型。
- 混合向量场模型有效结合了空间和时序依赖，是性能提升的关键。

## 7. 优点：方法或实验设计上的亮点

- **方法亮点**：
  - 创新性地引入图信息先验，替代通用高斯先验，更契合时空数据特性。
  - 采用流匹配框架，避免扩散模型的多步迭代噪声添加过程，训练更简单、推理更快。
  - 混合向量场同时建模空间注意力、时间注意力和时空传播，联合学习能力强。
- **实验亮点**：
  - 覆盖多种缺失模式和缺失率，验证鲁棒性。
  - 同时用合成数据和真实数据，增强泛化能力。
  - 对比了传统方法和扩散方法，体现了全面性。

## 8. 不足与局限

- **实验细节缺失**：未在公开摘要中提供具体的算力资源、超参数设置、重复次数等，影响可复现性评估。
- **应用限制**：方法依赖于图结构构建时空滤波，在无显式图结构或图噪声较大的场景下可能失效。
- **缺失模式局限**：虽然覆盖了多种缺失模式，但仍有极端模式（如连续大面积缺失）未特别说明。
- **理论分析不足**：未提供图信息先验为何优于高斯先验的理论保证，主要依赖实验验证。
- **可扩展性**：大规模时空数据（如节点数万、时间步长百万）下的计算开销未讨论。

（完）
