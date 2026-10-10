---
type: concept
title: SubAgent 生命周期工具化
tags: [subagent, function-calling, 工具设计, 生命周期管理]
related: [SubAgent记忆隔离, 受控子Agent机制, function-calling大地基论, agent-loop]
sources: ["[202604220830]AI实践基于SpringAI从0到1构建AIAgent.html"]
created: 2026-10-10
updated: 2026-10-10
---
# SubAgent 生命周期工具化

SubAgent 生命周期工具化是[[aiagentdemo]]项目的设计模式：SubAgent 的全部能力经 **3 个工具**暴露给主 Agent，本质是 Function Calling——「创建 → 多轮对话 → 销毁」的全生命周期由主 LLM 依对话上下文自主驱动，而非由硬编码流程控制。

## 三工具表

| 工具 | 参数 | 说明 |
|------|------|------|
| create_sub_agent | name、system_prompt、task | 创建 SubAgent 并执行首个任务 |
| chat_with_sub_agent | agent_id、message | 与已有 SubAgent 继续对话 |
| destroy_sub_agent | agent_id | 销毁 SubAgent，释放资源 |

## 设计含义

- **生命周期即工具调用序列**：主 LLM 根据任务需要自主决定何时创建、何时对话、何时销毁——资源管理与任务编排统一收敛到 [[agent-loop]] 的工具调用循环中；
- **记忆与生命周期绑定**：destroy 即释放独立 ChatMemory（[[SubAgent记忆隔离]] 三效果之三），无需额外垃圾回收机制；
- **LLM 自主驱动 vs 受控审查**：与 Hermes [[受控子Agent机制]] 的受控生成/审查路线形成对照——本文路线给 LLM 完全的子 Agent 治理权，Hermes 路线保留人工/规则审查节点，两者在失败处理与权限控制上的差异是值得补齐的对比维度；
- **能力 Tool 化的又一实证**：子 Agent 编排能力本身也被 Tool 化，是 [[function-calling大地基论]] 的机制级支撑。

## 待核

chat_with_sub_agent 的返回结果如何写回主 Agent 上下文（摘要/全文/工具消息）；SubAgent 数量上限、失败处理与资源回收策略未披露。