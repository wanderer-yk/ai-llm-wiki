---
type: concept
title: Agentic Search（无索引流派）
tags: [agentic-search, 代码搜索, 无索引, claude-code]
related: [claude-code, 代码理解的五种范式, rag技术瓶颈, anthropic, 代码索引]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604231830]从可观测到可理解用UModel构建Agent原生的代码知识图谱.html"]
---
# Agentic Search（无索引流派）

Agentic Search 是《[202604231830]从可观测到可理解用UModel构建Agent原生的代码知识图谱》划分的两大代码理解流派之一（范式一）：**不做任何预索引，Agent 以 grep/rg/glob 实时搜索代码库**，代表为 [[claude-code|Claude Code]]（信奉 Unix 哲学）。

## 优势

零预处理、永远新鲜、隐私友好、简单可靠。

## 工作流与 token 成本分层（原文 verbatim）

```
Agent 收到问题
 → Glob: 按文件名模式匹配（近零 token 成本）
 → Grep (ripgrep): 按内容正则搜索（低 token 成本）
 → Read: 读取完整文件（高 token 成本）
 → 判断 → 下一轮搜索或给出答案
```

## 天花板

1. 无结构感知——搜得到文本，推不出依赖链；
2. 每次从零开始——无跨会话积累；
3. 规模受限——5 万文件企业级单仓需 30+ 轮工具调用、几万 token，只能拼出局部视图，不可能拼出完整全局图（200 文件 TS 项目则无碍）；
4. 无法全局分析——影响范围、架构边界等全局性问题超出了逐次搜索的能力范围。

## 关键外部证据

文章引用 Anthropic 内部测试结论：agentic search 性能全面超越 RAG（"by a lot, and this was surprising"，出自 Boris Cherny 公开分享；Claude Code 早期版本曾用 RAG + 本地向量库后放弃）。这与 [[claude-code]] 既有"纯 grep 方案"记载一致，对 [[向量检索rag]] 构成反向证据；但其测试口径与边界条件未公开，本文为转述。

## 与 CodeIndex 流派的共同失效

在影响范围分析、跨域根因定位、架构边界检查三类任务上两流派共同失效——这是 [[代码理解的五种范式]] 框架与 [[umodel|UModel]] 方案的出发点。