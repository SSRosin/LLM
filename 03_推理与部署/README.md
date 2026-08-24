# 模型训练推理系统（怎么把大模型跑起来）

偏系统/工程方向，配套流程图见 `LLM推理加速流程.mermaid`（从prefill到解码的推理管线）。如果重点是算法而非系统工程，可以只看前两篇建立概念，其余按需查阅。

## 推理加速核心论文

**FlashAttention**
Dao et al., 2022, *FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness*
arXiv: [2205.14135](https://arxiv.org/abs/2205.14135)
理解现代Transformer为什么能训练/推理这么快的基础，核心是通过分块计算（tiling）避免把整个注意力矩阵写入显存（HBM），大幅减少显存读写次数。

**PagedAttention / vLLM**
Kwon et al., 2023, *Efficient Memory Management for Large Language Model Serving with PagedAttention*
arXiv: [2309.06180](https://arxiv.org/abs/2309.06180)
解决的是服务端问题：多个请求同时推理时，KV Cache如何像操作系统的虚拟内存一样分页管理，避免显存碎片化，这是目前几乎所有开源推理框架（vLLM、SGLang等）的基础设计。

**投机解码 Speculative Decoding**
Leviathan et al., 2022, *Fast Inference from Transformers via Speculative Decoding*
arXiv: [2211.17192](https://arxiv.org/abs/2211.17192)
用一个小模型先"猜"多个token，大模型一次性并行验证，猜对就省一步，是目前主流的解码加速手段之一。

## 测试时计算扩展（Test-Time Compute）

**Scaling LLM Test-Time Compute Optimally**
Snell et al., 2024
arXiv: [2408.03314](https://arxiv.org/abs/2408.03314)
讨论了一个关键的范式转变：与其一味增大模型参数，不如让模型在推理时"多想一会儿"（更长思维链、多次采样再投票），用推理时的计算量换准确率。这是本轮"推理模型"浪潮（o1、DeepSeek-R1、GLM-5.3等）背后的核心系统假设，建议配合第一部分的RLVR内容一起理解——训练阶段学会长链条推理，推理阶段则通过扩展计算量把这种能力发挥到极致。

## 分布式训练基础设施（进阶，按兴趣深入）

- **Megatron-LM系列论文**（NVIDIA）：了解张量并行、流水线并行的实现思路
- **ZeRO**：Rajbhandari et al., 2020, *ZeRO: Memory Optimizations Toward Training Trillion Parameter Models*（DeepSpeed背后的核心技术），了解优化器状态切分如何降低单卡显存压力

这两块偏基础设施，如果不是做训练系统工程，可以先了解概念再决定是否深入。

## 还需要覆盖（后续补充）

- **量化方法**：INT4/INT8/AWQ/GPTQ/FP8，各自的精度/速度trade-off
- **Continuous Batching**：为什么替代了Static Batching
- **推理框架对比**：vLLM vs TensorRT-LLM vs SGLang vs Triton
- **KV Cache优化**：PagedAttention、Prefix Caching、Token Dropping

## 面试高频考点

- KV Cache的原理：为什么decode阶段不需要重算之前token的K/V？
- PagedAttention如何管理显存？和操作系统虚拟内存的类比
- Continuous Batching vs Static Batching 的区别和优势
- 投机解码的accept/reject机制（为什么数学上不损失精度）
- 量化对模型质量的影响，什么场景选什么量化方案
- 如何估算一个模型需要多少显存？（参数量×精度+KV Cache×batch）
- FlashAttention的核心insight：为什么是IO-bound而不是compute-bound
- 设计题：如何设计一个支持100 QPS、P99 < 2s的LLM推理服务？
