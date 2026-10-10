---
type: entity
title: audit-context-building（Skill）
tags: [skill, 安全审计, 思维框架, 反幻觉]
related: [trailofbits-skills, skill思维框架模式, ai编程幻觉, 自我说服效应, reflection模式, skill嵌套编排]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604270830]工作流的Skill怎么写从7个顶级Skill中提炼的模式与最佳实践.html"]
---
# audit-context-building（Skill）

`audit-context-building` 是 [[trailofbits-skills|trailofbits/skills]] 仓库中的一个 302 行 Skill，是 [[青斧]] 七个分析对象中的第 7 个、特殊模式"思维框架"（见 [[skill思维框架模式]]）的代表。与控制行为的操作型 Skill 不同，它控制的是 LLM 的**思维质量**，适用于安全审计、代码审查、架构分析等深度思考场景。

结构首节 Purpose 显式声明"控制思维方式，不是控制行为"；主体为三阶段分析结构：Phase 1 定向扫描 → Phase 2 逐行分析（核心，含 Per-Function Checklist / Cross-Function Flow / Output Requirements / Completeness Checklist 四子节）→ Phase 3 全局理解，由局部到全局递进。关键技巧：量化阈值（硬性最低标准"每个函数最少 3 个不变量、5 个假设"，强制分析深度）；Non-Goals 非目标约束（"不要识别漏洞、不要提出修复"，克制 LLM 最想做之事，先理解再判断）；Stability Rules 反幻觉规则（"Never reshape evidence to fit earlier assumptions"，与 [[ai编程幻觉]]、[[自我说服效应]] 强互证）；思维工具注入（第一性原理、5 Why、5 How）；以及规定何时及如何调用内部子 Agent `function-analyzer` 的"分而治之"指导（与 [[skill嵌套编排]] 呼应）。其 Phase/Verify 交替结构与 [[reflection模式]] 互证。
