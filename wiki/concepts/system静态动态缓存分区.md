---
type: concept
title: system 静态/动态缓存分区
created: 2026-10-09
updated: 2026-10-09
tags: [claude-code, anthropic, prompt-cache, 上下文工程]
related: [anthropic, claude-code, rules被动注入机制, mcp内置工具同构论, messages注入四通道]
sources: ["[202604151800]读完ClaudeCode源码才发现SkillsMCPRules的区别远没有你想的那么大.html"]
---
# system 静态/动态缓存分区

system 静态/动态缓存分区是 Claude Code（v2.1.88 源码）组织 `anthropic.messages.create` 请求中 `system` 参数的方式：system 分为**静态区**（核心系统提示与角色定义，内容跨用户一致，可被 Anthropic org 级 Prompt Cache 缓存——KV 矩阵算一次全用户共享，后续调用仅 0.1x 费用）与**动态区**（git 状态经 `appendSystemContext` 追加、MCP Server 级 instructions 等，位于缓存边界标记之后）。

## 缓存经济学决定注入位置

核心推论：「注入位置由缓存经济学决定」——CLAUDE.md 等因项目而异的内容**不放入 system**，因为一旦混入会破坏跨用户共享的静态缓存；放入 messages 既不影响 system 缓存，又能在会话内轮次间复用。这一原理解释了 Rules 为何经 [[rules被动注入机制|messages 被动注入]] 而非写进系统提示。

## 动态内容对缓存的威胁与对策

MCP instructions 属动态内容：MCP Server 连接/断开会改变 system 动态区，进而破坏 prompt 缓存。为此源码设有 feature gate `isMcpInstructionsDeltaEnabled()`：开启时 MCP instructions 改走 attachment（即 messages 通道）以保护缓存——缓存保护优先于注入位置的语义语义整洁（默认状态与 rollout 未知，属开放问题）。

## 关联

- 与 [[messages注入四通道]] 共同构成完整的上下文组装图景；
- MCP 双位置注入的完整分析见 [[mcp内置工具同构论]]。
