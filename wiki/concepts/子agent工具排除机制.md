---
type: concept
title: 子agent工具排除机制
tags: [子agent, 权限控制, 工具系统, 能力边界]
related: [子agent单进程并发模型, tool抽象四要素, agent-control-plane, claude-code, pi-agent, 薄抽象设计哲学]
created: 2026-09-30
updated: 2026-09-30
sources: ["[202604241629]800行代码实现OpenClaw的Tool消息总线子Agent管理架构.html"]
---
# 子agent工具排除机制

子agent工具排除机制指**子 Agent 的工具集是主 Agent 工具集的受限子集**：通过 `ToolRegistry.exclude(names)` 从主 Agent 工具集中过滤掉危险工具，生成子 Agent 专用注册表。其核心主张是「工具层即能力边界，无需独立权限层」——权限控制内建于工具注册机制本身，是 [[薄抽象设计哲学]] 的实例。

## 实现

`exclude()` 返回**新** ToolRegistry 实例、不修改原注册表（不可变/函数式风格）：

```javascript
const subagentTools = tools.exclude(["spawn", "message", "edit_file", "cron"]);
```

## 四项排除及理由（逐项）

| 排除工具 | 理由 |
|----------|------|
| `spawn` | 防止子 Agent 递归创建子 Agent |
| `message` | 防止子 Agent 直接向用户发消息——结果应经 MessageBus 回传主 Agent 处理 |
| `edit_file` | 限制子 Agent 写入能力 |
| `cron` | 避免子 Agent 创建定时任务 |

## 设计谱系与关联

- 与 [[claude-code]] 的子 Agent 工具受限设计理念一致（防递归 spawn 是共同关注点）。
- 与 [[pi-agent]] 的极简 4 工具形态相对照：pi-agent 从源头只提供 4 个核心工具，本框架则提供完整工具面后按 Agent 角色裁剪。
- 从 [[agent-control-plane]] 视角看，这是轻量级权限边界的最小实现：完整 Control Plane 还需执行沙箱（本框架以 ExecTool 三层防护部分承担，作者明示生产环境应使用容器沙箱或受限用户）与审计能力。
