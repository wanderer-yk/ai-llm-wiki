---
type: concept
title: Skill 触发可靠性痛点
created: 2026-10-09
updated: 2026-10-09
tags: [claude-code, skills, 触发]
related: [agent欠触发倾向, claude-code, skill列表token预算, 触发质量决定论, skill强制触发指令, rules-skills-mcp选型指南]
sources: ["[202604151800]读完ClaudeCode源码才发现SkillsMCPRules的区别远没有你想的那么大.html"]
---
# Skill 触发可靠性痛点

Skill 触发可靠性痛点指：即便 Claude Code 在源码层面设置了 `skill_listing` 常驻注册（[[skill列表token预算]]）和 BLOCKING REQUIREMENT 强制触发指令（[[skill强制触发指令]]），Skill 的**自动触发仍然不可靠**——250 字符描述 + `whenToUse` 字段经常不足以让模型正确判断匹配，LLM 不自动触发是常态，实际使用中靠手动 `/commit`、`/review-pr` 兜底。

## 落地建议（原文立场）

- 不要迷信自动触发；
- 把核心 Skill 的快捷命令告诉团队成员，手动调用比自动识别靠谱；
- 当需手动 `/skill-name` 触发时，Skill 与 Rules 的实际效果几乎无区别（[[rules与skills等价论]]）。

## 源码级强互证

本痛点与既有 wiki 概念 [[agent欠触发倾向]] 构成三处互证：欠触发常态（chunk 8）、描述质量决定触发上限（[[触发质量决定论]]）、手动调用优先建议——将社区实践观察上升到泄漏源码证据层面。
