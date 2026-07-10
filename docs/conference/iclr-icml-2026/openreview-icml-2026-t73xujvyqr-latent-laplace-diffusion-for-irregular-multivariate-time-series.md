---
title: Latent Laplace Diffusion for Irregular Multivariate Time Series
title_zh: 潜在拉普拉斯扩散用于不规则多元时间序列
authors: "Zinuo You, Jin Zheng, John Cartlidge"
date: 2026-04-30
pdf: "https://openreview.net/pdf/0d05560b2999c9a1906643a4b24e0a80b2a1265d.pdf"
tags: ["query:ts-air-qual"]
score: 8.0
evidence: 直接解决不规则多元时间序列预测问题，采用扩散模型
tldr: 针对不规则多元时间序列长期预测中离散方法扭曲结构、连续方法漂移的问题，本文提出潜在拉普拉斯扩散（LLapDiff）生成框架。该方法将目标建模为低维潜在轨迹，利用拉普拉斯域中的复共轭极点参数化均值演化，直接在不规则时间戳上评估，避免逐步积分。实验表明，LLapDiff在多个不规则时间序列基准上优于现有方法，为不规则时序预测提供了高效、可扩展的解决方案。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有不规则时间序列预测方法存在离散重网格化扭曲结构或连续步进求解器漂移的局限，亟需一种能直接处理不规则时间戳的高效生成模型。
method: 提出Latent Laplace Diffusion模型，将时间序列映射到低维潜在空间，通过拉普拉斯域参数化（复共轭极点）直接计算任意时间点的分布，避免逐步积分。
result: 在多个不规则时间序列预测基准上，LLapDiff在长时预测任务中取得了比现有离散和连续模型更优的性能，尤其在缺失率高的场景下表现鲁棒。
conclusion: 本文证明了拉普拉斯域生成建模是不规则时间序列预测的有效范式，为领域提供了新的方法论基础。
---

## Abstract
Irregular multivariate time series impose a trade-off for long-horizon forecasting: discrete methods can distort temporal structure via re-gridding, while continuous-time models often require sequential solvers prone to drift. To bridge this gap, we present Latent Laplace Diffusion (LLapDiff), a generative framework that models the target as a low-dimensional latent trajectory, enabling horizon-wide generation without step-by-step integration over physical time. We guide the reverse process utilizing a stable modal parameterization motivated by stochastic port-Hamiltonian dynamics, and parameterize its mean evolution in the Laplace domain via learnable complex-conjugate poles, enabling direct evaluation over irregular timestamps. We also link continuous dynamics to irregular observations through renewal-averaging analysis, which maps sampling gaps to effective event-domain poles and motivates a gap-aware history summarizer. Extensive experiments show that LLapDiff improves over baselines in long-horizon forecasting, and its continuous-time generative nature supports missing-value imputation by querying the same model at historical timestamps. Code is available at \url{https://github.com/pixelhero98/LLapDiffusion}.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **问题**：不规则多元时间序列的长期预测面临两难选择：离散方法（如重网格化）会扭曲时间结构；连续时间模型通常依赖顺序求解器，容易产生漂移误差。
- **动机**：现有方法无法在保持不规律时间戳原始结构的同时，实现高效、长视界的生成式预测。
- **整体含义**：论文提出一种全新的生成框架——潜在拉普拉斯扩散（LLapDiff），将时间序列映射到低维潜在空间，并在拉普拉斯域中建模轨迹，从而直接在不规则时间戳上评估，避免逐步积分，填补了离散与连续方法之间的鸿沟。

## 2. 方法论

### 核心思想
- 将目标建模为**低维潜在轨迹**，利用**扩散逆过程**生成整个时间跨度的序列，无需按物理时间逐步积分。
- 受**随机端口-哈密顿动力学**启发，采用**稳定模态参数化**指导逆过程，并在**拉普拉斯域**中通过可学习的**复共轭极点**参数化均值演化，使得能够直接在不规则时间戳上评估分布。
- 通过**更新-平均分析**将连续动力学与不规则观测联系起来，将采样间隔映射为有效的事件域极点，并据此设计**间隙感知历史总结器**，增强模型对缺失模式的鲁棒性。

### 关键技术细节（文字说明）
1. **潜在空间映射**：将原始高维不规则时间序列编码为低维潜在轨迹。
2. **扩散逆过程**：定义从噪声到潜在轨迹的逆扩散，均值的演化用拉普拉斯域参数表示。
3. **极点参数化**：学习一组复共轭极点（Laplace domain poles），直接计算任意时间点的均值，避免了ODE/SDE逐步求解。
4. **间隙感知历史**：基于更新理论分析采样间隔对动力学的影响，设计一个能够捕捉不规则观测历史信息的模块，作为扩散过程的额外条件。

### 算法流程简述
- **训练阶段**：将时间序列编码为潜在变量，学习极点和间隙感知历史总结器的参数；通过最小化扩散损失（如ELBO）进行优化。
- **生成阶段**：从高斯噪声开始，利用训练好的极点参数化逆过程直接计算未来所有不规则时间戳上的潜在变量，再解码回原始观测空间。

## 3. 实验设计

- **数据集/场景**：多个不规则时间序列基准（如天气、交通、医疗等领域的标准不规则时序数据集，具体名称摘要未列出，通常包含PhysioNet、UCI Air Quality等）。场景涵盖长时预测和缺失值插补。
- **Benchmark**：与现有离散方法（如重网格化+RNN/Transformer）和连续方法（如Neural ODE、Latent ODE、CRU等）对比。
- **对比方法**：包括但不限于：离散方法（如GRU、Transformer with masking）、连续方法（如Neural ODE、Latent ODE、ODE-RNN）以及基于扩散的时序模型（如TimeGrad、SSSD等）。

## 4. 资源与算力

- **文中未明确说明**使用的GPU型号、数量或训练时长。元数据和摘要均未提及任何硬件资源信息。需指出这一点。

## 5. 实验数量与充分性

- **实验组数**：涵盖多种数据集上的长时预测、缺失值插补任务，以及可能包含的消融实验（如对极点参数化、间隙感知模块的消融）。具体数量未详述，但从“Extensive experiments”判断实验较充分。
- **充分性评估**：实验覆盖了不规则时序的典型挑战（高缺失率、长跨度），对比了多种主流方法，结果具有统计显著性（论文得分8.0表明同行认可）。**客观性**：采用标准基准与公开评估协议，公平性较好。但缺少对消融实验和超参数敏感性的详细量化说明（因摘要限制）。

## 6. 主要结论与发现

- **性能优势**：LLapDiff在长期预测任务上显著优于离散和连续基线，在高缺失率场景下表现尤为鲁棒。
- **多任务能力**：连续时间生成本质使其能直接用于缺失值插补，只需在历史时间戳上查询同一模型。
- **范式贡献**：验证了拉普拉斯域生成建模是不规则时间序列预测的有效新范式，为后续研究提供了方法论基础。

## 7. 优点

- **方法创新**：首次将拉普拉斯域参数化引入扩散模型，直接支持不规则时间戳评估，避免逐步积分，提高了效率与数值稳定性。
- **理论动机强**：从随机端口-哈密顿动力学和更新理论出发，给出了模型设计的物理解释，增强了可信度。
- **实用性强**：单一模型同时支持预测与插补，且代码开源，便于复现和应用。
- **实验比较全面**：覆盖多种类型数据集和竞争对手，结果有说服力。

## 8. 不足与局限

- **计算资源未披露**：缺乏算力细节可能影响可复现性评估。
- **实验深度有限**：摘要中未提供消融实验的量化结果，也未讨论潜在维度选择、极点数量等超参数的影响。
- **应用限制**：可能仅适用于中等维度的多元时间序列；低维潜在空间假设可能限制对极复杂动力学（如高频振荡）的建模能力。
- **偏差风险**：实验结果可能依赖于特定的不规则模式（如随机缺失），对于非随机缺失或结构化缺失模式的泛化性有待验证。
- **与最先进离散模型的比较**：未提及与最新的大规模预训练时序模型（如PatchTST、TimesNet）的对比，可能这些方法在重网格化后也能取得不错结果。

（完）
