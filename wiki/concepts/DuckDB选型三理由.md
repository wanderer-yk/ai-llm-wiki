---
type: concept
title: DuckDB 选型三理由
tags: [数据库选型, DuckDB, OLAP, 可观测性]
related: [DuckDB, 全链路可观测体系, RDSClaw]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604011800]OpenClawObservability基于DuckDB构建OpenClaw的全链路可观测体系.html"]
---

# DuckDB 选型三理由

DuckDB 选型三理由是《OpenClaw-Observability》一文在存储引擎选型章节给出的技术论证：面对海量结构化审计数据的聚合分析需求，[[DuckDB]] 优于最初考虑的 SQLite。

## 选型背景与对比测试

- 最初考虑 SQLite，但其在海量审计数据聚合分析上"表现不尽如人意"。
- 对比测试条件：同 Schema、同查询逻辑、**50 万条 observations 记录**（自报模拟测试）。
- 可信度注记：测试结果数值仅存于文章配图（imgfileid=100075651），正文无数值口径，只能保留"自报 SQLite 落后"的定性结论；该声明应纳入 [[ai工程量化效果声明追踪]]。

## 三理由

1. **列式存储天然适配聚合查询**：如过去 7 天 Token 求和、模型分布统计等审计场景典型查询。
2. **JSON 查询时解析**：`json_extract_string()` 等函数可在查询时直接解析 TEXT 字段中的嵌套 JSON，无需预展开 schema。
3. **单文件零部署可移植**：与 SQLite 一样单文件零部署，可拉到本地用 CLI 检查，或导出 Parquet 接入下游大数据体系。

## 与公开特性的一致性

上述三优势与 DuckDB 的公开特性一致，可信度较高；但 SQLite 对比因缺少数值口径，仅可作方向性参考。云上部署形态（RDS DuckDB 三优势）见 [[RDSClaw]] 页。
