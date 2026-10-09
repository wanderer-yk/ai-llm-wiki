---
type: concept
title: results.tsv 结构化归档
tags: [autoresearch, 可观测性, 落盘, 归档]
related: [smallnest-autoresearch, file-as-progress状态持久化, agent可观测性六维度, 双通道输出设计, 5维度量化评分]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604201800]我把Karpathy的AutoResearch搬到了软件开发领域效果炸了.html"]
---
# results.tsv 结构化归档

results.tsv 结构化归档是 [[smallnest-autoresearch]] Phase 4 的落盘设计：每个 Issue 处理完毕后以一行 TSV 记录追加到全量汇总文件 results.tsv，同时更新 workflows/issue-N/ 下的分目录日志（log.md 总日志记录迭代与评分历史，iteration-N-codex.log / iteration-N-claude.log / test-N.log 分轮存档）。它把"跑过哪些任务、几轮收敛、质量如何"变成可复查的结构化数据，是 [[file-as-progress状态持久化]] 与 [[agent可观测性六维度]]（step/tool/cost 维度）的直接工程实例。

## 字段与示例（原文保留）

```text
timestamp   issue_number  issue_title  status     iterations  tests_passed  score  branch_name
2026-04-01  15            event proto  completed  2           true          9.1    feature/issue-15
2026-04-01  6             web UI       completed  5           true          15     feature/issue-6
```

## 观察点与疑点

- **评分趋势观察**（最佳实践第 3 条）：每次迭代评分记录在 log.md，观察是否稳步上升——把收敛轨迹本身作为可观测指标；日志与结构化汇总并行的做法与 [[双通道输出设计]] 同构。
- **score=15 数据疑点**：示例行 issue 6 的 score=15 与 [[5维度量化评分]] 满分 10、达标线 9.0 的体系冲突；文中解释为"Claude 和 Codex 均评分最高"，疑为双审核者分数聚合字段或展示缺陷，字段语义待确认。
- **status 状态枚举**：状态定义表由文章图片承载，正文仅出现 completed。
