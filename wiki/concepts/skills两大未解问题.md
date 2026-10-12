---
type: concept
title: Skills 两大未解问题
tags: [skills, 局限, claude, 接驳]
related: [一切都是函数, lynxe, skills-sub-agent包装论, skills四阶段运行流程, mcp与skills竞争关系论, 沈询]
created: 2026-10-12
updated: 2026-10-12
sources: ["[202602260830]AIAgent系列深入解析FunctionCallingMCP和Skills的本质差异与最佳实践.html"]
---
# Skills 两大未解问题

Skills 两大未解问题是[[沈询]]在 Lynxe 实践总结中对 Skills（Anthropic Claude 机制）局限的判断：

1. **description 字段不够结构化**：复杂任务下，纯文本描述的不确定性被放大
2. **只能聊天式接驳**：Agent 只能以对话方式接入既有系统，无法融入多步骤表单、实时数据展示、UI 组件交互等真实业务场景——真实系统远不止对话框一种输入方式

## 文内"问题→解法"闭环

[[lynxe|Lynxe]] Func-Agent 能力清单的第②③④项（返回结构化结果方便与现有系统集成 / 支持多种调用方式不限聊天界面 / 与现有业务逻辑、表单、API 无缝对接）逐条正面回应问题②，使 Func-Agent 在文内被定位为 Skills 缺陷的解法（见 [[一切都是函数]]）。这一"Skills 缺陷→Func-Agent 解法"的论证闭环是文章收尾的核心逻辑。

## 开放部分

问题①（description 不结构化）在本文范围内未给出对应解法，仍为开放问题——这也构成 Func-Agent 路线之外继续探索 Skills 改进的空间。

## 关联

- 与 [[mcp与skills竞争关系论]] 的关系：两大未解问题是作者从 Skills 阵营内部视角给出的批评，为其"竞争大于互补"与函数化路线主张提供支撑
- 与腾讯 [[生产级skill]]（Skills 作为生产级 SOP 封装）的互补定位形成对照：见 [[skills与mcp是互补还是竞争]]
