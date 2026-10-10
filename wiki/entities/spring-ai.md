---
type: entity
title: Spring AI
tags: [框架, java, spring, ai-agent, llm]
related: [aiagentdemo, spring-ai-alibaba-studio, mcp, 三层上下文压缩, 可插拔工具注册机制, RRF排名融合算法]
sources: ["[202604220830]AI实践基于SpringAI从0到1构建AIAgent.html"]
created: 2026-10-10
updated: 2026-10-10
---
# Spring AI

Spring AI 是 Spring 生态官方推出的 AI 应用开发框架，为 Java 应用提供对接大语言模型的统一抽象（此身份为可独立核实的公开背景；下文技术能力描述均来自本 wiki 来源文章的实证）。在[[aiagentdemo]]项目中，Spring AI 通过 `spring.ai.openai.*` 的 OpenAI 兼容配置接入[[GLM-4]]（智谱 open.bigmodel.cn），支撑了从 Agent 编排到 MCP 集成的完整 AI Agent 技术栈，与 [[spring-ai-alibaba-studio]] 同属 Spring AI 生态。

## 文章实证的框架能力

**Agent Loop 内建于框架**：这是本文最重要的框架级发现——与 yabohe 手写 51 行 [[agent-loop]] 的自研路线不同，Spring AI 已通过 `org.springframework.ai.chat.client.advisor.ToolCallAdvisor#adviseCall` 的 do-while 循环封装了完整 ReAct 回路（`chatResponse.hasToolCalls()` 判断 + `toolCallingManager.executeToolCalls()` 执行），循环直至无 tool call；`ChatClient` 内置 ReAct 循环对开发者透明，LLM 可连续调用多工具直到信息充足。ToolCallAdvisor 还支持 `returnDirect`：工具结果直接返回客户端、中断 tool calling 循环。

**工具体系**：`ToolCallback` 为标准工具注册单元，`ToolCallbackBuilder` 组装名称/描述/JSON Schema/执行函数（配合项目自研的 [[可插拔工具注册机制]]）。

**抽象层**：`ChatClient`（对话客户端）、`ChatMemory`（对话记忆，注意：本项目中的三层压缩逻辑为项目定制）、`EmbeddingModel`（向量化抽象，用于 [[RRF排名融合算法]] 召回路与内存 VectorStore）。

**MCP 集成**：`McpSyncClient`、`McpSchema.InitializeResult`、`SyncMcpToolCallbackProvider`（builder → `getToolCallbacks()` 返回 `ToolCallback[]`，自动发现远程工具）。文章实证 Spring AI 支持两代 MCP 规范：Streamable HTTP（2025-03-26）与 SSE（2024-11-05），详见 [[MCP双规范版本锚定]]。

## 待辨归属

文章中部分能力（各 Splitter/Retriever、`ChatMemory.forSubAgent()` 工厂方法、ChatMemory 三层压缩）究竟是 Spring AI 原生 API 还是项目自研扩展，正文未明确区分，需对照 Spring AI 官方文档核实。