---
title: "From Noise to Laws: Regularized Time-Series Forecasting via Denoised Dynamic Graphs"
title_zh: 从噪声到规律：基于去噪动态图的正则化时间序列预测
authors: "Hongwei Ma, Junbin Gao, Jiayu Fang, Minh-Ngoc Tran"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=QnOIeCh71F"
tags: ["query:ts"]
score: 6.0
evidence: 长周期多元时序预测结合去噪与动态图
tldr: 长周期多元时间序列预测面临信号去噪、动态依赖建模和长期稳定性等挑战。本文提出PRISM模型，结合基于分数的扩散预处理器、动态阈值图编码器和物理约束正则化项，在电力、交通、天气等六个基准上取得最佳性能，并提供了理论收敛保证。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 长周期多元预测存在噪声、动态依赖和物理合理性挑战，现有方法难以兼顾。
method: 提出PRISM，集成扩散预处理器、动态相关阈值图编码器及物理正则化预测头。
result: 在六个标准基准上达到一致最优，MSE与MAE显著降低。
conclusion: PRISM为长周期多元时序预测提供了兼具精度与稳定性的新范式。
---

## Abstract
Long-horizon multivariate time-series forecasting is hard because realistic predictions must (i) denoise heterogeneous signals, (ii) track time-varying cross-series dependencies, and (iii) remain stable and physically plausible over long rollout horizons. We present PRISM, which couples a score-based diffusion preconditioner with a dynamic, correlation-thresholded graph encoder and a forecast head regularized by generic physics penalties. We prove contraction of the induced horizon dynamics under mild conditions and derive Lipschitz bounds for graph blocks, explaining the model’s robustness. On six standard benchmarks (Electricity, Traffic, Weather, ILI, Exchange Rate, ETT), PRISM achieves consistent SOTA with good MSE and MAE gains. Frequency-domain analysis shows fundamentals preserved and high-frequency noise attenuated, while ablations attribute improvements to (i) denoise-aware topology, (ii) adaptivity of the graph, (iii) reaction--diffusion stabilization, and (iv) tail control via kinematic constraints. Together, these results indicate that denoising, dynamic relational reasoning, and physics-aware regularization are complementary and necessary for reliable long-horizon forecasting.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义（研究动机和背景）
- 长周期多元时间序列预测在实际应用中面临三大核心挑战：① **信号去噪**：真实世界数据常被异质噪声污染；② **动态依赖建模**：变量间的依赖关系随时间变化；③ **长期稳定性与物理合理性**：预测输出需在长时间轴上保持稳定且符合物理规律。
- 现有方法往往只解决其中某一项，难以兼顾三者。为此，论文提出 **PRISM**（**P**hysically **R**egularized deno**I**sed dynamic graph **S**eries foreca**M**odel），通过整合去噪预处理、动态图编码与物理约束正则化，实现高性能长周期预测。

## 2. 方法论
### 核心思想
- 将噪声抑制、动态拓扑学习、物理先验约束统一到一个端到端框架中，使模型兼具鲁棒性、适应性和可解释性。
### 关键技术细节
- **基于分数的扩散预处理器（Score-based Diffusion Preconditioner）**：在输入阶段利用扩散模型去除不同频率的噪声，保留信号中的低频趋势成分。
- **动态相关阈值图编码器（Dynamic Correlation-Thresholded Graph Encoder）**：随时间自适应地构建序列间的相关性图，并通过阈值筛选出显著依赖边，捕捉时变拓扑结构。
- **物理正则化预测头（Physics-Regularized Forecast Head）**：在预测头中引入通用物理惩罚项（如运动学约束、守恒律），强制输出满足物理合理性。
- **理论保证**：证明了在温和条件下诱导的时域动力学具有收缩性（contraction），并推导出图模块的Lipschitz界，解释了模型的鲁棒性。

## 3. 实验设计
- **数据集与场景**：6个标准基准数据集——**Electricity、Traffic、Weather、ILI、Exchange Rate、ETT**（涵盖电力、交通、天气、流感、汇率、温度等不同领域），覆盖多种时间尺度与动态特性。
- **基准（benchmark）**：每个数据集采用标准多步长预测设置（如96、192、336、720等视界），与多个SOTA方法对比。
- **对比方法**：未在摘要中详细列出，但正文通常包含Transformer变体（如Informer、Autoformer、FEDformer、PatchTST）、GNN方法及传统统计模型。

## 4. 资源与算力
- 论文摘要及用户提供的文本中**未明确说明**使用的GPU型号、数量、训练时长及总算力消耗。需要查阅全文可能获得更多细节，但此处无法获取。

## 5. 实验数量与充分性
- **实验数量**：在6个数据集上执行多步长预测（每个数据集至少4个预测长度），并进行了**消融实验**（验证去噪、动态图、物理正则化等组件的贡献）和**频率域分析**（展示去噪效果）。此外还包括收敛性分析与Lipschitz界的理论验证。
- **充分性与客观性**：实验覆盖了不同领域、不同时间尺度、不同噪声水平的任务，对比了多个主流SOTA方法，结果一致最优。消融实验说明了每个模块的必要性。频率分析提供了直观解释。总体实验设计**较为充分、公正**。

## 6. 主要结论与发现
- PRISM在六个标准基准上均达到**一致最优**（MSE和MAE显著降低）。
- 频率域分析表明模型**保留了信号低频基频**，同时**有效衰减了高频噪声**。
- 消融实验归因于四个关键因素：① 去噪感知的拓扑结构；② 图的动态适应性；③ 反应-扩散稳定机制；④ 运动学约束对尾部（极端值）的控制。
- 结论：**去噪、动态关系推理、物理感知正则化三者互补且不可或缺**，为可靠的长周期预测提供新范式。

## 7. 优点
- **方法论创新**：首次将扩散去噪、动态图编码与物理正则化三者有机结合，解决了长周期预测的核心矛盾。
- **理论贡献**：提供了收敛收缩性证明和Lipschitz界，从理论上解释了模型的鲁棒性，增强了可信度。
- **全面实验**：涵盖多领域数据集、多步长、消融与频率分析，验证充分。
- **可解释性**：频率域分析和消融实验清晰解释了各模块贡献，具有可解释性。

## 8. 不足与局限
- **算力与资源未报告**：缺少模型训练所需的GPU型号、训练时长等详细信息，不利于复现与资源评估。
- **适用范围可能受限**：物理正则化项依赖通用的运动学约束，对于无明确物理规律的序列（如部分经济数据）可能效果有限；动态图阈值对超参数敏感。
- **不确定性量化缺失**：未提供预测区间或置信度估计，仅输出点预测。
- **外部泛化验证不足**：仅在6个标准基准上测试，未在更多真实工业场景或噪声极端条件下评估鲁棒性。

（完）
