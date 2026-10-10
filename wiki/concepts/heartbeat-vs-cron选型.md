---
type: concept
title: Heartbeat vs Cron 选型
tags: [openclaw, 心跳, 定时任务, 架构决策]
related: [心跳机制heartbeat, openclaw]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践.html"]
---

# Heartbeat vs Cron 选型

Heartbeat vs Cron 选型是 [[openclaw]] 对"周期性任务用心跳还是用 cron"给出的明确决策边界。心跳适合：多项检查可合并、需要近期会话上下文、时间允许漂移。cron 适合：精确时间触发、需要与主会话隔离、使用不同模型/思考级别、一次性提醒、输出直达渠道。

官方建议：将同类周期检查合并进 HEARTBEAT.md，而不是为每项检查建多个 cron——合并可摊薄每次心跳的固定 token 成本，并让检查共享会话上下文。该选型框架是 [[心跳机制heartbeat]] 的配套工程决策，体现了 OpenClaw"确定性机制用确定性工具、上下文相关任务用心跳"的分工思想。

来源：[[sources/[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践|[202604130830] 深度解析 OpenClaw]]。
