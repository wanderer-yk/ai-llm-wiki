---
type: concept
title: Harness 工程三部曲演进论
tags: [harness-engineering, prompt-engineering, context-engineering, 上下文窗口]
related: [harness-engineering, harness四要素, 上下文工程, LLM问答黑箱论, function-calling大地基论]
sources: ["[202604220830]AI实践基于SpringAI从0到1构建AIAgent.html"]
created: 2026-10-10
updated: 2026-10-10
---
# Harness 工程三部曲演进论

Harness 工程三部曲演进论是[[觖弦]]在《AI实践｜基于 Spring AI 从0到1构建 AI Agent》结尾感言中提出的判断：从 Prompt Engineering 到 Context Engineering 再到 Harness Engineering 的演进，三者本质解决的是同一个问题——**「有限的上下文窗口中该放什么内容」**。

> 「从 Prompt Engineering 到 Context Engineering 到 Harness Engineering 本质解决的就是『有限的上下文窗口中该放什么内容』」

## 与爱奇艺 Harness Engineering 的独立趋同

这是 Harness Engineering 概念在本 wiki 中的**第 5 次出现**，且首次获得「演进链」表述。爱奇艺数据库团队的 [[harness-engineering]] 从后端工程化视角提出五要素（任务入口、执行依据、工具边界、验证反馈、结果记录）约束 agent 行为；本文则从「上下文窗口内容取舍」的视角将 Harness 定位为 Prompt/Context Engineering 的演进终点。两个独立来源对同一概念的趋同表明：**Harness 的本质可以统一理解为「决定上下文窗口中放什么的工程化体系」**——爱奇艺回答「放哪些工程要素」，觖弦回答「放的内容如何随范式演进」。另见 [[harness四要素]]（侑夕）。

## 在本文中的落点

- [[LLM问答黑箱论]]：LLM 只有「你问，我答」，工程化即调整输入——三部曲的逻辑起点
- [[上下文窗口四要素]]：系统提示词、工具定义、历史对话、参考文档——三部曲所「放」内容的具体枚举
- [[function-calling大地基论]]：Function Calling 是 Harness 的大地基——Harness 的工程地基
- [[上下文工程]]（马上消费）：同以「上下文窗口放什么」为核心命题的平行表述

## 研究价值

三部曲为散落在各团队文章中的 Harness 概念提供了统一的时间轴解释，是潜在的跨源 synthesis 候选：Prompt（单轮指令）→ Context（结构化上下文组装）→ Harness（工具+验证+记忆的完整工程包裹层）。