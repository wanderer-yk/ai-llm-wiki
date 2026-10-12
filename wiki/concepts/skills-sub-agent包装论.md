---
type: concept
title: Skills sub-agent 包装论
tags: [skills, sub-agent, claude, 决策权下放]
related: [skills四阶段运行流程, skills两大未解问题, mcp与skills竞争关系论, 任务流程定义困难问题, 沈询, 生产级skill]
created: 2026-10-12
updated: 2026-10-12
sources: ["[202602260830]AIAgent系列深入解析FunctionCallingMCP和Skills的本质差异与最佳实践.html"]
---
# Skills sub-agent 包装论

Skills sub-agent 包装论是[[沈询]]对 Skills（Anthropic Claude 的新机制）本质的定位：Skills 实际是 sub-agent 的包装——用 SKILL.md 中的文字化流程，替代 MCP Server 函数内以代码写成的 API 串接逻辑。这一提法呼应其系列第二篇"Agent 将决策权完全下放给模型和 Prompt"的核心观点。

## 核心含义

- **决策权下放**：流程编排的控制权从代码转移到"模型 + 文字化流程描述"
- **适应性增强**：SKILL.md 支持更复杂的流程组合，长流程任务成功率更高（自报口径，无量化依据）
- **代价**：文字化流程不可能 100% 准确——灵活性与确定性之间的交换；"成功率更高"与"不可能 100% 准确"两种表述在文内并存，适用条件未界定

## 与 MCP 的竞争含义

选 Skills 则无需在 MCP Server 写串接代码，反之 Skills 的价值降低——这是 [[mcp与skills竞争关系论]] 的直接推论。技术上，Skills 场景中 bash/curl 直接本地调用即可替代 JSON-RPC 封装，说明 MCP 的协议封装在该路径下并非必需。

## 与腾讯 Skills 观念的对照

腾讯 [[生产级skill]] 将 Skills 定位为可复用 SOP 封装层（互补框架内的一层），而本文将 Skills 定位为 sub-agent 包装（竞争框架内的一方）——两种定位的差异是 [[skills与mcp是互补还是竞争]] 查询的核心证据之一。
