---
title: Learning Neural Operators from Partial Observations via Latent Autoregressive Modeling
title_zh: 通过潜在自回归建模从不完全观测学习神经算子
authors: "Jingren Hou, Hong Wang, Pengyu Xu, Chang Gao, Huafeng Liu, Liping Jing"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37001/40963"
tags: ["query:ts-air-qual"]
score: 8.0
evidence: 从不完全观测学习神经算子，适用于含缺失值的时空数据
tldr: 真实科学应用中经常面临观测数据不完整的问题，而现有神经算子假设完全观测输入。本文首次系统性地提出从不完全观测中学习神经算子的框架，解决了未观测区域的监督缺失和动态空间不匹配两个根本障碍。在偏微分方程求解任务上，该方法显著优于传统插值后训练的方法，为处理稀疏传感器数据提供了有效方案。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 传感器限制和成本导致观测数据不完整，但神经算子要求完全空间输入，限制了实际应用。
method: 提出潜在自回归建模框架，通过分解未观测区域和动态空间对齐来学习神经算子。
result: 在多个PDE基准上优于插值后训练的方法，有效处理部分观测数据。
conclusion: 首次系统解决了从不完全观测学习神经算子的问题，可推广到时空预测。
---

## Abstract
Real-world scientific applications frequently encounter incomplete observational data due to sensor limitations, geographic constraints, or measurement costs. Although neural operators significantly advanced PDE solving in terms of computational efficiency and accuracy, their underlying assumption of fully-observed spatial inputs severely restricts applicability in real-world application. We introduce the first systematic framework for learning neural operators from partial observation. We identify and formalize two fundamental obstacles: (i) the supervision gap in unobserved regions that prevents effective learning of physical correlations, and (ii) the dynamic spatial mismatch between incomplete inputs and complete solution fields. Specifically, our proposed LANO (Latent Autoregressive Neural Operator) introduces two novel components designed explicitly to address the core difficulties of partial observations: (i) a mask-to-predict training strategy that creates artificial supervision by strategically masking observed regions, and (ii) a Physics-Aware Latent Propagator that reconstructs solutions through boundary-first autoregressive generation in latent space. Additionally, we develop POBench-PDE, a dedicated and comprehensive benchmark designed specifically for evaluating neural operators under partial observation conditions across three PDE-governed tasks. LANO achieves state-of-the-art performance with relative error reductions ranging from eighteen to sixty-nine percent across all benchmarks under patch-wise missingness with missing rates below fifty percent, including real-world climate prediction. Our approach effectively addresses practical scenarios with missing rates of up to seventy-five percent, to some extent bridging the existing gap between idealized research settings and the complexities of real-world scientific computing.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与研究动机

- **研究背景**：神经算子（Neural Operators）在求解偏微分方程（PDE）方面取得了显著进展，能够高效、准确地学习输入-输出映射。然而，现有神经算子普遍假设输入空间完全观测，这在真实科学应用中往往不成立。真实场景（如气象观测、地震勘探、医学成像）中，由于传感器限制、地理障碍或测量成本，数据常呈现部分观测（Partial Observation）状态，即时空网格中大量区域缺失。
- **核心问题**：从部分观测数据中学习神经算子面临两大根本性障碍：
  - **监督缺失**（Supervision Gap）：在未观测区域没有真实解（ground truth），模型无法学习这些区域的物理相关性。
  - **动态空间不匹配**（Dynamic Spatial Mismatch）：部分观测输入与完整输出解场之间的空间域不对齐，且缺失模式随样本动态变化，现有架构无法处理。
- **研究目标**：首次系统性地提出一个框架，使得神经算子能够直接从部分观测数据中有效学习，并推广到未见过的部分观测输入。

## 2. 方法论

### 核心思想
- 结合**掩码预测训练策略**（Mask-to-Predict, MPT）与**物理感知潜在自回归框架**，在潜在空间内逐步从未观测区域边界向外传播信息，实现物理一致的重建。

### 关键技术细节

#### 2.1 掩码预测训练策略（MPT）
- **动机**：解决未观测区域监督缺失问题。受NLP和CV中掩码建模启发，在训练时对**已观测部分**额外施加随机掩码，制造伪缺失区域，同时保留真实观测输出作为监督。
- **操作**：对输入序列应用随机掩码 \(\hat{M}\)，进一步遮挡部分已观测点，模型需从更有限的上下文预测下一时刻完整解。通过一致性正则化（Consistency Regularization）确保原始输入与掩码输入的预测对齐。
- **效果**：鼓励模型学会从局部观测推断全局物理状态，提升对真实缺失区域的泛化能力。

#### 2.2 潜在自回归神经算子（LANO）
- **整体架构**：由三部分组成：时间聚合层（Temporal Aggregation Layer）、潜在算子层（Latent Operator Layer）、输出投影层（Output Projection Layer）。
- **时间聚合层**：将输入位置坐标与T帧观测场拼接，通过线性层嵌入为深度特征 \(Y^0\)。
- **物理感知潜在传播器（PhLP）**：核心创新，实现边界优先（Boundary-First）的自回归传播。
  - **物理交叉注意力（PhCA）**：在潜在空间中，编码器通过注意力将观测区域特征聚合为潜在令牌（latent tokens），解码时利用这些令牌和边界信息逐步重建。
  - **边界优先传播**：使用部分卷积（Partial Convolution, PConv）从观测区域向未观测区域逐步扩展，模拟PDE中边界条件向外传播的物理过程。
  - **注意力复用**（Attention Reusing）：编码器生成的注意力图在解码器中被复用，避免重新计算带来的不稳定性，增强空间一致性。
  - **理论依据**：PhLP等价于一个可学习的积分算子，该算子通过低秩分解近似核函数，并在观测点处包含残差自更新项（Theorem 3.1）。

#### 2.3 训练与推理
- 训练时采用一步预测损失（MSE），监督仅施加在观测区域（包括掩码后的伪观测），但MPT创建的伪缺失区域有真实标签，从而间接学习全局物理关系。
- 推理时，模型接受部分观测输入，通过PhLP逐步填充未观测区域，最终输出完整解场。

## 3. 实验设计

### 数据集与场景
- **POBench-PDE**：作者构建的面向部分观测条件下的PDE求解基准套件，包含三个任务：
  - **Navier-Stokes**：2D湍流模拟（20时间步，4096网格点）
  - **Diffusion-Reaction**：生物模式形成（20时间步，4096网格点）
  - **ERA5**：真实气候再分析数据（14时间步，16200网格点）
- **缺失模式**：
  - **点缺失**（Point-wise）：独立伯努利采样，模拟随机传感器故障。
  - **块缺失**（Patch-wise）：连续空间块遮蔽，模拟区域遮挡或传感器阵列失效。
- **缺失率设置**：训练时使用不同缺失率 \(s \in \{5\%, 25\%, 50\%\}\)，测试时在相应更高的缺失率下评估（例如训练5%缺失，测试5%和25%），评估模型泛化能力。

### 基准方法
- **MIONet**（Jin et al., 2022）：多输入DeepONet
- **OFormer**（Su et al., 2024）：基于Transformer的算子
- **CORAL**（Serrano et al., 2023）：神经辐射场启发的方法
- **GNOT**（Hao et al., 2023）：线性Transformer算子
- **IPOT**（Lee & Oh, 2024）：压缩潜在空间注意力算子
- **LNO**（Wang & Wang, 2024）：2024年最先进神经算子
- 对比的变体：LANO-S（使用显式位置编码重新计算注意力）和LANO（默认复用注意力）

### 评价指标
- 相对L2误差（Relative L2 Error），在**完整输出域**上计算（而非仅观测区域），考察模型对未观测区域的外推能力。

## 4. 资源与算力

- 论文未明确说明训练使用的GPU型号、数量及训练时长。仅在Table 3中给出了模型参数量、内存占用（MB）和每轮训练时间（秒/epoch），但未说明具体硬件环境。因此**缺乏明确的算力开销报告**，这是论文的一个信息缺失。

## 5. 实验数量与充分性

- **主要实验结果（Table 1）**：涵盖3个数据集 × 3种训练缺失率 × 2种缺失模式 × 2种测试缺失率 = 共36个分组实验（其中部分基线不适用），包含LANO及6个基线方法。
- **消融实验（Table 3）**：包括特征数量（#Feats）、核心组件（边界优先BF、令牌混合TM、MPT）、令牌混合器设计（MLP vs Attention），在Navier-Stokes和Diffusion-Reaction两个任务上评估。
- **额外分析**：
  - MPT有效性分析（图3(b)）：数据量200-800，缺失率0%-50%，块缺失。
  - 物理感知传播可视化（图3(a)）：不同缺失位置下的层间特征演变。
  - 补丁大小影响（图4）：块大小4-8，25%缺失率。
  - 可扩展性分析（图5）：数据量和层数对性能的影响。
- **充分性评估**：实验设计较为全面，覆盖了多种缺失模式、缺失率、数据规模，并进行了消融验证。但缺少对更高维PDE（如3D）或更复杂几何（不规则网格）的评估，且未与基于插值+完全观测训练的方法进行系统对比（仅在补丁大小实验中对比了FFNO-interp和LNO-interp）。

## 6. 主要结论与发现

1. **MPT策略至关重要**：直接训练会因监督缺失导致不收敛，MPT可带来高达84.3%的误差降低（Diffusion-Reaction，50%缺失率，200样本）。
2. **LANO显著优于现有方法**：在块缺失且缺失率<50%时，相对L2误差降低18%~69%；即使在75%极高缺失率下仍保持竞争力。
3. **边界优先自回归传播有效**：可视化显示特征从浅层稀疏碎片逐步演化为深层连贯物理结构，且不同缺失位置最终收敛到相似表示，体现物理一致性。
4. **注意力复用（LANO）优于重新计算（LANO-S）**：尤其在结构缺失区域，复用编码器生成的注意力提供稳定先验。
5. **LANO可扩展性好**：随着数据量和模型深度增加，性能持续提升，适合作为基础模型骨干。

## 7. 优点

- **问题新颖且实用**：首次系统性地将神经算子学习从完全观测推广到部分观测，直接面向真实数据稀缺陷阱。
- **方法合理且可解释**：MPT借鉴自监督学习思想，PhLP基于PDE边界条件物理直觉设计，理论证明其与积分算子的等价性。
- **基准构建完备**：POBench-PDE包含合成与真实数据、多种缺失模式与缺失率，为后续研究提供标准化评估平台。
- **消融实验充分**：逐一验证各组件贡献，并分析特征维度、令牌混合器选择等超参数影响，结论可靠。
- **结果显著**：在多个设定下大幅超越现有基线，且对缺失模式和缺失率均具有鲁棒性。

## 8. 不足与局限

- **缺失模式生成策略简单**：训练时使用固定随机掩码，未探索自适应或与任务相关的掩码策略，可能限制泛化。
- **未涉及不规则网格**：实验均基于规则网格（Navier-Stokes 64x64，ERA5 16200点），对不规则几何（如三角形网格、点云）未验证，而这些在真实应用中常见。
- **高维PDE未验证**：仅测试2D时间相关PDE，未扩展到3D或更高维问题，方法是否可扩展未知。
- **算力开销未报告**：缺乏GPU型号、训练总时长等关键信息，难以复现和比较计算成本。
- **部分基线不适用**：如CORAL在ERA5上无法运行，且部分基线在缺失率较高时性能极差，可能存在实现差异。
- **未与基于插值预处理的方法系统对比**：仅在小节4.4补丁大小实验中简单对比了FFNO-interp和LNO-interp，缺少更广泛的比较（如采用Kriging、Gaussian Process等物理插值后训练传统算子）。
- **理论分析相对简单**：Theorem 3.1证明PhLP等价于积分算子，但未分析近似误差界或收敛性。

（完）
