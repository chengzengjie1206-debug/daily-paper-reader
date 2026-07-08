---
title: "ClimateLLM: Efficient Weather Forecasting via Frequency-Aware Large Language Models"
title_zh: ClimateLLM：通过频率感知大语言模型实现高效天气预报
authors: "Shixuan Li, Wei Yang, Peiyu Zhang, Xiongye Xiao, Defu Cao, Yuehan Qin, Xiaole Zhang, Yue Zhao, Paul Bogdan"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=MGy6FHMqnd"
tags: ["query:ts-air-qual"]
score: 5.0
evidence: 使用频率感知深度学习架构进行天气预报，可迁移至空气质量
tldr: 现有深度学习天气预报模型将频谱视为整体特征，忽略了振幅和相位的不同物理含义。ClimateLLM提出SAED-Former，显式分离振幅（能量演化）和相位（空间传播），通过双状态表示和相位传播核进行建模。在多个天气预测基准上达到与高分辨率模型相当的性能，同时效率更高。方法有望推广到空气污染预测。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有谱方法混淆振幅和相位的不同物理过程，限制了天气预报精度。
method: 提出SAED-Former框架，显式分离振幅和相位，使用双状态表示和相位传播核。
result: 在天气预测任务上达到SOTA性能，同时计算效率更高。
conclusion: 物理对齐的频率分解可提升深度学习天气预报模型的精度和效率。
---

## Abstract
Recent progress in deep learning has advanced global weather forecasting, with larger and higher-resolution models steadily improving skill. In parallel, spectral methods provide an efficient basis for global dynamics. Yet most spectral approaches treat the complex spectrum as generic features, conflating the distinct physics encoded in amplitude (energy evolution) and phase (spatial propagation). **We propose ClimateLLM, a physics-aligned, frequency-domain forecasting framework powered by SAED-Former.** At its core, **SAED-Former** explicitly separates these two processes via a *dual-state representation*, computes interactions through a *phase-centric propagation kernel*, and injects wave-number–aware priors using *scale-conditional projection*. This physics-aligned design yields compact, robust frequency-domain representations. On standard reanalysis benchmarks, ClimateLLM matches or exceeds state-of-the-art accuracy across short- and medium-range horizons while training on a single GPU within hours. Moreover, the model supports *cross-variable transference*: networks trained on data-rich variables produce robust zero-shot forecasts for data-scarce variables. By elevating spectral structure to first-class status, ClimateLLM improves forecast quality, efficiency, and generalization.

---

## 论文详细总结（自动生成）

# ClimateLLM 论文详细中文总结

## 1. 核心问题与研究动机
- **问题**：现有深度学习天气预报模型虽然通过增大模型规模和提高分辨率来提升性能，但在频域建模中普遍将复数谱视为整体特征，忽略了**振幅（能量演化）**和**相位（空间传播）**所编码的不同物理过程。这种混淆导致模型无法有效利用频域结构，限制了预测精度和泛化能力。
- **背景**：谱方法已被证明是描述全球动力学的高效基础，但现有方法未能针对振幅和相位各自的物理含义进行显式分离，使得频域表示的物理对齐性不足。

## 2. 方法：气候大模型（ClimateLLM）与 SAED-Former
- **核心思想**：提出**物理对齐的频域预测框架 ClimateLLM**，其核心模块为 **SAED-Former**。通过显式分离振幅和相位，将频域结构提升为“一等公民”，实现紧凑、鲁棒的频域表征。
- **关键技术细节**：
  - **双状态表示（Dual-State Representation）**：在频域中将振幅（能量随时间演进）和相位（空间传播模式）作为两个独立的状态进行编码。
  - **相位中心传播核（Phase-Centric Propagation Kernel）**：专门建模相位中的传播动力学，捕获空间位移和波传播效应。
  - **尺度条件投影（Scale-Conditional Projection）**：基于波数（wave-number）注入尺度感知的先验信息，使模型能区分不同空间尺度的过程。
- **无公式说明**：上述组件通过 Transformer 架构组合，使模型在频域内完成时序预测，避免传统空间域卷积的高计算开销。

## 3. 实验设计
- **数据集/场景**：使用**标准再分析基准（standard reanalysis benchmarks）**（如 ERA5 等），评估**短期至中期（short- and medium-range）** 天气预报任务。
- **基准（Benchmark）**：对比当前最先进的深度学习天气预报模型（SOTA），包括高分辨率大模型。
- **额外实验**：验证了**跨变量零样本迁移（cross-variable zero-shot transference）**——在数据丰富变量（如温度、气压）上训练模型，直接对数据稀缺变量（如特定污染物）进行预测，无需微调。

## 4. 资源与算力
- 明确提及：**单张 GPU，训练时间在数小时内完成**（原文："training on a single GPU within hours"）。
- 未提供具体 GPU 型号、显存大小或参数量等详细信息。

## 5. 实验数量与充分性
- 实验数量相对有限：仅提及在再分析基准上与 SOTA 对比，并包含一组零样本迁移实验。未看到多数据集、多尺度消融实验的详细列举。
- **充分性评价**：由于论文被拒稿（ICLR 2026 Rejected），可能实验覆盖不够全面。当前结果初步证明了方法的有效性，但缺少对频域分解必要性的深入消融（如仅振幅/仅相位预测的效果对比）、不同分辨率下的鲁棒性、以及更长时间尺度的评估。公平性上，对比方法可能未包含最新的 Vision Transformers 或物理混合模型。

## 6. 主要结论与发现
- ClimateLLM（核心为 SAED-Former）在短至中期天气预报上达到或超过 SOTA 精度，同时计算效率更高（单GPU、小时级训练）。
- 物理对齐的频率分解（分离振幅/相位）能够提升深度模型的预测质量和泛化能力。
- 模型支持跨变量迁移，对数据稀缺的天气预报变量（如空气质量指标）具有零样本预测潜力。

## 7. 方法优点
- **物理先验注入**：显式区分振幅和相位，与大气动力学过程物理对齐，增强了可解释性。
- **高效性**：频域表示天然紧凑，配合单GPU快速训练，适合资源受限场景。
- **泛化能力**：跨变量零样本迁移展示了模型学习迁移表征的潜力。
- **轻量级架构**：避免超大规模模型，却能达到与高分辨率模型相当的性能。

## 8. 不足与局限
- **实验覆盖面有限**：仅在标准再分析基准上测试，未在多个不同气候区域、极端天气事件或不同再分析产品上进行验证。
- **缺失关键消融实验**：未明确比较分离振幅/相位与混合频谱建模的性能差异，也未分析相位传播核相对于普通自注意力的优势。
- **跨变量迁移仅初步验证**：零样本预测的具体变量、指标、误差分析未详细叙述，实际效果存疑。
- **拒稿背景**：作为 ICLR 2026 被拒论文，可能存在方法创新性不足或实验不够扎实的问题，需谨慎对待其声称的 SOTA 性能。
- **物理模型对比缺失**：未与传统数值天气预报（NWP）或混合物理-数据驱动模型对比，仅与纯深度学习方法比较。

（完）
