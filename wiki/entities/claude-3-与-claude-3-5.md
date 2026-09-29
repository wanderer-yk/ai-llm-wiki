---
type: entity
title: Claude 3 与 Claude 3.5
tags: [anthropic, llm, model-version]
related: [anthropic, harness衰变定律, prompt-engineering-李伟山版]
created: 2026-07-20
updated: 2026-07-20
sources: ["[202605201731]干货从PromptContext到Harness工程的三次进化与终局之战原创.html"]
---
# Claude 3 与 Claude 3.5

Claude 3 和 Claude 3.5 是 [[anthropic]] 开发的大语言模型版本。在 [[李伟山]] 的文章中，这两个版本的对比研究是 [[harness衰变定律]] 的核心实证依据。

## Claude 3

与 GPT-4 并列作为模型智能化提升的代表。Claude 3 时代需要极严格的 Harness 约束：

- 逐个功能点执行
- 频繁重置上下文
- 大量硬编码检查规则

## Claude 3.5

全局统筹能力、长上下文处理能力、自我校验能力大幅提升。Claude 3.5 时代许多 Claude 3.0 时代必需的 Harness 规则自然失效——模型逐步内化了原本需要外部约束才能保证的系统规则。

## 在文章中的方法论意义

Claude 3.0 → 3.5 的版本对比是文章第 06 章 [[harness衰变定律]] 的核心证据：模型能力与 Harness 复杂度呈反比关系。这一发现具有双重深意——第一，Harness Engineering 是当下的现实答案（模型尚未完美）；第二，它可能是过渡性技术（模型持续内化系统规则）。

Claude 3 也作为模型智能化提升的代表出现在 [[prompt-engineering-李伟山版]] 的论述中：从 GPT-3 时代需精心 Few-shot 到 GPT-4 / Claude 3 时代随便一句话即可理解意图，精心设计 Prompt 的边际效益显著降低。