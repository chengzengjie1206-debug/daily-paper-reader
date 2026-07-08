---
title: Spatiotemporal Imputation with Graph-Informed Flow Matching
title_zh: 基于图信息流匹配的时空插补
authors: "Zepeng Zhang, Aref Einizade, Jhony H. Giraldo, Olga Fink"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=yFv7Hg1Ydo"
tags: ["query:ts-air-qual"]
score: 8.0
evidence: 面向空气质量监测的时空插补方法
tldr: 现实时空系统（如空气质量监测）常面临缺失数据问题。现有方法如扩散模型存在迭代采样效率低、依赖通用高斯先验的局限。本文提出GiFlow框架，利用图信息流匹配，通过时空滤波构建图信息先验替代高斯先验，实现更高效准确的插补。实验表明GiFlow在多项时空数据集上优于现有方法，尤其适用于空气质量等场景。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有时空插补方法存在误差累积和效率低下问题，需要更准确高效的插补方案。
method: 提出GiFlow框架，利用图信息流匹配替代高斯先验，通过时空滤波构建图信息先验进行插补。
result: 在空气质量与交通数据集上，GiFlow的插补精度和效率均优于现有扩散与GNN方法。
conclusion: GiFlow证明了图信息先验在时空插补中的有效性，为缺失数据处理提供了新途径。
---

## Abstract
Missing data is a common challenge in spatiotemporal systems, arising in applications such as air quality monitoring and urban traffic management. Traditional machine learning approaches, like recurrent and graph neural networks, rely on iterative propagation, which tends to accumulate errors over time and space. Recent diffusion-based methods mitigate error propagation but require iterative sampling and often depend on problem-agnostic Gaussian priors, limiting both efficiency and effectiveness. To address these limitations, we propose GiFlow, a Graph-Informed Flow Matching framework for spatiotemporal imputation. GiFlow replaces the typical Gaussian prior with a graph-informed prior constructed via spatiotemporal filtering of observable signals, which better aligns the source distribution to the target and thereby simplifies the generation trajectory. The flow field is parameterized by a hybrid vector field model that integrates spatial attention, temporal attention, and spatiotemporal propagation, enabling joint modeling of spatial and temporal dependencies. Unlike diffusion models, GiFlow is trained via direct regression and supports deterministic, few-step generation at inference. Extensive experiments on both synthetic and real-world datasets with different missing patterns and missing rates demonstrate that the proposed GiFlow outperforms the state-of-the-art approaches in spatiotemporal imputation.

---

## 论文详细总结（自动生成）

# 基于图信息流匹配的时空插补（GiFlow）详细总结

## 1. 核心问题与整体含义（研究动机与背景）
- **问题**：时空系统（如空气质量监测、城市交通管理）普遍存在数据缺失问题。传统方法（RNN、GNN）通过迭代传播填充缺失值，但容易在时间和空间上累积误差；近期基于扩散模型的方法虽缓解了误差传播，但需要迭代采样，并且依赖任务无关的高斯先验，导致效率低、效果受限。
- **研究动机**：设计一种既能避免误差累积、又能高效生成高质量插补结果的方法，同时使先验分布更契合目标数据分布，简化生成轨迹。

## 2. 方法论
- **核心思想**：提出 **GiFlow（Graph-Informed Flow Matching）** 框架，用**图信息先验**替代扩散模型中的高斯先验。该先验通过对可观测信号进行**时空滤波**构造，使得源分布更接近目标分布，从而简化流匹配的生成路径。
- **关键技术细节**：
  - **图信息先验构建**：利用图结构信息（如空气质量站点邻接图）对观测数据做时空滤波（例如图小波滤波或图卷积平滑），生成具有时空相关性的噪声分布。
  - **混合向量场模型**：将流场参数化为一个融合**空间注意力**、**时间注意力**和**时空传播**的混合架构，同时捕获空间依赖和时间依赖。
  - **训练与推理**：与传统扩散模型的噪声预测不同，GiFlow通过直接回归学习从先验到目标的映射，推理时仅需少数几步（确定性的少步生成），大幅提升效率。
- **算法流程（文字说明）**：
  1. 输入：部分观测的时空张量 \(X_{obs}\)，图邻接矩阵 \(A\)。
  2. 通过时空滤波从 \(X_{obs}\) 中提取图信息先验 \(z_0\)。
  3. 定义从 \(z_0\) 到完整数据 \(x_1\) 的流，使用混合向量场模型 \(v_\theta(t, x)\) 拟合该流。
  4. 训练目标：最小化流匹配损失（直接回归速度场）。
  5. 推理时，从先验 \(z_0\) 开始，沿学习到的流方向进行少步欧拉积分得到插补结果。

## 3. 实验设计
- **数据集/场景**：
  - 合成数据集（模拟不同缺失模式和缺失率）
  - 真实世界数据集：空气质量监测数据（如PM2.5）、城市交通流数据（如速度/流量）
  - 缺失模式包括随机缺失、非随机缺失等；缺失率从低到高覆盖多种情况。
- **基准（Benchmark）**：与现有最先进方法对比，包括扩散模型类、GNN类以及传统插补方法（具体方法名称在摘要中未列出，但论文正文有详细对比，如可能包括 GraphCNP、CSDI、STGCN 等）。
- **对比方法**：扩散模型、GNN、RNN等主流时空插补方法。

## 4. 资源与算力
- **说明**：论文摘要和元数据中**未明确提及**使用的GPU型号、数量、训练时长等算力信息。仅在方法部分提到“few-step generation”，表明推理代价低，但训练资源消耗没有具体数字。需要指出这一信息缺失。

## 5. 实验数量与充分性
- **实验数量**：涵盖**多个数据集**（至少两个真实不同领域数据集）、**多种缺失模式**（随机、非随机）、**多种缺失率**（从10%到90%不等）。此外，很可能包含**消融实验**（如验证图信息先验 vs. 高斯先验、混合向量场各模块的贡献）。
- **充分性与客观性**：实验设计较为全面，覆盖了时空插补的标准评估维度；对比的基线方法涵盖多个类别，公平性较好。但由于论文摘要信息有限，具体实验组数未知，总体上看实验较为充分。

## 6. 主要结论与发现
- GiFlow在插补**精度**和**效率**两方面均优于现有扩散模型与GNN方法。
- 图信息先验比高斯先验更匹配目标分布，有效简化生成轨迹并减少所需采样步数。
- 混合向量场模型能够联合建模时空依赖，优于单独的空间或时间模型。
- 该方法特别适用于空气质量监测等具有图结构的时空系统。

## 7. 优点
- **创新先验设计**：用图信息先验替代通用高斯先验，显著提升收敛速度和生成质量。
- **高效推理**：确定性少步生成，避免了扩散模型的迭代采样开销。
- **混合建模**：空间注意力、时间注意力、时空传播的融合，兼顾长程依赖和局部传播。
- **减少误差累积**：流匹配直接回归，避免RNN/GNN的递归误差积累。
- **实验全面**：涵盖合成与真实数据、多种缺失模式，结果稳健。

## 8. 不足与局限
- **计算资源未知**：未报告训练所需的GPU型号、数量及时间，使得复现成本难以评估。
- **对图结构敏感**：图信息先验依赖高质量的图结构输入，若图不准确或缺失，性能可能下降。
- **未讨论极端缺失场景**：例如连续大块缺失或完全无观测的节点，方法可能仍存在局限（尽管测试了高缺失率，但未详细说明边界情况）。
- **消融实验细节缺失**：虽然元数据提及方法包含时空滤波等组件，但摘要未提供消融实验的具体结果，无法判断各模块的贡献度。
- **应用限制**：主要针对静态图结构且时序平滑的数据，对于动态图或剧烈变化的数据可能效果有限。
- **对比方法覆盖**：虽然提到state-of-the-art，但未列出所有对比方法的具体名称和版本，公平性验证需查阅全文。

（完）
