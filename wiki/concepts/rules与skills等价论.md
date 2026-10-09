---
type: concept
title: Rules 与 Skills 等价论
created: 2026-10-09
updated: 2026-10-09
tags: [claude-code, rules, skills]
related: [claude-code, api请求位置决定论, skills真正价值三场景, skill双执行模式, rules条件生效机制, 提示词工程统一论]
sources: ["[202604151800]读完ClaudeCode源码才发现SkillsMCPRules的区别远没有你想的那么大.html"]
---
# Rules 与 Skills 等价论

Rules 与 Skills 等价论是本文导读三问 Q1 的定谳结论：**Rules 与 Skills 对模型没有本质区别**——两者最终都是 messages 中 `role: "user"` 的文本注入；手动 `/commit` 与直接 `@commit-rules.md` 的效果几乎一样，Skills 多绕的 `tool_use` → 读文件 → 注入步骤「本质上只是提供了额外的工程便利」。

## 真正的区别落点（三点）

1. **触发方式**：Rules 始终生效（或经 `paths` 条件生效，[[rules条件生效机制]]），不依赖模型判断；Skills 需模型自主触发或用户 `/skill-name` 手动触发；
2. **执行隔离**：Skills 有 Fork 模式独立执行生命周期（[[skill双执行模式]]），Rules 没有；
3. **组织管理属性**：Skill 是「被组织管理的知识」——可经 `/skills` 浏览、随插件打包分发、作为团队协作标准化入口；Rules 路径则是「私人知识」。

## 方法论含义

既然对模型等价，写好 Skill 与写好 Rules 就是同一种能力——都是提示词工程（[[提示词工程统一论]]）；三者的选型依据不在「能力强弱」而在适用条件（[[rules-skills-mcp选型指南]]）。Skills 的不可替代价值收敛到三个场景（[[skills真正价值三场景]]）。
