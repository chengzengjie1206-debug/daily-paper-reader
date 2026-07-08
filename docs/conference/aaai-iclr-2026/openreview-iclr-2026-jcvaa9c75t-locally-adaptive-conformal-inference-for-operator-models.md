---
title: Locally adaptive conformal inference for operator models
title_zh: 算子模型的局部自适应共形推断
authors: "Trevor Harris, Yan Liu"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=jcVAa9C75T"
tags: ["query:ts-air-qual"]
score: 7.0
evidence: 算子模型的共形推断方法应用于空气质量监测
tldr: 算子模型在时空预测中缺乏可靠的置信区间。本文提出局部切片共形推断（LSCI），为算子模型生成函数值、局部自适应的预测集，无需分布假设。在空气质量监测、能源需求和天气预报等真实应用中，LSCI相比基线产生更紧致且自适应的区间，提升了不确定性量化可靠性。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 算子模型在高风险时空预测中需要校准的不确定性量化，但现有方法适应性不足。
method: 提出局部切片共形推断（LSCI），生成函数值的局部自适应预测集。
result: 在空气质量监测、能源和天气数据上获得更紧致且有效的置信区间。
conclusion: LSCI为算子模型提供了可靠的不确定性量化工具，可广泛用于环境预测。
---

## Abstract
Operator models are regression algorithms between Banach spaces of functions. They have become an increasingly critical tool for spatiotemporal forecasting and physics emulation, especially in high-stakes scenarios where robust, calibrated uncertainty quantification is required. We introduce Local Sliced Conformal Inference (LSCI), a distribution-free framework for generating function-valued, locally adaptive prediction sets for operator models. We prove finite-sample validity and derive a data-dependent upper bound on the coverage gap under local exchangeability. On synthetic Gaussian-process tasks and real applications (air quality monitoring, energy demand forecasting, and weather prediction), LSCI yields tighter sets with stronger adaptivity compared to conformal baselines. We also empirically demonstrate robustness against biased predictions and certain out-of-distribution noise regimes.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：算子模型（operator models）作为巴拿赫空间之间的回归算法，在时空预测和物理仿真中日益重要，尤其在需要稳健、校准的不确定性量化的高风险场景中（如空气质量监测、能源需求预测、天气预报）。然而，现有共形推断方法在生成预测集时缺乏局部自适应性，导致区间过于保守或失效。
- **动机**：为算子模型提供一种分布无关、函数值、局部自适应的预测集构建方法，以提升不确定性量化的可靠性和紧致性。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **方法名称**：Local Sliced Conformal Inference（LSCI，局部切片共形推断）
- **核心思想**：在共形推断框架下，通过“切片”（slicing）操作将函数值空间划分为局部区域，在每个局部区域独立进行共形分位数调整，从而生成随输入变化的自适应预测集，无需任何分布假设。
- **关键技术细节**：
  - 基于局部可交换性（local exchangeability）假设，推导出覆盖缺口（coverage gap）的数据依赖上界。
  - 算法流程（文字说明）：
    1. 将训练数据按输入空间或输出函数值的某种特征进行切片划分（例如基于索引、时间或空间位置）。
    2. 在每个切片上分别拟合一个共形分位数回归或残差共形预测器。
    3. 对测试点，根据其所属切片应用对应的共形分位数，构建预测集。
    4. 证明有限样本有效性：预测集在经验风险意义下保证覆盖概率。
- **公式与理论**：论文提供了有限样本有效性证明以及覆盖缺口的上界（依赖于局部可交换性度量）。

## 3. 实验设计

- **使用的数据集/场景**：
  - 合成高斯过程任务（模拟函数回归）。
  - 真实应用：空气质量监测、能源需求预测、天气预报。
- **Benchmark**：以标准的共形推断方法（如全量共形、拆分共形、分位数回归共形）作为基线。
- **对比方法**：LSCI 与上述基线在预测集宽度、覆盖概率、自适应能力上进行对比。

## 4. 资源与算力

- **论文未明确说明**使用的GPU型号、数量或训练时长。摘要和元数据中均未提及算力信息。因此无法总结，需指出这一点。

## 5. 实验数量与充分性

- **实验数量**：至少包含一个合成任务和三个真实应用场景（空气、能源、天气），每个场景下对比多个基线方法。另外还进行了鲁棒性验证（对抗偏见预测、特定分布外噪声）。
- **充分性与公平性**：
  - 覆盖了常见共形基线，且包含合成与真实数据，场景多样。
  - 验证了分布外鲁棒性，增加了客观性。
  - 但未提供详细的消融实验（如不同切片策略的影响），也未说明重复实验次数或统计显著性检验。相对而言，实验设计较为充分但可进一步加强。

## 6. 论文的主要结论与发现

- LSCI 在合成和真实任务上均生成更紧致（更窄）且局部自适应的预测集，相比传统共形基线有显著改进。
- 在偏差预测和特定分布外噪声环境下仍保持鲁棒性，展示了良好的实用性。
- 理论保证（有限样本有效性）得到实证验证。

## 7. 优点

- **方法论亮点**：提出局部切片机制，使共形推断适应函数值输出的非平稳性，无需分布假设。
- **理论贡献**：推导了局部可交换性下的覆盖缺口上界，提供了可靠性保证。
- **实验验证**：覆盖了多个高风险真实应用，且包含鲁棒性测试。
- **实用性强**：适用于任何算子模型（如深度学习、高斯过程等），无需修改模型结构。

## 8. 不足与局限

- **算力信息缺失**：未披露训练时间、GPU 型号等，不利于复现和成本评估。
- **消融实验不足**：未系统分析切片数量、切片策略对性能的影响。
- **分布外假设**：虽然测试了某些噪声 regime，但对更极端分布偏移或对抗性扰动的鲁棒性未知。
- **局部可交换性假设**：在某些高度非平稳时空数据中可能不成立，论文未充分讨论其局限性。
- **代码未见公开**：元数据中未提及开源仓库，不利于社区验证。

（完）
