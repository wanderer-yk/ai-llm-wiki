---
type: concept
title: Skill 列表 token 预算
created: 2026-10-09
updated: 2026-10-09
tags: [claude-code, skills, token-budget]
related: [claude-code, skill提示词注入本质, skill触发可靠性痛点, 触发质量决定论, skill强制触发指令]
sources: ["[202604151800]读完ClaudeCode源码才发现SkillsMCPRules的区别远没有你想的那么大.html"]
---
# Skill 列表 token 预算

Skill 列表 token 预算指 Claude Code 为 Skill 常驻注册设定的上下文开销上限：Skill 列表经 `skill_listing` attachment 注入，仅占**上下文窗口的 1%**（默认 **8000 字符**），每个 Skill 的描述上限 **250 字符**；当 Skill 过多时，列表会被截断甚至移除部分条目。

## 源码片段（v2.1.88）

```php
case "skill_listing": {
    return [createMessage({
        content: `The following skills are available for use with the Skill tool:\n\n${attachment.content}`,
        isMeta: true
    })];
}
```

## 设计影响

- **描述空间极度受限**：250 字符内要讲清「这个 Skill 做什么、何时用」，是自动触发可靠性的第一道瓶颈（[[skill触发可靠性痛点]]）；
- **数量不等于覆盖**：堆砌 Skill 数量会挤爆 1% 预算导致截断，自动触发上限取决于描述质量而非数量（[[触发质量决定论]]）；
- 预算约束与 [[skill强制触发指令|BLOCKING REQUIREMENT]]、`whenToUse` 字段共同构成 Skill 触发机制的三要素。
