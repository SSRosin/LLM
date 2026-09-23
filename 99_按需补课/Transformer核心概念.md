# Transformer 核心概念详解

学习 Transformer 核心机制时补充的概念笔记。

---

## Feed-Forward Network（前馈网络 / FFN）

Transformer每一层中，紧跟在Attention之后的**两层MLP**，对每个token独立计算：

```
FFN(x) = W₂ · 激活函数(W₁ · x + b₁) + b₂
```

- `W₁`：升维（d_model → 4×d_model），如 4096 → 16384
- 激活函数：现代LLM用 SwiGLU（不是 ReLU）
- `W₂`：降维（4×d_model → d_model）

**直觉**：Attention = token之间的信息交换（全局），FFN = 每个token独立做非线性变换（局部）。类比：Attention是"开会讨论"，FFN是"各自消化思考"。

**为什么先升维再降维？** 高维空间里做非线性变换更容易分离特征（表达能力更强），降维回来是为了控制参数量并保持维度一致。

---

## Pre-LN vs Post-LN

LN = LayerNorm（层归一化）：把向量的均值拉到0、方差拉到1。

### Post-LN（原始论文2017）
```
x → Attention → +x → LayerNorm → FFN → +x → LayerNorm
```
先做计算，再Norm。深层网络训练不稳定。

### Pre-LN（现代标准）
```
x → LayerNorm → Attention → +x → LayerNorm → FFN → +x
```
先Norm，再计算。残差主路径无非线性操作，梯度回传顺畅，训练稳定。

**记忆**：Pre-LN = 先规范再计算，所有现代大模型（GPT-4/Llama/DeepSeek）都用这个。

---

## RoPE（旋转位置编码，Rotary Position Embedding）

### 解决什么问题
Attention本身不感知token顺序，需要人为注入位置信息。

### 核心做法
把Q和K向量中每对相邻维度（第0-1维、第2-3维...）看成2D平面坐标，按token位置 m 旋转角度 mθ：

```
[q₀, q₁] → [q₀·cos(mθ) - q₁·sin(mθ),  q₀·sin(mθ) + q₁·cos(mθ)]
```

不同维度对用不同频率θ（低维变化快、高维变化慢）。

### 关键性质
位置m的Q和位置n的K做点积时，旋转效果自动变成角度差 (m-n)θ → attention score只取决于**相对位置**。

### 为什么好
- 天然编码相对位置（"隔3个词"比"在第47个位置"更有语义意义）
- 支持长度外推（训练4K，推理可扩展更长）
- Llama/Qwen/DeepSeek/GLM 全部采用

**直觉记忆**：RoPE = 用旋转角度编码位置，让attention自动感知相对距离。

---

## 因果掩码（Causal Mask）

### 解决什么问题
语言生成是自回归的——第3个token生成时，第4、5、6...个token还不存在，不能偷看未来。

### 做法
在 Q·K 分数矩阵的上三角部分设为 -∞：

```
加掩码后：
[0.5   -∞    -∞    -∞ ]    ← token 1 只看自己
[0.1   0.7   -∞    -∞ ]    ← token 2 看 1,2
[0.3   0.2   0.9   -∞ ]    ← token 3 看 1,2,3
[0.4   0.6   0.1   0.8]    ← token 4 看 1,2,3,4
```

-∞ 经过 softmax 变成 0 权重 → 完全忽略未来位置。

### 训练 vs 推理
- **训练时**：所有位置并行计算（掩码是固定矩阵操作，不需要逐步来）→ Transformer训练快的根本原因
- **推理时**：逐token生成（每步依赖上一步输出），用KV Cache避免重复计算

---

## 学习资源

| 资源 | 链接 | 形式 | 时长 |
|---|---|---|---|
| 3Blue1Brown · Attention机制 | https://www.youtube.com/watch?v=eMlx5fFNoYc | 视频 | ~26min |
| 3Blue1Brown · Transformer演讲版 | https://www.youtube.com/watch?v=KJtZARuO3JY | 视频 | ~50min |
| Andrej Karpathy · Let's build GPT | https://www.youtube.com/watch?v=kCc8FmEb1nY | 视频+代码 | ~2h |
| Jay Alammar · Illustrated Transformer | https://jalammar.github.io/illustrated-transformer/ | 图文博客 | ~20min |
| Jay Alammar · Illustrated GPT-2 | https://jalammar.github.io/illustrated-gpt2/ | 图文博客 | ~15min |

**建议顺序**：Illustrated Transformer（图文）→ 3B1B Attention视频 → Karpathy手写GPT
