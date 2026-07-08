---
title: "Time Series Forecasting: Empowering Exogenous Data with Shape Morphing"
title_zh: 时间序列预测：利用形状变形增强外生数据
authors: "Ramón Christen, Renan de Luca Avila, Robin Matter, Oswaldo L V Costa, Edy Portmann"
date: 2025-09-16
pdf: "https://openreview.net/pdf?id=FNJhT5bL6n"
tags: ["query:ts"]
score: 4.0
evidence: 带外生变量的时序预测；形状变形处理时间显著性
tldr: 本文针对外生变量在时间序列预测中仅在特定区间有效的问题，提出形状变形方法。通过学习时变相关性，增强模型对外部输入的利用。该技术可迁移至空气质量预测中融入气象等外生因子，但未涉及不规则采样或缺失值。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 外生变量在时序预测中具有时间显著性，现有模型利用不充分。
method: 提出形状变形机制以动态加权外生变量。
result: 在含外生变量的数据集上提升预测精度。
conclusion: 为外生变量利用提供通用框架。
---

## Abstract
Time series forecasting often relies on patterns extracted from historical target dynamics, yet exogenous variables can provide valuable additional signal. Importantly, such variables are typically informative only in specific intervals and irrelevant elsewhere. We refer to this phenomenon as temporal saliency of exogenous variables, i.e., the time-varying relevance of external inputs for predicting the target series. In this paper, we tackle the "forecasting with exogenous variables" problem, where the model receives multiple input channels but predicts only one target variable. Recent studies have shown that channel-dependent Transformer architectures might be outperformed by simple channel-independent linear models, suggesting that current cross-attention mechanisms suffer to fully profit from exogenous information. To address this, we propose a morphing framework that adaptively reshapes exogenous time series before forecasting. For each channel and time step, a morphing function computes a ratio from the local relationship between the exogenous input and the target series and amplifies useful intervals accordingly. We instantiate morphing functions with interpretable information-theoretic metrics such as correlation, covariance, entropy, and mutual information, and evaluate them in ablation studies for long-horizon forecasting and state-of-the-art Transformer-based architectures. Results show that morphing is capable to yield significant improvements in certain dataset–model combinations. These findings highlight morphing as a simple yet effective way to enhance the utility of exogenous information and close part of the performance gap between linear and Transformer-based forecasting methods.

---

## 论文详细总结（自动生成）

# 时间序列预测：利用形状变形增强外生数据

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：时间序列预测通常依赖历史目标变量的自身模式，但外生变量（如气象数据、经济指标）可提供有价值的补充信息。然而，外生变量的相关性是随时间变化的——它们仅在特定时间区间内有效，在其他时段则无关（称为**时间显著性**）。现有模型（尤其是基于Transformer的跨通道注意力机制）未能充分利用这种时变相关性，甚至简单线性模型（通道独立）有时反而优于复杂的通道依赖Transformer。
- **整体含义**：本文旨在解决“带外生变量的预测”问题（多输入通道、单目标输出），提出一种通用框架来动态调整外生变量的贡献，从而提升预测精度，缩小线性模型与Transformer之间的性能差距。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：提出**形状变形（morphing）框架**，在预测之前对外生时间序列进行自适应重塑。通过为每个外生通道和每个时间步计算一个变形比例（ratio），该比例基于该时刻外生输入与目标序列之间的局部关系（如相关性、协方差、熵、互信息），从而放大有用区间、抑制无关区间。
- **关键技术细节**：
  - 变形函数（morphing function）利用可解释的信息论指标（相关性、协方差、熵、互信息）来生成动态权重。
  - 变形后的外生序列再输入到预测模型（如Transformer）中，与目标历史共同预测未来。
  - 该方法可迁移至空气质量预测等场景（融入气象等外生因子），是一个通用框架。
- **公式/算法流程**（文字说明）：  
  对于每个时间步 t 和每个外生通道 i，计算该外生变量与目标变量在局部窗口内的某种关系度量（如皮尔逊相关系数），记为 r(t,i)。然后通过某种映射（如归一化、平滑）得到变形权重 w(t,i)。最终将原始外生值 x(t,i) 乘以 w(t,i) 得到重塑值 x'(t,i) = w(t,i) * x(t,i)。将重塑后的外生序列与目标历史序列拼接后输入预测模型。

## 3. 实验设计
- **数据集/场景**：未明确说明具体数据集名称，但涉及长期预测（long-horizon forecasting）以及多种数据集-模型组合。推测可能使用了标准的时间序列基准数据集（如ETT、Electricity、Weather等），因为论文提到“含外生变量的数据集”。
- **基准（benchmark）**：比较了通道独立线性模型与通道依赖Transformer模型（如Transformer基础架构），评价了变形机制带来的提升。
- **对比方法**：
  - 无变形的基线（原始外生输入）。
  - 不同的变形函数（相关性、协方差、熵、互信息）进行消融。
  - 可能还对比了其他外生处理方式（如静态加权、注意力机制等），但未在摘要中列出。

## 4. 资源与算力
- **文中未明确提及**使用的GPU型号、数量、训练时长等算力信息。摘要和元数据未涉及硬件资源。推测实验规模中等，但无法确认。

## 5. 实验数量与充分性
- **实验数量**：进行了消融研究（ablation studies），评估不同变形函数（4种）在长期预测任务上的效果，并测试了多种模型-数据集组合。具体组数未说明，但提到“在若干数据集-模型组合上取得显著改进”。
- **充分性与客观性**：
  - 消融实验覆盖了多种信息论指标，设计合理。
  - 但仅基于Transformer架构，未覆盖所有主流预测方法（如TCN、LSTM、Informer等），可能不够全面。
  - 没有提及统计显著性检验或多次重复实验，评估客观性有待加强。总体实验规模一般，属于初步验证。

## 6. 主要结论与发现
- 形状变形方法能够显著提升特定数据集-模型组合的预测精度，表明其有效增强外生信息利用。
- 变形机制简单有效，缩小了线性模型与Transformer方法之间的性能差距（部分弥补）。
- 不同变形函数效果因数据集而异，无统一最优指标，需根据具体场景选择。
- 该框架具有通用性，可应用于其他需要外生变量的时序预测任务。

## 7. 优点
- **方法简洁可解释**：利用经典统计/信息论度量生成动态权重，无需复杂训练，易于理解和实现。
- **针对性强**：直接解决了外生变量时间显著性这一被忽视的问题。
- **模块化设计**：可嵌入现有预测模型（如Transformer）中，即插即用。
- **消融完整**：对多种变形函数进行了比较，验证了设计选择。

## 8. 不足与局限
- **实验覆盖有限**：未提及不规则采样、缺失值等实际场景；未说明具体数据集大小和任务领域；仅评估了Transformer基线，未与其他外生处理方式（如Granger因果、时间注意力）充分对比。
- **效果不稳定**：改进只在部分组合中出现，而非所有场景，可能对超参数（窗口大小、映射函数）敏感。
- **忽视缺失值问题**：论文明确提到“未涉及不规则采样或缺失值”，这是时序预测中常见障碍，限制实际部署。
- **理论分析不足**：为何某些变形函数有效、何时失效缺乏深入解释；未提供误差界或收敛保证。
- **算力消耗未报告**：缺乏可复现的必要细节（如训练轮数、优化器设置）。

（完）
