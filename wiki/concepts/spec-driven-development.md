---
type: concept
title: 规格驱动开发（SDD）
tags: [ai-coding, 方法论, 规格驱动]
related: [specflow, vibe-coding, ai-bian-cheng-huan-jue, openspec, github-spec-kit, bmad-method]
created: 2026-06-08
updated: 2026-06-08
sources: ["治愈CursorAI编程的幻觉用它就够了.html"]
---
# 规格驱动开发（SDD）

**规格驱动开发（Spec-Driven Development, SDD）** 是一种将模糊需求拆解为机器可理解契约的开发方法论。其核心共识是：**AI 缺的不是代码能力，而是精准的"指令规格"**。

## 背景

[[tian-ji-qian-duan-tuan-dui|天玑前端团队]] 在一年 AI Coding 实践中发现，[[vibe-coding|Vibe Coding]] 在复杂场景下因上下文断层和需求共识缺失遭遇瓶颈，业界正从"对话驱动"转向"契约驱动"的 SDD 模式。

## 核心原则

1. **先规格后编码**：在 AI 开始写代码前，必须完成需求澄清和技术建模
2. **契约即约束**：将模糊需求转化为结构化、可验证的规格文档
3. **门控阻断**：前置阶段未达标时，硬性阻断后续编码阶段

## 社区方案

| 方案 | 核心思想 | 关键词 |
|------|---------|--------|
| [[openspec|OpenSpec]] | 原子化变更 | Proposal→Apply |
| [[github-spec-kit|GitHub Spec Kit]] | 门控 + 宪章 | Constitution + Gating |
| [[bmad-method|BMAD-METHOD]] | 角色思维隔离 | PM/架构师/QA |

## Specflow 的 SDD 实践

[[specflow|Specflow]] 集三家之长，提出四阶段 SDD 流程：

1. **Specify**：问题澄清（PM 角色）→ `specify.md`
2. **Plan**：技术建模（架构师角色）→ `plan.md`
3. **Implement**：原子化实现（工程师角色）→ 代码 + Log
4. **Archive**：知识归档（知识管理员角色）→ `summary.md`

## 与其他 Wiki 主题的关联

- [[ren-ren-dui-qi-ren-ji-dui-qi|人人对齐→人机对齐]]（美团）：SDD 的"契约"本质上就是"人人对齐"的产物，是"人机对齐"的前置条件
- [[ai-you-hao-yan-fa-gui-fan|AI 友好研发规范]]（美团）：SDD 的规格文档与研发规范都致力于约束 AI 产出，前者侧重流程级约束，后者侧重代码级约束
- [[context-engineering|上下文工程]]（马上消费）：SDD 的规格文档本质上是对上下文的精确工程化交付