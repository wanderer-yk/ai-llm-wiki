---
type: concept
title: Trace数据模型
tags: [可观测性, Trace, 数据模型]
related: [四层观测架构, 全链路可观测体系, 观测三视图]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604011800]OpenClawObservability基于DuckDB构建OpenClaw的全链路可观测体系.html"]
---

# Trace数据模型

Trace 数据模型是《OpenClaw-Observability》一文中建模层抽象出的核心字段集合，解决"可观测成立的关键不是采到数据，而是数据能被组织成可理解的执行过程"这一问题（[[四层观测架构]] 第二层）。

## 核心字段（原文保留）

| 字段 | 作用 |
|------|------|
| TraceID / ParentID | 表达父子调用，构成树状链路 |
| Observation Type | 区分 llm / tool / stream 等事件类型 |
| Run Lineage | 关联主任务与并行子任务，避免链路串线 |
| Snapshot | 记录 input_json / output_json，支持事后复盘 |

## 意义

- TraceID/ParentID + Run Lineage 共同解决多任务并行下的链路归属问题。
- Snapshot 使 [[观测三视图]] 的 Trace 视图能完整复盘每次执行的输入输出。
- Observation Type 是聚合分析（Token、耗时、失败率）与安全扫描（行为链告警）的事件分类基础。

## 备注

文章未给出对应的 SQL DDL / CREATE TABLE 语句，以上为正文列出的字段清单。
