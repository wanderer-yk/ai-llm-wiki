---
type: concept
title: 快照 vs 时间线
tags: [时间维度, UModel, LogSet, 维度三, 历史分析]
related: [umodel, Entity+Log+Link三元组建模, RCA三维度汇聚, 活文档机制]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604231830]从可观测到可理解用UModel构建Agent原生的代码知识图谱.html"]
---
# 快照 vs 时间线

"快照 vs 时间线"是 [[umodel|UModel]] 与既有方案的关键差异维度之三：**CodeIndex 是当前快照，Code Graph 叠加 commit 历史只算部分时间维度，UModel 以五种 LogSet 提供完整时间维度**。文章的凝练表述："只看结构不看历史，等于只看了一帧截图。"

## 时间维度光谱

| 方案 | 时间维度 |
|------|----------|
| CodeIndex（[[cursor|Cursor]] 等） | 当前快照，无历史 |
| Code Graph + RAG（Augment Context Lineage 等） | commit 历史/diff 摘要，部分维度 |
| [[umodel|UModel]] | commit_log、build_log、deploy_log、test_log、incident_log 五种 LogSet 完整时间线 |

## 时间窗查询能力

LogSet 经 DataLink 与 Entity 双向关联后，支持跨 Log 时间窗查询：

```text
上次部署之后有没有新增 incident？
  → deploy_log JOIN incident_log ON time_window
引入这个依赖之后，构建时间变长了吗？
  → build_log GROUP BY week，交叉 commit_log 的依赖变更时间
```

## 实战价值

案例二 RCA 的第 3 步是本维度的关键演示：Agent 从 commit_log 发现 a2a 模块 2 小时前有"refactor adapter interface"变更，从而将告警根因锁定为接口重构——没有时间线，这一步推理不存在。

## 关联

- `ingest --incremental` 增量同步保证时间线持续生长，与腾讯 [[活文档机制]]（archive 命令自动同步规范文档）同构：产物随代码自动更新。
- [[Agent自主维护图谱]] 愿景中的"过期数据巡检"是时间维度维护的延伸。