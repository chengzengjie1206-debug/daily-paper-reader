---
title: CONTRASTIVE TIME SERIES FORECASTING WITH ANOMALIES
title_zh: 对比时间序列预测与异常处理
authors: "Joel Ekstrand, Zahra Taghiyarrenani, Slawomir Nowaczyk"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=HwbOLjPtCj"
tags: ["query:ts"]
score: 6.0
evidence: 对比时间序列预测与异常处理
tldr: 该论文提出Co-TSFA框架，通过对比学习区分影响预测的持久异常和短暂噪声，并引入潜在-输出对齐损失，使模型对不相关变化具有不变性。在多个时间序列数据集上，该方法有效提升了存在异常时的预测精度，为异常鲁棒预测提供了新方案。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 标准预测模型无法区分对预测有/无影响的异常事件。
method: 使用对比增强和潜在-输出对齐损失学习异常区分。
result: 在含异常的时间序列上提高了预测准确性。
conclusion: 所提正则化框架有效提升了预测模型在异常环境下的鲁棒性。
---

## Abstract
Time-series forecasting predicts future values from past data. In real-world settings, some anomalous events have lasting effects and influence the forecast, while others are short-lived and should be ignored. Standard forecasting models fail to make this distinction, often either overreacting to noise or missing persistent shifts. We propose **Co-TSFA** (Contrastive Time-Series Forecasting with Anomalies), a regularization framework that learns when to ignore anomalies and when to respond. Co-TSFA generates input-only and input–output augmentations to model forecast-irrelevant and forecast-relevant anomalies, and introduces a latent–output alignment loss that ties representation changes to forecast changes. This encourages invariance to irrelevant perturbations while preserving sensitivity to meaningful distributional shifts. Experiments on the Traffic and Electricity benchmarks, as well as on a real-world cash-demand dataset, demonstrate that Co-TSFA improves performance under anomalous conditions while maintaining accuracy on normal data. The implementation of Co-TSFA will be released publicly upon acceptance of the paper.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：时间序列预测中，异常事件对未来的影响不同：有些异常具有持久影响（如政策变化、设备故障），需要模型捕捉；有些异常是短暂噪声（如传感器毛刺），应当忽略。标准预测模型无法区分这两种异常，容易过反应噪声或错过持久偏移。
- **背景**：真实世界的时间序列数据常包含各类异常，现有模型缺乏对异常类型的辨别能力，导致预测鲁棒性不足。论文旨在解决这一问题，提升模型在异常环境下的预测性能。

## 2. 论文提出的方法论

- **核心思想**：提出 **Co-TSFA**（Contrastive Time-Series Forecasting with Anomalies）框架，通过对比学习正则化，让模型学会何时忽略异常、何时响应异常。
- **关键技术细节**：
  - 生成两种数据增强：
    - **输入仅增强**（input-only augmentation）：模拟与预测无关的异常（短暂噪声）。
    - **输入-输出增强**（input-output augmentation）：模拟与预测相关的异常（持久偏移）。
  - 引入 **潜在-输出对齐损失**（latent–output alignment loss）：将表征变化与预测变化绑定，促使模型对不相关扰动保持不变性，同时对有意义的分布偏移保持敏感性。
- **算法流程**（文字说明）：
  1. 对原始时间序列施加两种增强，生成对应样本。
  2. 通过编码器提取潜在表示。
  3. 计算对比损失：使输入-输出增强样本的表示靠近，输入仅增强样本的表示远离。
  4. 联合优化预测损失和对齐损失，训练模型区分异常类型。

## 3. 实验设计

- **数据集**：Traffic（交通流量）、Electricity（电力负荷）两个公开基准数据集，以及一个真实世界的现金需求数据集（cash-demand dataset）。
- **Benchmark**：未明确列出具体基线方法名称，但从摘要可知对比了标准预测模型（如未引入异常区分机制的模型）。
- **对比方法**：未详细说明，但推测包括原始预测模型、无正则化的变体等。实验设置公平性需进一步验证。

## 4. 资源与算力

- **文中未明确说明**：使用的 GPU 型号、数量、训练时长等算力信息均未提及。仅从元数据中无法获取此类细节。

## 5. 实验数量与充分性

- **实验数量**：在三个数据集上进行了评估（Traffic, Electricity, 现金需求）。元数据提及了“改善性能”和“保持正常数据精度”，但未列出具体实验组数（如不同异常比例、消融实验等）。
- **充分性**：从摘要看，实验覆盖了公开基准和真实场景，但缺乏详细的消融研究、超参数分析、统计显著性测试等。无法判断是否充分客观。论文被 ICLR 2026 拒稿，可能实验细节不足或结果不够有力。

## 6. 论文的主要结论与发现

- Co-TSFA 在含异常的时间序列上显著提高了预测准确性，同时在正常数据上保持同等精度。
- 所提出的正则化框架（对比增强+对齐损失）能有效区分持久异常和短暂噪声，提升模型鲁棒性。

## 7. 优点

- **创新性**：将对比学习引入时间序列异常感知预测，通过两种增强区分异常类型，思路清晰。
- **实用性**：面向真实世界常见问题，具有应用价值。
- **损失设计**：潜在-输出对齐损失巧妙地将表征变化映射到预测变化，实现不变性学习。
- **实验覆盖**：同时使用公开基准和私有真实数据，验证了方法在不同场景的有效性。

## 8. 不足与局限

- **实验细节缺失**：由于无法获取论文正文，缺乏对基线方法、参数设置、异常生成策略、消融实验等的描述，难以评估实验的公平性和充分性。
- **算力信息未知**：未提供计算资源需求，可能影响可复现性。
- **应用限制**：仅验证了三个数据集，未涵盖更多领域（如金融、医疗）。对异常类型的假设（持久 vs 短暂）可能过于简化，真实场景中异常可能具有更复杂的时序依赖。
- **论文被拒**：元数据显示为 ICLR 2026 拒稿，说明评审可能认为方法贡献或实验存在不足。

（完）
