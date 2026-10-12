---
type: concept
title: ReAct 五要素
tags: [ReAct, agent实现, 提示词工程, lynxe]
related: [react-agent, 思考内化论, agent-loop, lynxe菱科斯原-jmanus, 沈询, agent框架三要素]
created: 2026-10-12
updated: 2026-10-12
sources: ["[202602240830]AIAgent系列什么是ReActAgent.html"]
---
# ReAct 五要素

ReAct 五要素是 [[沈询]] 基于 Lynxe 框架实战对 ReAct Agent 实现的结构化拆解：一次完整的 ReAct 轮次由五个要素协同完成——**历史上下文、当前环境信息、LLM Thinking、toolcall、观察结果**。它是 [[react-agent]] 范式在工程层面的核心框架。

## 要素清单

1. **历史上下文（History）**：ReAct 维护的对话历史，记录之前所有的思考、行动和观察。LLM 做决策时可参考之前发生了什么，避免重复操作或走错路。
2. **当前环境信息（Current Environment Information）**：Agent 在当前时刻接收到的外部信息，如用户输入、系统状态、待处理数据，作为 LLM 推理的输入。
3. **LLM Thinking（大脑）**：负责推理和决策，根据历史上下文、环境信息和观察结果生成下一步行动计划。思考过程隐藏，最终表现形式就是 toolcall（见 [[思考内化论]]）。
4. **工具/动作（toolcall）（手脚）**：执行具体操作，如搜索、查询 API、读写文件等。每个工具都有明确的输入输出。
5. **观察结果（toolcall 结果）**：每次执行动作后的返回——可能是成功的数据、错误信息或需进一步处理的内容——反馈给 LLM 作为下一轮推理的依据。

## 轮次推进机制

固定提示词模板中仅"当前历史上下文"与"当前环境信息"是占位符字段；**提示词跨轮次不变，仅历史上下文随轮次增长**，由此实现 ReAct 的轮次推进（模板全文见 [[react-agent]] 与来源页）。

## 关联

- 主循环工程实例（含 `最大轮次 = 10` 安全上限）：[[agent-loop]]
- 更宏观的框架要素视角（LLM Call + Tools Call + Context Engineering）：[[agent框架三要素]]
- 提出者：[[沈询]] / [[lynxe菱科斯原-jmanus]]
