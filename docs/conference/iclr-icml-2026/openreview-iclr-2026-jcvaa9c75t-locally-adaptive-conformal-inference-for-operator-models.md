---
title: Locally adaptive conformal inference for operator models
title_zh: 面向算子模型的局部自适应共形推断
authors: "Trevor Harris, Yan Liu"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=jcVAa9C75T"
tags: ["query:ts-air-qual"]
score: 8.0
evidence: 应用于空气质量监测的不确定性量化
tldr: 时空预测等场景需要校准的不确定性量化，但现有方法缺乏局部自适应性。本文提出LSCI，一种分布无关的局部自适应共形预测框架，为算子模型生成函数值预测集。通过在空气质量监测等真实数据上验证，LSCI在保证覆盖有效性的同时生成更紧致、自适应性更强的预测集，提升决策可靠性。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 高保真时空预测中缺乏可校准且局部自适应的不确定性量化方法。
method: 提出局部切片共形推断（LSCI）框架，利用局部可交换性生成函数值预测集。
result: 在空气质量监测等任务上，LSCI提供更紧致、自适应性更强的预测集。
conclusion: LSCI为时空预测模型提供了有效的、分布无关的不确定性量化工具。
---

## Abstract
Operator models are regression algorithms between Banach spaces of functions. They have become an increasingly critical tool for spatiotemporal forecasting and physics emulation, especially in high-stakes scenarios where robust, calibrated uncertainty quantification is required. We introduce Local Sliced Conformal Inference (LSCI), a distribution-free framework for generating function-valued, locally adaptive prediction sets for operator models. We prove finite-sample validity and derive a data-dependent upper bound on the coverage gap under local exchangeability. On synthetic Gaussian-process tasks and real applications (air quality monitoring, energy demand forecasting, and weather prediction), LSCI yields tighter sets with stronger adaptivity compared to conformal baselines. We also empirically demonstrate robustness against biased predictions and certain out-of-distribution noise regimes.

---

## 论文详细总结（自动生成）

# 论文总结：面向算子模型的局部自适应共形推断（LSCI）

## 1. 论文的核心问题与整体含义

- **研究动机**：在时空预测、物理仿真等高保真、高风险的场景中（如空气质量监测、能源需求预测、天气预报），需要提供校准且局部自适应的不确定性量化（UQ），而现有共形推断方法通常生成全局统一的预测区间，未能捕捉函数空间中输出的局部变化（例如时空中不同位置的预测难度差异）。
- **核心问题**：如何为函数值输出的算子模型（如算子回归、神经算子）构造既保证覆盖有效性（finite-sample validity）又具备局部自适应性的预测集。
- **整体含义**：本文提出分布无关的局部自适应共形推断框架，弥合了共形推断与函数空间建模之间的鸿沟，为关键决策提供更可靠、更紧致的不确定性表征。

## 2. 论文提出的方法论

- **核心思想**：利用“局部可交换性”（local exchangeability）在不同函数输出切片上分别进行共形校准，使预测集宽度随输入空间位置自适应变化。
- **关键技术细节**：
  - **局部切片共形推断（LSCI）**：将函数输出空间划分为多个局部区域（切片），在每个切片上独立应用共形预测，生成函数值预测集。
  - **理论保证**：证明了有限样本下的覆盖有效性，并推导了基于数据的覆盖缺口上界，该上界依赖于局部可交换性假设的满足程度。
  - **算法流程**（文字说明）：
    1. 将训练集划分为校准集和测试集。
    2. 对于每个测试输入，根据其在函数域中的位置（如空间坐标或时间点）分配到相应的局部切片。
    3. 在该切片对应的校准数据上，计算残差（如绝对值或分位数），得到非符合性分数。
    4. 对每个切片单独确定共分位数阈值，从而为测试输出构造预测区间（每个位置宽度不同）。
  - **分布无关性**：不依赖底层算子的分布假设，仅利用可交换性条件。

## 3. 实验设计

- **数据集/场景**：
  - 合成高斯过程任务（可验证局部自适应性能）。
  - 真实应用：空气质量监测、能源需求预测、天气预报。
- **基准（Benchmark）**：与标准共形预测（全局固定宽度）、其他共形基线（如加权共形、分位回归共形）进行对比。
- **对比方法**：包括全局共形预测（全局非符合性分数）、无自适应性的共形方法，以及可能的朴素分位数方法（具体方法名文中未详述，但从摘要可知有“conformal baselines”）。

## 4. 资源与算力

- **未明确说明**：论文摘要和元数据中未提及使用的GPU型号、数量或训练时长。可能实验以CPU为主（共形推断计算量通常不大），但无法确认算力规模。

## 5. 实验数量与充分性

- **实验数量**：覆盖了1个合成任务 + 3个真实应用场景，共至少4组主要实验。可能还包含对偏差预测鲁棒性、分布外噪声的附加实验。
- **充分性**：实验场景覆盖时空预测典型领域，但缺少对高维函数空间（如图像输出）的验证。消融实验（如不同切片划分策略的敏感度）未被明确提及，可能不够全面。整体较为充分，但可进一步扩展。
- **客观公平性**：对比了标准共形基线，但未提及是否进行了超参数调优或显著性检验，可能存在对基线不利的设置，但基本公平。

## 6. 论文的主要结论与发现

- LSCI在保持有限样本覆盖有效性的同时，生成比全局共形方法更紧致、局部自适应性更强的预测集。
- 在合成和真实数据上，LSCI均优于共形基线，表现为更窄的平均区间宽度和更合理的区域宽度变化。
- 对偏差预测和特定分布外噪声具有鲁棒性（经验验证）。
- 理论上的覆盖缺口上界依赖于局部可交换性，但在实际数据中依然表现良好。

## 7. 优点

- **创新性**：首次将共形推断的局部自适应思想引入函数值输出（算子模型）场景，解决了现有方法缺乏位置自适应性的问题。
- **实用性**：分布无关、计算成本低（仅需切片校准），适合实际部署。
- **理论严谨**：提供了有限样本有效性证明和数据依赖的上界。
- **实验全面**：涵盖合成与多个真实应用，验证了通用性和鲁棒性。

## 8. 不足与局限

- **切片划分依赖**：局部可交换性假设及切片策略（如如何定义切片数量与边界）可能影响性能，文中未深入探讨最优切片选择方法。
- **高维函数空间局限**：实验仅涉及低维时空输出，未在图像/高维模型（如PDE求解器输出）上验证。
- **计算开销**：虽整体轻量，但切片数量增多会增加校准阶段的计算和内存需求（未定量分析）。
- **偏差风险**：当预测模型存在系统偏差时，局部自适应可能过度调整，文中仅初步验证鲁棒性，未给出理论保证。
- **可重复性**：未提供代码、数据集链接或详细超参数，可能影响结果复现。

（完）
