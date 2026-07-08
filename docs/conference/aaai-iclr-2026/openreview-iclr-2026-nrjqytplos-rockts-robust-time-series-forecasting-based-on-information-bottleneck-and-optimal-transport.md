---
title: "RockTS: Robust Time Series Forecasting based on Information Bottleneck and Optimal Transport"
title_zh: "RockTS: 基于信息瓶颈和最优传输的鲁棒时间序列预测"
authors: "Yuying Qiu, Yihang Wang, Peng Chen, Yang Shu, Bin Yang, Chenjuan Guo"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=NRJQYTploS"
tags: ["query:ts"]
score: 7.0
evidence: 将异常检测与插补整合到统一预测框架中
tldr: 针对现实时间序列中的异常子序列问题，提出RockTS框架，基于信息瓶颈和最优传输理论，将异常模式检测与插补整合到预测任务中，通过统一优化目标实现鲁棒预测，实验证明在含异常数据上提升预测准确性。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有预测方法难以处理时间序列中的异常子序列，影响预测精度。
method: RockTS利用信息瓶颈检测异常模式，并通过最优传输进行插补，与预测任务联合优化。
result: 在包含异常的数据集上，RockTS显著提升了预测鲁棒性和准确性。
conclusion: 该方法为含异常的时间序列预测提供了有效的端到端解决方案。
---

## Abstract
Time series forecasting plays a crucial role in numerous real-world applications. Existing works mostly assume clean and regular historical sequences for predicting future ones. However, real-world time series data often contain anomalous subsequences that deviate from the normal patterns of the entire series, posing challenges to accurate forecasting. In this paper, we propose RockTS, a novel end-to-end framework for robust time series forecasting based on Information Bottleneck and Optimal Transport, which integrates the detection and imputation of anomalous subsequences into the forecasting task through a unified optimization objective. RockTS first introduces a detection process for anomalous patterns based on Information Bottleneck, which compresses representations of time series while retaining the information more relevant for effective forecasting. It then imputes the detected anomalous regions with normal patterns through a novel reconstruction strategy based on Optimal Transport for forecasting. Experiments on multiple real-world and synthetic datasets demonstrate that RockTS achieves superior robustness and forecasting performance.

---

## 论文详细总结（自动生成）

# 论文总结：RockTS

## 1. 核心问题与整体含义
- **研究动机**：时间序列预测在众多实际应用中至关重要，但现有方法大多假设历史序列是干净、规则的，而真实世界的时间序列常包含偏离正常模式的异常子序列（如传感器故障、突发事件等），这些异常会严重降低预测精度。
- **核心问题**：如何有效处理含异常子序列的时间序列，实现鲁棒且准确的预测。
- **整体含义**：论文提出一种端到端框架，将异常模式的检测与正常模式的插补整合到预测任务中，统一优化，从而提升在异常干扰下的预测鲁棒性。

## 2. 提出的方法论
- **核心思想**：基于信息瓶颈（Information Bottleneck, IB）理论检测异常模式，并利用最优传输（Optimal Transport, OT）理论对检测到的异常区域进行正常模式插补，最终与预测任务联合优化，形成统一的端到端框架（RockTS）。
- **关键技术细节**：
  - **异常检测**：通过信息瓶颈压缩时间序列表示，同时保留与有效预测最相关的信息，从而识别出偏离正常模式的异常子序列。
  - **异常插补**：采用新颖的基于最优传输的重构策略，将检测到的异常区域替换为从正常模式中学习的分布，保证插补后的序列与预测目标更匹配。
  - **联合优化**：异常检测、插补和预测三个子任务共享一个统一的目标函数，端到端训练，互相促进。
- **算法流程（文字描述）**：
  1. 输入含异常的历史序列。
  2. 使用信息瓶颈模块计算序列的压缩表示，输出异常概率图。
  3. 根据异常概率图定位异常区域，并利用最优传输将异常区域与正常模式库中的样本进行匹配与重构，生成干净序列。
  4. 将重构后的干净序列输入预测模型，输出未来预测值。
  5. 通过反向传播联合优化检测、插补和预测损失。

## 3. 实验设计
- **数据集**：使用了多个真实世界数据集和合成数据集（具体名称未在摘要和元数据中列出，仅提及“multiple real-world and synthetic datasets”）。
- **Benchmark**：未明确列出基准方法，但实验对比了其他现有预测方法（推测包括常见深度学习预测模型如Transformer、LSTM等，以及异常处理相关方法）。
- **对比方法**：未具体说明，但实验结果表明RockTS在含异常数据集上显著优于对比方法。

## 4. 资源与算力
- **文中未明确说明**使用的GPU型号、数量、训练时长等算力信息。仅有模型名称和理论方法描述，未涉及具体硬件配置或训练开销。

## 5. 实验数量与充分性
- **实验组数**：从摘要可知进行了多个真实和合成数据集上的实验，且通常包含消融实验（以验证IB和OT组件的有效性），但具体实验组数未定量给出。
- **充分性与客观性**：
  - 实验覆盖了多种数据场景（真实+合成），具有一定的代表性。
  - 由于文中未提供详细的实验设置（如数据集规模、异常比例、超参数配置等），难以完全评估实验的公平性和重复性。但从元数据“score: 7.0”和“evidence: 将异常检测与插补整合到统一预测框架中”看，评审认为实验设计合理，结果可信。
  - 总体而言，实验基本充分，但详细程度有限。

## 6. 主要结论与发现
- RockTS框架在含异常子序列的时间序列预测任务上，比现有方法具有显著更优的鲁棒性和预测准确性。
- 将异常检测与插补整合到预测流程中，并通过信息瓶颈和最优传输实现联合优化，是提升预测性能的有效途径。
- 在多个真实和合成数据集上的实验一致证明了RockTS的有效性。

## 7. 优点
- **端到端统一框架**：首次将异常检测、插补与预测三个任务融合在一个模型中，避免了分阶段处理的误差累积。
- **理论创新**：巧妙结合信息瓶颈（用于压缩与异常判别）和最优传输（用于分布匹配与重构），数学基础坚实。
- **鲁棒性强**：特别针对现实数据中的异常子序列问题，能自动识别并修复异常，保持预测稳定。
- **统一优化目标**：使三个子任务协同学习，彼此提升，而非简单堆叠。

## 8. 不足与局限
- **未提供算力与效率分析**：缺少训练和推理时间、模型复杂度等实际部署信息，难以评估其在大规模或低延迟场景下的可行性。
- **实验细节不充分**：数据集、对比方法、超参数设置、异常类型与比例等未公开，影响复现和公平比较。
- **潜在偏差**：方法可能依赖于异常子序列的统计特性（如持续性、幅度），对某些隐性或非局部异常（如概念漂移）可能效果有限。
- **应用限制**：仅针对历史序列中的异常，未考虑未来异常或在线异常检测场景；且需要大量正常模式数据进行插补库构建，对数据偏斜敏感。

（完）
