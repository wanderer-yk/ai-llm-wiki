---
type: entity
title: ReMe
tags: [memory, long-term-memory, agentscope, 服务化]
related: [mem0, agentscope, autocontextmemory, 长期记忆record-retrieve双向机制, 长期记忆六大核心组件]
created: 2026-10-13
updated: 2026-10-13
sources: ["[202601300830]AIAgent记忆系统从短期到长期的技术架构与实践.html"]
---
# ReMe

ReMe 是 [[agentscope]] 官方的长期记忆实现，与框架深度集成（此定位来自源文及其引用的 AgentScope 官方文档）。与 [[mem0]] 等第三方长期记忆组件不同，ReMe 走官方深度集成路线，支持用户级记忆隔离、独立服务化部署与可配置的记忆模式。


> [!warning] 代码语言待查证：源文全部 AgentScope 集成代码为 Java 语法（`data-lang="java"`），而其引用的官方文档为 Python 教程（doc.agentscope.io）——AgentScope 官方 Java SDK 是否存在待查证，本页代码引用在此之前按"作者转写"对待。


## 核心特性（源文口径）

- **userId 记忆隔离**：以用户 ID 隔离记忆空间，支撑多用户场景；
- **独立服务化部署**：ReMe 作为独立服务运行（示例配置 `apiBaseUrl: http://localhost:8002`），Agent 经网络接口访问；
- **BOTH 记忆模式**：经 `LongTermMemoryMode.BOTH` 配置记忆读写行为。

## AgentScope 集成方式（源文代码）

```java
// 初始化ReMe长期记忆
ReMeLongTermMemory remeMemory = ReMeLongTermMemory.builder()
    .userId("user123")  // 用户ID，用于记忆隔离
    .apiBaseUrl("http://localhost:8002")  // ReMe服务地址
    .build();
// 创建Agent并集成长期记忆
ReActAgent agent = ReActAgent.builder()
    .name("Assistant")
    .model(model)
    .memory(memory)  // 短期记忆
    .longTermMemory(remeMemory)  // 长期记忆
    .longTermMemoryMode(LongTermMemoryMode.BOTH)  // 记忆模式
    .build();
```

## 与 Mem0 的对照

| 维度 | ReMe | [[mem0]] |
|---|---|---|
| 归属 | AgentScope 官方实现 | 第三方开源框架（"几乎成为事实标准"，源文口径） |
| 集成深度 | 深度集成，含专属记忆模式 API | 经 `longTermMemory()` API 标准接入 |
| 隔离机制 | `userId` 记忆隔离 | 源文未展开 |
| 部署形态 | 独立服务（`apiBaseUrl`） | API Key 接入，源文未展开部署细节 |

ReMe 的 `userId` 隔离与 [[长记忆四件套]]（小红书 PMO 的 UserProfile 等用户记忆工程化）同属"按用户组织长期记忆"的思路。

## 开放问题

- ReMe 的完整功能边界（记忆巩固/遗忘策略、多模态支持）源文未展开，待官方文档补充。