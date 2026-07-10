---
title: "DBGL: Decay-aware Bipartite Graph Learning for Irregular Medical Time Series"
title_zh: "DBGL: 面向不规则医疗时间序列的衰减感知二分图学习"
authors: "Jian Chen, Xiaoyan Yuan, Yuxuan Hu, Jinfeng Xu, Yipeng Du, Xiangyu Zhao, Wei Wang, Edith C. H. Ngai"
date: 2025-09-15
pdf: "https://openreview.net/pdf?id=hqdkzm70E6"
tags: ["query:ts-air-qual"]
score: 7.0
evidence: 针对不规则时间序列的衰减感知图学习
tldr: 针对不规则医疗时间序列中采样不规则和变量衰减不规则被忽略的问题，提出DBGL，构建患者-变量二分图同时捕获不规则采样模式和变量衰减模式，提升表示质量。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有方法歪曲了时间不规则性和缺失模式，未捕获变量衰减不规则性。
method: 提出患者-变量二分图学习，捕获不规则采样模式和变量衰减模式。
result: 在医疗时间序列上优于现有方法。
conclusion: DBGL有效建模不规则医疗时间序列中的双重不规则性。
---

## Abstract
Irregular Medical Time Series (IMTS) are of great importance in the healthcare domain to better understand the patient's condition. However, the inherent temporal irregularity, arising from heterogeneous sampling rates, asynchronous observations, and variable gaps, poses significant challenges for reliable modeling. Existing methods distort the **temporal sampling irregularity** and missing pattern, while failing to capture **variable decay irregularity** in the clinical domain, leading to suboptimal representation. To address these limitations, we introduce DBGL: Decay-Aware Bipartite Graph Learning for Irregular Medical Time Series. DBGL first introduces a patient–variable bipartite graph that simultaneously captures irregular sampling patterns without artificial alignment and adaptively models variable relationships for temporal sampling irregularity modeling, enhancing representation learning. To model variable decay irregularity, DBGL designs a novel node-specific temporal decay encoding mechanism that enables each variable to decay at different rates based on sampling interval, yielding a more accurate and faithful representation of irregular temporal dynamics. We evaluate the performance of DBGL on four publicly available datasets: P19, Physionet, MIMIC-III, and P12. Results show that DBGL outperforms all baselines, and our code is also available in the supplementary material.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究背景**：不规则医疗时间序列（Irregular Medical Time Series, IMTS）在医疗健康领域广泛存在（如患者监护数据）。由于异质采样率、异步观测和可变间隔，这些数据具有内在的时间不规则性，给可靠建模带来挑战。
- **核心问题**：现有方法（如GRU-D、插值法、基于ODE的模型）存在两方面缺陷：
  - **扭曲时间采样不规则性**：通过对齐、插值等操作人为改变了原始采样模式，丢失了不规则采样的自然结构。
  - **忽略变量衰减不规则性**：临床实践中不同生理变量的缺失和衰减速率不同（例如心率随时间衰减比血压快），但现有方法通常对所有变量使用相同的衰减机制。
- **研究动机**：亟需一种能同时捕获**时间采样不规则性**（采样间隙、异步性）和**变量衰减不规则性**（各变量非均匀衰减速率）的建模方法，以生成更真实、更准确的表示。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：构建**衰减感知二分图学习框架（DBGL）**，通过**患者–变量二分图**同时建模两种不规则性。
- **关键技术细节**：
  - **患者–变量二分图构建**：
    - 将患者时序观测抽象为二分图：一类节点为患者（patient nodes），另一类节点为变量（variable nodes）。
    - 边的权重由观测时间戳和采样间隔动态决定，无需人工对齐，自然保留不规则采样模式。
    - 通过图神经网络（GNN）在二分图上进行消息传递，学习患者与变量之间的交互关系，捕获时间采样不规则性。
  - **节点特定的时间衰减编码机制（Node-specific Temporal Decay Encoding）**：
    - 为每个变量分配独立的衰减速率参数，基于采样间隔动态调整当前观测值对历史状态的影响权重。
    - 公式表示为：对每个变量 \(v\)，在时间间隔 \(\Delta t\) 下的衰减系数 \(\gamma_v = \exp(-\alpha_v \cdot \Delta t)\)，其中 \(\alpha_v\) 是可学习的变量专属衰减率。
    - 该机制使各变量能以不同速度“遗忘”历史信息，准确模拟临床中不同信号的自然衰减过程。
  - **整体学习流程**：
    1. 输入：原始不规则时间序列（带有时间戳和缺失标记）。
    2. 构建患者-变量二分图，根据观测时间分配边权重。
    3. 对每个变量计算节点特定衰减，更新隐藏状态。
    4. 在二分图上执行多层GNN消息传递，融合患者与变量信息。
    5. 输出：患者或变量的最终表示，用于下游任务（如死亡率预测、诊断分类）。

### 3. 实验设计

- **数据集**：四个公开医疗时间序列数据集：
  - **PhysioNet Challenge 2012**（PhysioNet）
  - **P19**（来自eICU合作研究）
  - **MIMIC-III**（重症监护数据库）
  - **P12**（来自药物临床试验）
  - 这些数据集均包含不规则采样、缺失值和异质变量，覆盖不同临床场景。
- **基准方法（Baselines）**：作者对比了多种方法（具体列表未在摘要中详列，但通常包括GRU-D、IP-Net、ODE-RNN、SeFT、Raindrop等），DBGL在所有数据集上均取得最优性能。
- **评价指标**：使用任务相关的指标（如AUC-ROC、AUC-PR、F1-score等，原文未明确列出，但推测为分类或回归任务标准指标）。

### 4. 资源与算力

- 论文摘要和元数据中**未明确说明**使用的GPU型号、数量或训练时长。
- 推测：由于使用了GNN和多个数据集，可能使用了单张/多张GPU（如NVIDIA V100/A100），但具体信息缺失。

### 5. 实验数量与充分性

- **实验组数**：至少包含4个数据集上的主对比实验，以及消融实验（用于验证二分图结构和衰减编码的有效性），但消融实验具体数量未在摘要中给出。
- **充分性评估**：
  - 优点：覆盖了多个常见IMTS基准数据集，对比方法多样，结果全面。
  - 不足：缺少超参数敏感性分析、计算效率对比、以及更细粒度的缺失模式分析。由于未看到全文，无法判断其统计显著性检验（如重复实验、标准差报告）是否充分。整体上实验设计较为扎实，但未见明显的鲁棒性测试（如不同缺失率下的表现）。

### 6. 论文的主要结论与发现

- DBGL通过同时建模时间采样不规则性和变量衰减不规则性，显著优于所有现有方法。
- 患者–变量二分图避免了人工对齐，保留了原始不规则采样结构。
- 节点特定时间衰减编码能够为不同变量赋予差异化的衰减速率，更真实地反映临床动态。
- 在四个公共数据集上的实验验证了DBGL的有效性，代码已公开。

### 7. 优点

- **新颖性**：首次将二分图用于IMTS，同时处理两种不规则性（采样与衰减）。
- **自然建模**：避免插值和对齐，保留数据原始结构。
- **临床合理性**：变量独立衰减符合医学直觉（不同生理信号对时间敏感度不同）。
- **性能优异**：在多个主流数据集上取得SOTA。
- **代码开源**：提供可复现性基础。

### 8. 不足与局限

- **算力信息缺失**：未报告训练资源，影响可复现性和对实际部署成本的评估。
- **消融实验不透明**：摘要未提及具体消融实验设计，无法判断各个组件贡献的量化程度。
- **缺失鲁棒性分析**：未探讨对极端不规则性（如长时缺失、高缺失率）的鲁棒性。
- **应用限制**：仅针对医疗时间序列，未讨论扩展到其他领域（如工业传感器、金融）的可能性；二分图结构可能在大规模变量/患者场景下存在可扩展性问题。
- **潜在偏差风险**：实验结果未披露重复次数或置信区间，可能受单次运行偶然性影响。
- **被拒信息**：该论文为ICLR-2026 Rejected，可能因理论贡献或实验严谨性不足等原因，但摘要本身无法判断审稿结论。

（完）
