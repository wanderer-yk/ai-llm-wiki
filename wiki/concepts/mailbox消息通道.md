---
type: concept
title: mailbox 消息通道
created: 2026-10-09
updated: 2026-10-09
tags: [agent-team, 消息通信, 协调机制]
related: [并行质量收益论, 两阶段迁移流程, 三层架构管住AI输出]
sources: ["[202605201800]网盘存量代码迁移实战我们如何用三层架构管住AI的输出.html"]
---
# mailbox 消息通道

**mailbox 消息通道**是本文 Agent Team 层的跨 Teammate 实时协调机制：四路并行并非完全独立，实际运行会遇到跨类型依赖，这类协调通过 Mailbox 消息通道实时处理——**无需等一个提取全部完成再补充，也无需重新启动一轮**。它解决了并行分工的固有短板：分工切断了信息流，Mailbox 又把信息流接了回来。

## 两类依赖场景

| 触发方 | 发现 | 通知 | 动作 |
|--------|------|------|------|
| UI 提取 Teammate | 代码里动态创建了 Dialog | 布局提取 Teammate | 补充该 Dialog 的布局文件 |
| 业务逻辑提取 | API 调用引用了某字符串资源 | 资源提取 Teammate | 补充 strings.xml/colors.xml 等资源文件 |

## 在流程中的位置

Mailbox 使提取阶段的并行（Agent Team 四路）不必退化为"串行 + 重跑"：依赖消息即时到达目标 Teammate，其产出并入各自的提取包。这与转化阶段通过 Agent-Memory 文件传递产出的"接力"模式互补——Mailbox 管并行中的实时协调，Memory 管串行步骤间的产出传递（[[两阶段迁移流程]]、[[agent-memory结构化提炼]]）。