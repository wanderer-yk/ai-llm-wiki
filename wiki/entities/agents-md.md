---
type: entity
title: AGENTS.md
tags: [agent指令文件, 编码规范, 约定]
related: [ralph-loop, 软件质量分级定义, 代码库模式大于指令, openclaw工作区md文件族]
created: 2026-10-13
updated: 2026-10-13
sources: ["[202601270830]从ReAct到RalphLoopAIAgent的持续迭代范式.html"]
---

# AGENTS.md

AGENTS.md 是一项仓库级 Agent 指令文件惯例：放在代码仓库中、用于向 AI 编码代理传达该仓库的指令、约束与质量标准的 markdown 文件（作为独立可验证的背景补充：该文件名约定已被多家 AI 编码工具采纳，作用类似 README.md 但面向 AI Agent 而非人类）。在本来源中，AGENTS.md 是传达仓库质量标准的三个载体之一（AGENTS.md / 技能 / 提示词）（[[sources/[202601270830]从ReAct到RalphLoopAIAgent的持续迭代范式]]）。

## 文章给出的质量标准模板要点

（存档中完整 markdown 块截断，以下为要点保留）

- 显式声明"这是生产代码库"
- TypeScript 严格模式
- 禁用 `any` 类型
- 函数级单元测试
- 遵循现有结构与命名
- 提交前 lint + 类型检查
- 优先级排序：可维护性 > 性能 > 快速交付

设计意图：仓库质量标准异质且 Agent 无法自行推断（[[软件质量分级定义]]），必须显式写入 AGENTS.md 让期望可见，以对抗"代码库模式大于指令"的模仿倾向（[[代码库模式大于指令]]）。

## Docker 沙箱权衡

使用 `docker sandbox run claude` 沙箱运行 Ralph 循环时，全局 AGENTS.md 与用户技能不会加载——这是 [[docker沙箱隔离]] 的已知代价（文章判断"对大多数 Ralph 循环没问题"）。

## 关联

与 [[openclaw工作区md文件族]] 同属"面向 Agent 的工作区指令文件"惯例家族；与 [[ai友好研发规范]]（美团）同构——都强调将约束固化为 AI 可读取的文件资产。