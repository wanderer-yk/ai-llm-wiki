---
type: concept
title: SubAgent 通告机制
tags: [openclaw, subagent, 通告队列, 推式通知, hook]
related: [openclaw, openclaw-subagent架构, subagent设计哲学, SubAgent生命周期工具化, 全生命周期hook机制]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202603190830]深入理解OpenClaw技术架构与实现原理上.html"]
---

# SubAgent 通告机制

SubAgent 通告机制是 [[openclaw]] 子智能体完成运行后向请求者聊天渠道**推送**结果摘要的机制（push-based，替代返回值轮询）。它由五步通告流程、三路由模式 × 三投递通道、嵌套冒泡与祖父回退、生命周期四钩子、通告队列五模式组成，覆盖《[[sources/[202603190830]深入理解OpenClaw技术架构与实现原理上|深入理解OpenClaw技术架构与实现原理（上）]]》3.7.2 通告部分、3.7.4–3.7.5 节。

## 通告五步流程（subagent-announce.ts:1053-1382）

```text
1. 等待运行结束：等待嵌入式运行完成 / agent.wait / 读取最新助手回复或工具结果
2. 构建通告消息：提取结果文本 / 运行统计（运行时间、token使用量、成本）/ 状态标签（成功/超时/失败）
3. 确定投递目标：bound 模式（线程绑定路由）/ hook 模式（subagent_delivery_target）/ fallback 模式（请求者来源）
4. 投递通告：直接投递（gateway.agent() / gateway.send()）/ 队列投递（请求者忙时入队）/ 嵌套处理（请求者是子智能体则向上冒泡）
5. 清理：更新会话标签 / 删除子会话（cleanup: "delete"）/ 触发 subagent_ended 钩子
```

**嵌套冒泡与祖父回退**（subagent-announce.ts:1222-1253）：请求者子智能体已结束且父会话存活→继续向父注入；父会话已删除→经 `resolveRequesterForChildSession` 回退到祖父。

## 三级权限视图（subagents 工具，subagents-tool.ts:342-680）

`subagents` 工具三动作：`list`（活跃和最近的子运行）/ `kill <target>`（停止，支持级联）/ `steer <target> <message>`（发送指导消息）。`resolveRequesterKey` 三级视图：

- 主智能体：看自己的子运行
- 编排者子智能体（callerDepth < maxSpawnDepth）：看自己的子运行
- 叶子子智能体：看父的子运行（兄弟运行，spawnedBy 回退）

## 级联停止递归（subagents-tool.ts:276-318）

```typescript
async function cascadeKillChildren(params: {
  cfg: ReturnType<typeof loadConfig>;
  parentChildSessionKey: string;
  cache: Map<string, Record<string, SessionEntry>>;
}): Promise<{ killed: number; labels: string[] }> {
  // 遍历 listSubagentRunsForRequester(parentChildSessionKey)
  // 对未结束（!run.endedAt）的运行执行 killSubagentRun
  // 递归 cascadeKillChildren({ parentChildSessionKey: run.childSessionKey }) 停止孙运行
}
```

## 生命周期四钩子（3.7.4）

| 钩子名称 | 触发时机 | 用途 |
|---------|---------|------|
| `subagent_spawning` | 派生前 | 准备线程绑定，验证权限 |
| `subagent_spawned` | 派生成功后 | 记录日志，更新UI状态 |
| `subagent_delivery_target` | 确定投递目标 | 自定义通告路由 |
| `subagent_ended` | 运行结束 | 清理资源，发送告别消息 |

## Discord 线程绑定扩展示例（extensions/discord/src/subagent-hooks.ts）

```javascript
// 注册钩子
export function registerDiscordSubagentHooks() {
  const hookRunner = getGlobalHookRunner();
  hookRunner.register("subagent_spawning", async (event, ctx) => {
    // 创建 Discord 线程并绑定到子智能体会话
    const thread = await createDiscordThread({
      channelId: event.requester.to,
      name: `Subagent: ${event.label || event.agentId}`,
    });
    return { status: "ok", threadBindingReady: true, threadId: thread.id };
  });
  hookRunner.register("subagent_delivery_target", async (event, ctx) => {
    // 将通告路由到绑定的 Discord 线程
    const binding = getThreadBinding(event.childSessionKey);
    if (binding) {
      return { origin: { channel: "discord", to: binding.channelId, threadId: binding.threadId } };
    }
    return { origin: event.requesterOrigin };
  });
}
```

## 通告队列五模式（3.7.5，subagent-announce.ts:645-702）

`maybeQueueSubagentAnnounce` 完整派发逻辑（空格伪影已还原）：

```javascript
async function maybeQueueSubagentAnnounce(params): Promise<"steered" | "queued" | "none"> {
  const queueSettings = resolveQueueSettings({ cfg, channel, sessionEntry });
  const isActive = isEmbeddedPiRunActive(sessionId);

  // 1. 尝试 steer 模式
  const shouldSteer = queueSettings.mode === "steer" || queueSettings.mode === "steer-backlog";
  if (shouldSteer) {
    const steered = queueEmbeddedPiMessage(sessionId, params.triggerMessage);
    if (steered) return "steered";
  }

  // 2. 尝试 followup/collect 模式
  const shouldFollowup =
    queueSettings.mode === "followup" ||
    queueSettings.mode === "collect" ||
    queueSettings.mode === "steer-backlog" ||
    queueSettings.mode === "interrupt";

  if (isActive && shouldFollowup) {
    enqueueAnnounce({
      key: buildAnnounceQueueKey(canonicalKey, origin),
      item: { announceId, prompt, sessionKey, origin },
      settings: queueSettings,
      send: sendAnnounce,
    });
    return "queued";
  }

  return "none";
}
```

队列模式 × 派发阶段矩阵：

| mode | steer 阶段 | 入队阶段 |
|------|-----------|---------|
| steer | ✅ | ✗ |
| steer-backlog | ✅（失败回退） | ✅ |
| followup | ✗ | ✅ |
| collect | ✗ | ✅ |
| interrupt | ✗ | ✅ |

派发顺序：先尝试 steer（steer 与 steer-backlog 参与），steer 失败则 steer-backlog 回退入队；`isActive && shouldFollowup` 才入队，全不中返回 none。`enqueueAnnounce` 载荷：`key = buildAnnounceQueueKey(canonicalKey, origin)`，`item = { announceId, prompt, sessionKey, origin }`，附 settings 与 send 回调。

## 通告抑制

`suppressAnnounceReason`（"steer-restart" / "killed"）可抑制通告——steer-restart 与 3.2 推理循环 steer 机制的关联待下文/下篇确认。

## 关联

- [[SubAgent生命周期工具化]]：通告即工具化的生命周期管理——结果回传不经轮询而由系统推送
- [[全生命周期hook机制]]：四钩子体系与 HermesAgent 系 hook 机制跨源互证
- [[心跳机制heartbeat]]：子智能体系统提示明令禁止心跳与主动行为，与主智能体心跳机制形成设计对照
- [[openclaw-subagent架构]]：通告机制依托其会话键与注册表
- [[subagent设计哲学]]：推式通知是五哲学之一

## 开放问题

- followup / collect / interrupt 三模式在共享入队门控之外的语义差异（collect 是否聚合多条、interrupt 是否真中断当前运行）
- steer-backlog 入队后的 backlog 消费时机与重放机制
- `/unfocus` 命令归属（核心 vs 扩展）
