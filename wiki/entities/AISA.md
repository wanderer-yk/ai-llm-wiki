---
type: entity
title: AISA
tags: [静态代码扫描, agent, rules, 百度]
related: [AICR, 柚漫剧团队, 测试左移AI助力, sonar]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604271800]柚漫剧AI全流程提效拆解从单点提效到工程融合.html"]
---

# AISA

AISA 是 [[柚漫剧团队]] 使用的静态代码扫描能力，基于 **Agent + rules** 构建，已在 **Comate IDE、VSCode、JetBrains 三端上线**供业务使用。文章研发章（4.2.3）记作"AI SA"、测试章（5.2）记作"AISA"，经比对确认为同一能力，规范名取 AISA。

## 职能与演进

- 在 Commit 阶段提前发现静态代码风险，是 [[AICR]] 工具族的扫描组件。
- 初期误报率较高，QA 按语言（Android / iOS / Go）针对性优化后有效率持续提升。
- 与"小码哥"智能评审融合，重复内容自动收起。

## 跨源对照

AISA 与转转 [[sonar]] 扫描 + [[六步智能提示词生成法]] 的"静态扫描结果 → AI 消费"双路径同构，两者都把误报优化作为工程化关键环节；AISA 的差异化在于直接以 Agent + rules 形态内嵌三端 IDE。
