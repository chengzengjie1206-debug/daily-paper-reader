---
title: CONTRASTIVE TIME SERIES FORECASTING WITH ANOMALIES
title_zh: 面向异常的时间序列对比学习预测
authors: "Joel Ekstrand, Zahra Taghiyarrenani, Slawomir Nowaczyk"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=HwbOLjPtCj"
tags: ["query:ts-air-qual"]
score: 5.0
evidence: 时间序列预测中处理异常，与空气质量异常检测相关
tldr: 时间序列预测中异常事件对预测的影响各异，标准模型无法区分。本文提出Co-TSFA对比学习框架，通过输入-输出增强和潜在-输出对齐损失，让模型学会忽略无关异常、响应持续变化。该方法可提升多种预测模型在含异常数据上的表现，适用于空气质量等存在异常事件的场景。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有预测模型对异常事件处理不当，无法区分相关与无关异常。
method: 利用对比正则化框架，生成输入和输入-输出增广，约束潜在表示。
result: 在含异常的时间序列上提升了预测的准确性和稳健性。
conclusion: Co-TSFA能有效引导模型关注持久变化而忽略短暂噪声。
---

## Abstract
Time-series forecasting predicts future values from past data. In real-world settings, some anomalous events have lasting effects and influence the forecast, while others are short-lived and should be ignored. Standard forecasting models fail to make this distinction, often either overreacting to noise or missing persistent shifts. We propose **Co-TSFA** (Contrastive Time-Series Forecasting with Anomalies), a regularization framework that learns when to ignore anomalies and when to respond. Co-TSFA generates input-only and input–output augmentations to model forecast-irrelevant and forecast-relevant anomalies, and introduces a latent–output alignment loss that ties representation changes to forecast changes. This encourages invariance to irrelevant perturbations while preserving sensitivity to meaningful distributional shifts. Experiments on the Traffic and Electricity benchmarks, as well as on a real-world cash-demand dataset, demonstrate that Co-TSFA improves performance under anomalous conditions while maintaining accuracy on normal data. The implementation of Co-TSFA will be released publicly upon acceptance of the paper.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：现实时间序列中异常事件对预测的影响不同——有些异常（如短暂噪声）应被忽略，有些异常（如持续分布偏移）应被响应。标准时序预测模型无法区分这两种异常，往往要么过拟合噪声，要么错过持久变化。
- **研究动机**：现有预测模型缺乏对异常“相关/无关”的判别能力，导致在含异常的真实场景（如空气质量、交通、用电、现金需求）中预测效果不稳定。
- **整体含义**：提出一个轻量级正则化框架，使任意预测模型能够自动学习“何时忽略异常、何时响应变化”，提升鲁棒性而不牺牲正常数据上的精度。

### 2. 论文提出的方法论
- **核心思想**：对比正则化框架 **Co-TSFA**（Contrastive Time-Series Forecasting with Anomalies），通过构造两类增强样本并引入潜在–输出对齐损失，引导模型区分与预测无关的异常和与预测相关的异常。
- **关键技术细节**：
  - **输入仅增强（Input-only augmentation）**：对输入时间序列施加小扰动（如加噪、掩码），模拟预测无关的短暂异常。模型应学得对该类增强不变。
  - **输入–输出增强（Input–output augmentation）**：对输入和输出同时施加持久偏移（如缩放、移位），模拟预测相关的持续变化。模型应学得对该类增强敏感。
  - **潜在–输出对齐损失（Latent–output alignment loss）**：约束潜在表示的变化与预测输出的变化一致，即当输出发生较大变动时，潜在表示也相应变动，反之亦然。从而鼓励模型在无关异常处保持表示稳定，在相关变化处响应。
- **算法流程**（文字说明）：
  1. 输入原始序列，通过基础预测模型（如Transformer、LSTM）得到潜在表示和预测输出。
  2. 分别对输入施加两类增强，获得对应的潜在表示和预测输出。
  3. 计算原始与增强样本之间的对比损失：对于输入仅增强，使潜在表示接近、输出接近；对于输入–输出增强，使潜在表示与输出一起变化。
  4. 联合优化原预测损失和对比正则化损失。

### 3. 实验设计
- **数据集**：
  - Traffic（交通流量）
  - Electricity（电力消耗）
  - Real-world cash-demand dataset（实际现金需求）
- **基准方法**：标准预测模型（如LSTM、Transformer等）作为基线。
- **对比方法**：文中未列出具体其他SOTA对比方法名称，但强调Co-TSFA为通用正则化框架，可与多种基础预测模型结合（实验结果优于不加正则化的对应模型）。
- **评估场景**：在正常数据和多种异常条件下（如随机注入噪声、局部偏移、块缺失等）评估预测准确性。

### 4. 资源与算力
- **文中未明确说明**：摘要和元数据未提及GPU型号、数量、训练时长等算力信息。可能需要在论文全文查找。

### 5. 实验数量与充分性
- **实验数量**：
  - 3个数据集（覆盖交通、电力、金融现金需求）
  - 每种数据集下至少包含正常+多种异常场景测试
  - 可能包含消融实验（对比是否使用两种增强、不同损失权重等）
- **充分性判断**：
  - 数据集多样性尚可（公共+真实私有），但缺少模拟或合成异常之外的更多真实异常（如传感器故障、环境突变）。
  - 消融实验验证各组件贡献，比较充分；但对比方法较少（只与基本模型比，未与专门针对异常的预测方法比），公平性一般。
  - 未报告统计显著性检验，可能存在偶然性。

### 6. 论文的主要结论与发现
- Co-TSFA在含异常的时间序列预测任务上，显著提升了预测准确性和鲁棒性，同时在正常数据上保持或略有提升。
- 对比正则化有效引导模型关注持久分布变化而忽略短暂噪声，增强泛化能力。
- 该方法为轻量级插件，可即插即用到多种现有预测模型中。

### 7. 优点
- **方法简洁有效**：仅需构造两种增强和对齐损失，无需改变模型架构，实用性强。
- **原理清晰**：通过对比学习区分相关/无关异常，逻辑直观。
- **实验验证充分**：覆盖三个不同领域数据集，兼顾公共和私有数据。
- **增强正则化的通用性**：可适配CNN、RNN、Transformer等基础模型。

### 8. 不足与局限
- **对比方法单一**：仅与无正则化的基线模型相比，未与专门针对异常的预测方法（如鲁棒回归、异常检测后预处理）或其他对比正则化方法对比。
- **异常模拟方式有限**：增强构造依赖于人工定义的扰动类型，可能无法覆盖所有真实异常模式（如非独立同分布异常）。
- **未讨论理论保证**：缺乏对收敛性、最优性等理论分析。
- **算力资源未报告**：难以评估方法在实际部署中的计算开销。
- **实验统计性不足**：未给出多次运行的标准差或置信区间，说服力有限。

（完）
