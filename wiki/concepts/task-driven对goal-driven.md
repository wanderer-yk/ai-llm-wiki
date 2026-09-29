---
type: concept
title: "Task-Driven 对 Goal-Driven"
tags: [agent, 范式, 认知跃迁, task-driven, goal-driven]
related: [zhiyuanfu, 24h打工人, state-yaml共享面板, 六步落地路径, 增强自我而非取代自我, plan-and-execute模式]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202605071734]十年老技术开发的AIAgent探索之路.html"]
---
# Task-Driven 对 Goal-Driven

[[zhiyuanfu]] 提出的 Agent 系统认知跃迁框架。Task-Driven 解决执行问题（让系统能跑），Goal-Driven 解决迭代问题（让系统持续向前）。

## 五维对比

| 维度 | Task-Driven | Goal-Driven |
|------|-------------|-------------|
| 人的角色 | 项目经理+执行监督 | 目标设定者/审核者 |
| Agent 角色 | 执行者 | 自主推进者 |
| 决策中心 | 在人脑子里 | 在目标+边界+系统状态里 |
| 主要成本 | 人持续编排 | 前期建模和约束设计 |
| 适用场景 | 简单一次性任务 | 长期复杂持续推进 |

## 核心瓶颈

"24h 在线 ≠ 24h 迭代"——Task-Driven 系统虽能 24h 执行，但"做什么/推进哪个方向/遇到阻塞怎么处理"等高层判断仍依赖人。只要任务还需人持续供给，人就仍是系统瓶颈。

## Goal-Driven 五前提

1. **目标清晰** — 可推进可判断的表达
2. **边界清晰** — 能做/不能做/资源上限
3. **状态可见** — 进度/卡点/原因
4. **过程留痕** — 成功/失败归因
5. **权限可控** — 工具调用范围/写入范围/兜底机制

## 有限自治原则

Goal-Driven 不是更放权，而是更强约束下的有限自治。5 个前提成立时自主推进才是资产，否则只会把错误放大得更快。别跳步——必须建立在成熟 Task-Driven 基础上。