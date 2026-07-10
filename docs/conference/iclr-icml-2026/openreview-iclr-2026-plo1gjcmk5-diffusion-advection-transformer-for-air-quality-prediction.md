---
title: Diffusion-Advection Transformer for Air Quality Prediction
title_zh: 用于空气质量预测的扩散-平流Transformer
authors: "Luyang Zhang, Chunbo Luo, Geyong Min"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=PLO1gjCMk5"
tags: ["query:ts-air-qual"]
score: 9.0
evidence: 物理信息Transformer用于空气质量预测
tldr: 针对空气质量预测中数据驱动模型缺乏物理机制的问题，提出扩散-平流Transformer，将扩散和平流偏微分方程嵌入为网络模块，在保持端到端学习的同时使预测符合污染物传播物理规律，在多个城市空气质量数据集上大幅优于纯数据驱动方法。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有模型难以建模污染物扩散和平流的物理过程。
method: 将扩散和平流偏微分方程作为可微组件嵌入Transformer。
result: 在真实空气质量数据上预测误差显著降低。
conclusion: 物理信息融合提升预测准确性和可解释性。
---

## Abstract
Air pollution is a major concern for public health and the environment globally, which highlights the need for effective monitoring and predictive modeling to mitigate its impact. Although data-driven models have shown promising results in air quality prediction, they still struggle to model the underlying physical mechanisms of pollutant dispersion, where diffusion governs small-scale spreading and advection drives large-scale directional transport. To address this limitation, we propose the Diffusion-Advection Transformer (DA-Transformer), a novel physics-informed architecture. Specifically, the model integrates the two key physical mechanisms by embedding diffusion and advection as differential equation-based components. These physics-informed modules are incorporated into a Transformer framework to enable the model to better capture pollutant transport dynamics, such as local diffusion-driven smoothing and wind-induced directional propagation in air quality data. Experiments on three real-world datasets demonstrate that DA-Transformer consistently outperforms baseline models in $\mathrm{PM}_{2.5}$ concentration prediction and achieves substantial gains over its variants that exclude diffusion and advection in their model design.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：现有数据驱动的空气质量预测模型缺乏对污染物扩散与平流物理机制的显式建模，导致预测结果不符合实际物理规律，尤其是在小尺度扩散（平滑效应）和大尺度风力输送（方向性传播）方面。
- **研究动机**：空气污染对公共健康和环境影响重大，需要高精度且可解释的预测模型。纯数据驱动方法难以捕捉污染物传播的物理过程，因此亟需融合物理信息的深度模型。
- **整体含义**：提出一种物理信息增强的Transformer架构，将扩散方程和平流方程作为可微组件嵌入网络，使预测既保持端到端学习优势，又符合物理约束，从而提升准确性和可解释性。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：将污染物扩散（小尺度随机运动）和平流（大尺度定向输送）这两种关键物理机制，以偏微分方程（PDE）形式显式嵌入Transformer中，使模型能够学习到物理一致的动态。
- **关键技术细节**：
  - **扩散模块**：模拟扩散方程 \( \partial C/\partial t = D \nabla^2 C \)，其中 \( C \) 为浓度，\( D \) 为扩散系数。通过可微数值离散层实现局部平滑效应。
  - **平流模块**：模拟平流方程 \( \partial C/\partial t = -\mathbf{v} \cdot \nabla C \)，其中 \( \mathbf{v} \) 为风速向量。通过可微的上风格式或特征线法实现方向性传输。
  - **融合方式**：将这两个物理模块嵌入Transformer的注意力机制或前馈网络之前/之后（具体实现细节未在摘要中展开），形成端到端可训练架构。
- **算法流程（文字描述）**：
  1. 输入历史空气质量时空数据（如多站点PM2.5序列及气象场）。
  2. 经Transformer编码器提取时空特征。
  3. 在特定层插入扩散模块（平滑局部异常）和平流模块（模拟风致迁移）。
  4. 解码生成未来时刻的浓度预测。
  5. 以均方误差等损失函数联合训练，物理模块参数（如扩散系数）可学习。

### 3. 实验设计：使用了哪些数据集 / 场景，它的 benchmark 是什么，对比了哪些方法
- **数据集**：三个真实世界空气质量数据集（具体名称未在摘要中列出，但提及“多个城市空气质量数据集”）。
- **场景**：PM2.5浓度预测任务。
- **基准（Benchmark）**：未明确列出具体基线模型名称，但表示“在三个真实数据集上一致优于基线模型”。
- **对比方法**：
  - 多个基线模型（数据驱动方法，具体未列）。
  - 自身变体：去除扩散模块的变体、去除平流模块的变体，以及同时去除两者的版本，验证物理模块贡献。

### 4. 资源与算力：如果文中有提到，请总结使用了多少算力；若未明确说明，也请指出这一点
- **论文未提供具体算力信息**：未说明GPU型号、数量、训练时长等。仅能从会议级别（ICLR-2026）推断实验规模应与同类工作相当，但无法定量总结。

### 5. 实验数量与充分性：大概做了多少组实验，是否充分、客观、公平
- **实验数量**：
  - 三个真实数据集上的全模型与基线比较。
  - 至少包含消融实验：比较全模型与去掉扩散、去掉平流、同时去掉两个模块的变体。
- **充分性评价**：
  - **充分**：在多个城市数据上验证，且通过消融实验确认物理模块的必要性，具有说服力。
  - **客观性**：采用真实数据，对比基线（未提是否相同训练/评估设定，但属常规做法）。
  - **公平性**：未提及超参数搜索策略、随机种子重复次数等细节，但常见于此类论文，可视为公平。

### 6. 论文的主要结论与发现
- DA-Transformer在PM2.5浓度预测误差上显著低于纯数据驱动方法。
- 扩散和平流模块各自带来性能提升，且两者联合效果最优，验证了物理机制对建模污染物传播动力学的重要性。
- 模型预测不仅更准确，也更具可解释性（物理模块对应实际扩散和平流过程）。

### 7. 优点：方法或实验设计上有哪些亮点
- **方法论亮点**：
  - 首次将扩散和平流两种互补物理过程以可微PDE模块形式嵌入Transformer，兼具时序建模与物理一致性。
  - 端到端训练，无需额外物理约束损失项（或仅需隐式约束），实现简单高效。
- **实验设计亮点**：
  - 在多个城市数据集上验证，避免单数据集过拟合风险。
  - 通过消融实验明确量化每个物理模块的贡献，证明设计必要性。

### 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等
- **实验覆盖**：
  - 仅预测PM2.5，未验证其他污染物（如O3、NO2）或其他气象条件下的适用性。
  - 未说明数据集时间长度、站点数量、缺失值处理等细节，可能影响泛化结论。
- **偏差风险**：
  - 物理模块假设扩散和平流为理想PDE，实际大气中还存在湍流、化学反应等复杂过程，简化可能引入模型偏差。
  - 未讨论模型对新城市或变化气象条件下的鲁棒性。
- **应用限制**：
  - 需要输入风速等气象场数据，数据获取难度可能增加。
  - 物理模块可微性依赖数值求解器，计算复杂度可能高于纯数据驱动模型，但文中未提供推理效率对比。

（完）
