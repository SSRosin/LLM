# 基础与架构

LLM的所有上层应用（训练、推理、Agent等）都建立在Transformer架构之上。这个模块不需要一次性学完，而是随着其他方向的深入，反复回来加深理解。

## 核心知识点

### 必须掌握（所有方向的前置）

**Attention Is All You Need**
Vaswani et al., 2017
arXiv: [1706.03762](https://arxiv.org/abs/1706.03762)
原始Transformer论文。重点理解：多头注意力的计算过程（Q/K/V矩阵乘法）、位置编码为什么需要、Encoder-Decoder结构。现代LLM基本只用Decoder-only架构，但理解原文有助于理解为什么做了这个简化。

**关键概念清单**（遇到时在 `99_按需补课/` 里展开）：
- 自注意力（Self-Attention）的计算复杂度 O(n²d)
- 位置编码：绝对位置编码 → RoPE（旋转位置编码，现在主流LLM的标准选择）
- Layer Normalization（Pre-LN vs Post-LN）
- Residual Connection 为什么对深层网络至关重要
- Tokenization：BPE / SentencePiece / Byte-level

### 进阶架构知识（第二阶段再深入）

**MoE（Mixture of Experts）混合专家**
现在几乎所有大参数模型（Qwen3.8-Max 2.4T、Kimi K3 2.8T、DeepSeek-V3等）都采用MoE架构。核心idea：不是所有参数都对每个token激活，而是通过一个路由器（Router）只激活一部分"专家"子网络，这样总参数量大但每次前向推理的计算量（FLOPs）可控。

**KDA（Kimi Delta Attention）/ 混合线性注意力**
Kimi K3采用的架构，在部分层用线性注意力替换标准注意力以降低长序列的计算成本，同时保留部分标准注意力层维持质量。这代表了"让百万token上下文实际可用"的工程方向。

**GQA（Grouped Query Attention）**
Llama 2/3 采用的注意力变体，介于MHA（多头注意力）和MQA（多查询注意力）之间。理解它对推理时KV Cache大小的影响，这对理解 `03_推理与部署/` 的内容很重要。

## 建议学习方式

不要一开始就读原始Transformer论文的全文——公式多且2017年的写法不够友好。建议：

1. 先看3Blue1Brown的可视化视频（~30min），建立直觉
2. 再看Jay Alammar的博客 "The Illustrated Transformer"（图解Transformer），把每一步的矩阵运算搞清楚
3. 然后回来看原始论文，这时候会轻松很多
4. 位置编码部分等到看具体模型（如Llama架构）时再补RoPE

## 面试高频考点

- 手写Self-Attention的计算过程（Q/K/V → scores → softmax → weighted sum）
- 解释为什么Attention的scale factor是 1/√d_k
- MHA vs MQA vs GQA 的区别和各自的trade-off
- RoPE的核心思想（用旋转矩阵编码相对位置）
- MoE的路由机制、负载均衡问题
