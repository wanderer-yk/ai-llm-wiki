---
type: entity
title: val loss 改善才 commit
tags: [val-loss, 单指标门控, 只保留改进, karpathy, autoresearch]
related: [karpathy, autoresearch, autoresearch三原则, 硬性保护与软性保护, 5维度量化评分]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604201800]我把Karpathy的AutoResearch搬到了软件开发领域效果炸了.html"]
---
# val loss 改善才 commit

"val loss 改善才 commit"是 Karpathy autoresearch 的核心门控机制：只有验证损失（val loss）改善的代码变动才被提交，否则 `git revert` 回滚——"只保留可测量的改进，其余全部回滚"，绝不将就。它是整个 AutoResearch 方法谱系中"量化目标 + 只保留改进"两原则的原型实现。

## 运行参数

- 判断标准：val loss 是**唯一**判断标准（量化目标原则）
- 实验节奏：单 GPU、5 分钟训练预算，每小时约 12 次实验，一夜可收获上百轮自动优化
- 保护方式：退化即回滚，属 [[硬性保护与软性保护]] 中的**硬性保护**阵营
- 人类角色：只维护 program.md 研究章程，不逐轮介入

## 迁移变形

在软件开发迁移中，单一客观的 val loss 无法直接照搬（软件质量无单一 metric），故 [[smallnest-autoresearch]] 将其改造为 [[5维度量化评分]]（达标线 9.0 才合并）；花叔的 [[达尔文skill]] 则改造为 8 维总分。三个变体共享"指标不达标即不保留"的门控结构，差异仅在指标形态与保护力度（硬回退 vs 软审核）。
