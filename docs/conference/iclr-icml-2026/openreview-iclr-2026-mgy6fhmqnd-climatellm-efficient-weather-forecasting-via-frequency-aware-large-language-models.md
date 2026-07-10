---
title: "ClimateLLM: Efficient Weather Forecasting via Frequency-Aware Large Language Models"
title_zh: ClimateLLM：通过频率感知大语言模型实现高效天气预报
authors: "Shixuan Li, Wei Yang, Peiyu Zhang, Xiongye Xiao, Defu Cao, Yuehan Qin, Xiaole Zhang, Yue Zhao, Paul Bogdan"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=MGy6FHMqnd"
tags: ["query:ts-air-qual"]
score: 4.0
evidence: 应用深度学习和频率分析进行天气预报，与空气质量间接相关
tldr: 现有频谱方法将复数谱视为通用特征，混淆了能量和空间传播。本文提出ClimateLLM，在频域分离幅度和相位处理，利用SAED-Former进行物理对齐的天气预报。实验表明该方法在高分辨率预测上更高效。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有频谱方法混淆了能量演化和空间传播的物理过程，导致预报精度受限。
method: 提出SAED-Former，显式分离幅度和相位，通过相位传播核和尺度条件投影注入先验。
result: 在多个天气基准上达到与SOTA相当的性能，同时计算开销更低。
conclusion: 频率感知的LLM框架在天气预报中具有潜力，未来可扩展至空气质量预测。
---

## Abstract
Recent progress in deep learning has advanced global weather forecasting, with larger and higher-resolution models steadily improving skill. In parallel, spectral methods provide an efficient basis for global dynamics. Yet most spectral approaches treat the complex spectrum as generic features, conflating the distinct physics encoded in amplitude (energy evolution) and phase (spatial propagation). **We propose ClimateLLM, a physics-aligned, frequency-domain forecasting framework powered by SAED-Former.** At its core, **SAED-Former** explicitly separates these two processes via a *dual-state representation*, computes interactions through a *phase-centric propagation kernel*, and injects wave-number–aware priors using *scale-conditional projection*. This physics-aligned design yields compact, robust frequency-domain representations. On standard reanalysis benchmarks, ClimateLLM matches or exceeds state-of-the-art accuracy across short- and medium-range horizons while training on a single GPU within hours. Moreover, the model supports *cross-variable transference*: networks trained on data-rich variables produce robust zero-shot forecasts for data-scarce variables. By elevating spectral structure to first-class status, ClimateLLM improves forecast quality, efficiency, and generalization.

---

## 论文详细总结（自动生成）

# 论文总结：ClimateLLM: Efficient Weather Forecasting via Frequency-Aware Large Language Models

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **背景**：深度学习在全球天气预报领域取得了显著进展，大模型和高分辨率模型持续提升预报技能；频谱方法作为高效建模全局动态的基础被广泛采用。
- **核心问题**：现有频谱方法将复数谱视为通用特征，混淆了幅度（能量演化）和相位（空间传播）所编码的不同物理过程，导致预报精度受限，且计算资源需求高。
- **整体含义**：本文提出一种物理对齐的频域预报框架 ClimateLLM，通过显式分离幅度和相位，并引入波数感知先验，实现高分辨率天气预测的高效与高精度，同时支持跨变量零样本迁移。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：在频域中分别处理幅度和相位，以对齐天气演化的物理本质（能量演化 vs 空间传播），从而获得紧凑且鲁棒的频域表示。
- **关键技术细节**：
  - **SAED-Former**：核心组件，包含双状态表示（dual-state representation），显式分离幅度和相位两种模态。
  - **相位传播核（Phase-centric Propagation Kernel）**：专门计算相位之间的相互作用，模拟空间传播过程。
  - **尺度条件投影（Scale-conditional Projection）**：注入波数感知的先验信息，使模型能识别不同波数尺度的物理行为。
  - **工作机制**：输入气象场先转换至频域，SAED-Former在频域中执行注意力与传播计算，再通过逆变换得到预报结果。
- **公式/算法流程**（文字说明）：
  - 输入：历史气象场序列 → 经FFT转换到频域 → 分离幅度谱和相位谱。
  - 双状态表示：幅度和相位分别作为两条并行的特征流。
  - 相位传播核：对相位流施加基于波数的卷积/注意力操作，模拟波动传播。
  - 尺度条件投影：根据波数大小对特征进行缩放和偏置调整。
  - 融合：幅度流和相位流交互后合并为复数谱 → iFFT得到下一时刻预测。
- 整体框架为ClimateLLM，底层使用SAED-Former作为主体结构。

## 3. 实验设计：使用的数据集、基准、对比方法

- **数据集**：标准再分析基准（standard reanalysis benchmarks），具体未在摘要中详列，推测为ERA5等常用数据集。
- **基准（Benchmark）**：短中期天气预报任务，与当前最先进（SOTA）模型对比。
- **对比方法**：未列出具体模型名称，但文中称“matches or exceeds state-of-the-art accuracy”，说明对比了主流深度学习预报模型（如FourCastNet、Pangu-Weather、GraphCast等可能）。

## 4. 资源与算力

- **明确说明**：训练可在单个GPU上于数小时内完成（training on a single GPU within hours）。
- **未说明**：GPU型号（如A100/V100）、显存大小、数据并行配置等细节未提供。未提及推理时的算力需求。

## 5. 实验数量与充分性

- **实验数量**：摘要中未列出具体组数，但提到在标准基准上对比了短中期预报，并进行了跨变量零样本迁移实验（cross-variable transference）。
- **充分性**：从摘要看，实验覆盖了主要性能对比和迁移能力验证，但缺少消融实验（如分离幅度/相位的重要性、相位传播核的有效性等细节）。由于论文被ICLR-2026拒稿（来源标注），可能研究尚不够全面或实验结果不够充分。

## 6. 论文的主要结论与发现

- ClimateLLM在短中期预报精度上达到或超过SOTA。
- 训练效率极高，单GPU数小时即可完成。
- 支持跨变量零样本迁移：在数据丰富变量上训练的模型可对数据稀缺变量进行鲁棒预报。
- 提升频谱结构作为第一公民（first-class status）能有效改善预报质量、效率和泛化性。

## 7. 优点：方法或实验设计上的亮点

- **物理对齐**：显式分离幅度和相位，契合能量演化与空间传播的物理机理，而非简单混合复数谱。
- **高效性**：单GPU小时级训练，远优于许多大模型（通常需要多天多卡训练）。
- **跨变量迁移能力**：零样本预报缓解了部分气象变量数据稀疏问题，具有实用价值。
- **框架可扩展性**：未来可推广至空气质量预测等类似时空序列问题。

## 8. 不足与局限

- **缺乏详细实验结果**：摘要未提供具体数值指标、数据集规模、对比模型的性能差距等定量信息，无法判断统计显著性。
- **消融实验缺失**：未在摘要中呈现组件分析（如去除相位传播核或尺度条件投影的影响）。
- **泛化性验证有限**：仅提及跨变量迁移，但未说明是否跨区域、跨季节或跨极端天气事件测试。
- **被拒稿提示风险**：标注为“ICLR-2026-Rejected-Public”，表明论文可能未被顶级会议接收，存在方法论或实验上的不足（如缺少与更多基线对比、物理合理性未充分验证）。
- **应用限制**：虽提及未来可扩展至空气质量，但当前仅验证天气预报任务；对非平稳或突发性天气的预报能力未知。

（完）
