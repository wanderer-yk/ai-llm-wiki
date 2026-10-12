---
type: comparison
title: "Function Calling、MCP 与 Skills 本质差异对比"
tags: [function-calling, mcp, skills, 对比]
related: [function-calling桥梁论, mcp薄协议转换层, skills-sub-agent包装论, skills四阶段运行流程, 任务流程定义困难问题, mcp与skills竞争关系论, skills两大未解问题, 一切都是函数, skills与mcp是互补还是竞争, 三大武器库, lynxe, 沈询]
created: 2026-10-12
updated: 2026-10-12
sources: ["[202602260830]AIAgent系列深入解析FunctionCallingMCP和Skills的本质差异与最佳实践.html"]
---
# Function Calling、MCP 与 Skills 本质差异对比

本比较页基于[[沈询]]《AI Agent 系列》第三篇（[[sources/[202602260830]AIAgent系列深入解析FunctionCallingMCP和Skills的本质差异与最佳实践|来源页]]）的正文论证构建。**说明**：原文末尾附有"三者的总结性对比表"，但仅以图片形式存在（data-imgfileid=100074561），文本不可提取；下表为依据正文论证的重构，非原文表格还原。

## 对比表（基于正文论证重构）

| 维度 | Function Calling | MCP | Skills |
|---|---|---|---|
| 本质定位 | 非结构化需求↔结构化函数调用之间的桥梁（[[function-calling桥梁论]]） | FC 之上很薄的协议转换层：FC + JSON-RPC 转换 + description 打包（[[mcp薄协议转换层]]） | sub-agent 的包装：文字化流程替代代码型 API 串接（[[skills-sub-agent包装论]]） |
| 层次关系 | 基础能力，后两者存在的前提 | 建立在 FC 之上 | 建立在 FC 之上，完全依赖 FC |
| 解决的问题 | LLM 与只处理结构化信息的既有系统对接 | 纯 FC 下每个系统单独集成、工具集成成本太高 | 任务流程定义困难（[[任务流程定义困难问题]]） |
| 流程载体 | —（单次调用机制） | MCP Server 内代码型 API 串接 | SKILL.md 文字化流程 |
| 接驳方式 | 结构化 JSON（tool_calls） | JSON+HTTP（JSON-RPC） | 聊天式接驳（未解问题）+ bash/read_file 经 FC |
| 优势 | 可预测、可靠（结构化输出） | 标准化接驳协议 | 适应性增强；长流程任务成功率更高（自报口径） |
| 局限 | 任务流程定义困难 | 与 FC 共有两难；流程仍需在 Server 内写代码 | 不可能 100% 准确；description 不结构化；聊天式接驳（[[skills两大未解问题]]） |
| 运行机制 | 四步工具调用循环 | LLM→FC→MCP Client→MCP Server→既有系统 | 初始化→发现→加载→执行（[[skills四阶段运行流程]]） |

## 竞争关系（本文核心主张）

MCP 与 Skills 解决**同一问题**（整合多既有系统实现复杂多步任务），采用**不同方法**（协议转换 Server + 代码型流程 vs 直接命令行 + 文字化流程），因此**选此失彼**：选 Skills 则无需在 MCP Server 写串接代码，反之 Skills 价值降低。详见 [[mcp与skills竞争关系论]]。该主张与腾讯 [[三大武器库]] 的互补三层定位构成跨源张力，裁决见 [[skills与mcp是互补还是竞争]]。

## 第三条路线：Func-Agent

文章以 [[lynxe|Lynxe]] 的 Func-Agent 哲学收尾（[[一切都是函数]]）：四项函数化能力（结构化参数输入/结构化结果返回/多种调用方式/与业务逻辑·表单·API 无缝对接）逐条回应 Skills 未解问题之"聊天式接驳"，构成文内"问题→解法"闭环——即函数化路线试图同时保留 Agent 的不确定性处理优势和 MCP 式的结构化集成能力。
