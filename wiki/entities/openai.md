---
type: entity
title: OpenAI
tags: [ai-company, llm-provider, agent-tools]
related: [anthropic, codex, human-steer-agents-execute, harness-engineering-李伟山版]
created: 2026-07-20
updated: 2026-07-20
sources: ["[202605201731]干货从PromptContext到Harness工程的三次进化与终局之战原创.html"]
---
# OpenAI

OpenAI 是 AI 研究公司，大语言模型 GPT 系列和编码 Agent 工具 [[codex]] 的开发者。在 [[李伟山]] 的文章中，OpenAI 是核心实践案例的主体。

## 百万行代码实验

文章引言引用：OpenAI 3-7 人小团队，在 5 个月内通过 AI 生成近 100 万行生产级代码，全程无工程师手写业务逻辑代码，效率约为纯人工的 10 倍。

## 三大 Harness 策略

文章第 04 章详述 OpenAI 实践中总结的三大 Harness 策略：

1. **上下文治理（Context Governance）**：将巨型规范文件 `agent.md` 压缩为百行以内的索引目录，动态加载子文档；将技术决策记录迁移至代码仓库。详见 [[context-engineering-李伟山版]]。
2. **[[验证闭环]]（Verification Loop）**：Chrome DevTools 视觉验证 + 可观测性工具 + 强制 Lint/自动化测试，将"声称完成"变为"验证完成"。详见 [[声称完成vs验证完成]]。
3. **技术债清理（Tech Debt Cleanup）**：后台 [[codex]] 任务定期扫描修复重复命名、风格不一致、废弃文档等技术债，类比操作系统垃圾回收机制。

## 工程哲学

文章引用 OpenAI 实验总结的工程哲学：**"Human steer, agents execute"**（人类掌舵，Agent 执行）。详见 [[human-steer-agents-execute]]。

## 模型代际

文章提及的 OpenAI 模型代际：
- **GPT-3**：Prompt Engineering 技术兴盛期代表模型，需 Few-shot 才能完成复杂任务
- **GPT-4**：语言理解能力强到随意表达即可理解意图，Prompt Engineering 边际效益递减