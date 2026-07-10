---
title: "WIND: Weather Inverse Diffusion for Zero-Shot Atmospheric Modeling"
title_zh: WIND：用于零样本大气建模的天气逆扩散模型
authors: "Michael Aich, Andreas Fürst, Florian Sestak, Carlos Ruiz-Gonzalez, Niklas Boers, Johannes Brandstetter"
date: 2026-04-30
pdf: "https://openreview.net/pdf/2afc72f9661b52400f9769c4a4a8023fad0006eb.pdf"
tags: ["query:ts-air-qual"]
score: 4.0
evidence: 天气基础模型，大气建模与空气质量预测相关
tldr: 大气建模任务分散，现有模型需针对各任务单独训练。本文提出WIND，一个基于无条件视频扩散的预训练基础模型，通过自监督视频重建学习鲁棒的气象先验，无需微调即可替换多种专用模型。实验证明其在多个任务上达到或超越基线，为空气质量预测等应用提供了统一框架。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有大气建模方法碎片化，缺乏统一预训练模型。
method: 采用无条件视频扩散模型进行自监督视频重建预训练。
result: 在多个大气建模任务上实现零样本性能超越专用基线。
conclusion: WIND作为统一基础模型，可有效支持大气相关预测任务。
---

## Abstract
Deep learning has revolutionized weather forecasting, but many challenges remain, including climate modeling. Moreover, the current landscape remains fragmented: highly specialized models are typically trained individually for distinct tasks. To unify this landscape, we introduce WIND, a single pre-trained foundation model capable of replacing specialized baselines across a vast array of tasks. Crucially, in contrast to previous atmospheric foundation models, we achieve this without any task-specific fine-tuning. To learn a robust, task-agnostic prior of the atmosphere, we pre-train WIND with a self-supervised video reconstruction objective, utilizing an unconditional video diffusion model to iteratively reconstruct atmospheric dynamics from a noisy state. At inference, we frame diverse domain-specific problems strictly as inverse problems and solve them via posterior sampling. This unified approach allows us to tackle highly relevant weather and climate problems, including probabilistic forecasting, spatial and temporal downscaling, reconstruction of spatial fields from sparse observations and enforcing global dry air mass conservation. We further demonstrate how WIND can be applied to explore extreme weather events under prescribed out-of-distribution thermodynamic perturbations. By combining generative video modeling with inverse problem solving, WIND offers a computationally efficient alternative for AI-based atmospheric modeling.

---

## 论文详细总结（自动生成）

# 论文总结：WIND：用于零样本大气建模的天气逆扩散模型

## 1. 论文的核心问题与整体含义
- **研究动机**：当前大气建模领域高度碎片化，不同任务（如预报、降尺度、重建等）需要分别训练专门的深度学习模型，缺乏统一的、可复用的预训练基础模型。
- **整体含义**：WIND旨在通过单个预训练模型，在不进行任何任务特定微调的情况下，替代多个专用基线，统一大气建模任务。这标志着向通用大气智能体的重要一步，有望降低研发成本并提升跨任务泛化能力。

## 2. 方法论
- **核心思想**：利用无条件视频扩散模型进行自监督视频重建预训练，学习鲁棒的、任务无关的大气动力学先验。推理时将各种领域问题严格形式化为逆问题，通过后验采样求解。
- **关键技术细节**：
  - 预训练阶段：对大气状态的视频序列（如再分析数据）添加噪声，训练扩散模型从噪声状态迭代重建原始状态，目标为最小化去噪误差。
  - 推理阶段：给定有噪声或不完整的观测（如稀疏站点的测量、粗分辨率场），定义逆问题（例如：给定稀疏观测，重建完整场），使用预训练的扩散模型作为先验，通过条件采样（如重建引导或测量一致性梯度）生成符合条件的完整状态。
  - 无需微调：所有任务共享同一个预训练模型，仅通过改变后验采样的约束（观测算子）来适配不同逆问题。
- **公式或算法流程**（文字说明）：
  1. 预训练扩散模型 \( p_\theta(x_t | x_{t+1}) \)，其中 \( x \) 为大气状态序列。
  2. 对每个下游任务，定义观测模型 \( y = H(x) + \epsilon \)，如 \( H \) 为降采样、掩膜等。
  3. 利用扩散模型进行后验采样：从先验 \( p(x) \) 出发，结合观测似然 \( p(y|x) \) 采样 \( x \sim p(x|y) \)，常用方法为扩散后验采样（如DPS、SMC等）。
  4. 迭代去噪过程生成与观测一致且符合物理先验的输出。

## 3. 实验设计
- **数据集与场景**：论文未明确列出具体数据集，但推断使用了ERA5等全球再分析数据。涵盖的任务场景包括：
  - 概率预报（短期或中期）
  - 空间降尺度（从粗网格到细网格）
  - 时间降尺度（低时间分辨率到高时间分辨率）
  - 从稀疏观测重建空间场（如站点插值）
  - 全局干空气质量守恒约束
  - 极端天气事件探索（在预设的出分布热力学扰动下）
- **Benchmark**：对比了各领域的专用基线模型（如专门的预报模型、降尺度模型、空间插值方法等），具体名称未在摘要中列出。
- **对比方法**：未明确给出，但提到“替代专用基线”，推测包括传统的统计方法及最近CNN/GAN/扩散模型等。

## 4. 资源与算力
- **未明确说明**：论文摘要及元数据未提及GPU型号、数量、训练时长、参数规模等算力信息。需要查看全文才能获得详细硬件和计算开销数据。

## 5. 实验数量与充分性
- **实验组数**：摘要提到在“多个任务”上验证，包括概率预报、降尺度、重建、守恒、极端天气探索等至少5类任务。但未给出每个任务的具体实验次数或消融实验。
- **充分性判断**：由于缺乏详细实验结果（如定量指标、误差分析、显著性检验），无法全面评估实验的充分性。但从任务多样性看，覆盖了大气建模的核心问题，设计思路较为系统。若全文包含完整的指标对比和消融研究，则实验可能是充分的。当前摘要信息不足以判断客观性和公平性。

## 6. 主要结论与发现
- WIND作为单个预训练模型，在多个零样本大气建模任务上达到或超越了专用基线模型的性能。
- 该方法无需任务特定微调，仅通过逆问题后验采样即可适配。
- 结合生成式视频建模与逆问题求解，提供了一种计算高效且统一的大气建模替代方案。
- 在极端天气探索任务中，能够处理分布外扰动，展示出泛化能力。

## 7. 优点
- **零样本泛化能力**：无需微调即可直接迁移到多种任务，极大降低了应用成本。
- **统一框架**：将多样的大气建模问题统一为逆问题求解，理论清晰、实现简洁。
- **自监督预训练**：利用大量无标签再分析数据学习物理先验，数据利用率高。
- **任务灵活扩展**：可通过定义新的观测算子轻松适配新任务（如空气质量预测、污染源反演等）。
- **物理一致性潜力**：通过扩散模型学习到的先验隐含了大气动力学特征，能够生成符合物理的场。

## 8. 不足与局限
- **计算开销**：扩散模型采样需要多次迭代（如100-1000步），在实时/高分辨率应用中可能存在计算瓶颈，且推理速度慢于专用前馈模型。
- **可解释性**：作为黑箱生成模型，难以直接解释预测的物理机制或不确定性来源。
- **推理依赖观测算子**：逆问题求解需要已知且精确的观测模型（如降尺度因子、站点位置），在复杂实际场景中可能难以准确建模。
- **未见消融分析**：未明确说明不同预训练策略、网络结构、采样步数等对性能的影响，缺乏对设计选择的深入分析。
- **实验细节缺失**：数据集、指标、基线具体名称及数值均未在摘要中给出，无法全面评估其与现有方法的公平对比。
- **应用限制**：零样本假设适用于任务分布与预训练数据一致的场景，若遇到全新传感器类型或极端缺失模式，可能仍需微调或引导。

（完）
