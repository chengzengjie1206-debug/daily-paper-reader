---
title: "TimeMosaic: Temporal Heterogeneity Guided Time Series Forecasting via Adaptive Granularity Patch and Segment-wise Decoding"
title_zh: TimeMosaic：通过自适应粒度块和分段解码实现时间异质性引导的时间序列预测
authors: "Kuiye Ding, Fanda Fan, Chunyi Hou, Zheya Wang, Lei Wang, Zhengxin Yang, Jianfeng Zhan"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39218/43179"
tags: ["query:ts"]
score: 6.0
evidence: 自适应块嵌入处理时间序列异质性，可用于含缺失值场景
tldr: 现有基于固定长度分块的时序预测方法忽略了局部时间动态的异质性。TimeMosaic提出自适应块嵌入，根据信息密度动态调整粒度，并结合分段解码器处理不同预测范围的复杂度。实验表明该方法在多个领域数据集上提升了预测精度，为处理动态时间模式提供了新思路。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 现有分块方法固定长度，忽略局部时间动态异质性和预测解码异质性，导致信息密集区细节丢失、稳定段冗余。
method: 提出TimeMosaic框架，包含自适应块嵌入动态调整粒度，以及分段式解码器分别处理短长期预测。
result: 在多个数据集上优于固定分块基线，有效捕捉多尺度时间动态。
conclusion: 自适应分块是处理时间异质性的有效策略，可提升预测性能。
---

## Abstract
Multivariate time series forecasting is essential in domains such as finance, transportation, climate, and energy. However, existing patch-based methods typically adopt fixed-length segmentation, overlooking the heterogeneity of local temporal dynamics and the decoding heterogeneity of forecasting. Such designs lose details in information-dense regions, introduce redundancy in stable segments, and fail to capture the distinct complexities of short-term and long-term horizons. We propose TimeMosaic, a forecasting framework that aims to address temporal heterogeneity. TimeMosaic employs adaptive patch embedding to dynamically adjust granularity according to local information density, balancing motif reuse with structural clarity while preserving temporal continuity. In addition, it introduces segment-wise decoding that treats each prediction horizon as a related subtask and adapts to horizon-specific difficulty and information requirements, rather than applying a single uniform decoder. Extensive evaluations on benchmark datasets demonstrate that TimeMosaic delivers consistent improvements over existing methods, and our model trained on the large-scale corpus with 321 billion observations achieves performance competitive with state-of-the-art TSFMs.

---

## 论文详细总结（自动生成）

# TimeMosaic 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：多变量时间序列预测中，现有基于固定长度分块（patch-based）的方法忽略了两个关键异质性：
  - **编码异质性**（Encoding Heterogeneity）：局部时间序列区域的信息密度变化很大（复杂突变区域信息密度高，平滑稳定区域密度低），固定长度分块导致信息密集区细节丢失，稳定段冗余。
  - **解码异质性**（Decoding Heterogeneity）：不同预测范围（如短期 vs 长期）的难度和信息需求不同，现有方法使用单一解码器统一处理所有预测范围，无法适应这种不对称性。
- **研究背景**：补丁方法在时间序列预测中表现优异（如PatchTST、TimeFilter、PathFormer等），但均采用固定长度分割；多注意力机制（motif reuse）和结构清晰性（structural clarity）之间存在矛盾：大补丁利于长程模式复用但模糊边界，小补丁增强分离性却碎片化长程模式。
- **核心意义**：提出时间异质性（Temporal Heterogeneity）这一更深层的数据属性，并针对性地设计解决方案，推动时间序列预测从“统一处理”走向“自适应差异化处理”。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
- 提出**TimeMosaic**框架，同时解决编码异质性和解码异质性。
- 输入侧：**自适应补丁嵌入（Adaptive Patch Embedding, APE）**，根据局部信息密度动态调整补丁粒度。
- 输出侧：**分段解码（Segment-wise Decoding）**，基于多任务提示调整（multi-task prompt tuning），不同预测范围作为相关子任务，使用范围感知提示（horizon-aware prompts）实现差异化预测。

### 关键技术细节
#### (1) 自适应补丁嵌入（APE）
- **补丁粒度搜索空间**：候选补丁大小集合 F = {f₁, f₂, …, f_K}，f₁ < f₂ < … < f_K。
- **区域划分**：将输入序列划分为长度 f_max 的非重叠区域（R = L / f_max）。
- **区域粒度分类器**：轻量级MLP分类器 G_θ，对每个区域预测最适合的补丁大小，使用Gumbel-Softmax实现端到端可微分。
- **补丁对齐与嵌入**：对不同粒度的补丁序列进行长度对齐（使用RepeatPad复制填充），保持时间连续性，然后拼接并加位置编码。
- **正则化（Budget Loss）**：防止分类器退化到总是选择最小粒度，通过L2损失约束每种补丁大小的使用比例接近预设目标。
- **总训练损失**：L_total = L_forecast + λ·L_budget，其中L_forecast为MSE。

#### (2) 分段解码（Segment-wise Forecasting via Prompt Tuning）
- 将多范围预测视为**分段多任务学习**问题，每个预测段对应一个子任务。
- 设计**可学习的范围感知提示（segment-aware prompts）**，插入到注意力机制的Key和Value路径，Query仅来自输入数据，实现参数高效的段级适应。
- 每段使用独立的预测头 f_k(·; θ_k) 生成最终预测。
- 训练时仅更新提示 {φ_k} 和解码头 {θ_k}，共享编码器参数冻结。

### 算法流程（文字描述）
1. 输入多变量时间序列 x ∈ R^{B×C×L}，划分为非重叠区域。
2. 通过分类器为每个区域选择最优补丁大小，根据选定大小进行补丁切分、嵌入、对齐、加位置编码。
3. 将处理后的序列输入共享编码器（如Transformer）。
4. 对每个预测段k，将段提示 φ_k 与编码器输出拼接，经过注意力交互，由段特定预测头生成预测。
5. 联合优化预测MSE损失和预算L2损失。

## 3. 实验设计

### 数据集
- **长期预测**：10个数据集，包括ETTh1、ETTh2、ETTm1、ETTm2、Weather、Traffic、Electricity、ExchangeRate、Solar-Energy、Wind（Location1-4）。
- **短期预测**：4个PEMS交通数据集（PEMS03/04/07/08）。
- 覆盖能源、交通、气候、金融、电力等多种领域。

### 基准方法（Baselines）
- 对比方法：TimeFilter、SimpleTM、PatchMLP、xPatch、DUET、PathFormer、iTransformer、TimeMixer、PatchTST、FreTS、DLinear、LightTS。
- 零样本对比：GPT4TS、LLMTime。
- 预训练基础模型（TSFMs）：TimeMoE、MOIRAI、Chronos、TimesFM、Moment。

### 实验设置
- **统一评估协议**：长程预测lookback=96，预测长度T=96/192/336/720；短程预测lookback=96，T=12。
- 统一实现框架，不使用测试时drop_last，保留EarlyStopping。
- 评价指标：MSE和MAE。
- 对比策略：表1为统一设置下各模型最优超参结果；表5为相同超参（更长lookback=320）的公平对比；表6为超参搜索（lookback选自{96,192,320,512}）下的最佳配置对比。

## 4. 资源与算力

- **实验硬件**：8块A800 GPU（用于标准实验）。
- **大规模训练**：在BLAST数据集上训练TimeMosaic（27M参数），使用2块V100 GPU，约40小时，输入长度512、输出长度720，训练数据3210亿个观测值。
- **注**：文中未明确给出单次实验的精确GPU小时数，但提到超参搜索需探索10,800种参数组合，计算成本极高。

## 5. 实验数量与充分性

- **实验覆盖**：
  - 主实验结果：表1（10个长程数据集+4个短程数据集）、表5（公平设置下10个数据集）、表6（超参搜索下5个数据集）。
  - 零样本实验：表4（ETTh1和ETTm2，对比TSFMs）、附录表23（跨数据集迁移，对比GPT4TS等）。
  - 消融实验：表2（APE和分段提示的贡献）、表3（补丁粒度组合影响）、图4（分段大小敏感性）。
  - 效率分析：附录表15（参数量、推理时间、GPU内存）。
  - 可视化分析：图5（注意力模式对比）、图6（lookback窗口敏感性）。
- **充分性与公平性**：
  - 采用统一实验设置（lookback、评价协议），避免参数过拟合。
  - 提供开源超参搜索脚本，确保各模型最优配置下的公平对比。
  - 覆盖了多个SOTA模型，不同规模（轻量到TSFM）。
  - 进行了零样本泛化和大规模预训练验证。
- **结论**：实验设计全面、对比公平，充分验证了方法有效性。

## 6. 主要结论与发现

- TimeMosaic在大多数数据集上取得最佳或第二最佳性能，在统一设置（表1）和公平对比（表5、表6）中均表现稳定。
- 自适应补丁嵌入（APE）显著提升性能（表2示为MSE降低约0.019），且对补丁粒度组合鲁棒（表3）。
- 分段解码进一步提升精度，尤其是结合APE后增益明显。
- 在零样本预测中，TimeMosaic在中等规模（27M参数）下达到与数十亿参数TSFM（TimeMoE、Chronos等）竞争的性能（表4）。
- 注意力可视化（图5）显示，TimeMosaic通过提示引导模型在不同预测段上聚焦不同时间区间，实现了差异化解码。
- 效率分析表明，TimeMosaic在保持较低参数量（数十万级）的同时，推理时间和内存使用合理。

## 7. 优点

- **问题洞察深刻**：首次明确提出编码异质性和解码异质性，并给出理论支持（Zipf分布、轮廓系数分析）。
- **方法创新性强**：自适应补丁嵌入（动态粒度）和分段提示（多任务提示调整）均具新颖性，且设计协同。
- **实现模块化**：通道建模策略支持通道独立/通道依赖，可即插即用。
- **实验公正严谨**：
  - 统一评估协议，避免选择性报告；提供超参搜索代码，确保公平性。
  - 覆盖大量数据集（17个）和基线方法（20+），消融、零样本、效率分析完整。
- **性能竞争力**：在标准设置和大规模预训练中均达到SOTA水平，证明方法可扩展性。
- **代码开源**：提供GitHub仓库，促进复现和应用。

## 8. 不足与局限

- **实验覆盖局限**：
  - 短期预测仅使用交通数据集（PEMS），未涉及其他领域（如金融高频数据）。
  - 零样本实验仅报告两个数据集（ETTh1、ETTm2），泛化验证范围有限。
  - 大规模预训练仅在一个语料库（BLAST）上验证，跨数据集零样本迁移尚未充分评估。
- **潜在偏差风险**：
  - 预算损失（Budget Loss）中预设比例 r_k 需人工设定，可能引入主观偏差。
  - 分段解码的段长度固定为L/3，虽宣称不cherry-pick，但可能不是最优划分。
- **应用限制**：
  - 自适应补丁选择依赖区域分类器G_θ，其复杂度随区域数R增加而上升，可能难以适应极长输入序列。
  - 分段解码逐步执行，推理时间高于极轻量模型（如iTransformer），在高速场景中可能受限。
  - 未讨论缺失值、异常值等实际数据质量问题。
- **理论深度**：虽然基于Zipf分布和轮廓分数给出动机，但未提供理论保证（如收敛性、最优补丁选择的泛化界）。

（完）
