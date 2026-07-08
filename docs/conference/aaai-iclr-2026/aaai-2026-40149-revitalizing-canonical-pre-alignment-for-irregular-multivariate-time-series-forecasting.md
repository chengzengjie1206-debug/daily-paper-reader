---
title: Revitalizing Canonical Pre-Alignment for Irregular Multivariate Time Series Forecasting
title_zh: 重振规范预对齐用于不规则多元时间序列预测
authors: "Ziyu Zhou, Yiming Huang, Yanyun Wang, Yuankai Wu, James Kwok, Yuxuan Liang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40149/44110"
tags: ["query:ts-air-qual"]
score: 9.0
evidence: 提出重振规范预对齐方法用于不规则多元时间序列预测
tldr: 该论文针对不规则多元时间序列预测中规范预对齐（CPA）方法因零填充导致计算开销大的问题，提出改进方案。作者认为CPA在消除变量间异步性方面仍有价值，因此设计了高效策略降低对齐后的序列长度。在多个数据集上验证，改进后的CPA在保持甚至提升预测精度的同时大幅减少了计算时间，为不规则时间序列预测提供了实用高效的基线方案。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 规范预对齐虽能处理变量异步但零填充带来高计算成本，需优化。
method: 保留CPA框架，通过高效策略压缩对齐后序列长度，减少冗余计算。
result: 在多个不规则时间序列数据集上，在保持精度的同时显著降低了计算开销。
conclusion: 优化后的规范预对齐方法是不规则时间序列预测高效且有效的选择。
---

## Abstract
Irregular multivariate time series (IMTS), characterized by uneven sampling and inter-variate asynchrony, fuel many forecasting applications yet remain challenging to model efficiently. Canonical Pre-Alignment (CPA) has been widely adopted in IMTS modeling by padding zeros at every global timestamp, thereby alleviating inter-variate asynchrony and unifying the series length, but its dense zero-padding inflates the pre-aligned series length, especially when numerous variates are present, causing prohibitive compute overhead. Recent graph-based models with patching strategies sidestep CPA, but their local message passing struggles to capture global inter-variate correlations. Therefore, we posit that CPA should be retained, with the pre-aligned series properly handled by the model, enabling it to outperform state-of-the-art graph-based baselines that sidestep CPA. Technically, we propose KAFNet, a compact architecture grounded in CPA for IMTS forecasting that couples (1) a Pre-Convolution module for sequence smoothing and sparsity mitigation, (2) a Temporal Kernel Aggregation module for learnable compression and modeling of intra-series irregularity, and (3) Frequency Linear Attention blocks for low-cost inter-series correlation modeling in the frequency domain. Experiments on multiple IMTS datasets show that KAFNet achieves state-of-the-art forecasting performance, with a 7.2× parameter reduction and an 8.4× training–inference acceleration.

---

## 论文详细总结（自动生成）

### 论文中文详细总结

#### 1. 核心问题与整体含义（研究动机和背景）
- **问题**：不规则多元时间序列（IMTS）存在**采样不均匀**和**变量间异步**的双重不规则性，传统方法（如RNN、Transformer）依赖规则对齐序列，而经典预处理技术——**规范预对齐（CPA）** 通过在各全局时间戳填充零来统一时间轴，能有效缓解变量异步性，但导致序列长度膨胀，尤其在变量多时带来巨大计算开销。
- **背景**：近期基于图神经网络的模型（如GraFITi、tPatchGNN）通过局部消息传递或分片策略绕过CPA，却**难以捕获全局变量间相关性**。因此，作者认为应**保留CPA**，并设计高效模型解决其效率问题，证明CPA仍具优越性。

#### 2. 方法论：核心思想、关键技术细节
- **核心思想**：在保留CPA框架前提下，通过**序列平滑、可学习的时序压缩、频域线性注意力**三个模块，在保持全局对齐优势的同时大幅降低计算复杂度。
- **关键模块**：
  1. **Pre-Convolution模块**：用两个轻量级1D卷积（Conv1×3 + Conv1×1）沿时间方向平滑序列，缓解零填充造成的信息稀疏，并加入时间嵌入。
  2. **Temporal Kernel Aggregation (TKA) 模块**：对每个变量，将时间戳归一化后，用**K个可学习高斯核**软划分时间轴，对观测值加权求和，将变长序列压缩为固定长度表征`z_n ∈ R^d`，同时编码不规则时序模式。
  3. **Frequency Linear Attention (FLA) 块**：在TKA输出上应用**实值FFT**转换到频域，使用**随机傅里叶特征（RFF）** 近似Softmax注意力，实现线性复杂度的全局变量相关性建模，再通过逆FFT恢复。
  4. **输出层**：将FLA输出的变量表征与查询时间戳嵌入拼接，经三层MLP逐点预测未来值。
- **公式流程**：输入O → CPA对齐 → Pre-Conv (式1-3) → TKA (式4-5, 含高斯核加权池化) → 多层FLA块 (式6-10) → 输出MLP预测 (式11) → MSE损失 (式12)。

#### 3. 实验设计
- **数据集**：4个公开IMTS基准：
  - **PhysioNet**（医疗，41变量）
  - **MIMIC**（医疗，96变量）
  - **Human Activity**（生物力学，12变量）
  - **USHCN**（气候，5变量）
- **对比方法**：分为4类，共23种基线：
  - 常规MTS预测：DLinear、TimesNet、PatchTST、Crossformer、GraphWaveNet、MTGNN、StemGNN、CrossGNN、FourierGNN
  - IMTS分类：GRU-D、SeFT、RainDrop、Warpformer
  - IMTS插值：mTAND
  - IMTS预测：Latent ODE、CRU、Neural Flows、tPatchGNN、GraFITi、TimeCHEAT、HyperIMTS
- **协议**：训练/验证/测试按60%/20%/20%划分，所有实验重复5个随机种子。

#### 4. 资源与算力
- **硬件**：单张NVIDIA RTX A6000 GPU。
- **软件**：Adam优化器，MSE损失。文中未提及具体训练时长（仅在图5中给出每batch时间），但明确指出KAFNet训练/推理速度最快。

#### 5. 实验数量与充分性
- **实验组数**：
  - 主实验：4个数据集×5种子，与23种基线对比，给出均值和标准差，并进行了**Friedman检验**（α=0.05）和**Nemenyi事后检验**，确认统计显著性。
  - 消融实验：7种变体（w/o CPA、w/o Pre-Conv、w/o T-Norm、w/o TKA、w/o FLA、w/o FLA & w/ SA）在4个数据集上对比。
  - 超参数分析：图4展示TKA核数（K）和隐藏维度d对MSE的影响。
  - 效率分析：图5比较5种模型的参数、FLOPs、训练/推理时间；图6可视化FLA与SA的注意力图。
- **充分性评价**：实验覆盖多领域、多种基线类型，消融充分，统计检验严谨，对比公平（部分基线统一复现）；但数据集仅4个，领域有限。

#### 6. 主要结论与发现
- KAFNet在4个数据集上**均取得最佳或次佳预测精度**（MSE/MAE），平均排名1.6，显著优于所有基线。
- 相比SOTA图模型，**参数减少7.2倍，训练-推理加速8.4倍**（平均）。
- 保留CPA并优化效率后，CPA模型的性能可超越规避CPA的图模型，证实CPA的潜力。
- 消融结果表明：每个组件（尤其CPA、TKA、FLA）均对性能有贡献；FLA优于标准Softmax注意力（计算更少且注意力分布更丰富）。

#### 7. 优点
- **方法亮点**：
  - 首个直接解决CPA长度爆炸问题的IMTS预测模型。
  - **紧凑高效**：总计算复杂度关于L和N为线性，TKA将后续处理与L解耦。
  - **全局建模能力强**：频域线性注意力捕获变量间全局依赖，克服了图模型局部性。
  - 设计简洁，模块可解释（高斯核压缩、频域注意力）。
- **实验亮点**：
  - 与大量基线（23种）全面对比，包括不同流派（连续时间、图、分片等）。
  - 提供统计显著性检验，增强可信度。
  - 效率分析覆盖参数、FLOPs、时间，直观展示优势。

#### 8. 不足与局限
- **实验覆盖**：仅4个数据集，且领域局限于医疗、生物力学、气候，缺乏大规模或产业级IMTS（如金融、物联网）。数据集规模较小（变量数≤96）。
- **偏差风险**：对CPA的依赖可能受缺失率、时间跨度影响，未探讨不同缺失模式下的鲁棒性。
- **应用限制**：
  - 仅针对**预测任务**，未验证在插值、分类等下游任务的泛化性。
  - TKA中高斯核数K和隐藏维度d需手动调参，对超参数敏感（图4显示）。
  - 假设所有变量共享同一时间网格，对于极端异步情况仍可能效率不佳。
- **资源说明不完整**：未提供总训练时间（如epoch数、总时长），仅给出每batch时间。

（完）
