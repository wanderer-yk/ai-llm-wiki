---
type: concept
title: Skill 嵌套编排
created: 2026-10-09
updated: 2026-10-09
tags: [claude-code, skills, 工作流]
related: [claude-code, skills真正价值三场景, skill流程非代码化, superpowers插件]
sources: ["[202604151800]读完ClaudeCode源码才发现SkillsMCPRules的区别远没有你想的那么大.html"]
---
# Skill 嵌套编排

Skill 嵌套编排指 Skills「模型自主触发」场景的延伸能力：主 Skill 可以在执行过程中触发子 Skill，形成完整的**多步工作流入口**——用户只需调用一个顶层 Skill，模型即可沿指令链依次展开多个子流程。

## 定位与前提

- 嵌套编排发生在提示词注入层面而非代码控制流层面：源码中没有任何代码逻辑控制编排顺序，全靠注入的 Markdown 指令与模型指令遵循能力（[[skill流程非代码化]]）；
- 因此编排可靠性受双重约束：顶层 Skill 的触发可靠性（[[skill触发可靠性痛点]]）与每级指令的遵循率（[[skill质量等式]]）。

## 互证

与 [[superpowers插件]] 的结构化工作流机制（brainstorming → writing-plans → executing-plans 等 Skill 链式调用）互证：主 Skill 编排子 Skill 是 Skills 工程化使用中已被实践验证的模式。
