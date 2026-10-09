---
type: concept
title: UModel 六阶段构建流水线
tags: [流水线, 图谱构建, UModel, 增量构建, 架构发现]
related: [umodel, vibeops-agents, tree-sitter, 架构发现非社区检测, AST确定性提取+LLM语义增强分层置信度, 两步查询模式]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604231830]从可观测到可理解用UModel构建Agent原生的代码知识图谱.html"]
---
# UModel 六阶段构建流水线

"UModel 六阶段构建流水线"是《[202604231830]从可观测到可理解用UModel构建Agent原生的代码知识图谱》给出的代码知识图谱完整构建流程：**DETECT → EXTRACT → RESOLVE → BUILD → SYNC → SERVE**，是 [[umodel|UModel]] 维度一"确定性 vs 概率性"的工程实现全貌。

## DETECT：增量变更检测

SHA256 内容指纹 + 构建缓存比对。实证：[[vibeops-agents]]（~2375 个 Go 文件）增量构建仅需处理几十个变更文件，秒级 vs 全量数分钟（自报数据）。

## EXTRACT：AST + LLM 双轨提取

- **AST 轨道**：[[tree-sitter]]（PEG 增量解析器，40+ 语言），`tags.scm` 规则跨语言一致提取定义/引用/结构/导入/调用/继承六类关系，置信度全部 1.0。
- **LLM 轨道**：模块摘要（定位为 **Agent 上下文注入片段而非人读文档**）、文档-代码关联、组件归属；标注 `__extraction_method__: INFERRED` + 置信度；Agent 按场景选信任阈值——RCA 优先高置信度、探索可放宽。

## RESOLVE：跨文件确定性解析

单文件 AST 无法解决的跨文件引用，由四类确定性解析完成、不依赖 LLM：

```
Go import github.com/org/repo/pkg/a2a → module_path pkg/a2a
方法 receiver type (s *Server) → 归属 code.type pkg/server.Server
调用 s.HandleRequest() → pkg/server.Server.HandleRequest
接口实现 type Adapter struct implements Handler → extends 关系
```

## BUILD：架构发现

明确"Louvain/Leiden 发现的是聚类，不是架构"（详见 [[架构发现非社区检测]]）。四步流程：加权有向图构建（calls > imports > extends）→ 依赖方向层次分析 → Leiden 有向图功能簇发现（resolution 控制粒度，~150 模块→~15 组件）→ 依赖方向三层标注（API/Gateway、Service/Business、Infrastructure/Utility）+ LLM 命名与文档交叉验证。

## SYNC：图谱写入

`starops` CLI 写入侧：

```bash
Entity 写入: starops umodel post-logs → __entity logstore
Topo 写入:   starops umodel post-logs → __topo logstore
Schema 同步: starops umodel sync（注册 EntitySet/Link 定义）
```

后端基于 SLS 存储引擎（高吞吐写入、秒级查询、graph-match、SQL 聚合、全文检索）。

## SERVE：查询服务

两步查询模式（graph-match 走拓扑取 id、entity 批量拉业务字段）+ 聚合统计类查询直接 SQL 查 `__topo` logstore（详见 [[两步查询模式]]）。当前规模自报约 11000 实体、19000 条边，单次查询端到端延迟百毫秒。

## 证据强度

流水线机制描述为作者自报口径；增量耗时、组件粒度等数字均为内部项目数据、无第三方口径；跨域关联图与整体流水线图为图片、文本不可核。