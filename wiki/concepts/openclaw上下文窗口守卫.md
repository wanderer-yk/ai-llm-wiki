---
type: concept
title: OpenClaw 上下文窗口守卫
tags: [openclaw, 上下文管理, 上下文窗口, 阈值守卫]
related: [openclaw, autocompact水位线机制, 上下文窗口四要素, openclaw运行时上下文注入]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202603190830]深入理解OpenClaw技术架构与实现原理上.html"]
---

# OpenClaw 上下文窗口守卫

OpenClaw 上下文窗口守卫是 [[openclaw]] 对模型上下文窗口有效性的启动级保护机制，实现位于 `src/agents/context-window-guard.ts`。它解决的问题是：当配置的上下文窗口解析值过小（如误配置）时，Agent 无法正常工作甚至产生危险行为，因此设置硬性阻断与警告双阈值。来源：[[sources/[202603190830]深入理解OpenClaw技术架构与实现原理上|深入理解OpenClaw技术架构与实现原理（上）]] 3.6.2 节。

## 双阈值常量（verbatim）

```ini
CONTEXT_WINDOW_HARD_MIN_TOKENS = 16_000   // 低于此值阻断运行
CONTEXT_WINDOW_WARN_BELOW_TOKENS = 32_000 // 低于此值警告
```

## 三种检查结果

- **shouldWarn**：< 32K，可能体验不佳
- **shouldBlock**：< 16K，无法正常工作
- **来源标记**：记录窗口值的解析来源

## 上下文窗口解析五级优先级

1. `contextTokensOverride`（直接使用）
2. `context1m: true`（Anthropic 1M 模型 → 1,048,576 tokens）
3. 模型注册表（`models.json` / provider catalog）
4. 配置文件覆盖（`models.providers.*.models[].contextWindow`）
5. Fallback（传入默认值）

**来源标记枚举**：`model | modelsConfig | agentContextTokens | default`

**粒度张力（待核证）**：来源标记仅 4 值，而解析优先级有 5 层——推测 `modelsConfig` 合并了 `contextTokensOverride` 与 `context1m` 两级，待确认。

## 关联

- [[autocompact水位线机制]]：Claude Code 的水位线机制与 OpenClaw 的 16K/32K 双阈值同为阈值化守卫，可对照；区别在于 Claude Code 阈值触发压缩，OpenClaw 阈值阻断/警告运行
- [[上下文窗口四要素]]：窗口解析优先级是上下文窗口配置要素的工程化实例
- [[openclaw运行时上下文注入]]：守卫确定的窗口值是注入预算（bootstrap 双上限、工具结果守卫）的计算基础

## 开放问题

- `context1m: true` 的适用模型范围
- 守卫检查时机（启动时/每轮推理时）
