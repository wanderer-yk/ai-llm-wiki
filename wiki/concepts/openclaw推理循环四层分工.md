---
type: concept
title: OpenClaw 推理循环四层分工
tags: [openclaw, agent-loop, 推理循环, 事件驱动]
related: [openclaw, pi-agent, agent-loop, compaction双触发模式, 上下文压缩策略, 三主干链路]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202603190830]深入理解OpenClaw技术架构与实现原理上.html"]
---
# OpenClaw 推理循环四层分工

OpenClaw 的推理循环（AgenticLoop / Pi Loop）是事件驱动架构，是"整个系统执行的大脑思考核心"，系统所有运行逻辑由其控制。其实现分四层职责，是 [[agent-loop]]（While 循环驱动的 LLM 推理+工具调用+上下文更新）概念的源码级实现样本。

## 四层分工

1. **主循环**（`runEmbeddedPiAgent`，run.ts:192）：`while(true)`（行 538）重试循环，含 `MAX_RUN_LOOP_ITERATIONS` 重试上限、异常三路分派、成功返回 payloads。
2. **尝试层**（`runEmbeddedAttempt`，run/attempt.ts:306）：单次 LLM 调用完整生命周期，四阶段——①准备（创建 workspace/session、解析 tools `createOpenClawCodingTools`、构建 system prompt、创建 session manager）②会话初始化（`createAgentSession()` 行 688、设置 streamFn、安装 `subscribeEmbeddedPiSession()` 行 921）③执行推理（`activeSession.prompt(effectivePrompt)` 行 1180-1182）④返回结果（assistantTexts / toolMetas / usage）。
3. **事件订阅**（`subscribeEmbeddedPiSession`，pi-embedded-subscribe.ts:34）：五类事件分发——`message_start/update/end`（text_delta、thinking 块、reasoning，回调 onPartialReply/onBlockReply）、`tool_execution_*`、`agent_start/end`。
4. **工具循环**：由 SDK `@mariozechner/pi-coding-agent` 的 `createAgentSession` 自动管理——模型返回 `tool_use` → 自动执行工具 → `tool_result` 回填消息历史 → 继续下一轮推理。

## 异常三路分派

| 异常 | 处理 |
|------|------|
| context overflow | 自动压缩（一等异常路径，与 [[compaction双触发模式]] 互证） |
| auth failure | profile 轮换（认证失败时轮换 profile 继续运行） |
| timeout | 重试或报错 |

## LLM 调用分层（streamFn）

- 默认 `streamSimple`（`@mariozechner/pi-ai` 包）
- Ollama 走 `createOllamaStreamFn()`
- `applyExtraParamsToAgent()` 可包装添加额外参数

## 关键调用链（原文）

```text
用户消息 → runAgentTurnWithFallback() (agent-runner-execution.ts:72)
→ runEmbeddedPiAgent() (pi-embedded-runner/run.ts:192)
→ [while循环 - 重试] runEmbeddedAttempt() (pi-embedded-runner/run/attempt.ts:306)
→ createAgentSession() + activeSession.prompt()
→ [LLM调用 + 工具循环] subscribeEmbeddedPiSession() → 事件处理器
→ onPartialReply / onBlockReply / onToolResult → 回复消息发送
```

本调用链与 Claude Code 源码拆解的 [[三主干链路]] 同属"推理循环函数级调用链"分析范式，可对照阅读。待证项：`MAX_RUN_LOOP_ITERATIONS` 具体数值、`runAgentTurnWithFallback()` 的 fallback 逻辑、profile 轮换的具体构成。