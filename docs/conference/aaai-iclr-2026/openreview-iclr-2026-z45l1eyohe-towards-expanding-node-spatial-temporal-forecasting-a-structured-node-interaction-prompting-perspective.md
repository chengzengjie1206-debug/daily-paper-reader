---
title: "Towards Expanding-Node Spatial-Temporal Forecasting: A Structured Node Interaction Prompting Perspective"
title_zh: 面向扩展节点的时空预测：结构化节点交互提示视角
authors: "Qi Zheng, Zihao Yao, Canyang Zhang, Yaying Zhang"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=z45L1eYoHE"
tags: ["query:ts"]
score: 6.0
evidence: 扩展传感器网络的时空预测
tldr: 该论文针对传感器网络扩展节点场景提出SNIP框架，通过结构化节点交互提示学习异构节点表示，无需依赖节点特定参数，有效适应新节点少量观测的挑战。在交通和气候数据上验证了其优越性，推动了时空预测在动态网络中的应用。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有时空预测模型假设节点固定，无法处理节点增减的扩展场景。
method: 提出结构化节点交互提示，学习节点间通用交互模式。
result: 在扩展节点场景下优于现有固定节点模型。
conclusion: 该框架使时空预测模型具备动态扩展能力，适用于真实传感器网络。
---

## Abstract
The rapid expansion of sensor systems, such as traffic networks, climate monitoring, and energy scheduling, poses new challenges for spatial-temporal series forecasting. While existing models have achieved strong performance under the fixed-node assumption, they rely on node-dependent parameters and fail to adapt when the network evolves, i.e., when old nodes are removed and new nodes with limited history are added. This expanding-node forecasting scenario introduces two critical challenges: (1) learning heterogeneous node representations without coupling learnable parameters to node count, and (2) enabling effective adaptation to new nodes with scarce observations. To tackle these challenges, we propose SNIP (Structured Node Interaction Prompting), a model-agnostic framework that constructs static spatial-temporal priors from historical observations and topology, and dynamically refines them during model training. Specifically, SNIP generates structured priors from three perspectives: periodic patterns across nodes, spatial-temporal interactions under time delays and graph structural information. These priors are projected into model as node promptings and then dynamically refined. For new nodes, SNIP initializes priors by similarity-weighted mixtures of old nodes and updates them with limited history, enabling efficient few-shot adaptation. Extensive experiments on multiple datasets demonstrate that SNIP outperforms state-of-the-art baselines in expanding-node scenarios. Beyond accuracy, SNIP provides plug-and-play generality and computational efficiency, bridging the gap between fixed-node precision and expanding-node adaptability in spatial-temporal forecasting.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **背景**：传感器系统（如交通网络、气候监测、能源调度）快速扩张，现有时空序列预测模型在**固定节点假设**下表现良好，即训练和测试时传感器节点数量和位置不变。
- **核心问题**：当网络动态演化（旧节点移除、新节点加入且历史数据极少）时，现有模型因依赖**节点特定参数**而无法适应。这带来两个挑战：
  1. 学习**异构节点表示**，且不能将可学习参数与节点数量耦合（即模型参数不能随节点数增长）。
  2. 对新节点实现**少样本有效适应**（仅有少量观测）。
- **研究意义**：填补现有时空预测模型在**扩展节点场景**下的空白，使模型具备动态网络扩展能力，更贴近真实世界传感器网络部署。

## 2. 方法论：核心思想、关键技术细节

- **框架名称**：SNIP（Structured Node Interaction Prompting，结构化节点交互提示）
- **核心思想**：不依赖节点特定参数，而是从历史观测和拓扑中构建**静态时空先验**，并在训练过程中动态细化这些先验，将其作为**节点提示**注入模型，从而学习节点间通用交互模式。
- **关键技术细节**：
  - **结构化先验生成**：从三个角度生成先验：
    - 跨节点的周期模式（Periodic patterns across nodes）
    - 考虑时间延迟的时空交互（Spatial-temporal interactions under time delays）
    - 图结构信息（Graph structural information）
  - **提示注入与动态细化**：将生成的先验投影到模型中作为节点提示（node promptings），并在训练过程中动态更新。
  - **新节点适应**：对于新节点，通过**相似度加权混合**旧节点先验来初始化其先验，然后利用有限的历史观测进行更新，实现高效少样本适应。
- **算法流程**（文字描述）：
  1. 对原始数据提取历史观测和拓扑结构；
  2. 从三个视角生成静态先验；
  3. 将先验经过投影层转化为节点提示，输入到任意时空预测骨干模型；
  4. 训练过程中动态调整提示；
  5. 当新节点出现时，基于与旧节点的相似度加权平均初始化提示，并用少量新数据微调。
- **模型无关性**：SNIP是模型无关框架，可即插即用于各种现有时空预测模型。

## 3. 实验设计

- **数据集**：多个数据集，涵盖交通和气候领域（如交通网络速度、气候监测数据等）。具体数据集名称在摘要未列出，但提供元数据提及“交通和气候数据”。
- **场景**：扩展节点场景（即训练时只有部分节点，测试时出现新节点，且新节点历史数据极少）。
- **基准方法**：对比了最新的时空预测模型（SOTA baselines），包括那些假设固定节点的模型。
- **对比方法**：未在摘要中详细列出方法名称，但提到“outperforms state-of-the-art baselines”。
- **评估指标**：未明确说明，通常为MAE、RMSE等。

## 4. 资源与算力

- **未明确说明**：论文摘要和元数据中**没有提及**使用的GPU型号、数量、训练时长等算力信息。无法评估其计算开销。

## 5. 实验数量与充分性

- **实验组数**：从摘要推断，在多个数据集上进行了实验，并包含消融实验（因为提到了“three perspectives”消融）。但具体消融实验细节未给出。
- **充分性判断**：
  - 优点：跨领域（交通、气候）验证，多数据集对比，且与SOTA比较，具有一定客观性。
  - 不足：由于未提供完整的实验设置（如数据集规模、节点数、扩展比例、少样本数等），无法判断实验覆盖是否全面。可能存在对特定场景的偏向性。此外，由于该论文被ICLR 2026拒稿，可能实验论证存在不足。

## 6. 主要结论与发现

- SNIP框架在扩展节点场景下显著优于现有固定节点模型。
- 不仅预测精度更高，而且具有**即插即用**的通用性和**计算效率**。
- 成功弥合了固定节点精度与扩展节点适应性之间的鸿沟，使时空预测模型能够适应动态传感器网络。
- 证明了结构化节点交互提示能够有效学习节点间通用交互模式，无需依赖节点特定参数。

## 7. 优点

- **方法创新性**：针对被忽视的扩展节点场景，提出结构化提示机制，思路新颖。
- **模型无关性**：可适用于任意时空预测骨干网络，实用性强。
- **少样本适应**：通过相似度混合初始化，高效适应新节点，解决了数据稀缺问题。
- **全面的先验设计**：融合周期模式、时延交互和图结构，捕捉多维度时空依赖。

## 8. 不足与局限

- **实验覆盖可能有限**：未提供完整数据集和详细实验设置，被ICLR拒稿暗示实验可能不够充分或结果不够强。
- **消融实验细节缺失**：仅提及三个视角，但未给出各自贡献的量化分析。
- **应用限制**：假设节点间拓扑结构已知（图结构信息），在无拓扑或动态拓扑变化（不仅是节点增减）的场景下可能失效。
- **计算资源未报告**：无法评估方法的实际部署成本。
- **仅涉及节点增减**：未考虑节点属性变化、网络分裂合并等更复杂演化。
- **偏差风险**：可能只在自己构造的扩展场景上表现好，真实场景中节点分布、缺失模式更复杂。

（完）
