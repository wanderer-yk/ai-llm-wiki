---
type: concept
title: agent开发经验主义
tags: [agent, 评测, 参数调优, 风险]
related: [agent系统模块六要素, 文档分块参数调优, 先易后难陷阱, agent评测四要素, llm不确定性物理不可消除]
created: 2026-10-12
updated: 2026-10-12
sources: ["[202603240830]业务逻辑的坍塌当应用层只剩下胶水代码在AIAgent时代我们该构建什么-上.html", "[202603240830]业务逻辑的坍塌当应用层只剩下胶水代码在AIAgent时代我们该构建什么-下.html", "[202603240830]业务逻辑的坍塌当应用层只剩下胶水代码在AIAgent时代我们该构建什么.html"]
---
# agent开发经验主义

[[韩春]] 的核心主张：Agent 系统"看似流程确定，但在细节中充斥着大量的不确定性和经验主义"。作者用一个"相对完整的 AI Agent 系统模块"伪代码论证——约 30 处反问注释标出所有关键参数与设计选择，均为"试出来的"魔数，无原理性推导。

## 经验魔数与作者质疑对照表（原文）

| 参数 | 取值 | 作者反问 |
|---|---|---|
| chunk_size / chunk_overlap | 1000 / 200 | 为什么是1000和200？怎么确定的？ |
| RAG query 历史拼接 | 最近 5 条 short_term_memory | 怕太长，但5条够吗？ |
| 向量检索 top_k | 3 | 类似llm chat调用，为什么是3？ |
| 历史注入条数 | 最后 10 条 | 为什么是取最后10条？ |
| MAX_STEPS / MAX_RETRIES | 5 / 3 | 为什么是5，而不是3或者7？ |
| 子 Agent max_steps | 3 | 不同子Agent执行参数怎么确定？ |
| 记忆压缩触发 | len(short_term_memory) > 20 | 总结一定对吗？总结会丢失细节 |
| 循环检测窗口 | 最近 3 次 action 相同 | 修改System Prompt还是直接中止？ |

## 七大类不确定性清单

RAG（chunk 参数/Embedding/阈值）· Memory（清理/索引）· Prompt Engineering（System Prompt 内容/Message 次序）· LLM 调用（模型与参数/失败处理/格式错误/结果评测）· Tool（过多精度下降/失败/高危误用）· 编排和 Multi-Agent（子 Agent 选择/结果整合/循环检测）· 安全和评测（Prompt Injection/权限/无统一质量标准/是否引入其他 LLM 评测）。

## 缓解之道

第 08 节 blockquote：理解基础原理后能预估参数效果（如 LangChain 的 chunk_size/overlap、Memory 逐出算法）——经验主义无法根除，但可通过原理性理解收窄试错空间。

## 关联

- 根源是 [[llm不确定性物理不可消除]]：底层不确定 + 语义失真，使确定性参数推导不可能。
- 与 [[文档分块参数调优]]（chunk_size=600/overlap=100 的有赞实证）、[[先易后难陷阱]]、[[agent评测四要素]] 互为印证：评测是应对经验主义的组织化手段。