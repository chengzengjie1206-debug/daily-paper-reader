---
title: "SynWeather: Weather Observation Data Synthesis Across Multiple Regions and Variables via a General Diffusion Transformer"
title_zh: SynWeather：基于通用扩散变压器的跨区域多变量天气观测数据合成
authors: "Kaiyi Xu, Junchao Gong, Zhiwang Zhou, Zhangrui Li, Yuandong Pu, Yihao Liu, Ben Fei, Fenghua Ling, Wenlong Zhang, Lei Bai"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37108/41070"
tags: ["query:ts-air-qual"]
score: 7.0
evidence: 合成天气数据填充时空缺失
tldr: 针对原始天气数据因仪器限制存在的时空缺失问题，本文提出SynWeather框架，利用通用扩散变压器实现多区域多变量天气观测数据统一合成。该方法突破了单变量单区域限制，利用跨变量互补性，避免过度平滑，为后续预测提供高质量完整数据集。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 现有数据合成方法局限于单变量单区域，且忽略跨变量互补性，导致结果过度平滑。
method: 提出SynWeather框架，采用通用扩散变压器实现多变量多区域联合数据合成。
result: SynWeather能有效填补时空缺失，生成更逼真多样化的天气观测数据。
conclusion: 该工作为天气数据补全和下游预测任务提供了统一高效的数据合成工具。
---

## Abstract
With the advancement of meteorological instruments, abundant data has become available. 
However, due to instruments’ intrinsic limitations such as environmental sensitivity and orbital constraints, raw data often suffer from temporal or spatial gaps, making it urgent to leverage data synthesis techniques to fill in missing information. 
Current approaches are typically focus on single-variable, single-region tasks and primarily rely on deterministic modeling. 
This limits unified synthesis across variables and regions, overlooks cross-variable complementarity and often leads to over-smoothed results. 
To address above challenges, we introduce SynWeather, the first dataset designed for Unified Multi-region and Multi-variable Weather Observation Data Synthesis. 
SynWeather covers four representative regions: the Continental United States, Europe, East Asia, and Tropical Cyclone regions, as well as provides high-resolution observations of key weather variables, including Composite Radar Reflectivity, Hourly Precipitation, Visible Light, and Microwave Brightness Temperature. 
In addition, we introduce SynWeatherDiff, a general and probabilistic weather synthesis model built upon the Diffusion Transformer framework to address the over-smoothed problem. 
Experiments on the SynWeather dataset demonstrate the effectiveness of our network compared with both task-specific and general models. 
Moreover, SynWeatherDiff is able to generate results that are both fine-grained and accurate in high-value regions.
Through the dataset and baseline model, we aim to advance meteorological downstream tasks and promote the development of general models for weather variable synthesis.

---

## 论文详细总结（自动生成）

# 论文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：气象观测仪器（如雷达、卫星）因环境敏感、轨道限制等固有缺陷，导致原始数据存在**时空缺失**（如雷达在复杂地形覆盖稀疏、可见光卫星夜间无数据）。需要数据合成技术填补缺失信息。
- **现有局限**：当前方法大多局限于**单变量、单区域**任务，依赖**确定性建模**（如像素级平方损失），忽略了**跨变量互补性**（如雷达反射率与降水之间存在Z-R关系），且结果往往**过度平滑**，难以捕捉高值区域和精细结构。
- **核心挑战**：如何实现**多区域、多变量**的统一通用天气观测数据合成。

## 2. 方法论
- **核心思想**：构建首个多区域多变量天气合成数据集 SynWeather，并基于**扩散变压器（Diffusion Transformer）** 提出通用概率生成模型 SynWeatherDiff，通过**文本提示**统一控制不同区域和变量的合成。
- **关键技术细节**：
  - **通用自编码器**：将所有天气变量（CR、降水、可见光、微波亮温）编码到共享潜空间，利用KL散度损失和对抗损失训练，减少冗余并利用变量间物理相似性。
  - **文本引导的扩散变压器**：
    - 任务提示格式：`"Synthesize the [变量] over [区域] region using corresponding satellite imagery."`
    - 提示经预训练 **CLIP文本编码器**（仅微调最后一个Transformer块）嵌入。
    - 卫星输入经 **ViT编码器**提取特征。
    - **早期融合策略**：将带噪潜变量 `z_t` 与卫星特征拼接后分块，再与提示token拼接，通过扩散变压器的自注意力层进行条件去噪。
  - **训练目标**：噪声预测损失（公式5-6）：
    \[
    L = \mathbb{E}_{z, \epsilon, t} \left[ \| \epsilon_\theta(z_t, t, X, P) - \epsilon \|_2^2 \right]
    \]
    其中 \( z_t = \sqrt{\bar{\alpha}_t} z + \sqrt{1-\bar{\alpha}_t} \epsilon \)。
- **概率建模优势**：克服确定性模型的过度平滑，生成精细结构和强中心区域。

## 3. 实验设计
- **数据集**：SynWeather，覆盖**四个区域**（美国本土CONUS、欧洲、东亚、热带气旋区域），**四个变量**（复合雷达反射率CR、小时降水量、可见光、微波亮温MWBT），来自**六个卫星源**（GOES-16/17/18、Himawari-8/9、Meteosat-11），统一分辨率4km/1小时。
- **任务设置**：
  - 6个**标准任务**：CONUS CR合成、CONUS/欧洲降水合成、欧洲/东亚可见光合成、热带气旋MWBT合成。
  - 1个**OOD任务**：东亚降水合成（训练中未见过的区域-变量组合）。
- **Benchmark与对比方法**：
  - 通用模型：**WeatherGFM**。
  - 任务特定模型：SRViT（VIL）、Deep-STEP（降水）、TomoPE（降水）。
  - 常见模型：**UNet**、**ViT**（调整为卫星到变量映射）。
  - 所有模型在SynWeather上使用**统一训练协议**。
- **评估指标**：
  - 事件检测：**CSI**（多个阈值：CR用25/35/40，降水用2/5/15，可见光用50，MWBT用300）。
  - 回归质量：**RMSE**。
  - 感知相似性：**SSIM、PSNR、LPIPS**。

## 4. 资源与算力
- **明确说明**：SynWeatherDiff训练使用 **4×80GB NVIDIA A100 GPU**，**batch size为16**，**训练600K步**。
- 优化器：AdamW，余弦学习率衰减（5e-4 → 1e-5）。

## 5. 实验数量与充分性
- **主要定量实验**（表2）：在全部6个标准任务上对比多种方法，包含多项指标（CSI多阈值、RMSE、SSIM、PSNR、LPIPS）。
- **可视化对比**（图4）：定性展示各任务生成效果。
- **消融实验**（表3）：任务采样比例—分别以CONUS CR、CONUS降水、欧洲可见光、MWBT作为主任务（采样率0.5），其他任务0.1，分析相互影响。
- **输入通道消融**（图5）：移除短波红外（SWIR）、水汽通道（WV）、长波红外（LWIR）、气体吸收通道（GAS）四组之一，考察对六个任务的影响。
- **OOD实验**（表4）：东亚降水合成任务，对比通用模型与专门模型。
- **充分性评价**：实验覆盖多个变量、区域、方法，进行了任务级和通道级消融，基准对比充分且使用统一协议，公平性良好；但未涉及模型规模消融或推理效率分析。

## 6. 主要结论与发现
- SynWeatherDiff在**大多数标准任务**上超越现有通用模型（WeatherGFM）和任务特定模型，尤其在**高CSI值**（捕捉强事件）上表现优异。
- 生成结果具有**精细结构**和**高值中心恢复能力**，避免确定性模型的平滑问题。
- 文本提示实现**统一框架**下的灵活生成（同一卫星输入可生成不同变量）。
- **任务间互补性**：
  - CR合成有利于降水（Z-R关系）和MWBT合成。
  - 可见光与MWBT相互增益（强对流系统的空间对应）。
  - CR与可见光存在冲突。
- **输入通道贡献**：所有红外通道均重要；水汽和长波红外对降水和雷达回波关键，短波红外对可见光关键；气体吸收通道影响较小。
- **OOD泛化**：SynWeatherDiff在东亚降水合成上超越专门模型（仅用CONUS降水训练），展示跨区域跨变量泛化能力。

## 7. 优点
- **数据集首创**：第一个支持**多区域、多变量**统一天气合成任务的标准化数据集。
- **模型创新**：首次将**扩散变压器**与**文本提示**结合用于天气变量合成，实现概率生成，显著提升精细度和高值恢复。
- **利用跨变量互补性**：统一建模减少计算开销并提升性能。
- **实验全面**：多任务、多方法、多指标对比，包含OOD评估和消融分析。
- **开源性**：提供了网站和代码链接，促进后续研究。

## 8. 不足与局限
- **OOD限制**：当前提示驱动框架**仅能处理训练集中出现过的区域和变量组合**，虽有泛化但仍不能完全推导未见区域-变量对。
- **可见光任务差距**：在可见光合成上，UNet（像素空间直接建模）优于SynWeatherDiff（潜空间重建丢失高频细节），作者承认需改进自编码器。
- **数据不平衡**：降水样本数量远少于可见光和CR，可能影响模型对稀疏事件的捕捉（虽已通过过滤处理）。
- **计算资源**：训练需4×A100 GPU 600K步，资源需求较高，未讨论推理效率或实时部署能力。
- **实验覆盖**：未进行模型参数量消融、采样步数影响、不同文本提示变化等分析。

（完）
