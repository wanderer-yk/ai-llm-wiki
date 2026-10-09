---
type: concept
title: messages 注入四通道
created: 2026-10-09
updated: 2026-10-09
tags: [claude-code, messages, 上下文工程]
related: [claude-code, api请求位置决定论, rules被动注入机制, system静态动态缓存分区, skill列表token预算, nested_memory按需加载]
sources: ["[202604151800]读完ClaudeCode源码才发现SkillsMCPRules的区别远没有你想的那么大.html"]
---
# messages 注入四通道

messages 注入四通道是对 Claude Code（v2.1.88 源码）中 `messages` 数组实际构成的源码级还原：`messages` 并非只有「真实对话历史」，而是混合了四类内容，其中前两类对用户不可见但参与模型推理。

| 注入通道 | 源码标识 | 内容 | 位置/标记 |
|----------|---------|------|----------|
| 系统上下文注入 | `prependUserContext` | CLAUDE.md 内容、当前日期等 | messages；`isHidden: true` + `isMeta: true`，`<system>` 包裹 |
| 系统提示上下文 | `appendSystemContext` | git 状态等 | system 参数（严格说属 system 通道，与 messages 通道并列） |
| 动态附件 | Attachments | Skill 列表（`skill_listing`）、计划模式指令、子目录 CLAUDE.md（`nested_memory`）等 | messages |
| 真实对话历史 | — | 用户输入、模型回复、工具调用结果 | messages |

## 标记体系

- `isHidden`：客户端侧 UI 标记，消息仍完整发送给 API，仅不在终端展示；
- `isMeta: true`：客户端标记，同样不影响 API 接收；
- `<system>`：**不是 API 字段**，而是 Claude Code 客户端与模型之间的约定格式——系统提示词告知模型 `<system>` 标签内容的权重等同系统指令。

## 关联

- `skill_listing` attachment 是第四通道（动态附件）的典型实例，见 [[skill列表token预算]]；
- `nested_memory` attachment 实现子目录 Rules 按需加载，见 [[nested_memory按需加载]]。
