---
type: concept
title: ReAct 推理模块
tags: [ReAct, 推理规划, 问题排查]
related: [问题排查场景方案, ReAct双引擎场景适配, 意图路由, plan-and-execute模式, react-agent, agent-loop, 结构化输出]
created: 2026-10-12
updated: 2026-10-12
sources: ["[202603230830]拒绝重复造轮子抽象80工作场景打造可复用的AI助手工厂.html"]
---
# ReAct 推理模块

ReAct 推理模块是问题排查场景方案（[[问题排查场景方案]]）中的智能决策代理（Agent）：采用 ReAct（Reason + Act）模式进行多轮推理与工具协调，核心约束是**"只思考，不执行"**——绝不自行执行任何工具调用，只输出推理结论交由执行引擎执行。

## 组装给大模型的输入五要素

- IntentResult：来自意图识别 Agent（[[IntentResult模型]]）
- 原始query：用户的原始输入请求（需作为**最终结果校验的基准**）
- tool_results：本轮已执行的工具调用结果
- 历史对话：用户与平台的过往交互记录
- mcp工具池：平台注册的工具信息

## 执行约束四条

1. 输出结构化模型 ToolExecutionChain（字段结构文中未给出，可与 IntentResult 七字段对照）
2. 只思考不执行
3. 多轮迭代基于历史推理
4. 保持严谨、安全性

## 关联

- "规划与执行分离"与 [[plan-and-execute模式]]、[[react-agent]]、[[agent-loop]] 的通用 Agent 循环同源，但本模块以 plan-only 为鲜明特征。
- 结构化输出（ToolExecutionChain）与 [[结构化输出]] 机制一致。
- 执行侧选型见 [[ReAct双引擎场景适配]]。
