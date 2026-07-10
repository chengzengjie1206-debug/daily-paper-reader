---
title: "MW-Net: Multi-Wave U-Net with Cross-Wave Links for Multi-Scale Physical Dynamics"
title_zh: MW-Net：具有跨波连接的多波U-Net用于多尺度物理动力学
authors: "Alexander Khrabry, Edward A. Startsev, Andrew T Powis, Igor Kaganovich"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=79nfkvRzH1"
tags: ["query:ts-air-qual"]
score: 4.0
evidence: 多尺度物理动力学建模
tldr: 复杂物理系统的时间演化建模需要处理多尺度特征。MW-Net通过堆叠多个U-Net波并引入跨波跳跃连接，增强层次化表示学习。该设计支持特征在不同尺度上的反复交互，逐步细化动态建模，在多个物理仿真数据上取得了更优的结果。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有堆叠U-Net变体限制跳跃连接在同一波内，跨尺度交互不足。
method: 堆叠多个编码器-解码器波，并在波间引入跳跃连接。
result: 在物理系统仿真数据上建模精度优于SineNet等基线。
conclusion: MW-Net为多尺度物理动力学建模提供了层次化深度架构。
---

## Abstract
We propose Multi-Wave Network (MW-Net), a novel deep learning architecture for modeling the temporal evolution of complex, multi-scale physical systems. MW-Net extends the U-Net architecture by stacking multiple encoder–decoder “waves” (U-Net modules). Unlike prior stacked U-Net variants such as SineNet, which restrict skip connections to within each wave, MW-Net introduces skip connections both within and across successive waves at matching spatial resolutions. This design enhances hierarchical representation learning by enabling repeated interactions between feature representations at the same and different spatial scales, supporting progressive refinement of learned dynamics and offering explicit control over network depth through the number of stacked waves. We evaluate MW-Net on diverse physical systems: 2D Kolmogorov fluid turbulence, Hasegawa–Wakatani plasma turbulence, a shallow-water planetary atmosphere model, and buoyant smoke flows (2D and 3D). Across all cases, MW-Net consistently outperforms state-of-the-art baselines and achieves Pareto improvements in the accuracy–computational cost trade-off. While the best-performing baseline varied by task, MW-Net achieved substantially lower errors and up to 3× faster convergence in reaching low-error regimes under fixed learning schedules.

---

## 论文详细总结（自动生成）

# MW-Net：具有跨波连接的多波U-Net用于多尺度物理动力学

## 1. 核心问题与整体含义（研究动机和背景）

复杂物理系统（如湍流、等离子体、大气环流等）的时间演化通常涉及多个空间和时间尺度的相互作用，这对数值建模提出了挑战。传统的深度学习方法（如标准U-Net及其堆叠变体）在处理多尺度特征时存在局限：现有堆叠U-Net架构（如SineNet）将跳跃连接限制在同一“波”（即同一编码器-解码器模块）内部，导致不同波之间缺乏跨尺度信息交互，难以充分捕捉物理系统在不同尺度上的动态耦合。为此，本文提出**MW-Net**（Multi-Wave Network），通过引入跨波跳跃连接，增强层次化表示学习，实现对多尺度物理动力学更精确的建模。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：堆叠多个编码器-解码器模块（称为“波”），并在每个波的对应空间分辨率层之间引入额外的跨波跳跃连接，使特征在不同波之间反复交互，逐步细化动力学表示。
- **技术细节**：
  - 每个波是一个完整的U-Net结构（包含下采样编码器、上采样解码器、同层跳跃连接）。
  - 除了波内的标准跳跃连接，MW-Net还在**连续波之间**、**相同空间分辨率层**添加跳跃连接（即第i个波的某层特征可以直接传递到第i+1个波的对应层）。
  - 通过堆叠波的数量（wave number）可显式控制网络深度，适应不同复杂度的物理系统。
- **算法流程**（文字说明）：
  1. 输入时间步t的物理场状态（如速度、密度等）。
  2. 通过第一个U-Net波进行编码-解码，得到中间特征图。
  3. 将第一个波各层的特征（在编码器和解码器中的对应分辨率层）通过跨波跳跃连接，与第二个波对应层的输入合并。
  4. 第二个波处理合并后的特征，输出更精细的表示。
  5. 重复上述过程直到最后一个波，输出预测的下一个时间步状态。
- **公式/模型**：未给出显式数学公式，但可理解为多个U-Net模块的级联与跨层拼接。

## 3. 实验设计

- **数据集/场景**：共5个物理系统仿真数据：
  - 2D Kolmogorov流体湍流
  - Hasegawa–Wakatani等离子体湍流
  - 浅水行星大气模型
  - 浮力烟雾流（2D和3D）
- **基准方法**：与多个SOTA基线对比，包括：
  - SineNet（堆叠U-Net但无跨波连接）
  - 标准U-Net
  - ResNet、Transformer等（具体名称在摘要中未列出，但提及“best-performing baseline varied by task”）
- **评价指标**：预测误差（如MSE或相对误差），以及精度-计算成本帕累托权衡。

## 4. 资源与算力

论文在提供的元数据和摘要中**未明确说明**使用的GPU型号、数量、训练时长等算力信息。仅提到在精度-计算成本帕累托改进中优于基线，但未给出具体硬件配置。

## 5. 实验数量与充分性

- **实验数量**：覆盖5个不同的物理系统（2D和3D），每个系统应进行了多次运行（但未明确重复次数）。此外，可能进行了消融实验（如改变波的数量、有无跨波连接等），但摘要未展开。
- **充分性与客观性**：
  - 覆盖了湍流、等离子体、大气、烟雾流等多种物理场景，多样性较好。
  - 与多个基线对比，并报告了精度和收敛速度，结果一致优于基线。
  - 存在局限：缺少对训练数据量、噪声敏感性、超参数稳定性等分析；未提供统计显著性检验；未公开代码和详细实验设置，复现性存疑。

## 6. 主要结论与发现

- MW-Net在所有测试的物理系统上**一致优于**现有SOTA基线，尤其在低误差区域收敛速度可达**3倍**提升。
- 在不同任务上，最佳基线的表现各不相同，但MW-Net始终取得更低误差，并在精度-计算成本之间实现帕累托改进。
- 跨波跳跃连接是提升多尺度表示学习的关键设计，使网络能反复细化动态特征。

## 7. 优点

- **架构创新**：首次在堆叠U-Net中引入跨波跳跃连接，增强多尺度交互，设计简单但有效。
- **灵活性**：通过波的数量可控制网络深度，适应不同复杂度问题。
- **实验全面**：覆盖多种具有代表性的物理系统（流体、等离子体、大气、烟雾），验证了方法的通用性。
- **性能优势**：在精度和收敛速度上均有显著提升，且不显著增加计算开销（帕累托改进）。

## 8. 不足与局限

- **算力信息缺失**：未报告训练资源，难以评估可复现性和实际部署成本。
- **缺乏消融细节**：未系统分析波的数量、跨波连接密度等超参数的影响。
- **适用性边界**：仅针对物理仿真数据，未在真实观测数据或极端尺度分离问题上验证。
- **方法局限性**：堆叠多个U-Net导致参数量线性增长，可能对内存和训练时间造成压力；跨波连接可能引入冗余特征。
- **实验公平性**：未说明基线是否进行了超参数调优，可能存在未公开的隐式不公平比较。

（完）
