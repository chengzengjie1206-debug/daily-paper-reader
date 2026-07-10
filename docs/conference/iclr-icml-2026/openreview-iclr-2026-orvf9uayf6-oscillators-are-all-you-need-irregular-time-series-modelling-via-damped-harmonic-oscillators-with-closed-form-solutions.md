---
title: "Oscillators Are All You Need: Irregular Time Series Modelling via Damped Harmonic Oscillators with Closed-form Solutions"
title_zh: 振荡器即所需：通过具有闭式解的阻尼简谐振子进行不规则时间序列建模
authors: "Debayan Gupta, ARITRA DAS, Yashas Shende, Reva Laxmi Chauhan, Arghya Pathak"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=oRVf9Uayf6"
tags: ["query:ts-air-qual"]
score: 9.0
evidence: 提出闭式阻尼简谐振子模型，避免数值求解器，高效处理不规则时间序列
tldr: 现有Transformer和Neural ODE处理不规则时间序列有局限性，而数值求解器计算昂贵。本文提出基于阻尼简谐振子的闭式解模型，无需数值求解即可高效建模不规则时序。在多个不规则时序任务上达到与复杂模型相当甚至更优的结果，且速度更快。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 不规则时间序列建模中，Neural ODE计算瓶颈大，且缺乏闭式解。
method: 提出线性阻尼简谐振子模型，推导出闭式解，替代数值ODE求解器。
result: 在多个不规则时间序列数据集上，模型性能与Neural ODE相当，但训练和推理速度显著提升。
conclusion: 闭式谐振子模型为不规则时序提供了一种高效、理论优雅的替代方案。
---

## Abstract
Transformers excel at time series modeling through attention mechanisms that capture long-term temporal patterns. However, they assume uniform time intervals and therefore struggle with irregular time series. Neural ODEs effectively handle irregular time series by modeling hidden states as continuously evolving trajectories. ContiFormers (Chen at al., 2024) combine Neural ODEs with Transformers, but inherit the computational bottleneck of the former by using heavy numerical solvers. This bottleneck can be removed by using a closed-form solution for the given dynamical system - but this is known to be intractable in general! We obviate this by replacing Neural ODEs with a novel linear damped harmonic oscillator analogy - which has a known closed-form solution. We model keys and values as damped, driven oscillators and expand the query in a sinusoidal basis up to a suitable number of modes. This analogy naturally captures the query-key coupling that is fundamental to any transformer architecture by modelling attention as a resonance phenomenon. Our closed-form solution eliminates the computational overhead of numerical ODE solvers while preserving expressivity.  We prove that this oscillator-based parameterization maintains the universal approximation property of continuous-time attention; specifically, any discrete attention matrix realizable by ContiFormer's continuous keys can be approximated arbitrarily well by our fixed oscillator modes. Our approach delivers both theoretical guarantees and scalability, achieving state-of-the-art performance on irregular time series benchmarks while being orders of magnitude faster.
All our code is available here: LINK

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- 现有Transformer擅长捕捉长期时间依赖，但假设均匀时间间隔，难以处理不规则时间序列（IRTS）。
- Neural ODE通过连续轨迹建模可有效处理IRTS，但依赖数值ODE求解器，计算开销大、速度慢。
- ContiFormer（Chen et al., 2024）融合Transformer与Neural ODE，继承了数值求解器的计算瓶颈。
- 核心挑战：如何在不牺牲表达能力的前提下，用闭式解替代数值求解，实现高效IRTS建模。
- 本文提出：利用阻尼简谐振子（Damped Harmonic Oscillator）——该物理模型具有已知闭式解——来建模键（key）和值（value），将注意力机制解释为共振现象，从而消除数值求解器，同时保持理论表达能力。

## 2. 论文提出的方法论
- **核心思想**：将Continuous-Time Attention中的键（key）和值（value）视为受阻尼驱动的简谐振子，查询（query）在正弦基上展开至一定模态数；注意力计算转化为谐振子共振现象的闭式表达。
- **关键技术细节**：
  - 线性阻尼简谐振子系统：`d²x/dt² + 2γ dx/dt + ω₀² x = f(t)`，其中γ为阻尼系数，ω₀为固有频率，f(t)为驱动力（类似查询）。
  - 利用已知闭式解（如正弦/余弦衰减组合）直接计算连续时间下的键值演变，无需数值积分。
  - 查询扩展为正弦基函数，与键的振荡模式自然耦合，形成闭式注意力权重。
- **公式/算法流程（文字说明）**：
  - 输入不规则时间戳序列，将每个时间点作为驱动源。
  - 为每个键（及对应值）分配一组振荡器参数（阻尼、频率、初相位），通过闭式解得到任意时刻的连续键表示。
  - 查询通过固定数量的正弦模态线性组合，计算与所有键的共振幅度，得到注意力分数。
  - 时间更新仅需解析计算，避免ODE求解器的迭代步骤。
- **理论贡献**：证明该参数化方式保持了连续时间注意力的通用逼近性质——任何ContiFormer产生的离散注意力矩阵都能被固定振荡器模态任意逼近。

## 3. 实验设计
- **数据集/场景**：标准不规则时间序列基准数据集，包括物理、生物医学、气候等多领域（具体名称未列出，但元数据表明涉及如空气质量等场景，tag `ts-air-qual`）。
- **基准（Benchmark）**：对比模型包括ContiFormer、Neural ODE变体、传统Transformer及其不规则时序适配方法。
- **对比方法**：至少包括ContiFormer（最直接基线），可能还有经典IRTS方法如GRU-ODE、ODE-RNN等。
- **任务类型**：时间序列分类、预测或缺失值插补（从“不规则时间序列建模”推断）。
- **评估指标**：推测使用准确率、RMSE、或负对数似然等，但摘要未明确说明。

## 4. 资源与算力
- **未明确说明**：论文摘要及元数据中未提及所用GPU型号、数量或训练时长。仅强调“orders of magnitude faster”（速度快几个数量级），但未给出具体硬件配置或时间对比表。
- 因此，无法评估其算力开销细节。

## 5. 实验数量与充分性
- **大致数量**：摘要只提到在多个不规则时序基准上达到SOTA，但未列出具体数据集个数。元数据中“result”提及“多个不规则时间序列数据集”，推测不少于3-5个常见基准。
- **消融实验**：未明确提及是否有消融实验（如不同振荡器模态数、阻尼参数的影响），但从理论部分（证明通用逼近）看可能缺少消融验证。
- **客观性与公平性**：对比方法包括最相关的ContiFormer，且声称速度更快性能相当或更优。但缺乏对超参数搜索、随机种子重复等细节说明，可能存在公平性风险。

## 6. 主要结论与发现
- 闭式谐振子模型在不规则时间序列任务上达到与Neural ODE/ContiFormer相当甚至更优的性能。
- 训练和推理速度显著提升（orders of magnitude faster），因为完全避免了数值积分。
- 理论证明了该闭式参数化能逼近任何连续时间注意力矩阵，保证了表达能力。
- 结论：闭式谐振子模型为不规则时序提供了一种高效、理论优雅的替代方案，有望取代现有的数值ODE求解器范式。

## 7. 优点
- **创造性**：将物理谐振子闭式解引入时间序列建模，巧妙利用共振类比解释注意力，思路新颖。
- **计算高效**：完全消除数值ODE求解器，训练与推理速度提升显著（数量级）。
- **理论保证**：提供了通用逼近性质证明，具备严谨性。
- **简洁优雅**：参数化简单（只有阻尼、频率、相位），易于实现和调参。

## 8. 不足与局限
- **实验细节缺失**：未列出具体数据集名称、评估指标、超参设置、消融实验等，无法完全复现和验证。
- **计算资源未说明**：缺少GPU型号、训练时间、模型参数量等，不利于公平比较。
- **未讨论局限性**：摘要与元数据中未提及模型的可能弱点，例如：
  - 阻尼谐振子是否适用于所有类型的不规则时序（如突变、非平滑动态）？
  - 固定模态数上限对复杂模式的拟合能力是否有限制？
  - 对比方法中是否包含了更近期的SOTA（如SNODEs、Flow-based模型）？
- **公开性**：虽提供代码链接，但未说明代码是否经过同行评审或可复现。

（完）
