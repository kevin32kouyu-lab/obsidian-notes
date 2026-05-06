
### 1.你的agent用的什么框架？是你自己搭的吗？

我的项目没有使用 LangChain、AutoGen 这类现成的 Agent 框架，而是自己在后端搭了一套轻量级的多智能体流程。

具体来说，后端用的是 **Python + FastAPI**。我自己封装了 AgentCoordinator 作为中心调度器，用来按顺序调度不同角色的 agent，比如 Intake、Planner、Data、Evidence、Bull、Bear、Arbiter、Report 和 Validator。每个 agent 负责一个明确步骤，完成后把结构化结果交给下一个 agent。

同时，我也封装了一个简单的工具调用层，包括 ToolRegistry 和 ToolRunner。它的作用是控制 agent 能调用哪些工具，比如市场数据工具、RAG 证据检索工具、报告生成工具和校验工具，并记录工具调用过程、耗时、错误和权限信息。

### 2.你的agent调用了哪些工具？比较细致地介绍有哪几类

严格来说，我这个项目里 Agent 通过 ToolRegistry / ToolRunner 封装了 **5 个核心内部工具**，可以分成 **4 类**来讲。
![[Pasted image 20260507024403.png|697]]
我的 Agent 并不是可以随便调用任意工具，而是通过一个受控的工具系统来调用内部服务。Planner Agent 会先决定后续 Agent 允许使用哪些工具，然后 ToolRunner 负责真正执行，并记录权限、耗时、失败原因和调用结果。
第一类是**数据研究工具**，主要是 market.research_package。它由 Data Agent 调用，负责把用户问题转成可分析的数据包。它底层会整合行情、新闻、SEC 披露、宏观数据、评分和候选股票结果。
第二类是**报告准备工具**，也就是 report.build_package。它不是直接生成最终报告，而是先把分析结果整理成结构化材料，比如用户目标、候选股票、评分表、风险摘要、证据摘要和报告输入。
第三类是**RAG 证据工具**，也就是 rag.attach_evidence。它会把本次研究中的新闻、SEC、评分、宏观信息等写入本地 SQLite 证据库，同时检索和当前问题最相关的证据，生成引用映射，供后续报告使用。
第四类是**报告生成和校验工具**。report.render 负责生成最终投资报告，主要由 Report Agent 调用；report.validate 负责在报告生成后做一致性检查，比如推荐标的是否和评分一致、风险是否覆盖、证据是否过期、数据是否有缺口。

需要注意的是，像 Alpaca、yfinance、SEC EDGAR、Yahoo RSS 这些外部数据源，不是每个 Agent 直接去调用，而是被封装在 market.research_package 背后的数据服务里。这样做的好处是 Agent 只面对少量受控工具，不会乱调用外部接口，整个流程也更容易追踪和校验。
### 3.单一agent有没有遇到上下文超限的问题
### 5.怎么解决幻觉、上下文超限、token爆炸的问题？

### 6.你的agent怎么解决上述问题？

### 7.你的PDF导出怎么实现的？是一次性导出的还是分模块导出的？
### 8.用了哪些技术栈？是用的python吗？是不是用了vibe coding工具？
