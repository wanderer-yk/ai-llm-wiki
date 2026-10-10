---
type: entity
title: aiagentdemo
tags: [开源项目, github, spring-ai, ai-agent, java]
related: [觖弦, spring-ai, GLM-4, 三层上下文压缩, 可插拔工具注册机制, RRF排名融合算法, SubAgent记忆隔离, RAG能力MCP服务化, MCP运行时动态管理]
sources: ["[202604220830]AI实践基于SpringAI从0到1构建AIAgent.html"]
created: 2026-10-10
updated: 2026-10-10
---
# aiagentdemo

aiagentdemo 是[[觖弦]]开源的 Spring AI Agent 示范项目（GitHub 仓库 `q644266189/aiagentdemo`），是《AI实践｜基于 Spring AI 从0到1构建 AI Agent》一文全部技术内容的载体。项目基于 [[spring-ai]] 与 [[GLM-4]]（智谱 OpenAI 兼容端点），环境要求 Java 21+、Maven 3.9+，内置 Web UI（`src/main/resources/static/index.html`，支持 SSE 流式对话、Markdown 渲染、`/` 命令面板、会话管理）。项目代码几乎全部由 AI 生成，知识库内容为 Java 学习资料（category 枚举：java_basic / jvm / concurrent / spring / design_pattern / all）。

## 六模块架构

1. **AgentCore**：核心编排器，五段编排（意图识别 → RAG 注入 → 记忆管理 → 模型调用 → 工具执行）；IntentRecognizer 意图门控；ChatMemory 三层压缩（[[三层上下文压缩]]）
2. **Tool**：`InnerTool` 统一接口 + Spring 自动扫描注册（[[可插拔工具注册机制]]）
3. **RAG**：8 种分块策略（默认 500/50）+ 三路召回 + [[RRF排名融合算法]]（k=60）+ LLM Rerank 9→3 + 内存 VectorStore
4. **Command & Skill**：Markdown 驱动的双 Prompt 模板机制（SkillManager 扫描 `classpath:skill/*.md`、CommandManager 扫描 `classpath:command/*.md`）
5. **SubAgent**：独立 ChatMemory + 共享 ChatClient 的子代理（[[SubAgent记忆隔离]]、[[SubAgent生命周期工具化]]），另有外部 IdeaLab Agent 形态
6. **MCP**：双端实现——SimpleMcpServer 对外暴露 knowledge_query（内部调用 RagService，即 [[RAG能力MCP服务化]]）；McpClient 双协议回退 + 工具自动发现 + `mcp-servers.json` 持久化（[[MCP运行时动态管理]]、[[MCP持久化与自动恢复]]）

## REST 端点清单

| 端点 | 方法 | 说明 |
|------|------|------|
| `/api/chat` | POST | 非流式对话，body: `{"message": "...", "sessionId": "..."}` |
| `/api/chat/stream` | POST | 流式对话（SSE） |
| `/api/command/execute` | POST | 用户主动执行 Command |
| `/api/manage/mcp/connect` | POST | 连接新的 MCP 服务，工具立即可用 |
| `/api/manage/mcp/disconnect` | POST | 断开 MCP 服务，移除对应工具 |
| `/api/manage/mcp/list` | GET | 查看所有 MCP 服务及其工具列表 |

## 内部组件一览

AgentCore、IntentRecognizer、ChatMemory（含 SummaryCompressor 私有静态内部类）、InnerTool/ToolCallbackBuilder、RagService、MultiRecaller、LlmReranker、各 Splitter/Retriever、SkillManager/SkillTool、CommandManager、SubAgent、SimpleMcpServer、McpClient（含 store 持久化）。这些组件的实现细节均归属本页与来源文章记录，不单独建页。