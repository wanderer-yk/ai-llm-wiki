---
type: concept
title: SubAgent 设计哲学
tags: [openclaw, subagent, 设计哲学, 认证]
related: [openclaw, openclaw-subagent架构, subagent通告机制, fork-sub-agent机制, 受控子Agent机制]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202603190830]深入理解OpenClaw技术架构与实现原理上.html"]
---

# SubAgent 设计哲学

SubAgent 设计哲学是 [[openclaw]] 作者在《[[sources/[202603190830]深入理解OpenClaw技术架构与实现原理上|深入理解OpenClaw技术架构与实现原理（上）]]》3.7.8 节对整个 SubAgent 子系统（3.7.1–3.7.7）的官方总结：五条设计哲学与四条关键设计决策，并附三个典型场景与认证继承机制。它是 wiki 各 SubAgent 概念页的官方锚点。

## 五条设计哲学（3.7.8 原表）

| # | 哲学 | 内涵 |
|---|------|------|
| 1 | 隔离与独立 | 独立会话、独立上下文、token 配额、工具集 |
| 2 | 推式通知 | 结果自动通告，避免轮询开销和复杂性 |
| 3 | 嵌套编排 | 支持多层嵌套，实现复杂编排模式 |
| 4 | 资源可控 | 深度限制 + 并发上限 + 工具策略控制资源消耗 |
| 5 | 可扩展性 | 插件钩子支持自定义行为（线程绑定、路由策略等） |

## 四条关键设计决策

1. **会话键深度跟踪嵌套层级**（非独立字段）
2. **通告机制而非返回值**（异步非阻塞语义，避免轮询）
3. **工具策略按深度区分**（编排者得管理工具、工作者专注任务）
4. **持久化注册表**确保网关重启不丢运行状态

## 三个典型场景（3.7.7）

```
① 并行研究：
用户: "研究这三个主题并生成报告"
主智能体: sessions_spawn(task:"研究主题A", label:"research-a") × 3
[等待通告] → research-a/b/c: ✅ 完成 → 主智能体综合生成最终报告

② 编排者模式（maxSpawnDepth=2）：
用户: "重构这个大型项目"
主智能体: sessions_spawn(task:"协调重构工作", agentId:"orchestrator", label:"refactor-coordinator")
refactor-coordinator（深度1编排者）: sessions_spawn("重构模块A/B/C", label:"worker-a/b/c")
[等待子运行通告] → worker×3 ✅ → coordinator 综合 → 通知主智能体 → 主智能体报告用户

③ 线程绑定会话：
用户（Discord 线程）: "监控这个服务的性能"
主智能体: sessions_spawn(task:"启动性能监控", thread:true, mode:"session")
[Discord 扩展创建专用线程，子智能体在其中运行]
用户（同线程）: "当前状态如何？" → 路由到绑定的子智能体会话 → "当前 CPU 45%, 内存 2.1GB."
用户: "/unfocus" → 解除线程绑定，后续消息路由回主智能体
```

## 认证继承（3.7.6，docs:196-204）

```javascript
// 子智能体认证由 agentId 决定，而非会话类型
const authStore = loadAuthStore({ agentId: targetAgentId });

// 主智能体认证作为回退合并
const mainAuthStore = loadAuthStore({ agentId: requesterAgentId });
const mergedAuth = mergeAuthStores(authStore, mainAuthStore, {
  agentProfilesOverride: true, // 子智能体配置优先
});
```

三段式：targetAgentId 加载 → requesterAgentId 回退 → 合并时子配置优先（`agentProfilesOverride: true`）。

**编辑伪影（记录备查）**：原文"允许列表（docs:122-128）"小节标题存在，但其下代码块与认证继承代码完全重复，疑为编辑错误；`allowAgents` 允许列表（`["*"]` 通配格式，见 [[openclaw-subagent架构]] 配置）的真实实现代码在本文缺失，待下篇或官方源码仓库补证。

## 跨产品对照（子 Agent 机制横向对比）

| 对照轴 | OpenClaw SubAgent | Claude Code [[fork-sub-agent机制]] | Hermes [[受控子Agent机制]] |
|--------|-------------------|-------------------------------------|------------------------------|
| 结果回传 | 推式通告（push-based 自动通告） | fork 回传 | 受控轮询 |
| 上下文继承 | 独立会话 + 系统提示注入 | fork 复制上下文 | 受控生成 |
| 生命周期治理 | 注册表 + 级联停止 + 四钩子 | — | — |
| 失败恢复 | 持久化注册表 + 嵌套冒泡/祖父回退 | — | — |
| 认证方式 | agentId 决定 + 主智能体回退合并 | — | [[密钥托管凭证隔离]] |
| 典型场景 | 并行研究 / 编排者 / 线程绑定 | — | — |

## 关联

- [[openclaw-subagent架构]]：五哲学中"隔离与独立/资源可控"的机制实现
- [[subagent通告机制]]：推式通知与可扩展性（四钩子）的机制实现
- [[SubAgent记忆隔离]]：隔离哲学与记忆隔离概念互证
- [[高阶模型审查低阶模型]]：编排者可配小模型（claude-3-haiku）的分层用模型策略
