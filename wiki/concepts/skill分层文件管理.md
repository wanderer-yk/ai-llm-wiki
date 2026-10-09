---
type: concept
title: skill 分层文件管理
created: 2026-10-09
updated: 2026-10-09
tags: [skill, 文件组织, 注意力管理]
related: [skill渐进式披露, skill目录结构与命名规范, agent-skill知识包, checklist驱动skill, skill稳定性决定论]
sources: ["[202605201800]网盘存量代码迁移实战我们如何用三层架构管住AI的输出.html"]
---
# skill 分层文件管理

**skill 分层文件管理**是本文的 Skill 文件组织方法：核心规则放主文件，边界情况放 references 目录。其原理是注意力预算——Skill 文件太长会导致 AI 注意力分散、关键规则被稀释；把低频的边界情况外置到 references 目录，保证主文件中关键规则的权重。

## 本文的实践含义

- 主文件只保留高频核心规则，使每次执行时关键约束都处在注意力中心（支撑 [[skill稳定性决定论]]）。
- references 目录承载边界情况与反例，这些内容正是 Skill 在迭代中由真实错误倒逼补充的（[[skill错误倒逼生成]]）。

## 与既有概念的同构性

该做法与 Anthropic Skill 体系的 references 分层同构：[[skill渐进式披露]]（按需加载细节以节省上下文）、[[skill目录结构与命名规范]]（SKILL.md 主文件 + 辅助文件的目录规范）、[[agent-skill知识包]]（Skill 作为渐进式加载的知识包）。本文提供了来自代码迁移实战的独立再验证。