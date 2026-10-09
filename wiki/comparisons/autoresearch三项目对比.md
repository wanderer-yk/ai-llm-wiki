---
type: comparison
title: AutoResearch 三项目对比
tags: [autoresearch, 对比, 质量保证, 人机协作]
related: [autoresearchkarpathy-原版, smallnest-autoresearch, 达尔文skill, autoresearch三原则, 硬性保护与软性保护, 人的参与程度反映领域特征, ralph-wiggum方法, 花叔]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604201800]我把Karpathy的AutoResearch搬到了软件开发领域效果炸了.html"]
---
# AutoResearch 三项目对比

来自《我把 Karpathy 的 AutoResearch 搬到了软件开发领域，效果炸了》"与同类项目对比"小节：作者 [[鸟窝]] 将 [[autoresearchkarpathy-原版]]（Karpathy 的 ML 研究自循环）、自己维护的 [[smallnest-autoresearch]]（通用软件开发）与 [[花叔]] 的 [[达尔文.skill]]（Skill 优化）三方并置，提炼 AutoResearch 方法跨领域迁移的三条洞见，是 [[autoresearch三原则]]"量化目标 + 自动迭代 + 只保留改进"跨领域普适性的直接证据。

## 对比表

| 维度 | Karpathy AutoResearch | smallnest/autoresearch | 达尔文.skill |
|------|----------------------|------------------------|--------------|
| 领域 | ML 研究（训练代码优化） | 通用软件开发（GitHub Issue 实现） | Skill / 提示词优化 |
| 被优化资产 | train.py 训练代码 | Issue 对应的代码实现 | Skill 本身 |
| 量化指标 | val loss（单一指标） | 5 维加权评分，达标线 9.0/10 | 8 维总分（构成未披露） |
| 质量保证机制 | git revert 硬保护（退化即回滚） | 多 Agent 交叉审核软保护（无回退机制） | git revert 硬保护 |
| 人的参与程度 | 全自主，人只维护 program.md | 循环外调优 program.md | 每轮暂停等人确认 |

## 三条洞见

1. **量化目标是共通核心**：val loss / 审核评分 / 8 维总分分别是三个领域对"什么是改进"的可测量定义；没有量化指标，循环就退回主观判断。
2. **质量保证机制各有侧重**：硬性回退（退化即 revert）与软性审核（交叉验证、依赖 Agent 智能自行取舍）代表两种保护哲学，详见 [[硬性保护与软性保护]]。
3. **人的参与程度反映领域特征**：ML 的 metric 客观所以可全自动；Skill 好坏需人判断所以每轮暂停；软件开发居中——大部分自动 + 关键节点介入，详见 [[人的参与程度反映领域特征]]。

## 基线与证据口径

- 三方共同的反面基线是 [[ralph-wiggum方法]]（`while true; do cat PROMPT.md | claude; done` 的单 Agent 盲循环）：无外部审核、无量化门槛、无反馈注入——本项目三大改进（交叉审核 / 5 维评分 / 反馈注入）均针对其缺陷设计。
- 证据口径注意："单 Agent 远不如双 Agent 交叉审核""不做回退机制足够"均为作者自报、无对照数据；达尔文.skill 的 8 维构成未在本文披露——统一挂 [[ai工程量化效果声明追踪]]。
