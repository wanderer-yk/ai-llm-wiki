---
type: concept
title: Skill 强制触发指令
created: 2026-10-09
updated: 2026-10-09
tags: [claude-code, skills, description]
related: [claude-code, skill提示词注入本质, description三大要素, 负向触发说明, skill触发可靠性痛点]
sources: ["[202604151800]读完ClaudeCode源码才发现SkillsMCPRules的区别远没有你想的那么大.html"]
---
# Skill 强制触发指令

Skill 强制触发指令指 Claude Code 中 `Skill` 工具的 description 内嵌的一条 BLOCKING REQUIREMENT 级指令：当用户的请求匹配某个 Skill 时，模型**必须先调用 Skill 工具，再生成关于该任务的任何其他响应**。这是源码层面为对抗模型欠触发而设计的提示词级强约束。

## 原文引文

> "When a skill matches the user's request, this is a BLOCKING REQUIREMENT: invoke the relevant Skill tool BEFORE generating any other response about the task"

## 设计意图与现实

- 意图：以最强的指令措辞压过模型的「直接作答」倾向，确保 Skill 指令文本先于模型自由发挥注入上下文；
- 现实：即便有 BLOCKING REQUIREMENT，250 字符描述 + `whenToUse` 字段仍经常不足以让模型正确判断，欠触发是常态（[[skill触发可靠性痛点]]）——强制指令解决「触发优先级」，解决不了「匹配判断」。

## 互证

与 [[description三大要素]]、[[负向触发说明]] 中「description 写法决定触发准确性」的实践结论互证：官方实现同样依赖 description 工程与指令强化来约束触发行为。
