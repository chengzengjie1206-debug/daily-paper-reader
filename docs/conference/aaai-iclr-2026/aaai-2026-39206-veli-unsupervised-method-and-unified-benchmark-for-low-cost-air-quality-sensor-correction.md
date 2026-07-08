---
title: "Veli: Unsupervised Method and Unified Benchmark for Low-Cost Air Quality Sensor Correction"
title_zh: Veli：用于低成本空气质量传感器校正的无监督方法与统一基准
authors: "Yahia Dalbah, Marcel Worring, Yen-Chia Hsu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39206/43167"
tags: ["query:ts-air-qual"]
score: 6.0
evidence: 用于低成本空气质量传感器校正的无监督贝叶斯模型，方法可迁移至污染预测
tldr: 该论文针对低成本空气质量传感器读数存在漂移和校准误差的问题，提出了一种名为Veli的无监督贝叶斯模型，利用变分推断在无需参考站的情况下校正传感器读数。实验表明Veli能有效减少误差，其方法可为后续空气质量预测提供更准确的输入数据，相关思路可迁移至深度学习预测架构。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 低成本空气质量传感器读数受环境影响不准确，需要无需参考站的校正方法。
method: 提出Veli无监督贝叶斯模型，利用变分推断构建传感器读数的解缠表示，实现校准。
result: 无需参考站即可有效校正低成本传感器读数，在统一基准上表现优异。
conclusion: Veli降低了空气质量监测部署成本，可为预测模型提供更可靠的数据源。
---

## Abstract
Urban air pollution is a major health crisis causing millions of premature deaths annually, underscoring the urgent need for accurate and scalable monitoring of air quality (AQ). While low-cost sensors (LCS) offer a scalable alternative to expensive reference-grade stations, their readings are affected by drift, calibration errors, and environmental interference. To address these challenges, we introduce Veli (Reference free Variational Estimation via Latent Inference), an unsupervised Bayesian model that leverages variational inference to correct LCS readings without requiring co-location with reference stations, eliminating a major deployment barrier. Specifically, Veli constructs a disentangled representation of the LCS readings, effectively separating the true pollutant reading from the sensor noise. To build our model and address the lack of standardized benchmarks in AQ monitoring, we also introduce the Air Quality Sensor Data Repository (AQ-SDR). AQ-SDR is the largest AQ sensor benchmark to date, with readings from 23,737 LCS and reference stations across multiple regions. Veli demonstrates strong generalization across both in-distribution and out-of-distribution settings, effectively handling sensor drift and erratic sensor behavior. Appendices are available in the extended version.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：城市空气污染严重，低成本空气质量传感器（LCS）虽能扩展监测覆盖，但其读数受漂移、校准误差和环境干扰影响，准确性不足。传统校正方法依赖与高成本参考站共定位，成本高、部署困难，且难以应对长期漂移和季节性变化。
- **研究背景**：超过90%的全球人口呼吸超标空气，实时监测至关重要。LCS成本低、可大规模部署，但原始数据不可靠。现有方法多为监督学习，需要共定位数据对，且缺乏标准化基准。
- **整体含义**：提出一种无需参考站的无监督校正方法，可降低部署门槛，并建立统一基准推动研究标准化。

### 2. 论文提出的方法论

- **核心思想**：使用变分推断构建概率模型，将传感器噪声与真实污染读数分离。模型利用变分自编码器（VAE）将高维噪声输入映射到低维潜变量，再解码得到校正后的清洁读数。无需参考站，仅利用多个LCS的读数（同一地点同时部署多台传感器）和缺失数据掩码。
- **关键技术细节**：
  - 假设LCS读数服从高斯分布：\( x_{\text{noise}} \sim \mathcal{N}(y + \mu_{\text{sens}}, \Sigma_{\text{sens}}) \)，其中 \( y \) 为真实值，\( \mu_{\text{sens}} \) 和 \( \Sigma_{\text{sens}} \) 为非线性偏差和异方差噪声。
  - 引入潜变量 \( z \) 辅助建模噪声，通过辅助参数 \( \psi \)（缺失掩码）条件化。
  - 先验分布：\( p(z|\psi) = \mathcal{N}(\mu(\psi), \Sigma(\psi)) \)，\( p(y|z, \psi) = \mathcal{N}(\mu(z,\psi), \Sigma(z,\psi)) \)。
  - 变分后验：\( q_\phi(z|x_{\text{noise}}, \psi) = \mathcal{N}(\mu_\phi^z, \Sigma_\phi^z) \)，\( q_\theta(y|z, x_{\text{noise}}, \psi) = \mathcal{N}(\mu_\theta^y, \Sigma_\theta^y) \)。
  - 优化目标：最小化负ELBO，包含三个项：\( \beta_z D_{KL}(q_\phi\|p(z|\psi)) \)、\( \beta_y D_{KL}(q_\theta\|p(y|z,\psi)) \) 和重构项（含 \( \mu_{\text{sens}}, \Sigma_{\text{sens}} \) 的采样）。
- **算法流程**：输入多个LCS的时齐读数（每小时）和缺失掩码 -> 编码器输出潜变量 \( z \) 的均值和方差 -> 采样 \( z \) -> 解码器输出 \( y \) 的均值和方差（校正值）-> 损失函数驱动分离噪声和信号。推理时直接取 \( \mu_\theta^y \) 作为校正结果。模型按快照处理（单时刻），不建模时间依赖，但利用 Lipschitz 连续层保留时间特征。

### 3. 实验设计

- **数据集**：自建基准 **AQ-SDR**（最大空气质量传感器数据集，含23,737个传感器，涵盖多区域，时间跨度80个月）。数据来源：SamenMeten、Sensor.Community、LASS社区（LCS），以及LuchtMeetNet、KNMI、EEA、台湾环保署（参考站）。
  - **分布内 (ID)**：荷兰99个站点（训练） + 5个站点（测试，每个站点有参考站），低污染水平。
  - **分布外 (OOD)**：台湾55个站点（训练） + 5个站点（测试），高污染水平。
  - 每个站点含10个LCS，每小时采样PM2.5，缺失值用掩码标记。
- **基准**：目前无参考免费校正方法的统一基准，故对比传统盲去噪方法：**Kalman Filter (KF)** 和 **PCA denoising**，均使用KNN填充缺失值。同时比较原始LCS读数的均值和Veli校正结果。
- **评价指标**：MAE（平均绝对误差）。报告5次不同随机种子的均值±标准差。

### 4. 资源与算力

- **算力**：文中明确说明：模型使用 PyTorch 2.3.1 实现，训练在 **NVIDIA RTX 3090 GPU** 上进行，训练 100 个 epoch，batch size 64，初始学习率 1×10⁻⁶。未提及GPU数量和总训练时长。数据集处理和模型调优的算力消耗未详述。

### 5. 实验数量与充分性

- **实验组数**：
  - 分布内 (ID) 测试：5个城市（阿姆斯特丹、鹿特丹、乌得勒支、艾默伊登、奈梅亨），每个城市做5次随机种子实验 → 25次。
  - 分布外 (OOD) 测试：5个城市（台中、台南、桃园、台北、朴子），包括零样本和微调两种设置，各5次随机种子 → 50次。
  - 消融实验：模拟传感器故障（注入1-9个缺失值）对5个ID城市的影响；不同传感器数量（3、5、7、10）的表现。
  - 时间分析：自相关对比（Raw LCS vs Veli vs Reference）。
  - 命中率分析（图6）：误差阈值与命中率关系。
  - 敏感性分析（扩展版本提及，但正文未详述）。
- **充分性评价**：实验覆盖ID和OOD场景，包含消融和鲁棒性分析，结果报告了标准差，表明多次重复。对比基线方法（KF、PCA）合理。但缺乏与更多现代无监督方法的比较（如其他VAE变体、生成模型）。整体实验设计较为充分、客观、公平。

### 6. 论文的主要结论与发现

- Veli在ID数据上大幅降低MAE（例如乌得勒支从24.77降至5.25），在OOD零样本设置下表现尚可，但方差大；微调后显著提升稳定性。
- Veli能处理传感器漂移、缺失值、极端噪声，校正后自相关性接近参考站。
- 即使在传感器故障（90%通道缺失）下，MAE仍低于10（图8）。
- 仅需3个传感器即可有效校正（图9），但推荐10个以保证数据可用性。
- 命中率分析表明：要覆盖80%数据，原始LCS需误差阈值20，Veli只需7.34。
- Veli是首个完全无参考的校正方法，结合AQ-SDR基准，为LCS校正提供了新范式。

### 7. 优点

- **方法创新**：首次提出无监督、无需参考站的贝叶斯校正模型，消除了共定位部署障碍。
- **数据集贡献**：AQ-SDR是迄今最大且最多样化的空气质量传感器基准，包含真实故障模式（漂移、缺失、尖峰），促进公平比较。
- **鲁棒性好**：对传感器数目减少、传感器故障、分布偏移均表现稳健。
- **实验严谨**：多次随机种子、ID和OOD评估、消融分析充分。
- **可复现性**：代码、数据集、扩展版本均公开。

### 8. 不足与局限

- **实验对比局限**：未与更多无监督方法（如CaliFormer的无监督阶段、其他VAE变体、时序模型）比较，仅对比传统去噪方法。
- **模型假设**：假设同一地点多个传感器读数可联合使用，单点部署时无法应用；且需至少3个传感器。
- **时间建模不足**：模型按快照处理，虽声称保留时间信息，但未与时间序列模型（如LSTM、Transformer）对比校正性能。
- **OOD零样本方差大**：表明模型对分布偏移敏感，微调虽改善但增加了计算成本。
- **超参数敏感性**：α, β_z, β_y 仅给出固定值，敏感性分析在扩展版本但正文未详述。
- **应用限制**：仅针对PM2.5，对其他污染物（如NO₂、O₃）的泛化性未验证。评价指标仅用MAE，未用更全面的指标（如R²、KGE）。
- **现实部署验证**：实验基于公开数据集，缺乏真实大规模部署中的长期运行验证（如一年以上连续校正效果）。

（完）
