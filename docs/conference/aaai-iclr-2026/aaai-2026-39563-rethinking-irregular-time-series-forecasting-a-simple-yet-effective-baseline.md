---
title: "Rethinking Irregular Time Series Forecasting: A Simple Yet Effective Baseline"
title_zh: 重新思考不规则时间序列预测：一个简单而有效的基线
authors: "Xvyuan Liu, Xiangfei Qiu, Xingjian Wu, Zhengyu Li, Chenjuan Guo, Jilin Hu, Bin Yang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39563/43524"
tags: ["query:ts-air-qual"]
score: 9.0
evidence: 提出APN框架，利用时间感知补丁聚合解决不规则时间序列预测
tldr: 该论文针对不规则多元时间序列预测中数据缺失和复杂度高的问题，提出了APN通用框架，核心是时间感知补丁聚合（TAPA）模块。TAPA通过学习动态调整的补丁边界和时间感知加权平均策略，将原始不规则序列转换为均匀表示。在多个基准上验证表明，APN在保持高效的同时达到了竞争性性能，为不规则时间序列预测提供了一个简单而有效的基线方法。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 不规则时间序列预测因数据缺失和模型复杂而困难，需要更高效的方法。
method: 提出APN框架，包含时间感知补丁聚合模块，动态调整补丁边界并加权平均。
result: 在多个不规则时间序列数据集上取得了高效且竞争性的预测性能。
conclusion: APN作为简单基线，可推动不规则时间序列预测领域的实用化发展。
---

## Abstract
The forecasting of irregular multivariate time series (IMTS) is crucial in key areas such as healthcare, biomechanics, climate science, and astronomy. However, achieving accurate and practical predictions is challenging due to two main factors. First, the inherent irregularity and data missingness in irregular time series make modeling difficult. Second, most existing methods are typically complex and resource-intensive. In this study, we propose a general framework called APN to address these challenges. Specifically, we design a novel Time-Aware Patch Aggregation (TAPA) module that achieves adaptive patching. By learning dynamically adjustable patch boundaries and a time-aware weighted averaging strategy, TAPA transforms the original irregular sequences into high-quality, regularized representations in a channel-independent manner. Additionally, we use a simple query module to effectively integrate historical information while maintaining the model's efficiency. Finally, predictions are made by a shallow MLP. Experimental results on multiple real-world datasets show that APN outperforms existing state-of-the-art methods in both efficiency and accuracy.

---

## 论文详细总结（自动生成）

# 中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：不规则多元时间序列（IMTS）预测在医疗、生物力学、气候科学和天文等领域至关重要，但面临两大挑战：
  - 数据本身的不规则性（非均匀采样间隔、通道异步）和缺失值使得建模困难。
  - 现有方法（如基于神经ODE、GNN、Transformer的模型）通常复杂且资源消耗大，难以在资源受限场景中部署。
- **研究动机**：现有固定长度补丁方法无法适应局部信息密度变化，导致信息不均或关键语义被错误分割。同时，模型复杂度过高限制了实际应用。本文希望设计一个简单且高效的基线，同时兼顾预测精度和计算效率。

## 2. 论文提出的方法论

### 核心思想
- **解耦不规则性处理与预测任务**：将原始不规则序列先通过一个新颖的**时间感知补丁聚合（TAPA）模块**转化为高质量、规则化的补丁表示，再使用简单的查询模块和浅层MLP进行预测，从而避免后端模型的高复杂度。
- **核心创新**：采用“软聚合”范式替代传统的“硬分割”，通过学习动态窗口和加权平均直接从原始观测值生成补丁表示，避免插值引入的伪影和信息丢失。

### 关键技术细节
1. **TAPA模块（时间感知补丁聚合）**：
   - **自适应补丁划分（Adaptive Patching）**：为每个通道的每个补丁学习动态边界参数（位置调整δ和宽度λ），基于参考中心c_p和初始宽度S_init计算左、右边界，宽度通过exp(λ)确保正性。
   - **加权聚合（Weighted Aggregation）**：
     - 首先对每个原始观测值添加可学习的时间嵌入TE（包含线性层和正弦激活层），得到增强表示˜v。
     - 使用软窗口函数计算每个观测值i对补丁p的权重α，该函数为两个Sigmoid函数的乘积，分别控制从左右边界的平滑衰减。温度参数κ控制边界的“软硬”程度。
     - 最终补丁表示˜h为所有观测值增强表示的加权平均（权重α归一化），再通过线性层映射到隐藏空间。
   - **关键设计**：Sigmoid函数始终为正，保证每个观测值对每个补丁都有贡献，实现完全信息覆盖，避免硬分割的信息丢失。
2. **查询模块（Query-based Aggregation）**：为每个通道引入一个可学习的查询向量q，通过点积注意力计算每个补丁的重要性分数，经Softmax归一化后加权求和得到单个上下文向量Hc。该模块轻量且有效。
3. **预测解码器（Forecasting Decoder）**：两层的MLP，输入为上下文向量Hc与查询时间qnk的时间嵌入的拼接，直接输出预测值ˆv。
- **损失函数**：MSE，端到端训练。

## 3. 实验设计

### 所用数据集（4个）
- **PhysioNet**：医疗ICU前48小时临床时间序列，36个变量，约11981个样本，平均观测数308.6。
- **MIMIC**：大型ICU去标识健康数据，96个变量，21250个样本，平均观测数144.6。
- **HumanActivity**：生物力学数据（3D位置变量），12个变量，1359个样本，平均观测数362.2。
- **USHCN**：气候科学数据，51114个变量？实际应为51114个样本？表中显示#Variables列有误，但文本说明是US气候历史网络站点的气象数据，样本数313.5，最大长度337。注意表中#Variables列可能写错，但原文如此。总之，涵盖医疗、生物力学、气候三个领域。

### 基准（对比方法，共11种）
- **IMTS分类/插补模型**：PrimeNet、SeFT、mTAN、GRU-D、Raindrop、Warpformer。
- **IMTS预测模型**：NeuralFlows、CRU、GNeuralFlow、tPatchGNN、GraFITi。

### 实现细节
- 使用PyTorch 2.6.0+cu124，NVIDIA A800 GPU。
- 训练：最多200个epoch，早停法（10个epoch无改进停止），AdamW优化器。
- 数据划分：80%训练，10%验证，10%测试。
- 超参数：遵循原文设置并进一步在验证集上调优，所有模型均进行5次不同随机种子（2024-2028）独立实验，报告均值和标准差。
- 未使用“Drop Last”技巧。

## 4. 资源与算力

- 论文明确说明实验在**单个NVIDIA A800 GPU**上完成，使用PyTorch框架。
- **训练时间**：图4(c)展示了在USHCN数据集上单步平均训练时间（ms）：APN为3.89ms，远低于CRU（1771ms）、GraFITi（34.92ms）、tPatchGNN（11.35ms）。但未给出整个训练流程的总时长。
- **推理时间**：图4(d)显示APN单步推理1.46ms，同样最优。
- **参数数量**：APN仅1.97M（百万），而GraFITi为989.44M，tPatchGNN为410.71M，CRU为31.25M。
- **峰值GPU内存**：APN为0.19GB，远低于其他模型（tPatchGNN 1.09GB，CRU 0.89GB，GraFITi 0.78GB）。
- **结论**：APN在算力资源消耗上具有显著优势，但文中未明确说明总训练时长或Energy消耗。

## 5. 实验数量与充分性

- **主实验**：在4个数据集上对比11种基线，报告MSE和MAE，每个实验5次独立重复，统计均值和标准差，结果见表2。
- **消融实验**：针对APN的三个核心组件（自适应补丁、加权聚合、查询模块）进行单独去除测试，在MIMIC和USHCN上验证性能下降（表3）。
- **参数敏感性分析**：对补丁数P、隐藏维度D、时间编码维度Dte进行变化实验（图3a-d），分析了影响。
- **效率和可扩展性分析**：在USHCN上比较4个模型（APN、CRU、GraFITi、tPatchGNN）的GPU内存、参数量、训练/推理时间（图4）。
- **总体评价**：实验数量较充分，覆盖多元领域，但与更多基线（如Transformer变体）对比有限；消融实验仅两个数据集，但关键组件均被验证；参数敏感性分析有助于理解模型稳健性；效率分析直观。公平性方面，所有模型均调至最优。总体而言，实验设计客观、充分。

## 6. 论文的主要结论与发现

- **主要结论**：提出的APN框架在4个公共数据集上，在预测精度（MSE和MAE）和计算效率（参数量、GPU内存、训练/推理时间）方面全面超越现有最先进方法（如GraFITi、tPatchGNN、Warpformer等）。APN成功实现了一个简单而有效的基线，其核心TAPA模块通过软聚合有效解决了不规则序列的信息密度变化和语义分割问题。
- **具体发现**：
  - 在主实验中，APN在所有数据集上取得最优或次优结果，且方差小，显示稳定性和泛化能力。
  - 消融实验验证了每个组件（自适应补丁、加权聚合、查询模块）的不可或缺性，其中查询模块对性能影响最大。
  - 模型对隐藏维度D和补丁数P不非常敏感，但对时间编码维度Dte需要适度调优。
  - 效率分析显示APN在内存和速度上比复杂模型（如CRU、GraFITi、tPatchGNN）提升一个数量级以上。

## 7. 优点

- **方法创新性**：提出“软聚合”范式替代固定硬分割，通过学习动态窗口和Sigmoid乘积的软权重实现完全信息覆盖，无需插值，保证数据保真度。
- **架构优雅性**：将不规则性处理集中在TAPA模块，后端使用极简的查询+MLP，实现高效与精度兼得。
- **实验结果强劲**：在多个领域数据集上全面领先SOTA，且计算资源消耗极低（仅1.97M参数，0.19GB GPU内存），非常适合实际部署。
- **实验设计严谨**：多次重复实验，报告均值和标准差，对比公平，消融和敏感性分析合理。
- **可复现性**：提供所有数据集和代码开源（GitHub链接），有助于后续研究。

## 8. 不足与局限

- **数据集规模与多样性**：虽然覆盖医疗、生物力学、气候，但数据维度相对较小（最大变量数96，MIMIC）；未在超大规模（如数千变量）或长时间跨度不规则序列上验证。另外，USHCN数据集的变量数量在表中显示为51114，疑似错误（可能为样本数？），但文本未澄清，存在模糊性。
- **实验覆盖**：
  - 消融实验仅在2个数据集（MIMIC和USHCN）上执行，未在所有4个数据集上验证，可能削弱结论的普遍性。
  - 未与一些最新的IMTS方法（如基于状态空间模型或扩散模型的方法，若存在）对比。
  - 缺乏对更复杂缺失模式（如块缺失、结构性缺失）的敏感性分析。
- **潜在偏差**：作者来自华东师范大学，与SOTA方法中的部分作者（如tPatchGNN、GraFITi）无直接关联，但与其他基线可能存在隐性关系；代码开源但未提及第三方复现。总体风险低。
- **局限性说明**：TAPA模块依赖超参数（如补丁数P、温度κ等），虽敏感性分析表明稳健，但在极端不规则数据上可能需要仔细调参。查询模块使用单一向量可能丢失多模态历史模式，对于需要多尺度上下文的任务可能不够灵活。当前仅预测点值（MSE），未提供不确定性估计（如概率预测），限制了在风险敏感领域的应用。
- **应用限制**：APN设计假定每个通道独立处理，无法显式建模跨通道依赖关系（除非通过共享参数？文中未提及）。虽然实验结果好，但理论上可能遗漏变量间交互关系，尤其在高度相关的多变量系统中。

（完）
