---
type: concept
title: skill 思维框架模式（特殊模式）
tags: [skill, 设计模式, 思维框架, 反幻觉, 深度分析]
related: [audit-context-building, trailofbits-skills, ai编程幻觉, 自我说服效应, reflection模式, skill嵌套编排, 模式选择决策树, 防止llm偷懒4种武器]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604270830]工作流的Skill怎么写从7个顶级Skill中提炼的模式与最佳实践.html"]
---
# skill 思维框架模式（特殊模式）

思维框架模式是与 5 大核心操作型模式并列的**特殊模式**：它不控制 LLM"做什么"，而控制 LLM"怎么想"，适用于安全审计、代码审查、架构分析等需要深度思考的场景。代表案例为 [[trailofbits-skills|trailofbits/skills]] 的 [[audit-context-building]]（302 行），一句话精髓"控制 LLM '怎么想'而非'做什么'"。

**定义性技巧**：Purpose 思维定位声明——结构首节显式声明"控制思维方式，不是控制行为"。

**控制思维质量的手段**：三阶段分析结构（Phase 1 定向扫描 → Phase 2 逐行分析[含 Per-Function Checklist / Cross-Function Flow / Output Requirements / Completeness Checklist 四子节] → Phase 3 全局理解，由局部到全局递进）；量化阈值（硬性最低标准强制分析深度："每个函数最少 3 个不变量、5 个假设"，同时是 [[防止llm偷懒4种武器]] 之一）；非目标约束（Non-Goals 明确禁止"不要识别漏洞、不要提出修复"，克制 LLM 最想做之事，先理解再判断）；反幻觉规则（Stability Rules："Never reshape evidence to fit earlier assumptions"，与 [[ai编程幻觉]]、[[自我说服效应]] 强互证）；思维工具注入（给分析框架而非具体命令：第一性原理、5 Why、5 How）；子 Agent 指导（规定何时及如何调用 function-analyzer，"分而治之"，与 [[skill嵌套编排]] 呼应）。

**选择判据**：需要控制的是"思维质量"而非"操作步骤"。其 Phase/Verify 交替结构与 [[reflection模式]] 互证。
