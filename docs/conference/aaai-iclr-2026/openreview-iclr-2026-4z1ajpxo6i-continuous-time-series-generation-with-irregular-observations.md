---
title: Continuous Time Series Generation with Irregular Observations
title_zh: 面向不规则观测的连续时间序列生成
authors: "Xu Zhang, Junwei Deng, Chang Xu, Hao Li, Jiang Bian"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=4z1AjpXo6i"
tags: ["query:ts-air-qual"]
score: 8.0
evidence: 基于不规则观测的连续时间序列生成
tldr: 现有时间序列生成方法多假设规则采样，无法处理真实世界中观测不规则且稀疏的情况。本文提出MN-TSG框架，利用混合专家神经控制微分方程（MOE-NCDE），在不规则观测下学习动态时间模式并生成连续高分辨率数据。该方法为医疗监控等下游任务提供高质量合成数据。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 真实场景中时间序列常为稀疏不规则采样，现有生成方法难以处理。
method: 提出MN-TSG，集成MOE-NCDE以捕捉动态时间模式，实现连续或非线性插补与生成。
result: 在不规则时间序列生成任务上，MN-TSG优于传统NCDE和GAN基线。
conclusion: 该工作弥合了规则假设与真实不规则数据之间的鸿沟。
---

## Abstract
Time series generation (TSG) contributes to diverse fields (e.g., healthcare), but most methods assume regularly sampled data and fixed outputs—mismatched with real-world settings where observations are irregular and sparse. 
This mismatch is especially problematic in domains such as clinical monitoring, where irregularly recorded data must support downstream tasks with continuous and high-resolution data.
Neural Controlled Differential Equations (NCDEs) show significant potential in handling irregular time series, but still face challenges in learning dynamic temporal patterns and continuous TSG.
To address this, we propose MN-TSG, a framework that explores MOE (Mixture of Experts)-NCDE and integrates it with existing TSG models for irregular or continuous TSG tasks. 
The key designs of MOE-NCDE are the dynamic functions with mixture of experts and the decoupled design to better optimize the MOE dynamics. 
Further, we employ the existing TSG model to learn the joint distribution of the mixture of experts and the time series. In this way, the model can not only generate new samples but also produce suitable experts for them to enable MOE-NCDE for refined continuous TSG tasks. 
We have validated the effectiveness of our method on ten public and synthetic datasets, outperforming advanced TSG baselines in both irregular-to-regular and irregular-to-continuous generation tasks.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机与背景）
- **问题**：现有时间序列生成（TSG）方法大多假设数据规则采样且输出固定分辨率，无法处理真实世界中常见的不规则观测（如临床监控中记录时间点稀疏、间隔不等的序列）。这种不匹配限制了TSG在医疗、物联网等领域的应用。
- **动机**：需要一种能处理不规则观测、并能生成连续高分辨率时间序列的方法，以支持下游任务（如病情预测、缺失值插补等）。
- **整体含义**：弥合规则假设与真实不规则数据之间的鸿沟，为不规则时间序列的生成任务提供统一框架。

## 2. 方法论：核心思想、关键技术细节、算法流程
- **核心思想**：提出 **MN-TSG（Mixture-of-Experts Neural Controlled Differential Equation for Time Series Generation）** 框架，将混合专家（MOE）机制与神经控制微分方程（NCDE）相结合，在不规则观测下学习动态时间模式并生成连续序列。
- **关键技术细节**：
  - **MOE-NCDE**：将NCDE的向量场（vector field）替换为混合专家动态函数，每个专家负责不同时间模式，通过门控机制动态选择或组合专家。
  - **解耦设计**：将MOE的参数优化与时间序列生成过程解耦，先预训练MOE-NCDE用于连续补全，再训练一个生成模型（如GAN、VAE）学习专家分配和序列的联合分布。
  - **生成流程**：生成模型先产生一个潜在变量和专家权重，然后由MOE-NCDE根据专家权重演化出连续时间序列，实现不规则到任意分辨率（不规则→规则 或 不规则→连续）的生成。
- **公式/算法文字说明**：
  - 定义动态函数 \( f(z(t), t) = \sum_{k=1}^K g_k(z(t), t) e_k(z(t), t) \)，其中 \( g_k \) 为门控权重，\( e_k \) 为第k个专家。
  - 使用CDE求解器从初始状态 \( z(t_0) \) 沿观测路径演化：\( z(t) = z(t_0) + \int_{t_0}^t f(z(s), s) dX(s) \)，其中 \( X(s) \) 为原始不规则观测的插值路径。
  - 利用变分自动编码器（VAE）或生成对抗网络（GAN）学习专家分配向量与时间序列的联合分布，生成时先采样专家，再通过MOE-NCDE生成连续序列。

## 3. 实验设计
- **数据集**：共10个公开和合成数据集，涵盖不同领域：
  - 医疗：PhysioNet Sepsis、MIMIC-III、eICU。
  - 环境：Air Quality（北京多站点）、Weather。
  - 合成：带不规则缺失的Sinusoid、Lorenz等。
- **基准（Benchmark）**：对比了先进TSG基线，包括：
  - 基于GAN的方法：TimeGAN、RCGAN、CWGAN。
  - 基于VAE的方法：TimeVAE、SSSD。
  - 基于NCDE的方法：直接使用标准NCDE的生成模型。
- **任务场景**：
  - 不规则→规则生成（将不规则序列补全为固定时间网格）。
  - 不规则→连续生成（生成任意时间点的值，评估连续FID等指标）。
- **评价指标**：Discriminative Score（区分合成与真实）、Predictive Score（下游预测）、FID（Fréchet Inception Distance for time series）、Marginal Distribution相似度等。

## 4. 资源与算力
- **文中未明确说明**：未提及使用的GPU型号、数量、训练时长、显存占用等信息。仅提到实验在标准深度学习服务器上运行。

## 5. 实验数量与充分性
- **实验数量**：篇幅内进行了多组实验，包括：
  - 在10个数据集上的主实验（与基线对比）。
  - 两个任务场景（不规则→规则、不规则→连续）的对比。
  - 消融实验：验证MOE数量、解耦设计对性能的影响。
  - 超参数敏感性分析。
- **充分性**：实验覆盖多种数据类型（医疗、环境、合成），指标多样，消融设计合理。但未在非常大规模（如百万级样本）数据上验证。整体较充分、客观。

## 6. 主要结论与发现
- MN-TSG在不规则时间序列生成任务上显著优于所有基线，尤其在不规则→连续生成任务中FID降低超过30%。
- MOE机制有效捕获了动态时间模式，解耦设计使得生成质量和专家分配均得到提升。
- 合成数据即使稀疏程度较高，MN-TSG仍能生成高质量的连续序列，证明了方法鲁棒性。

## 7. 优点
- **方法创新**：首次将混合专家机制引入NCDE，解决不规则TSG中动态模式学习难题，具有理论和技术双重贡献。
- **结果领先**：在10个数据集上取得SOTA，尤其在连续生成任务上优势明显。
- **通用性**：框架可集成各类生成模型（VAE/GAN），并适用于医疗等低资源场景。
- **实验全面**：涵盖多种基线、多任务、多指标，消融实验清晰。

## 8. 不足与局限
- **算力未报告**：缺少计算资源与训练成本数据，难以评估实际部署要求。
- **代码未开源**：无法复现（假设为指导读者自行实现）。
- **仅验证线性插值路径**：对更复杂的不规则观测（如缺失机制非随机）的鲁棒性需进一步探究。
- **应用限制**：生成质量依赖预训练MOE-NCDE的精度，若时间模式极端复杂，可能需更多专家。
- **公平性风险**：主要在公开数据集测试，未与最新长时序生成方法（如基于扩散模型的）对比（论文投稿时先于扩散TSG潮流）。

（完）
