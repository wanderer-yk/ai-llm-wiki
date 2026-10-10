---
type: concept
title: PromptMode 三级模式
tags: [openclaw, prompt-engineering, system-prompt]
related: [openclaw系统提示词23模块, openclaw, system-prompt动态组装机制, prompt-context-harness三阶段, context-window三段构成]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践.html"]
---

# PromptMode 三级模式

PromptMode 三级模式是 [[openclaw]] 对 System Prompt 加载范围的三档分级机制：full（完整模式）、minimal（精简模式）与 none（极简模式）。其存在动因是上下文窗口有限——不同场景对 token 预算要求不同，主 Agent 对话需要全量上下文，子 Agent 只需核心模块，极简场景甚至只需身份标识。

| 模式 | 适用场景 | 加载范围 |
|------|---------|---------|
| full（完整模式） | 主 Agent 与用户直接对话 | 所有模块全部加载 |
| minimal（精简模式） | 子 Agent 执行独立任务 | 仅核心模块（工具、工作区、运行时信息） |
| none（极简模式） | 极简场景 | 基本只有一行身份标识 |

none 模式下仍保留身份标识模块（"You are OpenClaw, a personal AI assistant."）与运行时信息模块。PromptMode 与加载条件矩阵共同决定 [[openclaw系统提示词23模块|23 个模块]] 哪些被拼装进最终 System Prompt，体现"按场景区分 token 预算"的 [[prompt极简主义]] 思路，并与 Claude Code 的 [[system-prompt动态组装机制]] 形成跨框架印证。

来源：[[sources/[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践|[202604130830] 深度解析 OpenClaw]]。
