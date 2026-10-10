---
type: concept
title: OpenClaw 定时任务系统
tags: [openclaw, cron, 定时任务, 指数退避, 持久化]
related: [openclaw, 心跳机制heartbeat, heartbeat-vs-cron选型, 任务隔离, 24h打工人, agent可观测性六维度, commandlane四通道]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202603190830]深入理解OpenClaw技术架构与实现原理上.html"]
---
# OpenClaw 定时任务系统

OpenClaw 定时任务系统是以 CronService（src/cron/service.ts）为核心的调度子系统，由 Timer/Store/State 三组件构成并共享 Jobs Collection（默认 `~/.openclaw/cron/jobs.json`）。作者定位其为 OpenClaw 重要基础设施：满足长任务后台单次/周期运行诉求，并与 Heartbeat 交互带来拟人化体验。

## 三种调度类型

```typescript
type CronSchedule =
  | { kind: "at"; at: string }                             // 一次性任务，指定时间
  | { kind: "every"; everyMs: number; anchorMs?: number }   // 周期性任务
  | { kind: "cron"; expr: string; tz?: string; staggerMs?: number }  // Cron表达式
```

`at` 一次性任务执行后自动禁用。

## 关键机制

- **定时器钳制**：`MAX_TIMER_DELAY_MS = 60_000`（最大延迟 60s 防时钟漂移）+ `MIN_REFIRE_GAP_MS = 2_000`（最小重触发间隔）；`armTimer` 计算下次唤醒后 setTimeout。
- **错误指数退避**：30s → 1min → 5min → 15min → 60min 五级退避；卡住任务 2 小时超时自动清理。
- **并发控制**：`maxConcurrentRuns`。
- **持久化**：原子写入（临时文件+rename）、自动备份、mtime 检测热重载；运行日志 `~/.openclaw/cron/runs/<jobId>.jsonl` 自动裁剪（默认 2MB、保留 2000 行）。
- **启动恢复五步**：加载存储 → 清理卡住任务（清除过期 runningAtMs）→ 运行错过任务 → 重算下次运行时间 → 启动定时器。
- **超时控制**：`resolveCronJobTimeoutMs(job)` + `Promise.race` + `abortController.abort()`，超时报 "cron: job execution timed out"。

## 双会话目标

| sessionTarget | payload | 行为 |
|---------------|---------|------|
| `main` | `{kind:"systemEvent", text:string}` | 注入 systemEvent 到主会话 |
| `isolated` | `{kind:"agentTurn", message:string, ...}` | 独立 agent 会话执行 agentTurn（支持模型覆盖、thinking 模式、超时设置） |

isolated 模式是 [[任务隔离]] 的架构级证据。

## 与 Heartbeat 的集成（详见 [[heartbeat-vs-cron选型]]）

`src/gateway/server-cron.ts` 向 CronService 注入三回调：`enqueueSystemEvent`（systemEvent 入队）、`requestHeartbeatNow`（请求立即心跳）、`runHeartbeatOnce`（单次心跳）。Wake 模式两种：`next-heartbeat`（等待下次心跳执行）/ `now`（立即触发心跳）。

## CLI 与七大设计特点

CLI 六命令：`cron status/list/add/edit/remove/run`。

七大设计特点：单一定时器（基于最近任务 nextRunAtMs）/文件持久化（JSON 跨进程共享）/错误隔离/自动恢复/并发控制/进度追踪/Agent 集成（cron 工具可在 agent 中管理任务）。运行日志 jsonl 自动裁剪为 [[agent可观测性六维度]] 的 cost/failure 维度提供素材。单一定时器+文件持久化设计与 [[24h打工人]] 的文件+轮询架构形成有趣对照。