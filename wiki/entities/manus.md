---
type: entity
title: Manus
tags: [ai-agent, 产品, c端, monica]
related: [openclaw, codeact架构, 上下文工程]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202604131736]详尽地带你从零开始设计实现一个AIAgent框架.html"]
---
# Manus

[[monica|Monica]] 公司发布的 Agent C 端产品，引领 AI Agent 进入大众视野。在 [[sources/[202604131736]详尽地带你从零开始设计实现一个AIAgent框架|yabohe 的文章]]中作为重要的工业实践案例来源。

## 关键技术决策

- **放弃微调路线**：转而深耕 [[上下文工程]]，2025 年 7 月工程博客详述经验教训
- **不使用 MCP**：首席科学家 Peak 公开表示 "Actually, Manus doesn't use MCP, inspired by [[codeact架构|CodeAct]]"
- **选择 CodeAct 路线**：以可执行代码统一行动空间

## 行业影响

- 验证了"文件系统作为上下文"的行业共识
- 验证了"代码作为通用问题解决手段"的理念
- Anthropic 后续受其启发：2025 年 10 月推出 Claude Skills，2025 年 11 月提出将 MCP 服务器作为代码 API