---
title: "FlowPath: Learning Data-Driven Manifolds with Invertible Flows for Robust Irregularly-sampled Time Series Classification"
title_zh: FlowPath：基于可逆流的数据驱动流形学习用于稳健不规则采样时间序列分类
authors: "YongKyung Oh, Dong-Young Lim, Sungil Kim"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39643/43604"
tags: ["query:ts-air-qual"]
score: 6.0
evidence: 基于可逆流的不规则采样时序分类
tldr: 不规则采样时间序列的连续动态建模对控制路径选取敏感，固定插值方法在高缺失率下表现不佳。本文提出FlowPath，利用可逆神经流学习控制路径几何，构建连续数据驱动流形。实验证明该方法在分类任务上优于现有固定插值方案，尤其在低观测率下鲁棒性显著提升。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 现有不规则时序分类依赖固定插值，假设过于简单。
method: 提出FlowPath，通过可逆神经流学习控制路径几何，构建连续数据流形。
result: 在多个不规则时序分类数据集上取得更高准确率，尤其在缺失率高时鲁棒性突出。
conclusion: FlowPath为不规则时序分类提供了数据自适应的插值替代方案。
---

## Abstract
Modeling continuous-time dynamics from sparse and irregularly-sampled time series remains a fundamental challenge. Neural controlled differential equations provide a principled framework for such tasks, yet their performance is highly sensitive to the choice of control path constructed from discrete observations. Existing methods commonly employ fixed interpolation schemes, which impose simplistic geometric assumptions that often misrepresent the underlying data manifold, particularly under high missingness. We propose FlowPath, a novel approach that learns the geometry of the control path via an invertible neural flow. Rather than merely connecting observations, FlowPath constructs a continuous and data-adaptive manifold, guided by invertibility constraints that enforce information-preserving and well-behaved transformations. This inductive bias distinguishes FlowPath from prior unconstrained learnable path models. Empirical evaluations on 18 benchmark datasets and a real-world case study demonstrate that FlowPath consistently achieves statistically significant improvements in classification accuracy over baselines using fixed interpolants or non-invertible architectures. These results highlight the importance of modeling not only the dynamics along the path but also the geometry of the path itself, offering a robust and generalizable solution for learning from irregular time series.

---

## 论文详细总结（自动生成）

# FlowPath论文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：不规则采样时间序列（ISTS）普遍存在于医疗、传感器等领域，但现有方法存在根本局限。离散模型（如RNN变体）只能近似连续动态，而连续模型（神经微分方程NDE）虽然能建模连续时间，但其核心控制路径通常由固定插值（线性、三次样条）构成，这些插值方法施加了过于简单的几何假设，尤其在高缺失率下会严重扭曲底层数据流形。
- **整体含义**：本文指出，控制路径的几何结构本身比路径上的动态更关键，提出通过可逆神经流学习数据自适应的、信息保持的连续流形，从而提升ISTSL分类的鲁棒性。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：用可逆神经流（invertible neural flow）替代固定插值作为神经控制微分方程（Neural CDE）的控制路径。可逆流能产生光滑、保结构的变换，保持信息完整且避免不约束路径的不稳定性。
- **关键技术细节**：
  - **控制路径学习**：定义 Φ(t) = F(t, x(0); θ_F)，其中 F 是一个可逆神经网络（如耦合流，保证双射性和光滑性）。Φ(t) 作为学习的连续路径，而不是线性插值。
  - **隐状态动态**：隐藏状态 z(t) 由 Riemann-Stieltjes 积分驱动：  
    z(t) = z(0) + ∫₀ᵗ f(τ, z(τ); θ_f) dΦ(τ)。等价 ODE 形式：ż(t) = f(t, z(t); θ_f) Φ̇(t)。
  - **性质保证**：  
    - **定理1（密度保持）**：Φ 是 C¹-微分同胚且 f 利普希茨时，概率密度的对数变化由散度控制，确保无概率质量崩塌或膨胀，保护潜在空间分类结构。  
    - **定理2（存在唯一性）**：给定初始条件，解存在且唯一。  
    - **定理3（泛化界）**：通过 Rademacher 复杂度得出经验风险与真实风险差距随样本量增大而减小（界包含利普希茨常数和 Φ̇ 上界）。

## 3. 实验设计
- **基准数据集与场景**：
  - **18个基准数据集**：来自UEA&UCR仓库，涵盖“Motion & HAR”、“ECG/EEG”、“Sensor”三大领域。对每个原始数据人为制造30%、50%、70%缺失率的变体，共4种设置（含原始数据）。
  - **真实世界数据集**：  
    - **PAMAP2（HAR）**：5333段，8类活动，17个传感器模态。使用张量脱落模拟传感器缺失（10%~50%）。  
    - **PhysioNet Sepsis**：40335患者，34个时间变量，评估AUROC。
- **对比方法**：
  - 离散模型：RNN、LSTM、GRU、GRU-Δt、GRU-D。
  - ODE/NDE类：GRU-ODE、ODE-RNN、ODE-LSTM、Neural CDE、Neural RDE、ANCDE、EXIT、LEAP、DualDynamics、Neural Flow。
  - 其他：Transformer、Trans-mean、SeFT、mTAND、Raindrop、DGM²-O、IP-Net、MTGNN、TITD、CoFormer 等（仅在PAMAP2中对比）。
- **评估指标**：分类准确率（基准）、F1-score（PAMAP2）、AUROC（Sepsis）。多次重复取均值和标准差。

## 4. 资源与算力
- **文中未明确说明**：没有提到使用的GPU型号、数量、训练时长等具体计算资源信息。

## 5. 实验数量与充分性
- **实验数量丰富**：  
  - 18个基准数据集 × 4种缺失率 × 5次重复 = 360组结果 + 额外真实世界实验（PAMAP2多脱落率、Sepsis）。  
  - 消融实验：对比Neural CDE、Neural CDE+MLP（非可逆）与FlowPath，验证可逆流贡献。  
  - 架构对比：使用ResNet、GRU、Coupling三种可逆流，确认鲁棒性。  
  - 计算分析：不同参数量下性能散点图（图7）。  
  - 定性可视化：流形对齐、KDE、轨迹图。
- **充分性与公平性**：  
  - 数据集覆盖面广（多领域）、缺失率梯度、使用相同预处理和划分。  
  - 与强烈的基线DualDynamics进行成对胜负统计（表2）。  
  - 统计测试（虽未详述，但提到提供补充材料）；总体较为公平。

## 6. 主要结论与发现
- **性能优势**：FlowPath在全部设置的平均准确率（0.730）显著超过所有基线（DualDynamics 0.708排第二），且在每项缺失率下均排第一或第二。高缺失率下优势更明显（如70%缺失时FlowPath 0.718 vs DualDynamics 0.697）。
- **鲁棒性**：Pairwise对比FlowPath vs DualDynamics，在缺失30%/50%/70%时FlowPath赢得更多、输得更少（表2）。
- **消融验证**：可逆流（FlowPath）始终优于非可逆MLP路径，后者在高缺失率下性能恶化（表3）。
- **定性分析**：FlowPath能恢复更光滑、更接近真实分布的流形（图3-6），而非可逆路径易过拟合稀疏点。
- **真实世界数据集**：PAMAP2上F1最高（表4），Sepsis上AUROC最佳（0.919±0.005）。

## 7. 优点
- **方法论创新**：将可逆流引入控制路径学习，提供信息保持、几何一致性的归纳偏置，超越固定插值和自由路径。
- **理论支撑**：给出了密度保持、解存在唯一性、泛化界，增强了方法的可信度。
- **实验充分**：多数据集、多缺失率、真实应用、大量消融与可视化，验证了可逆流的关键作用。
- **鲁棒性强**：对缺失数据具有优异抗性，且在不同流架构（ResNet/GRU/Coupling）下表现一致。

## 8. 不足与局限
- **计算开销**：可逆流会引入额外参数，增加训练成本（虽图7表明在合理参数量内性能更优）。
- **任务覆盖**：本文仅评估分类，未扩展到预测、生成等任务，能力边界未知。
- **缺失机制**：人工缺失为随机缺失（MCAR），真实场景可能更复杂（如结构性缺失），未讨论。
- **理论整合**：定理3的泛化界依赖利普希茨常数和Φ̇上界，但这些常数在实际中可能较大，界可能宽松。
- **代码可用性**：已开源（GitHub），但代码细节未在文中详述。

（完）
