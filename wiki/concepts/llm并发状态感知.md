---
type: concept
title: llm并发状态感知
tags: [agent设计, 工具设计, 并发, situational-awareness]
related: [子agent单进程并发模型, 子agent工具排除机制, 互斥锁与暂存队列, 轻量级单进程agent框架]
created: 2026-09-30
updated: 2026-09-30
sources: ["[202604241629]800行代码实现OpenClaw的Tool消息总线子Agent管理架构.html"]
---
# llm并发状态感知

llm并发状态感知指**将运行时并发状态编码进工具返回值**，使 LLM 在决策时对系统并发状况有感知（situational awareness）的设计手法。苏雄 800 行框架中的实例：SpawnTool 的返回值不仅包含新创建的子 Agent ID，还包含**当前运行中的子 Agent 数量**——设计意图是「让 LLM 对并发状态有感知」，从而决定是否继续 spawn 更多并行任务或等待。

## 接口规格

- 调用方式：function calling
- 入参：`task`（必填，任务描述）+ `label`（可选）
- 返回值：子 Agent ID + 当前运行中子 Agent 数量

## 设计意义

该手法把工具返回值从「操作结果回执」升级为「环境状态反馈」，与 [[子agent单进程并发模型]] 的 `runningTasks` Map（条目 `{ id, label, promise }`）直接对应：Map 是权威状态源，SpawnTool 返回值是其对模型的投影。对于无独立调度器的对话驱动主循环（REPL 场景，见 [[互斥锁与暂存队列]]），这是让模型参与并发治理的最低成本途径。遗留记录项：`label` 参数的具体用途文中未展开说明。
