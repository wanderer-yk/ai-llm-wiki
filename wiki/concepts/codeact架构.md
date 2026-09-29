---
type: concept
title: CodeAct 架构
tags: [ai-agent, 行动空间, codeact, manus, anthropic]
related: [agent-loop, agent框架三要素, react-agent, manus]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202604131736]详尽地带你从零开始设计实现一个AIAgent框架.html"]
---
# CodeAct 架构

由 UIUC 王星尧提出的 Agent 设计架构，核心理念是通过**生成可执行 Python 代码**统一 LLM Agent 的行动空间。

## 核心思想

传统 Agent 框架将不同类型的操作定义为不同的工具（搜索、计算、文件操作等），导致行动空间碎片化。CodeAct 将所有操作统一为代码生成——Agent 通过编写和执行 Python 代码来完成任何任务。

## 影响链条

1. **Manus** 的核心灵感来源。Manus 首席科学家 Peak 明确表示："Actually, Manus doesn't use MCP, inspired by CodeAct"
2. **Anthropic 验证**：2025 年 10 月推出 Claude Skills；2025 年 11 月博客提出将 MCP 服务器作为代码 API（而非直接工具调用），本质上是 CodeAct 与 MCP 的融合

## 与 Agent Loop 的关系

在 [[agent-loop|Agent Loop]] 的 While 循环中，CodeAct 改变了工具调用的形态——不再是多个独立工具的逐一调用，而是生成一段代码统一执行多步操作。yabohe 的极简框架实践仍采用传统多工具注册方式，但文章指出 CodeAct 代表了工具调用的演进方向。

## 在 Agent 框架三要素中的位置

CodeAct 属于 [[agent框架三要素|三要素]] 中"Tools Call"的演进脉络（Function Call → MCP → Skills/CodeAct），代表了从碎片化工具到统一代码行动空间的范式转变。