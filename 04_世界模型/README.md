# 世界模型（World Models）

被普遍认为是"后LLM"阶段最重要的方向之一。核心问题：语言模型只学会了预测下一个词，而世界模型要学会预测"世界的下一个状态"，从而支持规划、想象、具身智能。配套流程图见 `世界模型Dreamer循环.mermaid`（潜空间推演循环）。

## 两条主要路线

### 路线一：像素/视频生成式路线

直接生成未来画面，直觉上更接近人类的视觉想象，但计算成本高，因为要重建完整像素。

代表作：**Genie / Genie 2 / Genie 3**（Google DeepMind，交互式环境生成，可以从一张图生成可玩的3D世界）和 **Sora**（OpenAI，大规模视频生成作为"世界模拟器"的雏形）。这类工作目前更偏工程和产品报告，公开的论文细节相对少，建议直接看官方技术博客。

### 路线二：潜空间表征预测路线（JEPA系）

不重建像素，只在抽象的潜空间里预测"缺失部分的表征"，计算效率更高。

**理论起点**：Yann LeCun, 2022, *A Path Towards Autonomous Machine Intelligence*
论文：https://openreview.net/pdf?id=BZ5a1r-kVsf （相关的能量模型技术论文见 arXiv [2306.02572](https://arxiv.org/abs/2306.02572)）
提出了完整的世界模型+分层规划的认知架构构想，是理解LeCun为什么后来离开Meta单独创业做世界模型的关键背景文档，也是JEPA（Joint Embedding Predictive Architecture）架构的源头。

**V-JEPA 2**
Assran et al. (Meta, 含LeCun), 2025, *V-JEPA 2: Self-Supervised Video Models Enable Understanding, Prediction and Planning*
arXiv: [2506.09985](https://arxiv.org/abs/2506.09985)
在超过100万小时的网络视频上做自监督训练，然后零样本控制真实机械臂，是JEPA路线目前最具体的落地成果。

## 用世界模型做强化学习规划：Dreamer系列

**DreamerV3**
Hafner et al., 2023, *Mastering Diverse Domains through World Models*
arXiv: [2301.04104](https://arxiv.org/abs/2301.04104)
"用世界模型做强化学习规划"这条思路里最系统的工作：先学一个能预测latent state和奖励的循环状态空间模型（RSSM），然后完全在潜空间里"脑内推演"生成大量虚拟轨迹，用这些虚拟轨迹训练策略网络，不需要在真实环境里反复试错。这也是配套流程图对应的核心循环，用固定超参数就能在Minecraft等大量不同领域中取得效果，是该系列的代表性成果。

## 2026年的新进展（学习时留意持续更新）

- LeCun团队近期发表了对JEPA架构的形式化证明工作，说明了JEPA在什么条件下能恢复真实世界结构，同时配套benchmark发现当前模型在小扰动下表现脆弱，说明这条路线还远未成熟。
- NVIDIA开源了Cosmos（面向物理AI的世界基础模型平台）。
- Fei-Fei Li的World Labs发布了Marble（空间智能+可编辑3D世界）。
- 简化工作 *LeWorldModel*（arXiv [2603.19312](https://arxiv.org/abs/2603.19312)）把JEPA-as-world-model的做法做了进一步精简。

北京智源研究院2026年度AI十大趋势报告把"世界模型成为AGI共识方向"列为第一条，用"Next-State Prediction范式"概括这个转变，可作为中文语境下理解行业共识的补充读物。

## 建议学习顺序

先看LeCun的立场论文建立整体世界观 → DreamerV3论文理解"世界模型+RL做规划"的具体实现 → V-JEPA 2论文理解潜空间预测路线的另一种实现 → 按兴趣选择性看Genie/Sora这类生成式路线的技术报告。
