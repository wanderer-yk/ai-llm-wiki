---
type: concept
title: RCA 三维度汇聚
tags: [RCA, 根因分析, 三维度, 案例, UModel]
related: [umodel, Entity+Log+Link三元组建模, 代码域vs跨域, 快照vs时间线, traceai排障, code-wiki, vibeops-agents]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604231830]从可观测到可理解用UModel构建Agent原生的代码知识图谱.html"]
---
# RCA 三维度汇聚

"RCA 三维度汇聚"指《[202604231830]从可观测到可理解用UModel构建Agent原生的代码知识图谱》案例二演示的根因定位模式：**结构（代码图谱）+ 历史（commit_log）+ 生产（告警）三个维度在同一图谱内汇聚，支撑 Agent 端到端定位根因**。它是 [[umodel|UModel]] 三维度建模（[[Entity+Log+Link三元组建模]]）的实战验证。

## 案例二四步推理链（Demo）

```bash
# 1. 从运维实体定位代码模块
$ code-wiki query context pkg/server
# 2. 追踪调用链，定位可能出错的下游
$ code-wiki query callees pkg/server.handleRequest
# 3. 结合 commit_log 发现 a2a 模块 2 小时前有变更
#    author=xxx, message=refactor adapter interface
# 4. 确认变更影响
$ code-wiki query impact pkg/a2a
# → 根因：a2a 接口重构影响了 server 调用链，检查接口兼容性
```

三维度对应关系：步骤 1-2 为**结构维度**（模块上下文与调用链），步骤 3 为**历史维度**（变更时间线），步骤 0（告警触发，`service-vibeops error_rate > 5%`）为**生产维度**。

## 确定性前提

该推理链的可靠性依赖 [[AST确定性提取+LLM语义增强分层置信度]]：callees 关系若靠 LLM 猜测，整条推理链不可靠；AST 确定性关系可无条件信任。

## 与相关实践的对照

- 有赞 [[traceai排障]]：同为"Agent 结合代码与运行时信息排障"，但有赞以代码问答+链路追踪日志工具组合实现，UModel 以统一图谱实现——同主题不同实现路径。
- 与通用根因分析的区别：传统 RCA 需要人在监控台、代码库、Git 历史三个工具间切换并人工关联时间线；本模式将关联动作交给图谱的时间窗 JOIN（[[快照vs时间线]]）。

## 证据强度

案例为作者 VibeCoding 演示 Demo，非生产事故复盘实录；推理链结构与命令接口可核验，输出结论不可独立复核。