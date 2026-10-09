---
type: entity
title: Cheer
created: 2026-10-09
updated: 2026-10-09
tags: [作者, claude-code, 源码分析]
related: [百度Geek说, claude-code, mcp, api请求位置决定论]
sources: ["[202604151800]读完ClaudeCode源码才发现SkillsMCPRules的区别远没有你想的那么大.html"]
---
# Cheer

Cheer 是微信公众号「[[百度Geek说]]」的署名作者。其代表性贡献为 2026 年 4 月发表的《读完 Claude Code 源码才发现：Skills、MCP、Rules 的区别，远没有你想的那么大》——一篇基于 Claude Code v2.1.88 泄漏源码的技术分析长文（6858 字）。

## 主要观点贡献

- 提出「[[api请求位置决定论]]」：Rules/MCP/Skills 三机制的本质区别不是功能差异，而是信息在 `anthropic.messages.create` API 请求中的注入位置不同。
- 给出 Rules 被动注入（[[rules被动注入机制]]）、MCP 双位置注入与真实 RPC 执行链（[[mcp内置工具同构论]]）、Skills 两阶段机制（`skill_listing` attachment + `tool_use` 触发注入）的源码级还原。
- 收束导读三问：Rules 与 Skills 对模型等价（[[rules与skills等价论]]）、MCP 与内置工具对模型同构、Skill「标准化工作流」实为结构化 Markdown（[[skill流程非代码化]]）。
- 提出「写好 Skill 与写好 Rules 都是提示词工程」的[[提示词工程统一论]]，并给出 Rules/Skills/MCP 的[[rules-skills-mcp选型指南|选型清单]]（手动调用优先、Bash 优先）。

## 备注

- 关于该作者的身份信息目前仅有本来源记载的署名归属，其余背景未知。
- 本文论证基于泄漏源码（v2.1.88），版本特定、非官方口径，引用其结论时应保留该来源限定。
