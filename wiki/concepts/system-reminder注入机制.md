---
type: concept
title: system-reminder注入机制
created: 2026-10-10
updated: 2026-10-10
tags: [claude-code, context-engineering, prompt-injection-defense, harness-engineering]
related: [system-prompt动态组装机制, claude-md四路径分层, 六步交互流水线, verification-agent五大设计哲学, 钩子结构化JSON干预三能力, agent-control-plane, 双端上下文注入]
sources: ["[202604200830]深度解析ClaudeCode在PromptContextHarness的设计与实践.html"]
---
# system-reminder注入机制

**system-reminder 注入机制**是 [[claude-code]] 将系统级元信息统一包裹在 `<system-reminder>` 标签中注入对话的机制，显式隔离「用户输入」与「系统指令」，使上下文组装从"艺术活"变为标准化、可复用的"工程流水线"，降低格式混乱导致的幻觉风险。

## 双端注入

- `prependUserContext()`：将 CLAUDE.md 内容 + 当前日期包裹为 `<system-reminder>` 消息插入用户消息列表**最前**（带强调语 "Be sure to adhere to these instructions"，被当作高优先级用户指令对待）。
- `appendSystemContext()`：将 Git 状态快照（注明 snapshot in time）追加到 System Prompt **末尾**。

## 全生命周期应用（wrapInSystemReminder）

`utils/messages.ts` 的 `wrapInSystemReminder` 统一包裹，四类场景：①用户上下文初始化（CLAUDE.md/日期）；②工具结果反馈；③Hook 反馈（静态模块声明「hooks 反馈视为用户反馈」）；④周期性任务与能力描述（Skill List / Agent List）。`normalizeMessagesForAPI` 强制所有消息经过包裹处理。注入时机固定在 [[六步交互流水线]] 的第 1 步「消息预处理」；[[verification-agent五大设计哲学]] 中对 Verification Agent 反复注入 CRITICAL 提醒是"贯穿全生命周期"的典型实例。
