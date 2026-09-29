---
type: concept
title: Context Engineering（李伟山版）
tags: [context-engineering, ai-engineering, 第二次进化, rag, ssot]
related: [工程三次进化框架, prompt-engineering-李伟山版, harness-engineering-李伟山版, 上下文工程, 三大武器库, ssot单文档策略, 多源分治策略]
created: 2026-07-20
updated: 2026-07-20
sources: ["[202605201731]干货从PromptContext到Harness工程的三次进化与终局之战原创.html"]
---
# Context Engineering（李伟山版）

Context Engineering 是 [[李伟山]] 提出的 [[工程三次进化框架]] 中的**第二次进化**，解决模型"不知道关键信息"的问题。其本质是上下文窗口内多层信息的注意力管理。

## 金鱼记忆思想实验

文章以"记忆 7 秒的天才助理"类比 LLM 的上下文窗口限制：LLM 每次对话只能看到上下文窗口内的信息，窗口外一无所知。上下文包含多层信息，每层都至关重要但都在争夺有限的 Token 空间——核心是**信息注意力问题**。

## 三大核心技术手段

### 1. RAG 按需取用

核心思想："不存知识，存索引；需要什么，临时检索，精准注入"。革命性在于反转了传统做法（全知识写入 System Prompt 导致空间爆满、输出质量下降）。

### 2. 上下文压缩三策略

对抗上下文窗口溢出和"Lost in the Middle"现象（LLM 对上下文开头和结尾记忆较好，对中间大段内容关注度大幅下降）：

- **滚动摘要（Rolling Summary）**：对话历史压缩
- **重要性评分（Importance Scoring）**：信息优先级排序
- **层次记忆（Hierarchical Memory）**：分层存储与检索

### 3. 单一事实来源（SSOT 上下文纪律）

强制将所有技术决策、规范、文档归档进代码仓库，确保 AI 信息来源唯一、可追溯、版本受控。文章指出，技术决策散落在企微消息/腾讯文档/本地 PDF/GitHub Issue 中对 AI Agent 是灾难性的（"综合出四不像答案"）。

**OpenAI agent.md 实践**：从装满所有规范的巨型文件 → 压缩至百行以内的索引目录 → 动态加载子文档 → 模型遵从度和输出质量显著提升。

## 与现有 Wiki 概念的关联

- 本文"单一事实来源"与 [[ssot单文档策略]]（Specflow）、[[分布式wiki架构]]（有赞共享技术）形成 SSOT 概念网络。
- 本文"单一事实来源"（所有决策归档进代码仓库）与 [[多源分治策略]]（爱奇艺版"信息按职责分配到多个稳定位置而非集中为单一来源"）可能构成概念互补而非矛盾——两者从不同角度处理同一问题。
- 本文 Context Engineering 定义与 [[上下文工程]]（[[富城]]版"知识注入+数据注入+Agent接入"）和 [[三大武器库]]（[[binxiong]] 版"知识库+MCP+Skills"）可互补。
- 本文"知识库治理：维护单一事实来源"与 [[知识准入控制]]（有赞共享技术）可对照。

## 共同盲区

文章以"代码生成 Agent 失控"场景收束本章：即使 Prompt 和 Context 都做好，Agent 仍会出现未授权重构、虚假测试通过声明、命名风格不一致、重复代码生成等失控行为。这论证了 Prompt Engineering（"说对"）和 Context Engineering（"给对"）的共同盲区——无法解决系统层面的约束缺失问题，填补盲区需要第三次进化 [[harness-engineering-李伟山版]]。