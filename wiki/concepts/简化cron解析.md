---
type: concept
title: 简化cron解析
tags: [定时任务, cron, agent框架, 取舍]
related: [有意取舍边界声明, 入站消息总线, 同步异步双路径, 轻量级单进程agent框架]
created: 2026-09-30
updated: 2026-09-30
sources: ["[202604241629]800行代码实现OpenClaw的Tool消息总线子Agent管理架构.html"]
---
# 简化cron解析

简化cron解析指基于 `setInterval` 的近似 cron 实现：只支持常见的**等间隔模式**，不支持「每周三 14:30」这类精确时间点语义（`setInterval` 本身做不到）；**复杂表达式静默降级为每分钟执行且不报错**——作者原话：「这是一个已知的精度妥协」。该设计是苏雄 800 行框架 CronService 的核心，也是 [[有意取舍边界声明]] 中 cron 精度维度的完整实例。

## 支持的解析规则

| 表达式 | 语义 |
|--------|------|
| `*/N * * * *` | 每 N 分钟 |
| `0 */N * * *` | 每 N 小时 |
| `* * * * *` | 每分钟 |
| `0 * * * *` | 每小时 |
| `0 0 * * *` | 每天 |
| 非 5 段 / 复杂表达式 | 静默降级为每分钟（60 秒默认间隔） |

## 实现（由导出件 overlap 拼合并按语义修正损伤）

```typescript
private parseCronInterval(expr: string): number {
  const parts = expr.trim().split(/\s+/);
  if (parts.length !== 5) return 60_000;
  const [minute, hour] = parts;
  // */N * * * * → 每 N 分钟
  if (minute.startsWith("*/") && hour === "*") {
    const n = parseInt(minute.slice(2), 10);
    if (!isNaN(n) && n > 0) return n * 60_000;
  }
  // 0 */N * * * → 每 N 小时
  if (minute === "0" && hour?.startsWith("*/")) {
    const n = parseInt(hour.slice(2), 10);
    if (!isNaN(n) && n > 0) return n * 3600_000;
  }
  if (minute === "*" && hour === "*") return 60_000;     // 每分钟
  if (minute === "0" && hour === "*") return 3600_000;   // 每小时
  if (minute === "0" && hour === "0") return 86400_000;  // 每天
  return 60_000; // 复杂表达式降级为每分钟
}
```

## 工具层接口

CronTool 以 `add`/`list`/`remove` 三个 action 作为 CronService 的 function calling 接口；服务层支持 `every_seconds`（直接转毫秒）与 `cron_expr`（解析为近似间隔）两种模式。定时触发回调经 `bus.publish()` 写入 `system` channel（见 [[入站消息总线]]），构成 [[同步异步双路径]] 的异步源之一。精确 cron 语义需引入 cron 解析库（作者明示）。
