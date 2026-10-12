---
type: concept
title: agent系统模块六要素
tags: [agent, 架构, 工程化]
related: [agent开发经验主义, agent胶水层, agent评测四要素, agent-loop]
created: 2026-10-12
updated: 2026-10-12
sources: ["[202603240830]业务逻辑的坍塌当应用层只剩下胶水代码在AIAgent时代我们该构建什么-上.html", "[202603240830]业务逻辑的坍塌当应用层只剩下胶水代码在AIAgent时代我们该构建什么-下.html", "[202603240830]业务逻辑的坍塌当应用层只剩下胶水代码在AIAgent时代我们该构建什么.html"]
---
# agent系统模块六要素

[[韩春]] 从手搓渗透测试 Agent Demo 中提炼的 Agent 开发六个关键点：

| 要素 | 一句话定义 |
|---|---|
| Prompt 工程 | 用不严谨的自然语言表达确定的需求 |
| Context 管理 | 在有限窗口内塞入足够信息 |
| 输出解析 | 从自然语言提取结构化数据 |
| 工具编排 | 把 LLM 决策转换为真实动作 |
| 异常处理 | 大量兜底逻辑 |
| 质量评测 | 判断 LLM 输出质量 |

对应的完整 Agent 系统模块 = RAG + ReAct 循环 + Prompt Engineering + Memory 管理 + Tool 调用 + Multi-Agent 协作六件套。

Demo 层细节：`decision` 的 **thought/action/params 三字段**被作者称为"LLM 和代码世界的纽带"，与 [[结构化输出]]、[[react-agent]] 的 ReAct 循环直接对应。六要素与 [[agent开发经验主义]] 的七大类不确定性清单一一咬合——每个要素内部都藏有经验参数。