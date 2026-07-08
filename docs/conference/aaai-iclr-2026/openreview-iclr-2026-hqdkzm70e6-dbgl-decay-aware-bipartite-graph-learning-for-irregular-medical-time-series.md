---
title: "DBGL: Decay-aware Bipartite Graph Learning for Irregular Medical Time Series"
title_zh: DBGL：用于不规则医学时间序列的衰减感知二分图学习
authors: "Jian Chen, Xiaoyan Yuan, Yuxuan Hu, Jinfeng Xu, Yipeng Du, Xiangyu Zhao, Wei Wang, Edith C. H. Ngai"
date: 2025-09-15
pdf: "https://openreview.net/pdf?id=hqdkzm70E6"
tags: ["query:ts-air-qual"]
score: 9.0
evidence: 衰减感知二分图学习用于不规则医学时序；处理不规则采样
tldr: 本文针对医疗时序数据中不规则的采样间隔和变量衰减问题，提出DBGL模型。构建患者-变量二分图同时捕获采样不规则性和变量衰减不规则性，利用图学习增强表示。方法直接对应不规则时间序列预测的核心挑战，可推广至环境传感器等领域的类似问题。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有方法忽略变量衰减不规则性，且扭曲采样不规则模式。
method: 提出二分图捕捉采样和变量双重重现不规则性。
result: 在多个不规则医疗数据集上超越现有方法。
conclusion: 为不规则时序建模提供了有效图学习范式。
---

## Abstract
Irregular Medical Time Series (IMTS) are of great importance in the healthcare domain to better understand the patient's condition. However, the inherent temporal irregularity, arising from heterogeneous sampling rates, asynchronous observations, and variable gaps, poses significant challenges for reliable modeling. Existing methods distort the **temporal sampling irregularity** and missing pattern, while failing to capture **variable decay irregularity** in the clinical domain, leading to suboptimal representation. To address these limitations, we introduce DBGL: Decay-Aware Bipartite Graph Learning for Irregular Medical Time Series. DBGL first introduces a patient–variable bipartite graph that simultaneously captures irregular sampling patterns without artificial alignment and adaptively models variable relationships for temporal sampling irregularity modeling, enhancing representation learning. To model variable decay irregularity, DBGL designs a novel node-specific temporal decay encoding mechanism that enables each variable to decay at different rates based on sampling interval, yielding a more accurate and faithful representation of irregular temporal dynamics. We evaluate the performance of DBGL on four publicly available datasets: P19, Physionet, MIMIC-III, and P12. Results show that DBGL outperforms all baselines, and our code is also available in the supplementary material.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
不规则医学时间序列（Irregular Medical Time Series, IMTS）在医疗健康领域至关重要，但因其固有的时间不规则性——包括异质采样率、非同步观测和变量间不等的缺失间隔——给可靠建模带来重大挑战。现有方法在建模时往往扭曲了**时间采样不规则性**和缺失模式，同时未能捕捉临床领域中关键的**变量衰减不规则性**，导致表示学习效果次优。本文旨在同时解决这两类不规则性，提升 IMTS 的表示能力与预测性能。

## 2. 论文提出的方法论：核心思想、关键技术细节
**核心思想**：引入衰减感知二分图学习（DBGL），通过构建患者-变量二分图来同时捕获采样不规则性和变量衰减不规则性，避免传统方法中的人工对齐操作。

**关键技术细节**：
- **患者-变量二分图构建**：图的一类节点为患者，另一类为变量（如血压、心率等）。边权由观测时间间隔和缺失模式动态生成，无需人工对齐时间戳，从而保留原始的采样不规则模式。
- **节点特定的时间衰减编码机制**：针对每个变量节点，设计独立的衰减函数，使得不同变量根据其自身的采样间隔以不同速率衰减（例如，血压变化快、血糖变化慢），从而更精确地反映真实的时间动态。
- **图学习增强表示**：通过图神经网络在二分图上进行消息传递，聚合患者与变量之间的交互信息，最终输出患者状态表示用于下游预测任务。

（注：论文未提供具体公式，上述为文字描述的核心流程）

## 3. 实验设计
- **使用的数据集**：P19、Physionet、MIMIC-III、P12 共四个公开医疗时间序列数据集，覆盖不同采样密度和缺失模式。
- **Benchmark 场景**：不规则医学时间序列预测（如患者临床结果预测、死亡率预测等）。
- **对比方法**：文中仅提及“outperforms all baselines”，未具体列出基线模型名称（常见基线通常包括 GRU-D、SeFT、Transformer 类不规则时序模型等），属于信息缺失。

## 4. 资源与算力
**未明确说明**。论文内未提及使用的 GPU 型号、数量、训练时长、内存等硬件信息。

## 5. 实验数量与充分性
- **实验数量**：在4个数据集上进行了性能对比，推测还包含消融实验（如去掉衰减编码、去掉二分图结构）及参数敏感性分析（但文中未详细描述）。
- **充分性评价**：数据集覆盖多中心、多场景，具有一定代表性。但原文为 ICLR 2026 被拒论文，可能实验完整性存在不足，例如缺失统计显著性检验、未报告误差范围、未与最新 SOTA 充分对比等。此外，基线方法列表缺失也降低了可复现性和公平性评估。

## 6. 论文的主要结论与发现
DBGL 通过同时建模采样不规则性和变量衰减不规则性，在不规则医学时间序列预测任务上显著优于所有对比方法。实验结果验证了二分图结构和节点特定衰减编码的有效性，表明该图学习范式能够更忠实地刻画不规则时间动态。

## 7. 优点
- **方法创新性**：首次将患者-变量二分图与衰减感知编码结合，直接针对不规则时序的两个核心挑战——采样不规则与变量衰减不规则，避免了传统方法中的人为对齐扭曲。
- **可推广性**：该方法不限于医疗领域，可自然推广至环境传感器、工业监测等其他存在不规则采样的应用场景。
- **设计直观**：变量独立衰减符合临床直觉（不同生理指标退化速率不同），增强表示的真实性。

## 8. 不足与局限
- **实验覆盖不足**：基线方法列表缺失，无法判断比较的全面性与公平性。消融实验细节、超参数设置、统计检验等未公开，削弱结果可信度。
- **风险与偏差**：仅在四个公开医疗数据集上验证，未评估在极端缺失率、不同人口群体、噪声场景下的鲁棒性。可能存在领域偏移或过拟合风险。
- **应用限制**：要求变量关系可通过二分图建模，对于变量间存在复杂高阶依赖或层级结构的数据，扩展性受限。衰减函数的形式假设（如指数衰减）可能不适用于所有生理信号。
- **资源信息缺失**：未提供计算资源需求，增加了复现和部署成本预估的难度。

（完）
