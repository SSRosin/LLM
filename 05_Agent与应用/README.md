# Agent与应用

Agent本质上是把前两部分的能力（推理能力，未来可能还有世界模型）包装成一个能自主决策、调用工具、维护记忆的系统。配套流程图见 `Agent智能体ReAct循环.mermaid`（思考-行动-观察循环）。

## 必读框架性论文

**ReAct**
Yao et al., 2022, *ReAct: Synergizing Reasoning and Acting in Language Models*
arXiv: [2210.03629](https://arxiv.org/abs/2210.03629)
最基础也是至今仍被广泛使用的框架：让模型交替生成"思考(Thought)"和"行动(Action)"，行动后获得"观察(Observation)"，再继续思考，形成闭环。几乎所有现代agent框架（LangChain的Agent、AutoGPT等）底层逻辑都是ReAct的变体。这是整个Agent方向的起点，建议第一篇就读这个。

**Reflexion**
Shinn et al., 2023, *Reflexion: Language Agents with Verbal Reinforcement Learning*
arXiv: [2303.11366](https://arxiv.org/abs/2303.11366)
在ReAct基础上加了一层自我反思：任务失败后，让模型生成一段"经验教训"文字存入记忆，下次面对类似任务时先看这段反思。是"记忆驱动的agent自我改进"的早期代表作，注意标题里的"强化学习"是"用语言文字做反思"这种比喻意义上的强化学习，不是传统意义上梯度更新的RL。

**Toolformer**
Schick et al., 2023 (Meta), *Toolformer: Language Models Can Teach Themselves to Use Tools*
arXiv: [2302.04761](https://arxiv.org/abs/2302.04761)
研究模型如何学会自主判断"什么时候该调用工具、调用哪个工具"，是工具调用能力从"靠prompt硬凹"走向"模型自己学会"的关键一步。

## 综述类论文（适合先看，建立全局地图）

- 《The Landscape of Emerging AI Agent Architectures for Reasoning, Planning, and Tool Calling: A Survey》
  arXiv: [2404.11584](https://arxiv.org/abs/2404.11584)
  这个方向引用率很高的综述，把agent架构按推理/规划/工具调用三个维度做了系统梳理，适合作为入门第一篇综述。

- 《AI Agent Systems: Architectures, Applications, and Evaluation》
  arXiv: [2601.01743](https://arxiv.org/abs/2601.01743)
  更新的综述，把记忆管理、评测方法也纳入了框架，适合了解目前agent评测的痛点（比如怎么评估一个多步骤任务是否真的完成）。

- GitHub仓库 `VoltAgent/awesome-ai-agent-papers`
  https://github.com/VoltAgent/awesome-ai-agent-papers
  持续收录近期的agent论文，按agent工程、记忆、评测、工作流分类，适合当作一个持续更新的论文索引来订阅关注，而不是一次性读完的资料。

## 值得关注的细分方向（暂无公认"标准答案"论文，属于活跃研究前沿）

- **多智能体协作（Multi-Agent）**：让多个LLM分工扮演不同角色互相协作或辩论
- **长期记忆管理**：如何让agent的记忆既不无限膨胀又不丢失关键信息，与检索增强RAG技术高度重叠
- **Agent安全评测**：衡量agent自主执行任务时的风险边界（例如CyberGym一类的网络安全基准）

## 建议学习顺序

ReAct（打好基础框架）→ Toolformer（理解工具调用怎么学出来）→ Reflexion（理解记忆和自我改进）→ 挑一篇综述建立全局地图 → 自己搭一个完整Agent跑通 → 按兴趣深入多智能体协作或记忆管理方向。

## 还需要覆盖（后续补充）

- **RAG**（检索增强生成）：Embedding选型→向量数据库→检索策略→重排→生成
- **Function Calling / Tool Use**：OpenAI/Claude的工具调用协议与实现
- **多智能体框架**：AutoGen、CrewAI、LangGraph等的对比
- **Memory系统设计**：短期（上下文窗口）vs 长期（向量检索+摘要）

## 面试高频考点

- ReAct循环的实现：Thought/Action/Observation如何编排？循环退出条件？
- RAG系统设计：如何平衡召回率和精度？Chunk策略？重排模型选择？
- Tool Selection：给定10个工具，模型怎么选？Selection失败怎么容错？
- Memory设计：多轮对话的上下文管理，长期记忆的写入/淘汰策略
- 设计题：设计一个能自动调研+写报告的Agent系统
- 设计题：设计一个多Agent协作的代码审查系统
