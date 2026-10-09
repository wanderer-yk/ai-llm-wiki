---
type: concept
title: Issue 选择策略
tags: [autoresearch, github-issue, 任务选择, 调度]
related: [smallnest-autoresearch, 四阶段优化循环, agent权限边界清单, 5维度量化评分]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604201800]我把Karpathy的AutoResearch搬到了软件开发领域效果炸了.html"]
---
# Issue 选择策略

Issue 选择策略是 [[smallnest-autoresearch]] 决定"Agent 自动认领哪些 GitHub Issue"的三件套机制（由 issue-selector.md 承载）：排除规则过滤不应处理的 Issue，优先级公式给出处理顺序，复杂度评估预估任务难度。它回答的是 [[autoresearch软件开发迁移]] 后新出现的问题——"选什么任务进入循环"，且这一决策被收敛在循环之外的确定性规则中，而非交给 Agent 自由选题。

## 排除规则（原文保留）

不处理以下 Issue：
- 含 `wontfix` / `duplicate` / `invalid` / `blocked` / `needs discussion` / `on hold` / `external` 标签
- 标题含 `[WIP]` / `[DRAFT]`
- 正文含 `DO NOT IMPLEMENT`
- 已有 PR 关联

## 优先级公式（原文保留）

`分数 = 基础权重(15) + 标签权重 + 类型权重 + 时间因子`

| 因子 | 权重 |
|------|------|
| 标签 | critical(100) > high(50) > medium(20) > low(10) |
| 类型 | bug(30) > feature(20) > refactor(10) > test(5) > docs(3) |
| 时间 | 新 Issue +10 / 陈年 Issue +15 / 近期更新 +5 |

## 复杂度评估

评估细则由文章图片承载、未能转为文字。实战案例显示其产出为"中等/高"分档，并与迭代轮数正相关：Issue #21（复杂度中等，涉及 Job 结构体扩展、超时控制、API 增强）3 轮达标；Issue #6（复杂度高，涉及多个模块、需要设计决策）5 轮达标。

## 关联

- 排除规则 + 优先级公式 + [[agent权限边界清单]] 共同构成 [[smallnest-autoresearch]] 的"循环外控制面"：一个管"先做哪个"，一个管"能做什么"。
- 复杂度分档与 [[5维度量化评分]] 收敛轮数的正相关，是任务难度定价与迭代预算估算的观察线索。
