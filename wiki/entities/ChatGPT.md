---
type: entity
title: ChatGPT
tags: [OpenAI, 产品案例, 记忆系统, agent]
related: [chatgpt四层记忆, agent记忆四分类, Markdown优先记忆论, 记忆整合指针回退机制, openclaw, 长记忆四件套]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604280830]你不知道的Agent原理架构与工程实践.html"]
---
# ChatGPT

独立可核查的公开背景（非本文观点）：ChatGPT 是 OpenAI 推出的对话式 AI 助手产品。

在本文来源（[[sources/[202604280830]你不知道的Agent原理架构与工程实践]]）中，ChatGPT 作为记忆系统 / Agent 记忆架构的产品级剖析案例出现：其记忆架构采用四层设计，未使用向量数据库、未引入 RAG，结构比预期更简洁。这一关键发现使其成为 [[Markdown优先记忆论]]（大多数 Agent 不需一开始引入向量存储；该论点已并入 [[chatgpt四层记忆]] 页）的代表证据与产品级佐证。

## 四层记忆架构（本文档案例数据，出处未注明）

| 层 | 内容 | 持久化 |
|------|-----------|---------|
| Session Metadata | 设备、地点、使用模式 | 否，会话级 |
| User Memory | 约 33 条关键偏好事实 | 是，每次注入 |
| Conversation Summary | 约 15 个最近对话的轻量摘要 | 是，摘要预生成 |
| Current Session | 当前对话滑动窗口 | 否 |

设计要点：重要事实要留下来（User Memory 每次注入），注入模型的内容不能失控（摘要预生成而非全量历史）。

## 与 OpenClaw 记忆架构的产品取舍对比

来源将 ChatGPT 四层记忆与 [[openclaw]] 的混合检索方案（`memory/YYYY-MM-DD.md` 追加日志 + `MEMORY.md` 精选事实 + `memory_search` 70% 向量 / 30% 关键词混合检索）并置对照：ChatGPT 选用极简分层注入（无向量库、无 RAG），OpenClaw 保留原始日志 + 语义检索（可读、可改、可检索）。来源将此定位为不同产品取舍，而非优劣结论。

## 关联

- [[agent记忆四分类]]：ChatGPT 的 User Memory 对应“语义记忆”层的工程化实例。
- [[记忆整合指针回退机制]]：来源提出的通用记忆整合机制（阈值 0.5、只移动指针不删原始消息），ChatGPT 的摘要预生成可视为同类问题的另一解法。
- [[长记忆四件套]]：小红书 PMO 的工程化长记忆方案，可与 ChatGPT 消费级四层方案对照。