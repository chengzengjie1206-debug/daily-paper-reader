---
title: "Beyond Missing Data Imputation: Information-Theoretic Coupling of Missingness and Class Imbalance for Optimal Irregular Time Series Classification"
title_zh: 超越缺失数据插补：缺失与类别不平衡的信息论耦合以实现最优不规则时间序列分类
authors: "Xin Qin, Mengna Liu, Wenjie Wang, Shuxin Li, Tianjiao Li, Xiufeng Liu, Xu Cheng"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39682/43643"
tags: ["query:ts"]
score: 6.0
evidence: 处理缺失数据的不规则时间序列分类
tldr: 该论文针对不规则时间序列中缺失数据与类别不平衡问题，提出信息论耦合方法，利用缺失模式的结构信息来提升分类性能，在不规则分类基准上取得了最优结果，为不规则时间序列分析提供了新的视角。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 不规则时间序列中缺失模式常被忽略，且类别不平衡加剧分类困难。
method: 利用信息论将缺失模式与类别不平衡耦合，设计专用的深度学习架构。
result: 在不规则时间序列分类基准上达到最优。
conclusion: 缺失模式本身蕴含关键信息，有效利用可大幅提升分类性能。
---

## Abstract
Irregular time series (IRTS) are prevalent in real-world applications, where uneven sampling and missing data pose fundamental challenges to deep learning-based feature modeling. Although existing methods attempt to retain timestamp information, they often overlook the structured patterns embedded within the missingness itself, and tend to perform poorly when confronted with class imbalance exacerbated by data incompleteness. Specifically, temporal irregularity hinders the modeling of long-range dependencies
and local patterns, while sparse observations limit representational capacity, disproportionately impairing minority classes and leading to severe classification bias. To address these deeply coupled challenges, we propose SPECTRA (Structured Pattern and Enriched Context-aware Temporal Representation Architecture), a unified framework for robust IRTS classification. SPECTRA introduces a frequency-guided observation encoder that reconstructs temporal dependencies in a stable manner, mitigating spectral distortion and information corruption. Complementarily, a missingness pattern encoder explicitly captures the dynamic evolution of missing data and leverages it as a discriminative signal. In addition, a prototype-constrained classification paradigm directly optimizes the geometric structure of the feature space, enhancing intra-class compactness and alleviating generalization bottlenecks caused by class imbalance. Extensive experiments on three public IRTS datasets—P12, P19, and PAM—demonstrate the superior performance of SPECTRA under both missing and imbalanced conditions.

---

## 论文详细总结（自动生成）

好的，我将根据您提供的论文内容，生成一份详细且结构化的中文总结。

---

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现实世界的不规则时间序列（IRTS）同时面临两大挑战：**数据缺失**（因采样不规则、传感器故障等）和**类别不平衡**（关键事件属于少数类）。现有方法通常将两者视为独立问题分开处理，导致性能不佳。
- **关键洞察**：论文首次从信息论角度揭示两者存在内在的**“缺失-不平衡耦合”（Missing-Imbalance Coupling）**。在随机缺失（MAR）和完全非随机缺失（MNAR）机制下，缺失模式本身包含类别判别信息，且这种信息与类别频率呈负相关。多数类样本往往更完整，而少数类（如危重病情）样本缺失更严重、信息更匮乏，遭受双重信息损失。
- **整体含义**：忽视这种耦合会导致三个根本性问题：① 不规则的采样造成频谱扭曲且集中影响少数类；② 少数类表示坍塌，特征被噪声主导；③ 优化梯度偏向多数类，模型忽略少数类。因此，必须将缺失模式作为第一类信息源加以利用，联合建模才能实现最优分类。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：构建一个统一框架 **SPECTRA**，通过三个协同模块实现信息论最优的IRTS分类。核心是将缺失模式视为结构化信号，并利用原型学习修正特征空间几何结构。
- **技术细节**（三个模块）：
    1.  **缺失感知频率滤波模块 (MAFF)**：
        - **自我校准频谱增强 (SCSE)**：对输入信号做FFT后，通过可学习的注意力权重（低、中、高三个频段）生成样本自适应的频域滤波掩码，再通过逆变换恢复时域信号。这能自适应地调整滤波策略，强调有用频段、抑制噪声/缺失造成的频谱干扰。
        - **缺失引导动态感受野重构 (AG-DRR)**：根据局部缺失密度，通过一个元学习控制器动态生成核权重分布，作用于并行卷积核簇（不同膨胀率）。在信息密集区使用小膨胀率提取精细特征，在信息稀疏区使用大膨胀率跨缺失段聚合信息，避免零填充污染特征。
    2.  **缺失模式编码器 (MPE)**：
        - **局部缺失门控感知**：使用门控卷积单元对二进制缺失掩码进行特征提取，通过双路径（tanh候选+ sigmoid门控）自适应选择有意义缺失模式（如连续缺失、关键变量缺失）。
        - **全局缺失动态建模**：将局部特征输入GRU，建模缺失模式的时序演化（间歇性/持续性缺失、连接-断开-重连过程）。最终输出为高维缺失嵌入，作为显式信息源补偿观察数据的语义缺失。
    3.  **类别引导特征精炼模块 (CGFR)**：
        - 将观察数据特征和缺失模式特征通过注意力门控融合。
        - 在黎曼流形（Fisher信息度量）上学习每个类别的原型向量 \(c_k\)。分类时使用负欧氏距离 \( \text{logit}_k(z) = -\|z - c_k\|^2 \) 代替传统线性分类器。
        - 总损失为交叉熵损失 + 原型中心约束损失 \( \mathcal{L}_{\text{center}} \)，鼓励类内紧凑和类间分离，增强少数类判别力。

- **理论贡献**：给出了耦合强度 \(\kappa\) 的定义，证明了其在真实MAR下严格大于0。推导了耦合强度下的最小可达到误差下界，为模型最优性提供了理论保证。

### 3. 实验设计

- **数据集与场景**：
    - **P12** (PhysioNet 2012)：ICU死亡率预测，二分类，严重不平衡。
    - **P19** (PhysioNet 2019)：脓毒症早期预测，二分类，不平衡。
    - **PAM** (PAMAP2)：人类活动识别，多分类（12类）。
- **基准对比**：共对比14种主流方法，包括：
    - 基础方法：Transformer、Trans-Mean、GRU-D、IP-Net、SeFT、MTGNN、mTAND。
    - 前沿方法：Raindrop、WarpFormer、ContiFormer、MTSFormer、ViTST、MuSiCNet等。
- **评价指标**：P12/P19使用AUC和AUPR（对不平衡更敏感）；PAM使用Accuracy、Precision、Recall、F1 Score。

### 4. 资源与算力

- 论文明确提及实验在 **PyTorch 1.13.1** 平台、**NVIDIA RTX 4090 GPU** 上运行。
- 给出了推理速度参考：处理1000长度序列需要 **23ms**，与Transformer速度相当。
- **未提及**：训练时长、使用的GPU数量、总计算资源消耗（如GPU小时数）。因此无法给出完整算力统计。

### 5. 实验数量与充分性

- **实验种类全面**：
    - **主实验**：在三个数据集上与14个基线对比（表1），报告了所有指标。
    - **缺失-不平衡耦合验证**：计算了各数据集的互信息 \(I(Y;M)\)，验证了耦合存在（0.067–0.142）。
    - **缺失机制验证**：通过Little's MCAR检验和卡方检验验证了MAR假设。
    - **鲁棒性实验**：Leave-Random-Sensor-Out实验（图3），模拟传感器故障，缺失率从10%到50%。
    - **CGFR缺失鲁棒性分析**（图4）：对比有无CGFR模块在不同缺失率下的性能。
    - **消融实验**（表2）：依次移除MPE、SCSE、AG-DRR、MAFF、CGFR，验证各模块贡献。
    - **不平衡损失函数对比**（表3）：将CGFR与Focal Loss、LDAM、BSL、CB等主流不平衡学习方法对比。
- **充分性评价**：实验设计非常充分，覆盖了主对比、消融、鲁棒性、耦合验证等多个维度，且使用了多个数据集和多种指标。实验结论支持论文理论，对比基线都是近期SOTA，实验过程公平可靠。

### 6. 论文的主要结论与发现

- SPECTRA在三个数据集上均达到最优，尤其在AUPR（对少数类更敏感）上提升显著（P19上AUPR提升2.95%），证明了其对缺失-不平衡耦合建模的有效性。
- 提出的缺失-不平衡耦合理论得到实证支持：缺失模式确实携带类别判别信息（互信息显著非零），验证了 \(\kappa > 0\)。
- 各模块均有不可或缺的作用，其中 **CGFR** 对性能贡献最大，尤其在处理不平衡时效果显著；**MPE** 在高缺失率场景下至关重要；**MAFF** 中的SCSE和AG-DRR协同工作，共同维持模型稳定性。

### 7. 优点

- **理论创新**：首次为缺失与不平衡的耦合提供了信息论基础，给出了形式化定义、下界和最优性保证，开创了新视角。
- **方法设计巧妙**：将缺失模式从“噪声”提升为“信号”，通过MPE提取其结构化信息，结合自适应频率滤波和动态感受野，体系完整且新颖。
- **原型学习机制（CGFR）**：直接作用于特征空间几何，比传统不平衡损失（如Focal）更适应耦合场景，且具有理论收敛保证。
- **实验严谨**：多数据集、多基线、多维度实验，验证了理论和方法的有效性，代码已开源，可复现。
- **效率可接受**：复杂度 \(O(CL\log L)\)，推理速度与Transformer相当，具有实际部署价值。

### 8. 不足与局限

- **实验资源信息缺失**：未报告训练时长和总算力，不利于可重复性和资源评估。
- **缺失机制假设局限**：论文验证了MAR条件，但未对MNAR场景进行深入实验（尽管理论分析了MNAR）。实际场景中MNAR常见，泛化性需进一步验证。
- **超参数依赖**：CGFR中的\(\gamma\)（正则强度）、AG-DRR的核簇数K等需要调参，论文未给出具体选择敏感度分析。
- **可解释性不足**：虽然提出缺失模式是信号，但未具体分析哪些缺失模式对分类起作用（如连续缺失 vs. 随机缺失），可解释性有限。
- **应用局限性**：主要验证于医疗（ICU）和可穿戴（活动识别）领域，对于其他不规则时间序列（如金融、环境监测）的泛化能力未知。
- **潜在偏差风险**：少数类样本更稀缺且缺失更严重，尽管SPECTRA改善了性能，但在极端不平衡/高缺失场景下，模型可能仍存在过拟合风险。消融实验显示移除CGFR后AUPR下降极明显（P12上从54.4%降至44.6%），说明该方法对原型约束高度依赖。

（完）
