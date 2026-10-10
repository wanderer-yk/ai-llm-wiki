---
type: concept
title: fork-sub-agent机制
created: 2026-10-10
updated: 2026-10-10
tags: [claude-code, subagent, multi-agent, 上下文管理]
related: [六大系统内置AgentTool, 受控子Agent机制, SubAgent记忆隔离, 多agent设计四动机]
sources: ["[202604200830]深度解析ClaudeCode在PromptContextHarness的设计与实践.html"]
---
# fork-sub-agent机制

**fork-sub-agent 机制**是 [[claude-code]] 隐藏的第七个 Agent：主 Agent 的"分身"，与常规子 Agent 不同——它继承完整对话历史，用于在不污染主窗口的前提下并行处理分支任务。

## 四特点

1. **继承完整对话历史**，并**共享 prompt cache**（fork 成本低）。
2. **输出约束**：报告必须以 `Scope:` 开头且 ≤500 字。
3. **防递归**：通过检测 `<fork-boilerplate>` 标签防止 fork 再 fork。
4. **隔离运行**：可在独立 git worktree 中执行，物理隔离文件变更。

## 跨框架对比

与 HermesAgent 的 [[受控子Agent机制]] / [[SubAgent记忆隔离]] 构成子 Agent 设计的两种取向：Fork 强调「同上下文分身 + 输出压缩回流」，受控子 Agent 强调「干净上下文 + 受控交接」；两者共同回答 [[多agent设计四动机]] 中的上下文管理问题。
