---
type: entity
title: AutoContextMemory
tags: [agentscope, memory, context-compression, java]
related: [agentscope, google-adk, langchain, 三大框架上下文处理策略对比, 上下文缩减卸载隔离三分法, 上下文压缩策略]
created: 2026-10-13
updated: 2026-10-13
sources: ["[202601300830]AIAgent记忆系统从短期到长期的技术架构与实践.html"]
---
# AutoContextMemory

AutoContextMemory 是 [[agentscope]] 框架的智能化上下文记忆组件，直接实现框架的 Memory 接口，在上下文超出阈值时自动应用渐进式压缩策略（源文口径；该组件为 AgentScope 官方组件，此归属来自源文及其引用的 AgentScope 官方文档）。它是源文三框架对比中着墨最多的对象，与 Google ADK 的 `events_compaction_config`、[[langchain]] 的 `SummarizationMiddleware` 形成对照（见 [[三大框架上下文处理策略对比]]）。


> [!warning] 代码语言待查证：源文全部 AgentScope 集成代码为 Java 语法（`data-lang="java"`），而其引用的官方文档为 Python 教程（doc.agentscope.io）——AgentScope 官方 Java SDK 是否存在待查证，本页代码引用在此之前按"作者转写"对待。


## 配置 API

```java
AutoContextMemory memory = new AutoContextMemory(
    AutoContextConfig.builder()
        .msgThreshold(100)
        .maxToken(128 * 1024)
        .tokenRatio(0.75)
        .build(),
    model
);
ReActAgent agent = ReActAgent.builder()
    .name("Assistant")
    .model(model)
    .memory(memory)
    .build();
```

- `msgThreshold`：消息数阈值（示例 100），超阈值触发压缩；
- `maxToken`：token 上限（示例 128×1024）；
- `tokenRatio`：token 占比参数（示例 0.75）。

## 渐进式压缩策略

超阈值时自动应用 **6 种由轻到重的渐进式压缩策略**。源文正文仅列举其中 3 种：

1. 压缩历史工具调用
2. 卸载大型消息
3. 摘要对话轮次

完整清单需参考外链文章《AgentScope AutoContextMemory：告别Agent上下文焦虑》（微信文章，`MzIzOTU0NTQ0MA==`，待抓取补充）。

## 四层存储架构

工作内存 → 原始内存 → 卸载上下文 → 压缩事件。四层结构支持**完整历史追溯**——这是其他框架通常不具备的能力（它们通常只保留压缩后结果）。

## 相比 ADK / LangChain 的三点优势（源文主张）

1. 更精细的 6 种渐进式压缩策略（vs ADK 的事件压缩、LangChain 的单一摘要中间件）；
2. 实现 Memory 接口，与 Agent 构建流程无缝集成（vs ADK/LangChain 上下文策略与记忆存储解耦）；
3. 四层存储架构，压缩后仍可完整追溯历史。

## 关联

- 策略思想对应 [[上下文缩减卸载隔离三分法]] 中的缩减与卸载两策略。
- 与 [[上下文压缩策略]]（Claude Code/OpenClaw 侧对照）、[[compaction双触发模式]]、[[自适应分块压缩]] 属同一问题域的不同工程实现。