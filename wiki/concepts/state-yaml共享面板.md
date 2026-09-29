---
type: concept
title: STATE.yaml 共享面板
tags: [agent, 多agent协调, 状态管理, yaml]
related: [zhiyuanfu, 24h打工人, task-driven对goal-driven, 多源分治策略, 多智能体消息机制]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202605071734]十年老技术开发的AIAgent探索之路.html"]
---
# STATE.yaml 共享面板

[[zhiyuanfu]] 提出的多 Agent 共享状态协调机制。用 YAML 文件作为共享任务面板，每个 Agent 自行读取状态、写回进度，主会话只负责高层目标和验收。

## 解决的问题

主 Agent 单点调度的三大痛点：
1. **上下文过重** — 主 Agent 需维护所有子 Agent 状态
2. **通信变慢** — 消息量随 Agent 数量指数增长
3. **单点故障** — 主 Agent 挂了全局瘫痪

## 结构示例

```yaml
goal: "修复搜索分页 bug"
owner: zhiyuanfu
deadline: 2026-05-08
constraints:
  - 不修改数据库 schema
  - 保持 API 向后兼容
agents:
  profiler:
    status: done
    output: "bug定位: RadioGroup onChange 未绑定"
  backend_dev:
    status: in_progress
    depends_on: [profiler]
  test_runner:
    status: pending
    depends_on: [backend_dev]
```

## 与其他方案的关联

- 与 [[多源分治策略]]（爱奇艺数据库）的核心理念一致——信息按职责分配到稳定位置而非集中
- 与 [[多智能体消息机制]]（AgentScope 的 MsgHub）属于同一问题域的不同解法