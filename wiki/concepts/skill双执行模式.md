---
type: concept
title: Skill 双执行模式
created: 2026-10-09
updated: 2026-10-09
tags: [claude-code, skills, 执行隔离]
related: [claude-code, skill提示词注入本质, skills真正价值三场景, rules与skills等价论]
sources: ["[202604151800]读完ClaudeCode源码才发现SkillsMCPRules的区别远没有你想的那么大.html"]
---
# Skill 双执行模式

Skill 双执行模式指 Claude Code 中 Skills 的两种执行方式：**Inline**（默认）与 **Fork**（需显式配置 `context: 'fork'`）。这是 Rules 与 Skills 真正的功能性差异之一——Rules 没有执行隔离能力（[[rules与skills等价论]]）。

## Inline 模式（默认）

Skill 指令包装为 `isMeta: true` 的 user 消息注入主对话历史，`tool_use`/`tool_result` 全部写入主对话；`tool_result` 仅返回标签 `"Launching skill: <name>"`，随后模型按注入的指令执行。

## Fork 模式（`context: 'fork'`）

- Skill 的 `tool_use`/`tool_result` **不写入主对话**，主对话保持干净；
- 拥有独立的文件缓存、独立的权限拒绝记录、独立的 abort 控制；
- 形成**独立执行生命周期**，是长流程多步任务特别适合的模式。

## 价值定位

执行隔离与「模型自主触发」「可发现可分发」并列，构成 Skills 相对手动引用 Rules 不可替代的三大价值场景（[[skills真正价值三场景]]）。Fork 的独立权限跟踪与 abort 控制也使其成为需要中途终止的长任务的唯一选择。
