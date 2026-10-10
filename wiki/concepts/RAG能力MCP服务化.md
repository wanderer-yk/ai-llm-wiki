---
type: concept
title: RAG 能力 MCP 服务化
tags: [mcp, rag, 能力复用, mcp-server]
related: [mcp, mcp工具封装模式, 代码问答, MCP运行时动态管理, RRF排名融合算法]
sources: ["[202604220830]AI实践基于SpringAI从0到1构建AIAgent.html"]
created: 2026-10-10
updated: 2026-10-10
---
# RAG 能力 MCP 服务化

RAG 能力 MCP 服务化是[[aiagentdemo]]项目第六章的设计：项目的 SimpleMcpServer 对外暴露 `knowledge_query` 知识库检索工具，**内部调用 RagService**——使「本项目的 RAG 能力可以被任何支持 MCP 协议的 AI 应用复用」。

## knowledge_query 参数表

| 参数 | 类型 | 说明 |
|------|------|------|
| keyword | String | 检索关键词 |
| category | String | 知识分类（java_basic / jvm / concurrent / spring / design_pattern / all） |
| maxResults | int | 返回的最大结果条数，默认 3 |

## 设计含义

- **能力从「自用」到「服务」**：RAG 流水线（[[RRF排名融合算法]] 三路召回 + Rerank）原本只服务本项目的 [[RAG工具化双路径]]，经 MCP Server 化后成为跨应用的基础设施——同一套检索后端可同时被本项目 Agent 与任意外部 MCP 客户端消费；
- **双向 MCP 的实证**：同一项目既做 Client（连接外部服务）又做 Server（对外暴露能力），验证了 MCP 协议「标准化连接」定位的双向实用性；
- **跨源印证**：与 [[mcp工具封装模式]]（转转：内部流水线封装为 MCP 工具）同构；与有赞 [[代码问答]]（代码问答能力平台化）同属「检索/问答能力服务化」的独立样本。

## 附带证据

knowledge_query 的 category 枚举揭示该知识库内容为 Java 学习资料（java_basic / jvm / concurrent / spring / design_pattern），与作者「让 AI 生成学习资料」的元叙事互相印证。

## 待核

SimpleMcpServer 采用的传输协议（stdio / SSE / Streamable HTTP）未披露；MCP Server 侧的鉴权与限流未提及。