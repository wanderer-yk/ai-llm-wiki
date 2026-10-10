---
type: concept
title: SubAgent 记忆隔离
tags: [subagent, 记忆隔离, 上下文污染, chatmemory]
related: [受控子Agent机制, 任务隔离, 1项目×n人×m-session模型, SubAgent生命周期工具化, 追加式上下文, 三层上下文压缩]
sources: ["[202604220830]AI实践基于SpringAI从0到1构建AIAgent.html"]
created: 2026-10-10
updated: 2026-10-10
---
# SubAgent 记忆隔离

SubAgent 记忆隔离是[[aiagentdemo]]项目中 SubAgent 的核心机制：每个 SubAgent 拥有**独立的 ChatMemory 实例（`ChatMemory.forSubAgent()`）与独立的 systemPrompt**，同时**共享主 Agent 的 ChatClient（即共享同一大模型连接）**——「共享模型、隔离记忆」的双层结构。

```java
public SubAgent(String id, String name, String systemPrompt, ChatClient chatClient) {
    this.memory = ChatMemory.forSubAgent();  // 独立记忆！
    this.memory.setSystemPrompt(systemPrompt);
    // ...
}
```

## 动机：上下文污染防护

需要独立上下文的任务（例：写一篇技术文章需多轮完善），不应污染主对话记忆——SubAgent 是 [[追加式上下文]] 必然膨胀问题的架构级解法之一。

## 记忆隔离三效果

1. SubAgent 内部多轮对话**不影响主对话上下文**；
2. 主 Agent 可同时管理**多个 SubAgent 互不干扰**（并存）；
3. SubAgent 销毁后**记忆随之释放**（生命周期与记忆绑定）。

## 隔离粒度谱系

SubAgent 记忆隔离补全了本 wiki 的上下文隔离粒度谱系：

- **Session 级**：本项目的 `ConcurrentHashMap<String, ChatMemory>` 按 sessionId 隔离（多客户端并发）；
- **任务级**：zhiyuanfu 的 [[任务隔离]]（每个任务独立窗口）；
- **SubAgent 实例级**：本文——隔离粒度最细，且隔离与生命周期绑定；
- **项目级多 Session**：小红书 [[1项目×n人×m-session模型]]（多渠道共享项目上下文）。

## 与 Hermes 受控子 Agent 的对照

Hermes 的 [[受控子Agent机制]] 以受控生成/审查方式管理子 Agent，本文则以纯 Function Calling 三工具（[[SubAgent生命周期工具化]]）由主 LLM 自主驱动——LLM 自主驱动 vs 受控审查是两条值得对比的子 Agent 治理路线（失败处理与权限控制差异待补）。

## 待核

`ChatMemory.forSubAgent()` 是 Spring AI 原生 API 还是项目自研扩展未澄清；chat_with_sub_agent 返回结果如何写回主上下文（摘要/全文/工具消息）、SubAgent 数量上限与资源回收策略未披露。