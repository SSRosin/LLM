# 职业发展

把学习成果转化为职业竞争力。覆盖求职面试、在职晋升、技术影响力建设三个方面。

## 目标岗位能力矩阵

你的目标是"全栈AI工程师→技术leader"，需要在以下维度建立能力：

| 维度 | 初级（能用） | 中级（能优化） | 高级（能设计） |
|---|---|---|---|
| **模型训练** | 能用trl/transformers跑通微调 | 能设计数据配比、调RLHF/DPO/GRPO流程 | 能从头设计训练方案、评估trade-off |
| **推理部署** | 能用vLLM部署模型 | 能做量化+优化达到生产标准延迟 | 能设计多模型路由、成本优化架构 |
| **Agent/应用** | 能用LangChain搭简单agent | 能设计RAG pipeline + 多步骤agent | 能设计复杂多智能体协作系统 |
| **技术判断** | 能读懂新论文 | 能评估一项技术的落地价值 | 能做技术选型并推动组织采纳 |
| **技术影响力** | 能写内部技术文档 | 能做部门级技术分享 | 能做公司级/社区级技术布道 |

当前建议先把"模型训练"和"推理部署"推到中级，"Agent/应用"推到初级→中级，同时建立技术判断能力。

## 面试准备框架

基于2026年大模型岗位面试趋势，核心考察四个模块：

### 模块一：Transformer与架构原理（20%）
- Self-Attention计算过程、复杂度分析
- RoPE位置编码原理
- MoE路由机制与负载均衡
- MHA/MQA/GQA对比

### 模块二：训练全景与后训练（30%）
- 预训练→SFT→RLHF完整流程
- RLHF vs DPO vs GRPO 对比（优劣、适用场景）
- LoRA/QLoRA微调原理与实践
- Scaling Law与数据工程
- 评测方法（Perplexity、事实一致性、人工评估）

### 模块三：推理与部署工程（30%）
- KV Cache原理、PagedAttention
- 量化方法（INT4/INT8/AWQ/GPTQ/FP8）
- Continuous Batching vs Static Batching
- 投机解码原理
- FlashAttention的IO-Awareness思想
- vLLM / TensorRT-LLM / SGLang对比

### 模块四：Agent与系统设计（20%）
- ReAct/Reflexion/Plan-and-Execute架构
- RAG pipeline设计（Embedding→检索→重排→生成）
- Tool Selection机制设计
- 多智能体协作架构
- Memory系统设计（短期/长期/向量检索）

### 系统设计类面试题（高频）
- 设计一个支持100 QPS的LLM推理服务
- 设计一个多轮对话系统的记忆管理方案
- 设计一个Agent的工具选择与容错机制
- 设计一个混合检索的RAG系统（稠密+稀疏+重排）
- 给定预算约束，如何选择模型（大vs小+多次采样）

## 晋升述职素材积累

每完成一个学习+实践周期，都应该产出可量化的成果：

| 类型 | 怎么积累 | 怎么用 |
|---|---|---|
| 技术博客 | 每月至少1篇，把学习笔记整理为公开文章 | 面试时展示技术深度 + 社区影响力 |
| 内部分享 | 把论文精读做成15min分享 | 晋升述职时的"技术引领"证据 |
| 落地项目 | 把学到的技术推动在工作中应用 | 晋升答辩的核心素材 |
| 开源贡献 | 给vLLM/LangChain等项目提PR或写插件 | 简历亮点 + 圈内认可 |

## 时间线建议

| 时间 | 里程碑 | 职业动作 |
|---|---|---|
| 1-2月 | 搭好认知框架，能做内部技术分享 | 在团队内建立"关注LLM前沿"的标签 |
| 3-4月 | 跑通训练+部署实验 | 写2-3篇技术博客；找一个工作中能用的点推动落地 |
| 5-6月 | Agent项目完成 | 把项目做成可展示的demo；准备晋升述职材料 |
| 6月+ | 持续跟进前沿，建立技术判断力 | 可以开始看更高级别的机会；或内部承担更大技术scope |

## 推荐面试/学习资源

- [wdndev/llm_interview_note](https://github.com/wdndev/llm_interview_note) — 中文LLM面试笔记，按模块分类，覆盖全面
- [小林面试笔记·大模型工程](https://xiaolinnote.com/ai/llm/llm_info.html) — 结构清晰的面试题解析
- DataCamp "Top 36 LLM Interview Questions" — 英文版面试题（偏应用侧）
- [Exponent AI Engineer面试题](https://www.tryexponent.com/blog/ai-engineer-interview-questions) — 系统设计类题目（Anthropic/Google等公司真题）
