---
type: entity
title: Mem0
tags: [memory, long-term-memory, open-source, agentscope]
related: [reme, agentscope, 长期记忆record-retrieve双向机制, 长期记忆六大核心组件, 参数化记忆与外部记忆增强对比, 向量检索rag, rag技术瓶颈]
created: 2026-10-13
updated: 2026-10-13
sources: ["[202601300830]AIAgent记忆系统从短期到长期的技术架构与实践.html"]
---
# Mem0

Mem0 是一个开源的 AI Agent 长期记忆框架，为智能体提供记忆的写入（Record）与检索（Retrieve）完整能力。*（此身份为可独立验证的公开背景。）* 据源文（2026-01）所述，Mem0 在长期记忆开源产品中"几乎成为事实标准"：各方以它为评测基准，综合技术指标与 GitHub star/issues 社区活跃度，Mem0 居长期记忆产品领头地位。


> [!warning] 代码语言待查证：源文全部 AgentScope 集成代码为 Java 语法（`data-lang="java"`），而其引用的官方文档为 Python 教程（doc.agentscope.io）——AgentScope 官方 Java SDK 是否存在待查证，本页代码引用在此之前按"作者转写"对待。


## 架构定位

- 作为**独立第三方长期记忆组件**经 API 集成到 Agent 框架（见 [[长期记忆record-retrieve双向机制]] 的第三方组件化背景）；同类产品还包括 Zep、Memos、ReMe（后两者细节在源文中仅为图片对比表，未展开）。
- 内部支撑体系对应 [[长期记忆六大核心组件]]：LLM 事实提取、Embedder 向量化、VectorStore 向量存储、GraphStore 关系存储、Reranker 重排、SQLite 审计日志。

## 与 RAG 的关系（源文主张）

Mem0 类长期记忆系统与 RAG 在技术上相似（向量化存储、相似性检索、注入上下文生成三点），但**功能与场景有明显区别**——差异对比表在源文中为图片，未提取。可对照 [[向量检索rag]] 与 [[rag技术瓶颈]]（跨文件/调用链等长链条结构化任务上的 RAG 局限）。

## AgentScope 集成方式（源文代码）

```java
// 初始化Mem0长期记忆
Mem0LongTermMemory mem0Memory = new Mem0LongTermMemory(
    Mem0Config.builder()
        .apiKey("your-mem0-api-key")
        .build()
);
// 创建Agent并集成长期记忆
ReActAgent agent = ReActAgent.builder()
    .name("Assistant")
    .model(model)
    .memory(memory)  // 短期记忆
    .longTermMemory(mem0Memory)  // 长期记忆
    .build();
```

即 `Mem0LongTermMemory` + `Mem0Config`，经 `ReActAgent.builder().longTermMemory()` 接入 [[agentscope]]。

## 关联

- 与 [[reme]]（AgentScope 官方实现、深度集成、服务化部署）构成"第三方标准件 vs 官方深度集成"的对照。
- 属于"外部记忆增强"路线（当前主流），与参数化记忆路线的对比见 [[参数化记忆与外部记忆增强对比]]。