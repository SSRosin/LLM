# 训练与对齐

过去两年最重要的技术脉络：如何用强化学习让LLM具备推理能力，而不只是模仿人类写的文字。配套流程图见 `RLHF与RLVR训练流程.mermaid`（两条后训练路线对比）和 `GRPO算法细节.mermaid`（GRPO内部计算过程）。

## 必读经典

**InstructGPT / RLHF论文**
Ouyang et al., 2022, *Training language models to follow instructions with human feedback*
arXiv: [2203.02155](https://arxiv.org/abs/2203.02155)
整套体系的起点：先SFT，再训练奖励模型，再用PPO优化策略。理解这篇之后，后面所有变体都是在这个框架上做减法或改造。

**PPO原始论文**
Schulman et al., 2017, *Proximal Policy Optimization Algorithms*
arXiv: [1707.06347](https://arxiv.org/abs/1707.06347)
不是LLM相关论文，但PPO是RLHF阶段的核心优化器，建议补一下背景，否则后面理解GRPO"去掉了critic"这句话意义不大。

## 关键转折点：DeepSeek-R1

**DeepSeek-R1 / R1-Zero**
DeepSeek-AI, 2025, *DeepSeek-R1: Incentivizing Reasoning Capability in LLMs via Reinforcement Learning*
arXiv: [2501.12948](https://arxiv.org/abs/2501.12948)
近两年最重要的一篇。核心发现：不需要奖励模型，只要给一个可以自动判断对错的验证器（比如数学题答案是否匹配、代码是否跑过测试），配合GRPO算法，模型就能自发学会长链条推理、自我检查、回溯。这解释了为什么RLVR（Reinforcement Learning with Verifiable Rewards）路线能规模化——不再需要昂贵的人类偏好标注。

**GRPO / DeepSeekMath**
Shao et al., 2024, *DeepSeekMath: Pushing the Limits of Mathematical Reasoning in Open Language Models*（GRPO在这篇里首次提出）
arXiv: [2402.03300](https://arxiv.org/abs/2402.03300)
值得单独看一下算法细节：对同一个问题采样一组回答，组内做相对打分归一化，用排名代替绝对价值估计，从而省掉PPO里的critic网络，训练成本大幅下降。对应 `GRPO算法细节.mermaid`。

## 延伸与近期跟进工作

- **TR-GRPO**（Token-Regulated Group Relative Policy Optimization）
  arXiv: [2511.00066](https://arxiv.org/abs/2511.00066)
  针对GRPO训练不稳定的问题，提出按token粒度调节贡献度，在数学、逻辑、agentic推理任务上都有提升。

- **RLVR机制的理论分析**：*Reinforcement Learning with Verifiable Rewards Implicitly Incentivizes Correct Reasoning in Base LLMs*
  arXiv: [2506.14245](https://arxiv.org/abs/2506.14245)
  指出RLVR起作用的前提是预训练阶段已经建立了正确/错误思维链的知识先验，RL本质上是把已有能力"挑选"出来放大，而不是从零学会新能力——这对理解RLVR的能力边界很关键。

- **R1-Zero训练的批判性分析**：*Understanding R1-Zero-Like Training: A Critical Perspective*
  arXiv: [2503.20783](https://arxiv.org/abs/2503.20783)

- Sebastian Raschka的综述博客《The State of Reinforcement Learning for LLM Reasoning》
  https://magazine.sebastianraschka.com/p/the-state-of-llm-reasoning-model-training
  适合作为路线图式的补充阅读，把各家（DeepSeek、Qwen、Kimi等）的RL训练方案做了系统对比，非arXiv论文但信息密度很高。

## 还需要覆盖（后续补充）

- **DPO**（Direct Preference Optimization）：把RLHF简化为纯监督式偏好优化，无需训练RM
- **LoRA / QLoRA**：参数高效微调，面试高频考点
- **Scaling Law**：参数量/数据量/计算量如何配比
- **数据工程**：指令数据构造、去重、质量过滤

## 面试高频考点

- RLHF完整流程画图讲解（SFT→RM→PPO三步）
- DPO vs PPO vs GRPO 的核心区别和适用场景
- GRPO为什么能去掉critic？advantage怎么算？
- LoRA的数学原理（低秩分解、哪些层加adapter）
- 奖励模型的过度优化问题（Reward Hacking）怎么缓解
- Scaling Law的三个变量关系、Chinchilla最优配比

## 建议学习顺序

InstructGPT（理解RLHF全貌）→ PPO论文（理解优化器）→ DeepSeek-R1（理解RLVR为什么可行）→ GRPO原文（理解算法细节）→ DPO（对比另一条路线）→ LoRA（微调实践）→ 挑1-2篇近期跟进论文看社区在优化什么方向。
