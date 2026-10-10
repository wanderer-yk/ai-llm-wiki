---
type: concept
title: 心跳机制（Heartbeats）
tags: [openclaw, 心跳, 主动agent, 定时任务]
related: [openclaw, heartbeat-vs-cron选型, openclaw工作区md文件族, openclaw双层记忆系统, 全生命周期hook机制]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践.html"]
---

# 心跳机制（Heartbeats）

心跳机制（Heartbeats）是 [[openclaw]] 定期唤醒 Agent 主动执行定时任务的机制：按周期检查 HEARTBEAT.md 中的任务清单（查邮件/日历/提及/天气等），无事可做时精确回复 `HEARTBEAT_OK` 而不产生噪音。它是 OpenClaw 从"被动应答"走向"主动服务"的核心设计。

默认心跳提示词（原文）：

```markdown
Read HEARTBEAT.md if it exists (workspace context). Follow it strictly.
Do not infer or repeat old tasks from prior chats. If nothing needs attention, reply HEARTBEAT_OK.
```

关键设计：

- **空文件省 token**：HEARTBEAT.md 保持为空（或仅注释）可跳过心跳 API 调用，是显式的省钱设计；心跳检查须控制规模以限 token 消耗。
- **轮询节奏**：默认每日 2-4 次轮询（邮件/日历/提及/天气）。
- **状态追踪**：`memory/heartbeat-state.json` 记录各项检查的最后时间戳：

```json
{
  "lastChecks": {
    "email": 1703275200,
    "calendar": 1703260800,
    "weather": null
  }
}
```

- **静默时段**：深夜 23:00-08:00 保持沉默，不主动联系用户。
- **心跳期记忆维护**：每隔几天用心跳回顾 `memory/YYYY-MM-DD.md`，将值得长期保留的洞察蒸馏进 MEMORY.md 并清理过时项——"Daily files are raw notes; MEMORY.md is curated wisdom"（与 [[openclaw双层记忆系统]] 衔接）。
- **Harness 规定动作**：HEARTBEAT.md 是 Harness 强加给 Agent 的"规定动作"（强制定期巡检），而非模型自发行为；与 BOOTSTRAP.md 并列。

与 cron 的选型边界详见 [[heartbeat-vs-cron选型]]。

来源：[[sources/[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践|[202604130830] 深度解析 OpenClaw]]。
