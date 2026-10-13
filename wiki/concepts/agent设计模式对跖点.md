---
type: concept
title: Agent 与设计模式的对跖点
tags: [agent, 设计模式, 范式映射, react-agent, mcp, hitl]
related: [human-in-the-loop, react-agent, mcp, hitl通用工具设计, 工具树按需加载, 工厂模式自适应agent生成, 道法术分析框架, agent-loop]
created: 2026-10-13
updated: 2026-10-13
sources: ["[202601080830]从HITLHumanInTheLoop实践出发看Agent与设计模式的对跖点.html"]
---
# Agent 与设计模式的对跖点

**Agent 与设计模式的对跖点**是奕寰在《从HITL(Human In The Loop) 实践出发看Agent与设计模式的对跖点》中提出的中心论题：Agent 的每一项机制都能在经典工程设计/设计模式中找到对应物——“对跖点”意为两者互为地球两端的对应存在。该论题的主张是新 Agent 范式并非无根之木，而是经典工程底座在新介质上的重演；作者由此展望**设计模式驱动 Agent 进化论**：把映射关系告诉 LLM，让 LLM 推理 Agent 还能如何进化，并断言“Agent 目前的玩法只是冰山一角”。

## 八组映射

| Agent 机制 | 对跖点（工程设计/设计模式） | 作者论据 |
|---|---|---|
| HITL | Hook、拦截（Intercept）、切面（SpringBoot AOP） | 多轮对话有冗余；工程中用钩子解耦充分利用 Context，或切面/拦截对上下文前置加工一次利用；Spring AI Alibaba 亦用 Hook 提供 HITL |
| Dify、LangGraph 流程编排 | 数据结构（图） | BPMS 类产品早已成熟流程编排；Dify 将图结构节点部分替换为 LLM；“图结构+LLM 无疑是初代 Agent 最成功的范式” |
| ReactAgent | 循环 + 责任链 | LLM 一次输入一个输出，重复思考+工具集合选择工具；循环结束条件交由 LLM 自行判断 |
| MCP | 防腐层、适配器模式 | OpenAI FunctionCall 缺标准调用协议与客户端，需 if-else 判断 RPC/HTTP 调用姿势；MCP 屏蔽调用差异 |
| 按需加载工具（Aone Copilot 工具树） | 懒加载、循环依赖（SpringBoot Bean 加载） | 首次仅加载根节点，运行时按依赖关系按需增加工具到循环调用提示词 |
| 记忆（Mem0 / Mem0-MCP / Spring AI） | 多级缓存 | Mem0 云端库+本地缓存归纳压缩；Mem0-MCP 本地内存短期记忆；Spring AI 数据库+本地内存多级缓存 |
| Skills | 简单工厂模式 | 事先利用父类原子能力创作的专有任务技能文档，每个都可看作一个工厂 |
| （展望）主 Agent 动态生成专业子 Agent | 工厂模式多态（作者称“复杂工厂模式”） | 一次对话中自适应生成解决本次任务所需的 Agent 实例，仅负责本次任务领域/规划子节点 |

## 关键论证要点

- **HITL 组**是全文论证弧线的起点：多轮对话的冗余（第二次对话重新理解提示词、重新执行前置处理）由工程上的钩子/拦截/切面手段化解，作者实现自白为 `if (toolName.equals("HITL"))` 的拦截判断，详见 [[hitl通用工具设计]] 与 [[human-in-the-loop]]。
- **ReactAgent 组**：循环+责任链组合定性，终止条件交 LLM 判断；作者反事实推断“若一开始从设计模式出发设计 Agent，ReAct 模式会出现的更早也说不定”（与 [[agent-loop]] 互补）。
- **MCP 组**：OpenAI FunctionCall 先发却因缺标准调用协议与客户端被 Anthropic 的 MCP“偷了家”（作者戏称）；与沈询文章簇的 [[function-calling桥梁论]]、[[mcp薄协议转换层]] 形成视角互补。
- **流程编排组**：图结构+LLM 是初代 Agent 最成功范式；Dify 类流程编排被本文定位为已落地 Agent 中最可靠模式，但其能力源自 BPMS 时代成熟传统（与有赞生产瓶颈经验存在跨文章张力，见 [[dify]]、[[workflow优先于agent]]）。

## 术语注记

- 作者展望中称“复杂工厂模式”，结合“多态”语境疑指抽象工厂模式；按原文措辞保留并加注。
- functionCall 定义语法 vs xml 流式解析为两个层面，非矛盾。

## 关联

- 与《AI工程vs传统工程道法术中的变与不变》（[202601130830]）同持“新范式根植经典工程底座”立场，可与 [[道法术分析框架]] 关联。
- 第八组映射的动态子 Agent 生成提案展开见 [[工厂模式自适应agent生成]]；按需工具加载的两种机制对照见 [[工具树按需加载]] 与 [[渐进式工具加载]]。