---
type: concept
title: skill 自动创建触发条件
tags: [hermes-agent, skill, self-improving]
related: [hermes-agent, skill局部patch修补, skill轻量索引按需加载, memory与skill职责边界, 动态skill生成]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604230830]深入源码HermesAgent如何实现SelfImproving.html"]
---
# skill 自动创建触发条件

skill 自动创建触发条件是 [[hermes-agent]] Skill 自进化的入口机制：`skill_manage` 工具 schema（`tools/skill_manager_tool.py:681-701`，SKILL_MANAGE_SCHEMA）**以声明式触发条件内置在工具定义中**，Agent 无需用户指示即可自主判断何时沉淀 Skill。这是文章"一个靠人喂，一个自己长"对比中"自己长"一侧的实现基础。

## 触发条件（文章归纳）

- 复杂任务成功完成（5+ 工具调用）；
- 错误被克服后的经验；
- 用户纠正过的做法；
- 非平凡工作流。

简单一次性任务不记；困难任务完成后主动向用户提议保存。

## SKILL.md 结构与 Pitfalls 追加

SKILL.md 典型结构为 YAML frontmatter（`name`/`description`/`version`）+ "When to use" + "Steps" + "Pitfalls"。关键点：**Pitfalls 不是预先写好的，而是 Agent 踩坑后追加的**——通过 [[skill局部patch修补]] 机制实现"踩坑当场补"。

## 维护责任提示词

系统提示词含 "Skills that aren't maintained become liabilities"——通过提示词灌输维护责任感，防止 Skill 只建不管。

## 关联

与 0424 飞樰文的 [[动态skill生成]] 属同一问题域，两来源互为印证；创建后的 Skill 经 [[skill轻量索引按需加载]] 进入上下文、经 [[skill安全扫描统一门禁]] 保证安全。