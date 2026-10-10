---
type: concept
title: prompt-context-harness三阶段
created: 2026-10-10
updated: 2026-10-10
tags: [prompt-engineering, context-engineering, harness-engineering, claude-code, agent]
related: [harness三层次定位, harness四要素, harness-engineering, harness工程三部曲演进论, agent框架三要素, system-prompt动态组装机制, memdir结构化记忆系统, permission-engine三行为模型, sandbox按需隔离, ai工程量化效果声明追踪]
sources: ["[202604200830]深度解析ClaudeCode在PromptContextHarness的设计与实践.html"]
---
# prompt-context-harness三阶段

**prompt-context-harness 三阶段**是飞樰在解析 [[claude-code]] 时提出的 Agent 工程能力递进框架：Prompt Engineering 解决「如何说」，Context Engineering 解决「让 AI 看什么」，Harness Engineering 解决「给 AI 构建怎样的运行环境」。三者层层递进，Prompt 是基石。

## 三阶段与评分类比

| 阶段 | 核心问题 | 文中评分类比 |
|------|----------|--------------|
| Prompt Engineering | 如何说（提示词本身质量） | 约 70+ 分 |
| Context Engineering | 让 AI 看什么（上下文注入/压缩/记忆） | 提升至 80~85 分 |
| Harness Engineering | 构建怎样的运行环境（权限/钩子/沙箱/校验） | 提升至 90~95 分 |

注意：该评分是**思想实验式类比**，无评测口径，不应记入量化实证证据（参见 [[ai工程量化效果声明追踪]]）。作者同时强调 70+ 分底子是前提，Context/Harness 无法拯救低基线 Prompt。

## 提示词组装范式转变

文章据此提出提示词工程内涵的质变：从「提示词如何写好」转向「提示词如何组装」——按身份人设、系统行为、安全守则、任务要求、工具规范、Skill 要求、约束条件等模块动态拼接。[[claude-code]] 的 System Prompt 正是这一范式的典型实现（见 [[system-prompt动态组装机制]]）。

## 跨源关联

- 与文章后文的 [[harness三层次定位]] 对应：Prompt = What & How；Context = How Better；Harness = How Controlled。
- 与爱奇艺 [[harness-engineering]]（五要素工程化约束）及 [[harness四要素]] 高度同构，属不同团队对同一问题的独立收敛；与 [[harness工程三部曲演进论]]（HermesAgent 系列）构成同作者前后呼应。
