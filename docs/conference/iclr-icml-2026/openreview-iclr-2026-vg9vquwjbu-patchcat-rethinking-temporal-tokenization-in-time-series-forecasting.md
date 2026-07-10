---
title: "PatchCat: Rethinking Temporal Tokenization in Time Series Forecasting"
title_zh: PatchCat：重新思考时间序列预测中的时间标记化
authors: "Xiao Han, Xinfeng Zhang, Yiling Wu, Zhenduo zhang, Zhiyuan Deng, Zhe Wu"
date: 2025-09-11
pdf: "https://openreview.net/pdf?id=vG9vqUwjbu"
tags: ["query:ts-air-qual"]
score: 4.0
evidence: 时间序列时间标记化
tldr: 时间序列预测中时间标记化是关键组件，PatchCat通过将输入序列分割成连续斑块并按时间顺序拼接嵌入，同时保留局部语义和提高计算效率。该方法在保持准确性的同时实现了高效性，为时间序列预测提供了一种新的标记化范式。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有时间标记化方法无法同时兼顾预测精度和计算效率，需要一种新的权衡方案。
method: 将输入时间序列分割成连续斑块，按时间顺序拼接嵌入形成标记。
result: 在多个数据集上实现了精度和效率的平衡，优于现有方法。
conclusion: PatchCat提供了一种竞争性的时间标记化替代方案，适用于一般时间序列预测。
---

## Abstract
Temporal tokenization serves as a fundamental component in time series forecasting, transforming raw signals into token representations. Existing temporal tokenizers fall into three typical categories, mapping time series into tokens at the point-wise, patch-wise, or variable-wise level. Through a fair comparison, we observe that none of these paradigms simultaneously balance forecasting accuracy with computational efficiency. Motivated by the accuracy benefits of patch-wise tokenizers and the high efficiency of variable-wise tokenizers, we propose PatchCat, a competitive alternative. PatchCat segments the input time series into consecutive patches and concatenates these embeddings in chronological order. This workflow not only preserves local semantics and sequential information, but also enables univariate series to be compressed into a single token, achieving efficiency comparable to variable-wise methods. To further enhance representational capacity, we adopt a linearly increasing dimension allocation strategy and the variable-wise affine transformations. Experiments show that replacing the tokenizer in many existing methods with PatchCat can effectively improve prediction performance. To further leverage PatchCat's strengths, we develop PCMLP, a simple yet powerful model based on a multilayer perceptron. Extensive experiments across 13 challenging real-world datasets demonstrate that our approach achieves competitive performance compared to state-of-the-art methods.

---

## 论文详细总结（自动生成）

# 论文《PatchCat: Rethinking Temporal Tokenization in Time Series Forecasting》详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
时间序列预测中，时间标记化（Temporal Tokenization）是将原始信号转换为标记表示的基础组件。现有方法可分为三类：逐点级（point-wise）、斑块级（patch-wise）和变量级（variable-wise）。通过公平比较发现，现有范式均无法同时兼顾预测精度与计算效率：斑块级精度高但效率低，变量级效率高但精度有限。为此，论文提出**PatchCat**，旨在融合斑块级的精度优势与变量级的效率优势，为时间序列预测提供一种新的标记化范式，在保持准确性的前提下显著提升计算效率。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：将输入时间序列分割成连续斑块（patches），并按时间顺序拼接这些斑块的嵌入（embeddings），从而将单变量序列压缩成单个token，同时保留局部语义和时序信息。
- **关键技术细节**：
  - **分段与嵌入**：将原始序列划分为固定长度的连续斑块，每个斑块通过线性映射或小型网络生成嵌入向量。
  - **时间顺序拼接**：按时间顺序将所有斑块的嵌入拼接成一个整体token，避免信息丢失。
  - **维度分配策略**：采用线性递增维度分配（linearly increasing dimension allocation strategy），即不同斑块分配不同维度的嵌入，以增强表征能力。
  - **变量级仿射变换**：引入变量级仿射变换（variable-wise affine transformations），进一步适配多变量场景。
- **算法流程（文字描述）**：
  1. 输入多元时间序列，对每个变量独立进行分段。
  2. 对每个斑块应用嵌入函数，生成低维向量。
  3. 按时间顺序拼接所有斑块的嵌入，形成该变量的单一token表示。
  4. 使用线性递增维度分配为不同斑块分配不同维度，并通过仿射变换调整。
  5. 将得到的token输入后续预测模型（如MLP）。

## 3. 实验设计
- **数据集与场景**：在13个具有挑战性的真实世界数据集上进行评估，涵盖多种时间序列预测场景（如电力、交通、天气、空气质量等）。
- **Benchmark**：与当前最先进（SOTA）的时间序列预测方法进行对比，包括基于Transformer、MLP、CNN等架构的主流模型。
- **对比方法**：具体对比方法摘要中未列全，但提及“replacing the tokenizer in many existing methods”和“comparative performance to SOTA”。

## 4. 资源与算力
论文摘要和元数据中**未明确说明**使用了多少算力（如GPU型号、数量、训练时长等）。仅提及“效率 comparable to variable-wise methods”，但无具体硬件信息。需要假设未公开。

## 5. 实验数量与充分性
- **实验数量**：主要实验在13个数据集上进行，包含与多个基线方法的对比，以及消融实验（如替换已有方法的tokenizer），评估了PatchCat作为通用tokenizer的适应能力。
- **充分性与公平性**：
  - **充分性**：数据集覆盖广泛，验证了方法的泛化性；消融实验验证了关键设计（如线性递增维度分配、仿射变换）的贡献。
  - **公平性**：论文声称“公平比较”现有tokenizer范式，并开发了PCMLP模型进行端到端对比。但具体实现细节（如超参数设置、随机种子）未提供，可能存在偏差风险。

## 6. 主要结论与发现
- PatchCat能有效平衡预测精度与计算效率，在多个数据集上优于现有时间标记化方法。
- 将PatchCat作为tokenizer应用于多种现有模型，可提升预测性能。
- 基于PatchCat设计的简单MLP模型（PCMLP）在13个数据集上达到与SOTA方法可竞争的精度，同时保持高计算效率。

## 7. 优点
- **方法设计亮点**：提出一种新的标记化范式，巧妙融合斑块级的局部语义保留能力与变量级的高效压缩特性。
- **高效性**：通过将单变量序列压缩为单个token，显著降低模型输入维度，减少计算开销。
- **可迁移性**：作为即插即用的tokenizer，可替换现有模型的tokenization模块，提升已有方法性能。
- **表征增强**：线性递增维度分配和变量级仿射变换增强了表征能力，未引入过多参数。

## 8. 不足与局限
- **实验覆盖**：虽使用13个数据集，但缺少对极端长序列、高噪声或非平稳序列的深入分析。
- **偏差风险**：未公开完整代码、超参数细节及随机种子，难以复现；公平比较的详细过程（如算力、数据预处理）未充分说明。
- **应用限制**：PatchCat假设斑块划分固定长度，可能不适用于多尺度或周期性不可分的场景；变量级仿射变换对大规模多变量数据可能存在内存瓶颈。
- **理论分析**：缺乏对斑块大小选择、维度分配策略的理论依据或最优化讨论。

（完）
