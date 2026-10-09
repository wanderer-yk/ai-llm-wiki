---
type: concept
title: Entity+Log+Link 三元组建模
tags: [数据建模, 知识图谱, 时序, 可观测性, UModel]
related: [umodel, code-wiki, RCA三维度汇聚, 快照vs时间线, 代码域vs跨域]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604231830]从可观测到可理解用UModel构建Agent原生的代码知识图谱.html"]
---
# Entity+Log+Link 三元组建模

"Entity+Log+Link 三元组建模"是 [[umodel|UModel]] 的数据模型：**EntitySet 表达实体现态，LogSet 表达时序事件，MetricSet 表达度量指标，Link（含 EntitySetLink 与 DataLink 两种机制）负责组网**。该模型从可观测域（对象、关系、时序的统一建模）迁移到代码理解域。

## 建模构件与代码域映射

| 构件 | 作用 | 代码域内容 |
|------|------|-----------|
| EntitySet | 实体现态 | 5 种，`repo_id` 复合主键，多仓库共存 |
| LogSet | 时序事件 | commit_log、build_log、deploy_log、test_log、incident_log，经 DataLink 关联 EntitySet |
| MetricSet | 度量指标 | 可观测域原语，代码域用法未展开 |
| EntitySetLink | 结构关系 | contains、imports、calls、extends、describes、belongs_to 共 6 种 |

关键标识符：`Entity ID = md5(repo_id:pk_value)`——`repo_id` 参与主键计算实现多仓库同名模块不冲突。

## Log 的分水岭地位

文章主张：**Log 是 Code-WIKI 与所有纯图谱工具的关键分水岭**。代码领域的 Log 远不止 Git Commit——五种 LogSet 经 DataLink 与 Entity 双向关联，支持时间窗查询：

```text
这个模块最近一周被谁修改过？     → commit_log WHERE module_path = X AND time > now()-7d
上次部署之后有没有新增 incident？ → deploy_log JOIN incident_log ON time_window
引入这个依赖之后，构建时间变长了吗？ → build_log GROUP BY week，交叉 commit_log 的依赖变更时间
```

"只看结构不看历史，等于只看了一帧截图。"

## 两个维度的承载

- **代码域 vs 跨域**（[[代码域vs跨域]]）：EntitySetLink 将 `code.module` 连接到 `ops.service`、`event.alert`、`req.issue`，Agent 推理不跳出图谱。
- **快照 vs 时间线**（[[快照vs时间线]]）：LogSet 提供完整时间维度，案例二 RCA（[[RCA三维度汇聚]]）是该模型的端到端演示。

## 待核实细节

五种 EntitySet 的确切名称与字段、Log 类型明细表承载于原文图片，文本不可核验；DataLink 与 EntitySetLink 的机制差异、MetricSet 在代码域的对应物（调用频次/错误率？）均未在文章中展开。