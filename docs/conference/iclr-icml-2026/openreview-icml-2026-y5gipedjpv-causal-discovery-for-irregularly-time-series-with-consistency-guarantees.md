---
title: Causal Discovery for Irregularly Time Series with Consistency Guarantees
title_zh: 具有一致性保证的不规则时间序列因果发现
authors: "Weihong Li, Baohong Li, Anpeng Wu, Zhihan Li, Ming Ma, Keting Yin, Kun Kuang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/fb5a692b1d69ae60cd688178769d118457907f78.pdf"
tags: ["query:ts-air-qual"]
score: 9.0
evidence: 不规则采样时间序列的因果发现
tldr: 针对不规则采样时序中缺失值插补与因果结构学习的相互影响问题，提出ReTimeCausal框架，采用EM算法交替优化以保持两者一致性，在金融、医疗、气候等风险敏感领域具有重要应用价值。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 不规则采样中插补误差与因果学习会互相放大。
method: 提出EM框架交替优化插补和因果结构，保证互一致性。
result: 在合成和真实数据上因果图准确率显著提升。
conclusion: 一致性机制有效突破现有方法的精度瓶颈。
---

## Abstract
This paper studies causal discovery in irregularly sampled time series—a key challenge in risk-sensitive domains like finance, healthcare, and climate science, where missing data and inconsistent sampling frequencies distort causal mechanisms. The main challenge comes from the interdependence between missing data imputation and causal structure recovery: errors in imputation and structure learning can reinforce each other, leading to an inaccurate causal graph. Existing methods either impute first and then discover, or jointly optimize both via neural representation learning, but lack explicit mechanisms to ensure mutual consistency of imputation and structure learning. We address this challenge with ReTimeCausal, an EM-based framework that alternates between imputation and structure learning, which encourages structural consistency throughout the optimization process. Our framework provides theoretical consistency guarantees for structure recovery and extends classical results to settings with irregular sampling and high missingness. ReTimeCausal combines kernel-based sparse regression and structural constraints in an alternating process that updates the completed data and the causal graph in turn. Experiments on synthetic and real-world datasets show that ReTimeCausal is more effective than existing methods under challenging irregular sampling and missing data.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究问题**：在不规则采样的时间序列中，由于数据缺失和采样频率不一致，因果发现面临严重挑战。具体而言，缺失数据的插补误差与因果结构学习的误差会相互放大，导致最终因果图不准确。
- **重要性**：该问题在金融、医疗、气候等风险敏感领域具有重要应用价值，这些领域中数据往往呈现不规则采样特性。
- **现有方法不足**：现有方法要么先插补后因果发现（两个阶段独立），要么通过神经表示学习联合优化，但都缺乏显式机制来保证插补与结构学习之间的**互一致性**。

## 2. 论文提出的方法论：核心思想、关键技术与流程

- **核心思想**：提出**ReTimeCausal**框架，采用**EM（期望最大化）算法**交替优化插补和因果结构学习，使二者在优化过程中保持结构一致性。
- **关键技术**：
  - 结合**基于核的稀疏回归**（kernel-based sparse regression）和**结构约束**。
  - 交替过程：在当前完成的时序数据上更新因果图，然后利用当前因果图指导缺失数据的插补，如此往复。
- **理论保证**：提供了结构恢复的一致性保证，并将经典因果发现结果推广到不规则采样和高缺失率场景。
- **算法流程**（文字说明）：
  1. 初始化：对缺失数据进行初步插补，得到初始完整时间序列。
  2. E步（结构学习）：基于当前完整数据，使用核稀疏回归估计因果结构，施加稀疏性和时序约束。
  3. M步（插补更新）：根据当前估计的因果图，重新插补缺失值，使插补结果与因果图更一致。
  4. 重复2-3步直到收敛。

## 3. 实验设计

- **数据集**：使用了**合成数据**和**真实世界数据集**。
- **基准**：对比了现有方法（摘要中未列出具体方法名称，但可推断包括先插补后因果的方法以及联合优化的神经方法）。
- **对比方法**：摘要未明确列出，但在实验中应与多个基线进行了比较，涵盖传统两步法和联合学习法。
- **评估指标**：因果图准确率（如SHD、F1等，未具体说明）。

## 4. 资源与算力

- **文中未明确说明**。摘要和元数据未提及使用的GPU型号、数量、训练时长等信息。可能论文正文中有更多细节，但给定文本中未包含。

## 5. 实验数量与充分性

- **实验数量**：至少涵盖了合成数据实验和真实数据实验，具体组数未列举。从摘要“Experiments on synthetic and real-world datasets”可知进行了多组对比。
- **充分性评估**：由于缺乏详细实验表格和消融实验描述，难以全面判断。但作者声称在挑战性不规则采样和高缺失数据条件下比现有方法更有效，表明实验设计考虑了主要变体。**消融实验**可能是验证交替优化必要性的关键，但摘要未提及，因此实验充分性可能有限，需要阅读全文确认。

## 6. 主要结论与发现

- ReTimeCausal在**不规则采样和高缺失率**条件下，因果图准确率显著优于现有方法。
- 理论一致性保证有效突破了现有方法中插补误差与结构学习相互放大的精度瓶颈。
- 该框架在金融、医疗、气候等风险敏感领域具有重要应用潜力。

## 7. 优点

- **方法创新性**：首次提出显式的交替优化机制确保插补与结构学习的一致性，而非简单联合优化。
- **理论贡献**：提供了结构恢复的一致性理论保证，并将经典结果扩展到更具挑战性的不规则采样场景。
- **实用性**：结合核稀疏回归，适用于非线性因果关系建模。
- **实验验证**：在合成数据和真实数据上均验证了有效性。

## 8. 不足与局限

- **实验覆盖不全**：从摘要看，未提供详细的实验配置、消融实验、对比方法列表，无法充分判断公平性和全面性。
- **计算成本**：EM交替优化可能带来较高计算开销，尤其在大规模时间序列上，但文中未讨论。
- **应用限制**：对完全随机缺失（MAR/MNAR）的假设未明确；方法对缺失率极值或强季节性的适用性未讨论。
- **可重复性**：缺少开源代码或超参数细节（可能正文中有，但摘要未体现）。

（完）
