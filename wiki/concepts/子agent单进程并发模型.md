---
type: concept
title: 子agent单进程并发模型
tags: [子agent, 并发模型, promise, nodejs, agent框架]
related: [子agent工具排除机制, 入站消息总线, 互斥锁与暂存队列, llm并发状态感知, agent-loop, pi-agent, 轻量级单进程agent框架, claude-code]
created: 2026-09-30
updated: 2026-09-30
sources: ["[202604241629]800行代码实现OpenClaw的Tool消息总线子Agent管理架构.html"]
---
# 子agent单进程并发模型

子agent单进程并发模型指**每个子 Agent 是一个 Promise**、共享同一 Node.js 事件循环的单进程并发方案——无多进程、无 Worker。每个子 Agent 拥有独立 `AgentLoop` 实例与自己的 ReAct 循环，**无历史上下文、无持久记忆**（每次从零开始），单任务生命周期（处理完一个任务即结束）。该模型是苏雄 800 行框架的第三块核心设计（「Promise 并发的子 Agent」），与 [[pi-agent]] 的单循环形态和 [[claude-code]] 的子 Agent 工具受限设计形成对照。

## 生命周期实现

`spawn()` 自增 ID、异步启动、立即返回 ID 不阻塞调用方，`promise.finally()` 自动清理（导出件疑有渲染损伤：`...` 疑折叠为 `.`；label 赋值疑丢失 `??`）：

```javascript
spawn(params: { task: string; label?: string; ... }): string {
  const id = `subagent-${++this.counter}`;
  const label = params.label ? `Task ${this.counter}`;
  const promise = this.runSubagent(id, params.task, label, ...);
  promise.finally(() => {
    this.runningTasks.delete(id);
  });
  this.runningTasks.set(id, { id, label, promise });
  return id;
}
```

`buildMessages` 回调只传当前任务、不带历史：

```javascript
buildMessages: (_history, userMessage) => [
  { role: "user" as const, content: userMessage },
],
```

完成回传经 `bus.publish()` 写入 `system` channel，成功和失败走同一路径、仅 content 不同：

```javascript
await this.bus.publish({
  channel: "system",
  senderId: "subagent",
  chatId: `${originChannel}:${originChatId}`,
  content: `[Subagent "${label}" (${id}) completed]\n\n${result}`,
});
```

## 关键参数与协议

- **迭代上限不对称**：子 Agent 最大迭代 15 次，主 Agent 10 次——暗示子 Agent 被预期承担更长的独立执行链。
- **`runningTasks` 自动清理**：Map 条目结构 `{ id, label, promise }`，`promise.finally()` 删除已完成任务。
- **结果注入协议**：子 Agent 结果包装为 `"[SYSTEM NOTIFICATION - Subagent Result]"` + 原始内容 + `"Please summarize the above subagent result for the user."` 三段 join 注入主 history，由主 Agent 摘要转述给用户（详见 [[互斥锁与暂存队列]]）。
- **触发接口**：SpawnTool 经 function calling 调用，返回子 Agent ID + 运行中数量（见 [[llm并发状态感知]]）。
- **工具边界**：子 Agent 工具集为主 Agent 受限子集（见 [[子agent工具排除机制]]）。

## 适用边界（作者明示）

每次 spawn 从零开始，适合一次性并行任务（搜索、分析、计算），**不适合需要跨任务积累上下文的场景**。
