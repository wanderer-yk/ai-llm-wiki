---
type: entity
title: 小红书PMO团队
tags: [组织, PMO, 小红书, agent, 项目管理]
related: [小红书技术REDtech, pmo-bp-agent, 项目注册平台, openclaw]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202605111758]打造AI时代项目管理新范式小红书PMO团队的Agentic探索之路.html"]
---
# 小红书PMO团队

小红书公司旗下项目管理办公室（PMO），2025Q1 起从零构建项目管理 AI Agent，已完成四轮迭代升级。核心团队成员包括白也、聂风、广见、唐泽、若臻、浩宇。

## 四轮迭代历程

| 阶段 | 产品 | 核心命题 | 关键技术 |
|------|------|----------|----------|
| 1.0 | 项目管理知识问答薯 | 让 AI 理解项目管理 | RAG 知识问答，简单工作流 |
| 2.0 | Agent + 多渠道 | 从知道到做到 | 原子/复合 Agent 架构，IM 集成 |
| 3.0 | OpenClaw 个人助理 | 跨会话跨渠道长记忆 | 长记忆四件套，项目注册平台 |
| 4.0 | [[pmo-bp-agent]] | 专属 BP | 1项目×N人×M Session 模型 |

## 核心理念

- AI 是新生产力，应接管 routine 工作（拉群、催更、总结、识别 todo、写纪要），将人力留给决策（目标、阵型、资源）
- 领域 Agent = 领域 Source of Truth（[[领域Agent即Source-of-Truth]]）
- PMO 人人都是 Builder：AI 时代 PMO 应为 Builder + PMO
- 知识问答只覆盖 30% 提效，执行动作占 70%

## 技术选型

- 1.0-2.0：自建工作流平台
- 3.0-4.0：切换至 [[openclaw]] 框架，原有能力蒸馏为 Skill 上架内部 Skill Hub
- 同时探索个人分身和 PMO Team Claw 集体喂养模式

## 发布渠道

技术文章通过 [[小红书技术REDtech]] 微信公众号发布。