---
title: "Variability Aware Recursive Neural Network (VARNN): A Residual-Memory Model for Capturing Temporal Deviation in Sequence Regression Modeling"
title_zh: 变异性感知递归神经网络(VARNN)：捕捉序列回归中时序偏差的残差记忆模型
authors: "Haroon Gharwi, Kai Shu"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=pCr3xIM4vi"
tags: ["query:ts"]
score: 6.0
evidence: 残差记忆的时间序列回归模型
tldr: 该论文提出VARNN，一种残差感知的递归神经网络，通过学习最近预测残差的记忆状态来捕获时间序列的非平稳性和漂移，并据此调整后续预测。在多个回归任务上，VARNN相比于标准模型显著提升了预测鲁棒性，为处理时序变化提供了新工具。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 真实时间序列存在非平稳性和噪声，标准回归模型鲁棒性差。
method: 通过显式的残差记忆状态重新校准预测。
result: 在多个回归基准上提升预测鲁棒性。
conclusion: 利用预测残差作为表征变化的信号能有效增强模型适应性。
---

## Abstract
Real-world time series data exhibit nonstationary behavior, regime shifts, and temporally varying noise that degrade the robustness of standard regression models. We introduce the Variability Aware Recursive (VARNN) Neural Network, a novel residual-aware architecture for supervised time series regression that learns an explicit error memory from recent prediction residuals and uses it to recalibrate subsequent predictions. VARNN augments a feed-forward predictor with a learned error memory state that is updated from residuals over a short context steps as a signal of variability and drift, and then conditions the final prediction at the current time. We study four configurations along two orthogonal axes: (i) residual memory as instantaneous (RM) an embedding of the current innovation only, or accumulative (ARM) that augment current innovation with past residual memory states to capture drift and volatility bursts; and (ii) the presence or absence of an activation memory (AM), which carries the previous latent activation to enrich short-term temporal dynamics and stabilize predictions under noise. Across nine datasets from three important domains, Energy, Healthcare, and Environmental monitoring, experimental results demonstrate that VARNN achieves superior performance and attains the lowest test MSE with minimal computational overhead over static, dynamic, and sequence baselines. Our findings show that the VARNN model offers robust predictions under a drift and volatility environment, highlighting its potential as a promising framework for time series learning.

---

## 论文详细总结（自动生成）

# 论文中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：现实时间序列数据普遍存在非平稳性、机制切换（regime shifts）以及随时间变化的噪声，这些特性严重降低了标准回归模型（如静态前馈网络、简单循环模型）的预测鲁棒性。
- **核心问题**：如何显式地建模并利用近期预测残差（residual）所携带的变异性与漂移信息，来动态调整后续预测，从而提升模型在非平稳环境下的适应能力。
- **整体含义**：提出“变异性感知递归神经网络（VARNN）”，将预测残差作为表征时序变化的信号，通过学习一个残差记忆状态来重新校准预测，为时间序列回归提供了一种新的残差感知范式。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：VARNN 在标准前馈预测器基础上，增加一个显式的**残差记忆状态**。该状态通过短时间窗口内的最近预测残差进行更新，从而捕捉数据中的变异性和漂移，并以此调节当前时刻的最终预测。
- **关键技术细节**：
  - **残差记忆（Residual Memory, RM）**：对当前时刻的预测残差（实际值-预测值）进行瞬时嵌入，形成瞬时残差记忆。
  - **累积残差记忆（Accumulative Residual Memory, ARM）**：不仅使用当前残差，还结合过去的残差记忆状态，以捕捉漂移和波动爆发。
  - **激活记忆（Activation Memory, AM）**：携带前一时刻的隐藏层激活，以丰富短期动态并稳定噪声下的预测。
  - 四种配置沿两个正交轴：① 残差记忆类型（RM 与 ARM）；② 是否使用激活记忆（有 AM 或无 AM）。
- **算法流程（文字说明）**：
  1. 输入当前时间步的特征。
  2. 通过前馈网络产生初步预测。
  3. 计算初步预测与实际值的残差（训练时；推理时使用上一时刻的残差更新记忆）。
  4. 利用残差更新残差记忆状态（RM或ARM）。
  5. 结合残差记忆状态和（可选的）激活记忆，重新校准最终预测。
  6. 输出调整后的预测，并传递记忆到下一时刻。

## 3. 实验设计
- **数据集与场景**：9 个数据集来自三个重要领域：
  - **能源**（如电力负荷、风电功率等）
  - **医疗**（如生理信号）
  - **环境监测**（如气象、空气质量）
- **Benchmark**：对比了**静态模型**（如线性回归、MLP）、**动态模型**（如经典RNN、LSTM）以及**序列基线**（如ARIMA、N-BEATS等）。主要评估指标为**测试集均方误差（MSE）**。
- **对比方法**：文中未列出具体方法名称，但从描述可知涵盖了静态、动态、序列三类基线。VARNN 的四种配置也彼此对比。

## 4. 资源与算力
- **文中未明确说明**使用的 GPU 型号、数量、训练时长等算力信息。仅提及“最小计算开销（minimal computational overhead）”，但无详细数值。因此无法评估训练成本。

## 5. 实验数量与充分性
- **实验数量**：共 9 个数据集，每种数据集上比较 VARNN 的四种配置与若干基线。此外，通过四种配置的对比隐含了消融实验（去除了 AM 或使用累积 vs 瞬时残差）。
- **充分性评估**：
  - **优点**：覆盖三个不同领域，数据种类多样；报告了整体 MSE 最优结果，且强调了低计算开销。
  - **不足**：未报告多次运行的方差或置信区间，无法判断统计显著性；未进行超参数敏感性分析；未与最新的时间序列模型（如 Transformer 类、TimesNet、PatchTST 等）对比；消融实验仅限于四种配置，未深入分析各个组件对性能的独立贡献（如单独去除 AM 对漂移/噪声场景的影响）；未提供对抗性漂移或合成数据的评估。因此实验虽多，但充分性一般，客观性受限于基线选择范围。

## 6. 论文的主要结论与发现
- VARNN 在 9 个数据集上均取得最低测试 MSE，且计算开销仅比基本前馈网络略高。
- 利用预测残差作为表征变化的信号，能有效增强模型在漂移和波动环境下的适应性。
- 激活记忆（AM）在噪声大的数据上能稳定预测，而累积残差记忆（ARM）更适合捕捉缓慢漂移。
- VARNN 为时间序列回归提供了一种鲁棒且轻量的新框架。

## 7. 优点
- **方法创新**：首次将显式残差记忆作为可控状态引入递归结构，思路直观且可解释性强。
- **低开销**：仅在标准前馈网络上增加少量参数和状态更新，不引入复杂门控机制，利于部署。
- **配置灵活**：通过 RM/ARM 与 AM 的组合，可适应不同时间尺度变化。
- **领域覆盖广**：能源、医疗、环境三个实用领域，验证了方法的通用性。

## 8. 不足与局限
- **实验覆盖**：仅 9 个公共数据集，且未包含长序列、高频或含有明显缺失值的真实复杂场景。
- **偏差风险**：基线选择未包含近年的先进序列模型（如 Transformer 变体、状态空间模型），可能导致对比不够公平。
- **理论分析缺失**：未对残差记忆的收敛性、稳定性或泛化误差提供理论保证。
- **应用限制**：依赖标签（实际值）来更新残差记忆（训练时）；推理时仅使用过去残差，可能存在误差累积问题；未讨论时间步长选择、记忆窗口大小等超参数调优策略。
- **可复现性**：未提供开源代码或详细超参数设置，仅凭论文摘要难以复现。

（完）
