---
type: concept
title: Skill 五种设计模式
created: 2026-10-13
updated: 2026-10-13
tags: [skill, 设计模式, google-adk, 架构]
related: [google-adk, skill四类型分类法, skill渐进式加载机制, human-in-the-loop, 结构化输出, 机器可验证完成条件]
sources: ["[202605120830]AgentSkill规范构建与设计模式.html"]
---
# Skill 五种设计模式

Skill 五种设计模式是 Google ADK 团队跨生态研究（Anthropic 仓库、Vercel、Google 内部指南，原文出处 Google Cloud Tech X 帖）归纳的 Skill **内部逻辑设计**的五种反复出现模式。其前提论断：规范（SKILL.md 格式）只定义"Skill 长什么样"，不定义"内部逻辑该怎么设计"。

## 五种模式

| 模式 | 核心机制 | 关键约束 |
|---|---|---|
| Tool Wrapper | 按需加载领域知识；SKILL.md 告诉 Agent 去哪加载而非内置规范 | 示例 api-expert 指向 references/conventions.md；即 [[skill渐进式加载机制]] 的设计模式化表达 |
| Generator | 模板+风格指南强制输出一致 | 关键在 Step 3 主动提问不瞎猜："缺什么直接问"；示例 report-generator 加载 style-guide + template |
| Reviewer | 查什么与怎么查分离 | "WHY not WHAT"——不只指出问题，还要解释为什么是问题；输出按严重等级分组 |
| Inversion | 翻转交互：Agent 先采访用户收集完整需求再动手 | 硬约束"在所有阶段完成之前，请勿开始构建"；全程禁建（同 [[human-in-the-loop]]） |
| Pipeline | 严格顺序+每步输入输出+通过条件 | 不得跳步；"用户不点头，Agent 不能往下走"（确认硬门控） |

## 选择指南（4.2）

| 你需要什么？ | 选择哪种模式 |
|---|---|
| 特定技术栈的专家知识 | Tool Wrapper |
| 一致的结构化输出 | Generator |
| 自动化代码/内容审查 | Reviewer |
| 需求不明确，需先收集信息 | Inversion |
| 复杂的多步骤任务 | Pipeline |
| 不确定？ | **从 Tool Wrapper 开始**（默认策略） |

## 组合推荐（4.3）

| 组合 | 说明 | 场景 |
|------|------|------|
| Pipeline + Reviewer | 管道最后一步加自动审查 | 文档生成后自动质量检查 |
| Generator + Inversion | 先收集信息再填充模板 | 需用户输入的结构化文档生成 |
| Pipeline + Tool Wrapper | 管道某些步骤加载专家知识 | 多步骤代码生成 |
| Inversion + Pipeline | 先完成需求收集再进入执行流水线 | 复杂项目全流程 |

## 跨来源收敛

Pipeline 的确认硬门控与爱奇艺 [[blocker-gate]]、vivo [[验证门禁化]] 三方独立趋同——硬门控跨厂商共识的强证据；Generator 与 [[结构化输出]]、Pipeline 通过条件与 [[机器可验证完成条件]]、Reviewer 与 [[mr双重视角审查]] 分别同构。与 Anthropic 侧 [[skill四类型分类法]] 的映射关系来源未给出（见 [[queries/五种设计模式与四类型分类法如何映射|五种设计模式与四类型分类法如何映射]]）。
