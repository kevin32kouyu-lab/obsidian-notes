
### 1.你的agent用的什么框架？是你自己搭的吗？

我的项目没有使用 LangChain、AutoGen 这类现成的 Agent 框架，而是自己在后端搭了一套轻量级的多智能体流程。

具体来说，后端用的是 **Python + FastAPI**。我自己封装了 AgentCoordinator 作为中心调度器，用来按顺序调度不同角色的 agent，比如 Intake、Planner、Data、Evidence、Bull、Bear、Arbiter、Report 和 Validator。每个 agent 负责一个明确步骤，完成后把结构化结果交给下一个 agent。

同时，我也封装了一个简单的工具调用层，包括 ToolRegistry 和 ToolRunner。它的作用是控制 agent 能调用哪些工具，比如市场数据工具、RAG 证据检索工具、报告生成工具和校验工具，并记录工具调用过程、耗时、错误和权限信息。

### 2.你的agent调用了哪些工具？比较细致地介绍有哪几类

### 3.单一agent有没有遇到上下文超限的问题
### 5.怎么解决幻觉、上下文超限、token爆炸的问题？

### 6.你的agent怎么解决上述问题？

### 7.你的PDF导出怎么实现的？是一次性导出的还是分模块导出的？
### 8.用了哪些技术栈？是用的python吗？是不是用了vibe coding工具？
