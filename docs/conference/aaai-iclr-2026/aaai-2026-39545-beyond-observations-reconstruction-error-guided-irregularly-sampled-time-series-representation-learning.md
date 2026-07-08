---
title: "Beyond Observations: Reconstruction Error-Guided Irregularly Sampled Time Series Representation Learning"
title_zh: 超越观测：基于重构误差引导的不规则采样时间序列表示学习
authors: "Jiexi Liu, Meng Cao, Songcan Chen"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39545/43506"
tags: ["query:ts-air-qual"]
score: 7.0
evidence: 不规则采样时间序列表示学习
tldr: 不规则采样时间序列（ISTS）建模常忽视训练中产生的重构误差所隐含的未观测值信息。本文提出iTimER，一种自监督预训练框架，利用重构误差引导表示学习，无需显示插补即可捕获数据结构。在下游分类与回归任务中，iTimER在多个ISTS基准上取得竞优性能。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 现有ISTS方法忽略重构误差中的潜在信号，限制了表示学习能力。
method: 提出iTimER，利用训练中的重构误差作为自监督信号指导表示学习。
result: 在分类与回归任务中优于现有ISTS表示学习方法。
conclusion: iTimER揭示了重构误差在ISTS表示学习中的价值，是一种简单有效的预训练方案。
---

## Abstract
Irregularly sampled time series (ISTS), characterized by non-uniform time intervals with natural missingness, are prevalent in real-world applications. Existing approaches for ISTS modeling primarily rely on observed values to impute unobserved ones or infer latent dynamics. However, these methods overlook a critical source of learning signal: the reconstruction error inherently produced during model training. Such error implicitly reflects how well a model captures the underlying data structure and can serve as an informative proxy for unobserved values. To exploit this insight, we propose iTimER, a simple yet effective self-supervised pre-training framework for ISTS representation learning. iTimER models the distribution of reconstruction errors over observed values and generates pseudo-observations for unobserved timestamps through a mixup strategy between sampled errors and the last available observations. This transforms unobserved timestamps into noise-aware training targets, enabling meaningful reconstruction signals. A Wasserstein metric aligns reconstruction error distributions between observed and pseudo-observed regions, while a contrastive learning objective enhances the discriminability of learned representations. Extensive experiments on classification, interpolation, and forecasting tasks demonstrate that iTimER consistently outperforms state-of-the-art methods under the ISTS setting.

---

## 论文详细总结（自动生成）

# 中文总结

## 1. 核心问题与整体含义（研究动机与背景）

- **问题**：不规则采样时间序列（ISTS）普遍存在于医疗、气象、交通等领域，其特点是采样间隔不均匀、变量间异步、天然缺失率高。现有方法主要依赖观测值进行插补或推断潜在动态，但忽略了训练过程中自然产生的**重构误差**这一关键信号。
- **动机**：重构误差隐含着模型对数据结构的理解程度，可以作为未观测位置学习信号的有用代理。若能利用这一信号，可以无需依赖标签或显式插补，更充分地指导表示学习。
- **意义**：论文揭示重构误差的未被挖掘价值，提出 iTimER 框架，将重构误差分布建模为自监督信号，从而生成伪观测，增强训练信号并提升 ISTS 表示质量。

## 2. 方法论：核心思想、关键技术细节与算法流程

- **核心思想**：将观测区域的重构误差建模为高斯分布，从中采样误差并与最近观测值混合（mixup），生成未观测时间点的伪观测，使模型获得噪声感知的训练目标；再通过对齐真实观测与伪观测序列的重构误差分布（Wasserstein 距离）和对比学习，提升表示的可判别性与鲁棒性。
- **关键技术细节**：
  1. **重构不确定性建模**：对观测值计算重构误差 \( \epsilon_t = x_t - \hat{x}_t \)，假设其服从高斯分布 \( \mathcal{N}(\mu_\epsilon, \sigma_\epsilon^2) \)，并通过动量更新累积历史信息。
  2. **伪观测生成**：对未观测时间点，从误差分布中采样 \( \tilde{\epsilon}_t \)，与最后可用观测值 \( x_{t-1} \) 通过 mixup 策略生成伪观测 \( \tilde{x}_t = \alpha_t \cdot x_{t-1} + (1-\alpha_t) \cdot \tilde{\epsilon}_t \)。其中 \( \alpha_t \) 控制混合比例，保留局部时序连续性。
  3. **完整伪序列构建**：保留原观测值，用伪观测填充缺失位置，得到完整序列 \( \tilde{X} \)。
  4. **分布对齐**：通过 2-Wasserstein 距离对齐真实观测与伪观测区域的重构误差分布，促使模型在未观测区域保持相似的误差模式。
  5. **对比学习**：对原始序列与伪序列的表示（共享编码器）施加对比损失，增强嵌入的语义一致性。
  6. **双重建构损失**：同时优化原始观测和伪观测序列的重构损失，确保编码器-解码器有效捕获数据结构和分布。
- **损失函数**：\( L = \alpha L_W + \beta L_{\text{contrast}} + \frac{1}{2}(L_{\text{orig rec}} + L_{\text{pseudo rec}}) \)

## 3. 实验设计

- **数据集**：
  - 分类任务：P12（11,988 患者，36 传感器，缺失率 88.4%）、P19（38,803 患者，34 传感器，缺失率 94.9%）、PAM（5,333 序列，8 类活动，缺失率 60.0%）。
  - 插补与预测任务：PhysioNet（41 传感器，缺失率 85.7%）、MIMIC（96 变量，缺失率 96.7%）、Human Activity（12 传感器，缺失率 75.0%）。
- **基准方法与对比**：
  - 分类基线：Transformer、MTGNN、DGM²-O、IP-Net、GRU-D、SeFT、mTAND、Raindrop、Warpformer、ViTST、FPT（PLM）、Time-LLM、PrimeNet（自监督 ISTS 预训练）。
  - 插补/预测额外对比：Latent-ODE、Neural Flow、CRU、t-PatchGNN、ISTS-PLM。
- **任务设置**：
  - 分类：8:1:1 划分，报告 AUROC/AUPRC（P12/P19）和 Accuracy、Precision、Recall、F1（PAM）。
  - 插补：随机屏蔽 30% 观测，用未屏蔽数据重构，报告 MSE/MAE。
  - 预测：使用前 24 小时预测后 24 小时（PhysioNet/MIMIC），或前 3000ms 预测后 1000ms（Human Activity），报告 MSE/MAE。
- **评估**：5 次独立运行取均值与标准差。

## 4. 资源与算力

- 论文未明确说明使用的 GPU 型号与数量。
- 在 P12 分类任务（batch size=50）上给出了训练时间（0.161 s/批）和内存占用（5.16 GB），但未报告总训练时长或大规模实验的算力需求。
- 图表中展示了同时考虑 AUROC、训练时间和内存的对比，iTimER 在时间/内存效率上优于多数方法（如 ViTST、Warpformer），接近 Raindrop 和 MTGNN。

## 5. 实验数量与充分性

- **实验数量**：覆盖 3 大类下游任务（分类、插补、预测），共 9 个数据集场景；包含消融实验（对比 8 种伪观测策略变体、去除 Wasserstein 损失、去除对比学习）；还提供了效率对比。
- **充分性**：实验设计较为全面，与主流基线及最新自监督/PLM 方法对比，验证了 iTimER 的先进性。消融实验逐一分析各组件贡献，结果清晰。但缺乏在更多样化缺失模式（如块缺失、高度稀疏）或更大规模数据集上的验证，且未进行超参数敏感性分析。
- **客观性**：使用固定数据划分和 5 次重复，报告均值和标准差，结果可信；但 P12/P19 的 AUPRC 方差较大，可能与类别不平衡和缺失率高有关。

## 6. 主要结论与发现

- iTimER 在三个任务上一致优于现有 SOTA 方法，尤其在插补任务上提升显著（PhysioNet 上 MSE 从 4.55×10⁻³ 降至 2.86×10⁻³，MIMIC 上从 1.47×10⁻² 降至 0.13×10⁻²）。
- 重构误差分布作为自监督信号，能有效引导模型学习未观测区域的表示；分布对齐和对比学习均为关键组件，移除后性能下降。
- 伪观测生成中使用局部运动平均（MAve）优于常数或仅误差的方式，表明保留局部动态的重要性。
- iTimER 在计算效率上介于高效与高精模型之间，无需额外标签或显式插补，具有良好实用性。

## 7. 优点

- **创新性**：首次将重构误差提升为主动学习信号，而非仅作损失项，开辟了 ISTS 表示学习新视角。
- **方法简洁有效**：通过误差分布采样+ mixup 生成伪观测，无需复杂生成模型（如扩散模型）或外部监督。
- **任务无关性强**：框架为预训练，可应用于分类、插补、预测等多种下游任务，通用性好。
- **鲁棒性好**：实验结果标准差低，表明模型对不同初始化与数据划分稳定可靠。
- **效率尚可**：相比类似精度的方法（如 ViTST），训练时间与内存更低。

## 8. 不足与局限

- **计算开销**：尽管整体效率不错，但相比最轻量基线（如 Raindrop），仍存在额外计算（误差分布估计、动量更新），在极大规模数据上可能成为瓶颈。
- **高斯假设局限性**：论文假设重构误差服从高斯分布，但复杂模型（如神经网络）的重构误差可能呈现重尾或多峰结构，高斯假设可能不严谨。
- **伪观测生成策略**：默认使用最后观测值作为 mixup 锚点，对长间隔缺失场景可能累积误差；其他锚点（如均值、滑动平均）在消融中表现相近，但未深入探讨自适应选择。
- **实验覆盖不足**：未在缺失率极高（>99%）或多样化缺失模式上进行测试；未分析超参数 α 和 β 的敏感性；缺乏真实世界部署中的延迟和吞吐量评估。
- **可解释性**：未探讨学习到的重构误差分布如何反映模型不确定性，或如何用于决策解释。
- **与 ODE 方法比较**：虽然对比了 Latent-ODE 等，但 iTimER 在预测任务上 Human Activity 数据集未达到 SOTA（仅次于 t-PatchGNN），表明在强时序依赖性场景下可能仍有提升空间。

（完）
