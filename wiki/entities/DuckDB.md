---
type: entity
title: DuckDB
tags: [数据库, OLAP, 列式存储, 嵌入式数据库, 可观测性]
related: [openclaw, 全链路可观测体系, 四层观测架构, 异步非阻塞观测写入, DuckDB选型三理由, 可观测性基础能力论, RDSClaw, 阿里云]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604011800]OpenClawObservability基于DuckDB构建OpenClaw的全链路可观测体系.html"]
---

# DuckDB

DuckDB 是一款嵌入式列式 OLAP（在线分析处理）数据库，以单文件存储、零部署、进程内运行为特征，定位类似"分析型的 SQLite"——此定性为可独立验证的公开背景信息，并与本来源中"与 SQLite 一样单文件零部署"的描述一致。在本 Wiki 的语境中，DuckDB 是《OpenClaw-Observability》一文为 [[openclaw]] 构建全链路可观测体系所选定的存储与分析基座，阿里云亦在 RDS for MySQL 产品线中提供官方的 DuckDB 分析实例。

## 在可观测体系中的角色

- `openclaw-observability` 插件安装并重启 Gateway 后，本地自动创建并加载 DuckDB 单文件。
- Trace 与 Metrics 观测事件经 [[异步非阻塞观测写入]] 批量落库。
- 作为展示分析层 [[观测三视图]] 的查询底座，使观测数据"可分析"而不只是被记录（见 [[可观测性基础能力论]]）。

## 选型论证（对比 SQLite）

最初考虑 SQLite，但海量结构化审计数据的聚合分析表现不佳。对比测试条件：同 Schema、同查询逻辑、50 万条 observations 记录（自报模拟测试，具体数值仅存于文章配图 100075651，正文无数值口径）。详见 [[DuckDB选型三理由]]。

## 文中总结的三优势

1. 列式存储天然适配聚合查询（如过去 7 天 Token 求和、模型分布统计）。
2. `json_extract_string()` 等函数可在查询时直接解析 TEXT 字段中的嵌套 JSON。
3. 与 SQLite 一样单文件零部署，可拉到本地用 CLI 检查，或导出 Parquet 接入下游大数据体系。

## 阿里云生态中的 DuckDB

- RDS for MySQL 提供 DuckDB 分析实例（官方帮助文档：`https://help.aliyun.com/zh/rds/apsaradb-rds-for-mysql/duckdb-analysis-instance/`）。
- 云上 RDS DuckDB 三优势：稳定性（备份/容灾/高可用）、多租户管理（租户隔离/权限控制/资源配额）、弹性性能（应对流量波动查询峰值）——产品能力陈述，带营销色彩。
- [[RDSClaw]] 控制台直接集成可观测插件，开箱即用。

## 开放问题

- 50 万条 SQLite vs DuckDB 对比的具体数值缺失（存于图片，文本不可提取）。
- 文章未给出 DuckDB 表结构 DDL / CREATE TABLE 语句。
- Hook 采集对主链路的开销量化数据缺失。
