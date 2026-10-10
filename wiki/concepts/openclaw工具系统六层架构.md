---
type: concept
title: OpenClaw 工具系统六层架构
tags: [openclaw, 工具系统, 插件, hook, schema]
related: [openclaw, 七步工具策略管道, agent-control-plane, 全生命周期hook机制, plugin能力组合包, pi-agent]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202603190830]深入理解OpenClaw技术架构与实现原理上.html"]
---
# OpenClaw 工具系统六层架构

OpenClaw 工具系统是连接 AI 模型与外部能力的核心子系统，采用六层架构：**Tool Creation → Tool Definition → Schema Normalization → Policy Pipeline → Execution+Hook → Plugin System**，外加 HTTP Invocation API 作为对外延伸。作者总结其六大设计特点：分层架构/策略管道/Provider 适配/插件扩展/Hook 机制/沙箱支持。

## 工具创建七步流程（pi-tools.ts:182 主入口 `createOpenClawCodingTools()`）

```text
1. resolveEffectiveToolPolicy()      // 解析策略
2. codingTools.flatMap()             // 处理基础编码工具（pi-coding-agent 的 read/write/edit/bash 整体并入）
3. createExecTool()                  // 创建执行工具
4. createOpenClawTools()             // 创建 OpenClaw 工具
5. applyToolPolicyPipeline()         // 应用策略管道
6. normalizeToolParameters()         // 规范化 Schema
7. wrapToolWithBeforeToolCallHook()  // 添加钩子
```

## 工具定义层

`AnyAgentTool` 核心类型（tools/common.ts:8）：`name`（小写唯一）/`label`/`description`（给 AI 看）/`parameters`（JSON Schema / TypeBox）/`execute`/`ownerOnly`（仅所有者可用）。8 个内置工具（src/agents/tools/）：browser（浏览器控制）、memory（记忆搜索）、message（消息发送）、exec（命令执行）、canvas（画布操作）、gateway（网关管理）、tts（语音合成）、process（进程管理）。

## Schema 规范化（`normalizeToolParameters()`，pi-tools.schema.ts）

| 提供商 | 处理方式 |
|--------|---------|
| Anthropic | 保持完整 JSON Schema draft 2020-12 兼容 |
| OpenAI | 确保顶层有 `type: "object"` |
| Google/Gemini | 清理不支持的 `format`/约束关键字 |
| 所有 | 合并 `anyOf`/`oneOf` union schemas |

## 执行与 Hook 层

事件处理位于 `pi-embedded-subscribe.handlers.tools.ts`：`handleToolExecutionStart()` 记录开始时间发事件，`handleToolExecutionEnd()` 处理结果跑 after hook。双 hook 语义：`before_tool_call`（可修改参数/阻止调用/日志）+ `after_tool_call`（记录结果/循环检测/计时）——与 Hermes 系 [[全生命周期hook机制]] 跨源互证。

## 插件层与 HTTP API

插件工具注册：`resolvePluginTools()`（plugins/tools.ts）+ `PluginToolRegistration` 类型（pluginId/factory/names/optional/source），extensions 目录即工具来源（msteams/matrix/zalo/voice-call）。HTTP 调用 API：`POST /tools/invoke`（gateway/tools-invoke-http.ts），支持认证验证/策略应用/结果返回，是 Gateway 控制面能力的对外延伸。

## 工具调用 12 步完整流程（原文）

```text
用户消息 → Gateway 接收 → 构建工具列表 (createOpenClawCodingTools())
→ 应用策略过滤 (applyToolPolicyPipeline()) → 规范化 Schema (normalizeToolParameters())
→ 发送给 AI 模型 → AI 生成 tool_use → 工具执行前检查 (before_tool_call hook)
→ tool.execute() → 工具执行后处理 (after_tool_call hook) → 返回 tool_result → AI 继续推理
```

关键点：策略过滤与 Schema 规范化发生在"发送给 AI 模型"之前，双 hook 环绕 `tool.execute()`。