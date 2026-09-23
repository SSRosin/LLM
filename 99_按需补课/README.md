# 按需补课

这个文件夹不需要提前填充。随着你学习主线论文过程中遇到不懂的概念，在这里记录并展开。

## 使用方式

遇到不理解的数学/概念时：
1. 先记一条"待补清单"（下面的表格）
2. 碎片时间找资源看一下直觉解释
3. 周末集中时间做推导/写笔记
4. 笔记就存在这个文件夹下，文件名格式：`知识点名称.md`

## 待补清单

| 知识点 | 从哪里遇到的 | 状态 | 优先级 |
|---|---|---|---|
| 信息熵 / 交叉熵 | Loss函数理解、3B1B Cross-Entropy视频 | ✅ 已通过视频+对话理解 | 高 |
| Softmax 函数 | Attention权重归一化 | ✅ 已理解 | 高 |
| 矩阵乘法与转置 | QK^T 计算、多头拼接 | ✅ 已通过Gemini对话深入 | 高 |
| 低秩分解 | W_V降维/W_O升维 | ✅ 已通过Gemini对话理解 | 中 |
| 反向传播 / 链式法则 | 3B1B Ch3-4、GPT对话 | ✅ 已理解核心直觉 | 高 |
| 梯度下降 | 3B1B Ch2、GPT对话 | ✅ 已理解 | 高 |
| 激活函数(Sigmoid/ReLU/GELU/SwiGLU) | GPT对话 | ✅ 已理解各自特点和选型 | 中 |
| 策略梯度 Policy Gradient | 读InstructGPT/PPO时 | 待补 | 高 |
| KL散度 | RLHF里的KL约束 | 待补 | 高 |
| Importance Sampling | PPO的Clipping推导 | 待补 | 中 |
| 贝叶斯定理 | 概率模型基础 | 待补 | 低 |
| 矩阵分解（SVD） | LoRA的低秩分解 | 待补 | 中 |

> 上面这几条是根据你的学习路线预测的高概率需要补的点，实际遇到时再标记为"进行中/已完成"。

## 推荐补课资源

| 类别 | 资源 | 特点 |
|---|---|---|
| RL基础 | OpenAI Spinning Up | 代码+讲解一体，直接能跑 |
| 线代直觉 | 3Blue1Brown "Essence of Linear Algebra" | 最好的可视化，30min建立直觉 |
| 概率统计 | StatQuest YouTube | 每个概念5-10min讲清楚 |
| 优化方法 | Sebastian Ruder "An overview of gradient descent optimization algorithms" | 一篇博客讲清所有优化器 |
| 信息论 | Chris Olah "Visual Information Theory" | 用可视化讲清熵/KL/互信息 |
