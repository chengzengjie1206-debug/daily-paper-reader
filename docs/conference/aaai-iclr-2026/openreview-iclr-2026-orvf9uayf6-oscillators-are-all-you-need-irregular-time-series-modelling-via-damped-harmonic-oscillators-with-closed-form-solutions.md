---
title: "Oscillators Are All You Need: Irregular Time Series Modelling via Damped Harmonic Oscillators with Closed-form Solutions"
title_zh: 只需振荡器：基于阻尼谐振子闭式解的不规则时间序列建模
authors: "Debayan Gupta, ARITRA DAS, Yashas Shende, Reva Laxmi Chauhan, Arghya Pathak"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=oRVf9Uayf6"
tags: ["query:ts-air-qual"]
score: 9.0
evidence: 不规则时间序列建模与闭式解
tldr: 现有基于神经ODE的不规则时间序列方法依赖数值求解器导致计算瓶颈。该文提出用线性阻尼谐波振荡器替代神经ODE，通过解析闭式解高效建模不规则时间序列，避免了数值求解开销。所提方法在多个基准数据集上实现了与最优方法相当或更优的性能，且速度显著提升。该工作为不规则时间序列预测提供了轻量级有效解决方案。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: Transformer处理不规则时间序列有局限，神经ODE虽有效但需数值求解器，计算代价高。
method: 用线性阻尼谐波振荡器替代神经ODE，通过解析闭式解对不规则时间序列建模，无需数值积分。
result: 在多个不规则时间序列基准上达到最优或接近最优性能，同时大幅减少计算时间。
conclusion: 提供了一种高效、可解释的不规则时间序列建模新范式。
---

## Abstract
Transformers excel at time series modeling through attention mechanisms that capture long-term temporal patterns. However, they assume uniform time intervals and therefore struggle with irregular time series. Neural ODEs effectively handle irregular time series by modeling hidden states as continuously evolving trajectories. ContiFormers (Chen at al., 2024) combine Neural ODEs with Transformers, but inherit the computational bottleneck of the former by using heavy numerical solvers. This bottleneck can be removed by using a closed-form solution for the given dynamical system - but this is known to be intractable in general! We obviate this by replacing Neural ODEs with a novel linear damped harmonic oscillator analogy - which has a known closed-form solution. We model keys and values as damped, driven oscillators and expand the query in a sinusoidal basis up to a suitable number of modes. This analogy naturally captures the query-key coupling that is fundamental to any transformer architecture by modelling attention as a resonance phenomenon. Our closed-form solution eliminates the computational overhead of numerical ODE solvers while preserving expressivity.  We prove that this oscillator-based parameterization maintains the universal approximation property of continuous-time attention; specifically, any discrete attention matrix realizable by ContiFormer's continuous keys can be approximated arbitrarily well by our fixed oscillator modes. Our approach delivers both theoretical guarantees and scalability, achieving state-of-the-art performance on irregular time series benchmarks while being orders of magnitude faster.
All our code is available here: LINK

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **问题**：不规则时间序列（即样本时间间隔不统一）在医疗、传感器等场景广泛存在。传统Transformer通过固定时间间隔的注意力机制处理规则序列，难以直接处理不规则数据；神经ODE通过连续轨迹建模可适应不规则时间，但依赖数值求解器（如RK4）导致计算瓶颈。
- **动机**：现有方法（如ContiFormers）将神经ODE与Transformer结合，虽有效但继承了数值求解的高计算成本。作者希望找到一种无需数值积分、具有闭式解且保持表达能力的替代方案。

## 2. 论文提出的方法论

### 核心思想
- 用**线性阻尼谐波振荡器**（Damped Harmonic Oscillator）替代神经ODE，其运动方程有已知的闭式解，从而避免数值O(N)求解。
- 将Transformer中的**键（Key）和值（Value）**建模为阻尼受驱振荡器，将**查询（Query）**展开为正弦基（有限个模式），通过**共振现象**自然模拟查询-键耦合（注意力机制的核心）。

### 关键技术细节
- **闭式解形式**：利用二阶线性微分方程的解（指数衰减正弦波），可直接计算任意时刻的键/值状态，无需逐步积分。
- **注意力作为共振机制**：查询与键在频域匹配时产生强耦合，与注意力分数的计算本质一致。
- **通用逼近性证明**：证明了基于固定振荡器模式的参数化能够任意逼近ContiFormer连续键实现的所有离散注意力矩阵，保留了连续时间注意力的通用逼近性质。

### 算法流程（文字说明）
1. **输入**：不规则时间序列（时间戳 $t_i$，观测值 $x_i$）。
2. **编码**：将每个时间点的输入映射到隐空间。
3. **KV状态计算**：根据闭式解公式直接计算任意查询时间点对应的键和值，无需数值求解。
4. **Q展开**：将查询表示为有限个正弦/余弦基函数的加权和。
5. **注意力计算**：通过查询-键基函数的点积模拟共振，得到注意力权重。
6. **输出**：加权聚合值产生预测。

## 3. 实验设计

- **数据集/场景**：论文未明确列出，但声称在“多个不规则时间序列基准”上测试。常见基准包括PhysioNet、MIMIC-III、UEA等不规则分类/回归任务（推测）。
- **Benchmark**：最先进的方法包括ContiFormers (Chen et al., 2024) 及其他基于ODE的模型，以及标准Transformer变体。
- **对比方法**：未详细列举，但提及与ContiFormers对比速度和性能。

## 4. 资源与算力

- **文中未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。仅声称“orders of magnitude faster”（速度提升数个数量级），但未提供具体硬件配置对比。

## 5. 实验数量与充分性

- **实验数量**：依据摘要和元数据，主要在两个层面验证：①多个标准基准数据集上的性能（达到SOTA或接近SOTA）；②计算效率对比（速度显著优于ContiFormers）。未提及消融实验数。
- **充分性与公平性**：由于论文被ICLR 2026拒绝，且摘要未提供完整实验细节，尚无法判断实验设计的完整性和公平性。元数据中的“result”提到“达到最优或接近最优”，“速度更快”，但缺乏详细统计表。

## 6. 论文的主要结论与发现

- 用线性阻尼谐波振荡器代替神经ODE，可实现**闭式解的不规则时间序列建模**，避免数值求解器瓶颈。
- 所提方法在多个基准上达到**与现有最优方法相当或更优的性能**，同时**计算速度提升数个数量级**。
- 提供了**理论保证**：振荡器参数的参数化保留了连续时间注意力的通用逼近性质。
- 该工作为不规则时间序列预测提供了**轻量级、可解释**的新范式。

## 7. 优点

- **创新性**：首次将物理振荡器与Transformer注意力机制有机结合，将数值积分替换为闭式解，理论简洁。
- **计算高效**：闭式解直接计算，避免了神经ODE的数值求解开销，理论上可大幅加速训练和推理。
- **可解释性**：振荡器参数（阻尼、频率）与物理意义对应，可解释模型行为（如共振频率对应重要模式）。
- **理论证明**：证明了通用逼近性质，增强了方法的理论可靠性。

## 8. 不足与局限

- **实验细节缺失**：被拒稿论文可能未提供充分的实验设置、超参数、消融实验和统计显著性检验，需等待正式版本。
- **适用范围**：基于线性振荡器假设，可能无法建模高度非线性或混沌的动力学系统（相比神经ODE）。
- **基准覆盖**：未明确列出所有数据集和评价指标，难以判断泛化能力。
- **硬件信息缺失**：缺乏算力对比细节，导致速度提升的声称难以验证。
- **公开代码**：文末提到代码链接，但未检查是否开源可用。

（完）
