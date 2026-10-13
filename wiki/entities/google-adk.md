---
type: entity
title: Google ADK
tags: [agent-framework, google, memory, context-engineering]
related: [agentscope, langchain, autocontextmemory, 三大框架上下文处理策略对比, 记忆会话边界二分, 上下文缩减卸载隔离三分法]
created: 2026-10-13
updated: 2026-10-13
sources: ["[202601300830]AIAgent记忆系统从短期到长期的技术架构与实践.html"]
---
# Google ADK

Google ADK（Agent Development Kit）是 Google 推出的智能体（Agent）开发框架。*（此身份为可独立验证的公开背景，非源文陈述。）* 在本 Wiki 收录的文献中，ADK 以其 Session/Memory 记忆模型与事件压缩配置被作为三大框架上下文处理策略对比对象之一（见 [[三大框架上下文处理策略对比]]）。

## 记忆模型（源文口径）

- **Session**：表示单次持续交互。
- **Memory**：长期知识库，可包含多次对话的信息。

这一模型遵循"会话级/跨会话级"两分，与 [[记忆会话边界二分]] 的判据一致。

## 上下文压缩实现

ADK 通过 App 级配置 `events_compaction_config` 实现事件压缩，压缩配置与 Session 数据存储**相互独立（解耦）**：

```python
from google.adk.apps.app import App, EventsCompactionConfig

app = App(
    name='my-agent',
    root_agent=root_agent,
    events_compaction_config=EventsCompactionConfig(
        compaction_interval=3,  # 每3次新调用触发压缩
        overlap_size=1          # 包含前一个窗口的最后一次调用
    ),
)
```

## 在三框架对比中的位置

- 与 [[langchain]]（`SummarizationMiddleware` 摘要中间件）同为"配置与记忆存储解耦"路线；
- 与 [[agentscope]] 的 [[autocontextmemory]]（实现 Memory 接口、四层存储可追溯）形成集成深度上的对照。

## 官方文档

- Google ADK Memory：https://google.github.io/adk-docs/sessions/memory/ （源文参考文献 [2]）