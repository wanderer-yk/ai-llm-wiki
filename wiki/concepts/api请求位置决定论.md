---
type: concept
title: API 请求位置决定论
created: 2026-10-09
updated: 2026-10-09
tags: [claude-code, rules, mcp, skills, 上下文工程]
related: [claude-code, mcp, rules被动注入机制, mcp内置工具同构论, skill提示词注入本质, rules与skills等价论, 三大武器库, 提示词工程统一论]
sources: ["[202604151800]读完ClaudeCode源码才发现SkillsMCPRules的区别远没有你想的那么大.html"]
---
# API 请求位置决定论

API 请求位置决定论是 Cheer 通过分析 Claude Code v2.1.88 泄漏源码提出的核心论断：Rules、MCP、Skills 三机制的「本质区别不在功能，而在信息被塞入 `anthropic.messages.create` API 请求的位置不同」。社区通常将三者视为本质不同的能力层（行为规范/工具协议/工作流），源码视角则显示它们最终都以文本形态进入模型上下文，差异主要来自工程包装。

## 定谳后的机制定位

| 机制 | 注入位置 | 执行性质 |
|------|---------|---------|
| Rules | messages 最前部（`prependUserContext()`，`<system-reminder>` 包裹） | 被动注入，**不走 `tool_use`**（[[rules被动注入机制]]） |
| MCP | `tools[]` 注册 + system 动态区 instructions | 真实 JSON-RPC 调用（[[mcp内置工具同构论]]） |
| Skills | `skill_listing` attachment 常驻 + `tool_use` 触发注入 SKILL.md | 提示词注入，非函数调用（[[skill提示词注入本质]]） |

## 论证过程中的两次修正

1. **「同构于 tool_use 协议」口径收窄**：初版导读称三者底层同构建于 `tool_use` 协议之上，正文 3.1.4 节明确「Rules 不走 `tool_use` 协议」，最终图景修正为：Rules = 被动注入（非工具），MCP = 工具注册 + 真实 RPC，Skills = `tool_use` 触发但本质是提示词注入。
2. **Skills 注入路径两阶段定谳**：「Skill 列表经 attachment 注入」与「Skill 经 `tool_use` 触发后注入」两种表述均正确——列表常驻注册走 attachment，执行时指令文本经 `tool_use` 触发注入。

## 三问收束

该论点由导读三问逐一坐实：Q1（[[rules与skills等价论]]）、Q2（MCP 与内置工具对模型无区别）、Q3（[[skill流程非代码化]]）。

## 与分层叙事的张力

本文「同源/位置差异」视角与 [[三大武器库]]（腾讯 binxiong 的知识库+MCP+Skills 三层赋能架构）的分层叙事构成持续张力：前者强调底层同构，后者强调能力分层——两者可能分别对应「模型视角」与「工程组织视角」。

## 证据等级

基于 v2.1.88 泄漏源码，版本特定、非官方口径；引用时应保留来源限定。
