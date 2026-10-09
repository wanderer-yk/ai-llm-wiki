---
type: concept
title: Agent Teams 最后选项论
tags: [多agent, 协作, harness-engineering]
related: [任务边界三模式, SubAgent与AgentTeams双模式, 子任务CLI化]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604081800]HarnessEngineering让CodingAgent可靠完成长程任务.html"]
---
# Agent Teams 最后选项论

百度Geek说《Harness Engineering》对多 Agent 网状协作（Agent Teams）的定位：因**通信开销与不确定性显著**，仅作最后选项——只有当物理隔离不可行、子任务必须**实时交换中间结果**时才使用。

## 论证逻辑

- 可拆解为无依赖/可拓扑排序/可物理隔离的任务，都应优先用脚本调度的独立子任务（[[任务边界三模式]]），冲突推迟到**静止状态**（所有子任务执行完毕）由 Agent 一次性解决；
- 静止状态解冲突的效果远好于多 Agent 竞态实时协调；
- 因此网状协作的适用空间被压缩到"必须实时交换中间结果"的窄场景。

## 潜在对比点（非矛盾）

ConardLi 在 [[SubAgent与AgentTeams双模式]] 中将 Agent Teams 列为**可用模式之一**；百度版则明确降级为最后选项。二者语境不同（场景、任务类型不同），构成有价值的视角张力，可作 comparison 立项候选。