---
type: entity
title: AutoResearch 三原则
tags: [autoresearch, 方法论原则, 量化目标, 自主循环]
related: [karpathy, autoresearch, smallnest-autoresearch, val-loss改善才commit, 5维度量化评分, 达尔文skill, 人的参与程度反映领域特征]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604201800]我把Karpathy的AutoResearch搬到了软件开发领域效果炸了.html"]
---
# AutoResearch 三原则

AutoResearch 三原则是对 Karpathy autoresearch 方法论精髓的提炼：**① 量化目标**（val loss 是唯一判断标准）、**② 自主循环**（无需人类每轮介入）、**③ 只保留改进**（退化就回滚，绝不将就）。

## 跨领域验证

本文"与同类项目对比"小节以三方实践验证三原则的跨领域共通性：

| 原则 | Karpathy（ML 研究） | [[smallnest-autoresearch]]（软件开发） | [[达尔文skill]]（Skill 优化） |
|------|------|------|------|
| 量化目标 | val loss | 5 维审核评分（达标线 9.0，见 [[5维度量化评分]]） | 8 维总分 |
| 自主循环 | 全自主 | 循环外调优 program.md | 每轮暂停等人确认 |
| 只保留改进 | git revert 硬回退（见 [[val-loss改善才commit]]） | 交叉审核软保护（无回退） | git revert 硬回退 |

## 洞见

三方对比的三条洞见中，第一条即"量化目标是共通核心"——无论被优化资产是模型训练代码、软件仓库还是 Skill，自主循环都依赖一个可机器判定的"什么是改进"定义；而"人的参与程度反映领域特征"（见 [[人的参与程度反映领域特征]]）解释了三者在原则②上的分化：量化指标越客观，循环越自主。
