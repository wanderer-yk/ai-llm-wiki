---
type: concept
title: Vibe Engineering
created: 2026-10-13
updated: 2026-10-13
tags: [vibe-engineering, superpowers, 软件工程纪律, ai-coding]
related: [superpowers插件, writing-skills, vibe-coding, agentic-engineering, 像做tdd一样做skill, 像做机器学习一样做prompt工程]
sources: ["[202605120830]AgentSkill规范构建与设计模式.html"]
---
# Vibe Engineering

Vibe Engineering 是 Superpowers 框架的定位术语，[[sources/[202605120830]AgentSkill规范构建与设计模式|《Agent Skill规范、构建与设计模式》]] 将其概括为"在 AI 快速迭代的基础上强制注入软件工程纪律"——既保留 Vibe Coding 的迭代速度，又以测试、验证与证据链约束 AI 产出。术语说明：本文将该词记录为 Superpowers 框架的自我定位术语，非 Anthropic 官方规范术语。

## 载体：Superpowers 框架

- 官方仓库：https://github.com/obra/superpowers（见 [[superpowers插件]]）
- 目标平台：Claude Code、Cursor、Codex
- 构成：14 个可组合 Skill，其中 [[writing-skills]]（教 Agent 如何创建新 Skill 的元技能）隶属其中

## 四大核心理念

| 理念 | 英文 | 含义 |
|---|---|---|
| 测试先行 | TDD | 先构造失败场景再写规则，见 [[像做tdd一样做skill]] |
| 系统化优于随机化 | Process over Guessing | 用可重复流程替代碰运气式调 Prompt |
| 复杂度缩减 | Simplicity as Primary Goal | 把"保持精简"作为第一设计目标 |
| 证据优于声明 | Verify before Declaring Success | 宣称完成前必须先验证 |

## 概念三角：自由度-纪律光谱

Vibe Engineering 与 [[vibe-coding]]、[[agentic-engineering]] 构成 AI 编程纪律化程度的三级光谱：

- [[vibe-coding]]：无纪律的自由生成，"提示即祈祷"，仅适用原型验证；
- **Vibe Engineering**：承认 AI 快速迭代的价值，用 TDD、验证器、证据声明等纪律手段收束（Superpowers 路线）；
- [[agentic-engineering]]：人定义目标/约束/质量标准，AI 在结构化全流程中自主执行（seanguo/腾讯路线）。

三者共享同一问题意识——裸奔式 Vibe Coding 无法进入生产环境——差异在于注入纪律的位置与强度。

## 关联

- [[像做tdd一样做skill]] 是测试先行理念在 Skill 开发层的落地；
- 与 [[像做机器学习一样做prompt工程]]（Skill-Creator 的评测路线）同属"给 AI 快速迭代加护栏"的方法论家族。
