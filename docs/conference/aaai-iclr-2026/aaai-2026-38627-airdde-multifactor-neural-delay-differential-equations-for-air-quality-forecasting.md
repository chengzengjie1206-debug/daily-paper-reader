---
title: "AirDDE: Multifactor Neural Delay Differential Equations for Air Quality Forecasting"
title_zh: AirDDE：用于空气质量预测的多因子神经延迟微分方程
authors: "Binqing Wu, Zongjiang Shang, Shiyu Liu, Jianlong Huang, Jiahui Xu, Ling Chen"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38627/42589"
tags: ["query:ts-air-qual"]
score: 9.0
evidence: 提出AirDDE神经延迟微分方程框架用于空气质量预测
tldr: 该论文针对现有空气质量预测模型忽略污染物传播延迟的问题，提出AirDDE，首个将延迟建模融入神经微分方程的框架。通过记忆增强注意力模块自适应捕获多因子延迟效应，并结合物理指导进行连续时间演化。在真实空气质量数据上的实验表明，AirDDE优于现有深度学习方法，为空气质量预测提供了更准确的物理一致模型。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 现有方法将污染物动态视为瞬时过程，忽略了传播延迟，预测精度受限。
method: 提出AirDDE，结合记忆增强注意力和物理引导的延迟微分方程进行连续时间预测。
result: 在真实空气质量数据集上，AirDDE的预测性能显著优于现有深度模型。
conclusion: 纳入延迟机制和物理指导可提升空气质量预测的准确性。
---

## Abstract
Accurate air quality forecasting is essential for public health and environmental sustainability, but remains challenging due to the complex pollutant dynamics. Existing deep learning methods often model pollutant dynamics as an instantaneous process, overlooking the intrinsic delays in pollutant propagation. Thus, we propose AirDDE, the first neural delay differential equation framework in this task that integrates delay modeling into a continuous-time pollutant evolution under physical guidance. Specifically, two novel components are introduced: (1) a memory-augmented attention module that retrieves globally and locally historical features, which can adaptively capture delay effects modulated by multifactor data; and (2) a physics-guided delay evolving function, grounded in the diffusion-advection equation, that models diffusion, delayed advection, and source/sink terms, which can capture delay-aware pollutant accumulation patterns with physical plausibility. Extensive experiments on three real-world datasets demonstrate that AirDDE achieves the state-of-the-art forecasting performance with an average MAE reduction of 8.79% over the best baselines.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有深度学习空气质量预测方法通常将污染物动态建模为**瞬时过程**（依赖当前状态），忽略了污染物在传播中固有的**延迟效应**（例如上游排放需数小时才能影响下游），导致预测精度受限。
- **研究意义**：准确预测空气质量对公共卫生和环境可持续性至关重要，而延迟效应是大气物理过程的固有特征，必须显式建模才能提升预测的物理一致性和准确性。
- **背景**：传统方法（物理化学模拟、浅层机器学习）能力有限；深度学习方法（CNN、RNN、STGNN、Transformer）虽强，但多在离散时间步上建模；神经 ODE（NODE）虽可连续时间建模，仍假设瞬时响应，未考虑延迟；现有 NDDE 仅支持全局统一延迟，无法捕捉位置-时间特异的延迟。

### 2. 论文提出的方法论

#### 核心思想
提出 **AirDDE** —— 首个将**延迟建模**融入**神经延迟微分方程（NDDE）** 的空气质量预测框架。通过多因子数据（气象、地理等）自适应捕获延迟效应，并在物理方程（扩散-平流方程）指导下进行连续时间演化。

#### 关键技术细节
- **整体架构**：STGNN 编码器 → 扩散/平流图构建 → 记忆增强注意力（MAA）模块 → 物理指导延迟演化（PDE）函数 → DDE 求解器 → STGNN 解码器。
- **扩散-平流图构建**：
  - 扩散图：基于 Haversine 距离和高斯核构建，模拟邻近扩散。
  - 平流图：利用实时风速、风向和距离，构造**有向边**，边表示从上游到下游的污染物传输路径，边权重对应**传输延时**（τ）。
- **记忆增强注意力模块（MAA）**：
  - **全局记忆**：可学习全局记忆单元，通过注意力聚合全局历史模式。
  - **局部记忆**：基于平流图定义动态邻域，对邻居历史特征（含时间延迟 τ）进行注意力聚合，捕获局部瞬态事件。
  - 输出融合全局和局部信息，为延迟微分方程提供初始状态。
- **物理指导延迟演化函数（PDE）**：
  - 基于扩散-平流方程，显式建模三项：
    - 扩散项：`D·GNN_diff(Adiff, ht)`
    - 延迟平流项：`GNN_adv(At_adv, h_{t-τ})` —— 依赖历史状态
    - 源/汇项：`f(ht || M)` 从多因子特征学习
  - 使用 Chebyshev GNN 近似空间算子，提高效率。
- **DDE 求解器**：采用四阶 Runge-Kutta 积分（torchdiffeq），并维护历史状态缓冲区以处理延迟。
- **损失函数**：Huber loss，对异常值鲁棒。

### 3. 实验设计

#### 数据集
- **KnowAir**：中国 184 城市，PM2.5 + 17 气象因子，粒度 3h，2015-2018。
- **China-AQI**：中国 209 城市，AQI + 7 气象因子，粒度 1h，2017-2019。
- **US-PM**：美国 175 城市，PM2.5 + 7 气象因子，粒度 1h，2020-2021。

#### 对比方法（共 19 个基线）
- **STGNN 类**：DCRNN, STGCN, ASTGCN, MTGNN, PM25GNN, GAGNN, MegaCRN, HimNet
- **注意力类**：Crossformer, AirFormer, PDFormer, iTransformer, STMFormer
- **NODE 类**：STGODE, STG-NCDE, SGODE, STDDE, AirPhyNet, AirDualODE

### 4. 资源与算力

论文明确说明：“All experiments are conducted on a single A100 GPU”，并采用 Adam 优化器，初始学习率 0.005，最大 100 epoch，早停 patience=10。**未提供具体训练时长和 GPU 内存数值**，但效率研究中给出了 China-AQI 数据集上的对比（AirDDE GPU 内存 10.46 GB，每 epoch 训练时间 9.24 分钟）。

### 5. 实验数量与充分性

- **主实验**：3 个数据集 × 19 个基线，报告 MAE/RMSE/SMAPE 或 MAPE，结果全面。
- **消融实验**：在 KnowAir 上对 MAA 模块（去掉整体、去掉全局、去掉局部）和 PDE 函数（去掉整体、去掉源汇项、用注意力替代物理项）共 6 个变体，分短期/中期/长期分析。
- **长时预测**：固定输入 96 步，输出 24/48/96/168 步，对比 AirFormer 和 AirDualODE。
- **超参数研究**：调节时间延迟 τ（0-3）和全局记忆单元数（8/16/32/64），在 China-AQI 和 US-PM 上分析。
- **效率研究**：对比 GPU 内存、训练时间、MAE（China-AQI 单数据集）。
- **鲁棒性研究**：缺失率（10%/30%/50%）和噪声（SNR 80/60/40 dB）测试，对比 5 个强基线。
- **案例研究**：两个可视化案例（城市间和区域间平流延迟效应）。
- **结论**：实验设计充分、对比公平，覆盖多个维度，验证了方法的有效性、鲁棒性和物理合理性。

### 6. 论文的主要结论与发现

1. **AirDDE 在所有指标上取得最好性能**：在 KnowAir、China-AQI、US-PM 上 MAE 分别比第二名降低 9.23%、9.85%、7.3%，平均降低 8.79%。
2. **延迟建模有效**：对比无延迟的 NODE 方法（如 AirDualODE），AirDDE 实现了显著提升。
3. **物理指导有益**：移除物理先验（改用纯数据驱动）后性能下降，表明扩散-平流方程提供了结构先验。
4. **鲁棒性强**：在高缺失率和高噪声条件下，AirDDE 优势更明显，得益于全局记忆和物理约束。
5. **长时预测能力突出**：随着预测步长增加，AirDDE 优势扩大，延迟建模尤为关键。

### 7. 优点

- **创新性**：首次将 NDDE 引入空气质量预测，显式建模延迟效应，并融合多因子动态调制延迟。
- **物理一致性**：基于扩散-平流方程构建演化函数，避免纯数据驱动的黑箱不足。
- **模块化设计**：MAA 模块可独立提取全局和局部延迟特征；PDE 函数可解释性强。
- **全面实验**：超参数、消融、鲁棒性、案例研究覆盖全面，结果可信。
- **开源代码**：提供 GitHub 仓库，可复现。

### 8. 不足与局限

- **效率问题**：DDE 求解器需要维护历史状态缓冲区，训练时间略长于一些离散模型（如 AirFormer），尽管精度有提升。
- **延迟建模假设**：平流图构造假设风场均匀、直线传播，未考虑复杂地形和湍流影响。
- **未考虑不确定性**：风场存在随机性，延迟应为概率分布而非确定性值，论文未讨论。
- **复合延迟**：论文提到“compound delays”（中间区域的复杂路径）为未来工作，当前模型未显式建模。
- **实验范围**：仅使用三个数据集（中国大陆和美国），未验证其他区域（如欧洲、东南亚）的泛化能力。
- **算力细节**：未提供总训练花费、超参搜索次数等，不利于成本评估。

（完）
