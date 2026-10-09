---
type: concept
title: 代码域 vs 跨域
tags: [跨域关联, UModel, 维度二, 工具链孤岛]
related: [umodel, Entity+Log+Link三元组建模, RCA三维度汇聚, 代码理解的五种范式]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604231830]从可观测到可理解用UModel构建Agent原生的代码知识图谱.html"]
---
# 代码域 vs 跨域

"代码域 vs 跨域"是 [[umodel|UModel]] 与既有代码理解方案的关键差异维度之二：**从 Agentic Search 到 Code Graph+RAG 的所有方案止步代码域，UModel 通过 EntitySetLink 将代码实体连接到运维/事件/需求域实体，Agent 沿链路推理不需跳出图谱**。

## 批判性例证

既有方案无法回答："这个模块对应的生产服务上周出过几次告警？"——因为运维域数据根本不在其索引/图里。

## 工程化叙事：五孤岛统一入图

文章以工具链孤岛叙事展开：需求在 Jira、代码在 Git、构建在 Jenkins、运行在 K8s、告警在监控——UModel 的主张是"所有实体活在同一个图里"，跨域连接示例：

```text
EntitySetLink: code.module ↔ ops.service / event.alert / req.issue
```

## 实战价值

案例二 RCA（[[RCA三维度汇聚]]）是该维度的端到端演示：从 `service-vibeops` 生产告警（运维域）出发，回落到代码模块（代码域），再经 commit_log（历史域）定位根因——跨域关联使这条推理链在一次图谱会话内完成。

## 与本 Wiki 相关内容的关联

- 两流派共同失效三类问题中的"生产 SLO 破线是否代码变更所致"直接由本维度回应。
- 与有赞 [[traceai排障]]（Agent 结合代码问答 + 链路追踪日志排障）同主题：均为打通代码域与运行域的实践，但 UModel 以统一图实现，有赞以工具组合实现。