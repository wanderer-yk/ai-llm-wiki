---
type: concept
title: rules-skills-mcp 选型指南
created: 2026-10-09
updated: 2026-10-09
tags: [claude-code, rules, skills, mcp, 选型]
related: [rules与skills等价论, mcp与bash对比, mcp内置工具同构论, skill触发可靠性痛点, 提示词工程统一论, claude-code]
sources: ["[202604151800]读完ClaudeCode源码才发现SkillsMCPRules的区别远没有你想的那么大.html"]
---
# rules-skills-mcp 选型指南

rules-skills-mcp 选型指南是本文 4.4 节给出的落地决策清单：在理解三者底层等价性之后，选型依据是文本长度、触发方式、执行隔离、连接需求四类适用条件，而非机制强弱。

## 原文清单

| 何时使用 | 适用条件 |
|---------|---------|
| Rules | ①项目级编码规范、技术栈约定、代码风格要求；②文本短（几百字以内），每次注入不心疼 token；③需要「始终生效」的指令，不依赖模型判断 |
| Skills | ①指令文本较长（几百行级别），不适合每次注入；②有明确触发时机（用户主动 `/commit`、`/review-pr`）；③需要执行隔离（Fork 模式独立上下文，不污染主对话） |
| MCP | ①需要持久化连接/状态管理（数据库连接池、认证 session）；②复杂多步操作需要原子封装；③需要权限隔离，不想给模型万能 Bash；④简单 CLI 操作（`gh`、`curl`、`psql`）直接用 Bash，别折腾 MCP |

## 两条现实提醒

1. **手动调用优先**：不要迷信自动触发（受 1% 预算、250 字符描述限制，[[skill触发可靠性痛点]]）；把核心 Skill 快捷命令告诉团队成员，手动调用比自动识别靠谱；
2. **Bash 优先**：引入 MCP 前先想 Bash 能否搞定（[[mcp与bash对比]]）。
