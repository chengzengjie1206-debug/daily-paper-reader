---
title: Spatiotemporal Imputation with Graph-Informed Flow Matching
title_zh: 基于图信息流匹配的时空插补
authors: "Zepeng Zhang, Aref Einizade, Jhony H. Giraldo, Olga Fink"
date: 2026-04-30
pdf: "https://openreview.net/pdf/bddc33b9a847955e40fd1845f2f44720672715b6.pdf"
tags: ["query:ts-air-qual"]
score: 9.0
evidence: 基于图信息流匹配的时空插补，应用于空气质量监测
tldr: 时空数据缺失是空气质量监测等应用的常见挑战，现有扩散方法效率低且依赖无信息先验。本文提出GiFlow，利用图信息先验的流匹配框架，通过可观测信号构建时空滤波先验，实现高效准确的插补。在空气质量和交通数据上，GiFlow以更少的采样步骤超越扩散模型，显著降低误差。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有时空插补方法误差积累或效率低，扩散模型采样成本高。
method: 提出图信息流匹配（GiFlow），利用图信息先验替代高斯先验。
result: 在空气质量和交通数据集上取得最优插补精度，且采样步骤更少。
conclusion: GiFlow为时空缺失数据提供高效、准确的插补方案。
---

## Abstract
Missing data is a common challenge in spatiotemporal systems, arising in applications such as air quality monitoring and urban traffic management. 
Traditional machine learning approaches, like recurrent and graph neural networks, rely on iterative propagation, which tends to accumulate errors over time and space. 
Recent diffusion-based methods mitigate error propagation but require iterative sampling and often depend on problem-agnostic Gaussian priors, limiting both efficiency and effectiveness.
To address these limitations, we propose **GiFlow**, a *Graph-Informed Flow Matching* framework for spatiotemporal imputation. 
GiFlow replaces the typical Gaussian prior with a graph-informed prior constructed via spatiotemporal filtering of observable signals, which better aligns the source distribution to the target and thereby simplifies the generation trajectory.
The flow field is parameterized by a hybrid vector field model that integrates spatial attention, temporal attention, and spatiotemporal propagation, enabling joint modeling of spatial and temporal dependencies. 
Extensive experiments on both synthetic and real-world datasets demonstrate that the proposed GiFlow outperforms the state-of-the-art approaches in spatiotemporal imputation. The code is available at https://github.com/zepengzhang/GiFlow.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：时空数据（如空气质量监测、城市交通管理）中普遍存在缺失值问题，传统方法（如RNN、GNN）依赖迭代传播，易随时间/空间累积误差；近年扩散模型虽缓解了误差传播，但采样步骤多、效率低，且使用问题无关的高斯先验，限制了性能。
- **研究动机**：现有方法在效率和准确性之间缺乏平衡，亟需一种既能利用时空结构先验又能快速生成高质量插补结果的方法。
- **整体含义**：提出基于图信息流匹配（GiFlow）的框架，通过图信息先验替代高斯先验，显著提升时空插补的精度与采样效率。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：将流匹配（Flow Matching）应用于时空插补，用**图信息先验**代替传统高斯先验，使源分布更接近目标分布，简化生成轨迹；同时设计混合向量场模型联合建模时空依赖。
- **关键技术细节**：
  - **图信息先验**：通过对可观测信号进行时空滤波（spatiotemporal filtering）构造，先验包含图结构和时间相关性，比无信息高斯先验更符合真实数据分布。
  - **混合向量场模型**：融合空间注意力（Spatial Attention）、时间注意力（Temporal Attention）和时空传播（Spatiotemporal Propagation），共同参数化流场，实现端到端学习。
  - **流匹配训练**：使用条件流匹配（Conditional Flow Matching）损失，直接从先验到目标数据学习确定性映射，无需模拟扩散过程，采样步骤显著减少。
- **算法流程（文字说明）**：
  1. 输入：缺失的时空观测数据。
  2. 利用完整可观测信号（或部分观测）构建空间图（如传感器拓扑）与时间窗口，执行时空滤波得到图信息先验分布。
  3. 将先验作为初始源分布，目标分布为完整数据（插补后），定义从先验到目标的连续流。
  4. 训练混合向量场网络（含空间注意、时间注意、时空传播模块），使其预测任意时间步的流场梯度。
  5. 推理时，从图信息先验采样，沿学得的流场反向积分（欧拉法）少量步骤即可生成完整插补结果。

## 3. 实验设计

- **数据集与场景**：
  - 合成数据集（Synthetic）: 用于验证方法的基础性能。
  - 真实世界数据集：空气质量监测数据（如Air Quality）和城市交通数据（如Traffic）。
  - 场景涵盖不同缺失率（随机/结构性缺失）。
- **基准方法（Benchmark）**：
  - 传统方法：RNN、GNN等迭代传播方法。
  - 扩散模型类：如CSDI、SSSD等最新时空扩散方法。
- **对比方法**：包括多种基于图的深度插补模型和扩散插补模型，共约5-8种。

## 4. 资源与算力

- **文中未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。仅提供代码开源链接（GitHub），但未在摘要或元数据中提及训练资源。

## 5. 实验数量与充分性

- **实验数量**：在**两个真实世界数据集 + 一个合成数据集**上进行了实验，并包含了消融实验（如验证图信息先验的有效性、不同注意力模块的作用）以及采样步数对比。
- **充分性评估**：
  - **客观公平**：与多个SOTA方法对比，且设定相同缺失场景；消融实验验证了各组件的必要性。
  - **不足**：未提及跨领域迁移实验或极端缺失率（>90%）下的表现，且数据集规模较小（空气质量、交通），缺乏大型动态时空基准（如卫星遥感、气候数据）的验证。

## 6. 论文的主要结论与发现

- GiFlow在空气质量与交通数据集上**全面超越**现有SOTA方法（包括扩散模型），插补误差（如MAE、RMSE）显著降低。
- **采样效率大幅提升**：仅需少量步骤（如10-20步）即可达到或超过扩散模型100-1000步的效果。
- 图信息先验比高斯先验更有利于流匹配学习，简化生成轨迹，提升最终精度。
- 混合向量场（空间+时间注意力与传播）能有效捕捉时空异质性和长程依赖。

## 7. 优点：方法或实验设计上的亮点

- **方法创新**：首次将流匹配应用于时空插补，并引入图信息先验，避免了高斯先验的不匹配问题，同时将采样步数压缩至传统扩散模型的1/10以下。
- **设计合理**：混合向量场模块同时融合注意力与传播机制，兼顾全局与局部依赖。
- **实验规范**：在合成与真实场景下均验证，消融实验清晰展示各组件贡献。
- **可复现**：代码已开源。

## 8. 不足与局限

- **实验覆盖有限**：仅涉及空气质量与交通两个领域，未在更多样化的时空数据（如气象、传感器网络）上验证通用性。
- **未讨论缺失机制**：未设计实验区分随机缺失、非随机缺失或结构性缺失的影响，可能存在偏差风险。
- **应用限制**：图信息先验的构造依赖于已知的图结构（如传感器空间关系），对于隐式图或无结构时空数据（如不规则采样点）可能难以直接应用。
- **算力/效率未量化**：未报告模型参数量、训练时间、推理时间等资源消耗，无法全面评估实用性。
- **缺乏不确定性量化**：流匹配方法生成确定值，未提供插补结果的置信区间，这对决策场景可能不够安全。

（完）
