---
type: concept
title: OpenClaw SubAgent 架构
tags: [openclaw, subagent, 会话隔离, 多智能体]
related: [openclaw, subagent通告机制, subagent设计哲学, 任务隔离, SubAgent记忆隔离, agent-control-plane]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202603190830]深入理解OpenClaw技术架构与实现原理上.html"]
---

# OpenClaw SubAgent 架构

OpenClaw SubAgent（子智能体）是从现有 Agent 运行中生成的**后台独立运行实例**，具备四大特征：会话隔离、后台执行、结果通告、嵌套支持。其架构由结构化会话键系统、持久化注册表、派生流水线、按深度分级的系统提示与工具策略组成，完整覆盖《[[sources/[202603190830]深入理解OpenClaw技术架构与实现原理上|深入理解OpenClaw技术架构与实现原理（上）]]》3.7.1–3.7.3 节。

## 会话键系统（三级深度）

| 深度 | 会话键格式 | 角色 | 能否派生子智能体 |
|------|-----------|------|----------------|
| 0 | `agent:<id>:main` | 主智能体 | 总是可以 |
| 1 | `agent:<id>:subagent:<uuid>` | 子智能体（编排者） | 仅当 `maxSpawnDepth >= 2` |
| 2 | `agent:<id>:subagent:<uuid>:subagent:<uuid>` | 子子智能体（叶子工作者） | 永远不能 |

深度通过会话键结构（而非独立字段）跟踪嵌套层级，这是官方确认的关键设计决策。

## 深度计算三路回退（subagent-depth.ts:124-176）

```typescript
export function getSubagentDepthFromSessionStore(
  sessionKey: string | undefined | null,
  opts?: { cfg?: OpenClawConfig; store?: Record<string, SessionDepthEntry> }
): number {
  // 从会话键解析基础深度
  const fallbackDepth = getSubagentDepth(raw);

  // 读取会话存储中的 spawnDepth 字段
  const storedDepth = normalizeSpawnDepth(entry?.spawnDepth);

  // 或通过 spawnedBy 链递归计算父深度 + 1
  const parentDepth = depthFromStore(spawnedBy);
  return parentDepth + 1;
}
```

## 注册表五职责（subagent-registry.types.ts:6-35 + subagent-registry.ts:488-518）

①运行跟踪（活跃+历史记录）②生命周期监听（onAgentEvent 监听 start/error/end）③持久化（磁盘落盘，网关重启可恢复）④级联停止（停父自动停子）⑤孤儿检测（恢复时清理缺失会话条目的运行）。

核心数据结构（verbatim）：

```typescript
export type SubagentRunRecord = {
  runId: string;                     // 运行标识符
  childSessionKey: string;           // 子会话键
  requesterSessionKey: string;       // 请求者会话键
  requesterOrigin?: DeliveryContext; // 请求者来源（渠道、账号等）
  task: string;                      // 任务描述
  cleanup: "delete" | "keep";        // 清理策略
  label?: string;                    // 显示标签
  model?: string;                    // 使用的模型
  runTimeoutSeconds?: number;        // 运行超时
  spawnMode?: SpawnSubagentMode;     // 运行模式
  createdAt: number;                 // 创建时间
  startedAt?: number;                // 开始时间
  endedAt?: number;                  // 结束时间
  outcome?: SubagentRunOutcome;      // 运行结果
  suppressAnnounceReason?: "steer-restart" | "killed"; // 抑制通告原因
  endedReason?: SubagentLifecycleEndedReason;          // 结束原因
};
```

初始化恢复（`restoreSubagentRunsOnce`，三步）：磁盘恢复（mergeOnly 合并）→ 孤儿协调（变更则持久化）→ 逐 runId 恢复未完成工作。

## 派生五步流程（subagent-spawn.ts:166-550）

```text
1. 权限与深度检查
   - 检查调用者深度 < maxSpawnDepth
   - 检查活跃子运行数 < maxChildrenPerAgent
   - 检查 agentId 允许列表
2. 创建子会话
   - 生成子会话键: agent:<id>:subagent:<uuid>
   - 通过 sessions.patch 设置 spawnDepth
   - 设置模型和思考级别
3. 线程绑定（可选）
   - 调用 subagent_spawning 钩子准备线程绑定
   - 失败时回滚删除会话
4. 启动子运行
   - 构建子智能体系统提示
   - 调用 gateway.agent() 启动运行
   - 使用专属 lane: AGENT_LANE_SUBAGENT
5. 注册运行记录
   - 调用 registerSubagentRun() 注册到 registry
   - 开始等待完成
   - 触发 subagent_spawned 钩子
```

**spawn 三闸门**：调用者深度 < maxSpawnDepth、活跃子运行数 < maxChildrenPerAgent、agentId 允许列表。

## 子智能体系统提示（六规则）

buildSubagentSystemPrompt（subagent-announce.ts:921-1025）：

```typescript
export function buildSubagentSystemPrompt(params: {
  requesterSessionKey?: string;
  childSessionKey: string;
  label?: string;
  task?: string;
  childDepth?: number;
  maxSpawnDepth?: number;
}) {
  const canSpawn = childDepth < maxSpawnDepth;
  // "# Subagent Context" / "## Your Role" / "## Rules"（六条）
  if (canSpawn) {
    // "You CAN spawn your own sub-agents using `sessions_spawn`."
    // "Use the `subagents` tool to steer, kill, or check status."
  } else if (childDepth >= 2) {
    // "You are a leaf worker and CANNOT spawn further sub-agents."
  }
}
```

六条规则：①Stay focused；②Complete the task（最终消息自动上报）；③Don't initiate（无心跳、无主动行为、无 side quests）；④Be ephemeral；⑤Trust push-based completion（后代结果自动通告）；⑥从压缩截断的工具输出中恢复（用更小块重读）。

## 核心配置（3.7.3，verbatim）

```json
{
  agents: {
    defaults: {
      subagents: {
        maxSpawnDepth: 2,           // 最大派生深度（1-5，默认1）
        maxChildrenPerAgent: 5,     // 每个会话最大活跃子运行数（1-20）
        maxConcurrent: 8,           // 全局并发上限（默认8）
        runTimeoutSeconds: 900,     // 默认超时（0=无超时）
        archiveAfterMinutes: 60,    // 自动归档时间（默认60分钟）
        model: "claude-3-haiku",    // 子智能体默认模型
        thinking: "medium",         // 默认思考级别
      },
    },
    list: [{
      agentId: "orchestrator",
      subagents: {
        allowAgents: ["*"],         // 允许派生任意 agentId
      },
    }],
  },
  tools: {
    subagents: {
      tools: {
        deny: ["gateway", "cron"],  // 工具黑名单
        allow: ["read", "exec"],    // 工具白名单
      },
    },
  },
}
```

## 工具策略三级默认（docs:229-236）

```javascript
// 深度1编排者获得会话工具
if (isSubagentSessionKey(sessionKey) && depth === 1 && maxSpawnDepth >= 2) {
  allowedTools.push("sessions_spawn", "subagents", "sessions_list", "sessions_history");
}

// 深度2叶子工作者无会话工具
if (depth >= 2) {
  denySet.add("sessions_spawn");
}
```

- 叶子：无 `sessions_*` 会话工具
- 深度 1 编排者（仅当 maxSpawnDepth ≥ 2）：获 `sessions_spawn`/`subagents`/`sessions_list`/`sessions_history`，仍拒 `sessions_send`/`sessions_delete`
- 深度 ≥ 2：强制 deny `sessions_spawn`

## 执行通道（CommandLane，src/process/lanes.ts）

```javascript
export const enum CommandLane {
  Main = "main",         // 主会话
  Cron = "cron",         // 定时任务
  Subagent = "subagent", // 子智能体
  Nested = "nested",     // 嵌套调用
}
```

## maxSpawnDepth 默认值张力（已基本化解）

3.7.1 正文称嵌套"最大 5 层"，深度表标注深度 2"永远不能"派生。chunk 17 系统提示代码显示真实门控为 `canSpawn = childDepth < maxSpawnDepth`；chunk 18 配置注释明确"默认 1"、示例设 2。裁决：**"5 层"为 maxSpawnDepth 可配置上限（1-5），默认配置为单层派生，设 2 才启用深度 1 编排者**。`DEFAULT_SUBAGENT_MAX_SPAWN_DEPTH` 精确数值待证（低优先级）。

## 关联

- [[任务隔离]] / [[1项目×n人×m-session模型]]：会话隔离是该两概念的架构级实现
- [[SubAgent记忆隔离]]：独立会话即独立上下文
- [[SQLite全量对话持久化]]：注册表持久化同属"重启可恢复"设计谱系
- [[agent-control-plane]]：三闸门 + 并发三上限 + 工具策略分级是控制平面在 SubAgent 维度的具体化
- [[高阶模型审查低阶模型]]：子代理默认 claude-3-haiku（小模型干活）+ thinking medium 的"小模型默认"策略
- [[agent可观测性六维度]]：runTimeoutSeconds / archiveAfterMinutes / 级联停止对应 recovery 维度
- [[subagent通告机制]] / [[subagent设计哲学]]：结果回传与官方设计总结
