---
type: entity
title: KAT-Coder
tags: [llm, code-model, ai-coding]
related: [kwaipilot, 快手技术团队]
sources: ["[202602112001]快手万人组织AI研发范式跃迁之路.html"]
created: 2026-06-22
updated: 2026-06-22
---
# KAT-Coder

KAT-Coder 是快手自研的代码大模型，内置在 [[kwaipilot]] 中以提升内部场景的代码生成效果。

## 核心特点

KAT-Coder 通过定期注入快手真实代码和研发过程数据进行训练更新，使模型"懂快手系统"——理解公司内部的业务概念、存量系统架构和编程规范。这是快手解决 [[通用工具通用效果瓶颈]]（"通用的工具只能达到通用的效果"）的核心手段之一。

在使用 KAT-Coder 之前，开发者需本地保存大量 Prompt 或手动配置规则，效果不理想。KAT-Coder 配合业务 & 研发知识库建设，共同解决了 AI 不"懂"业务语义和开发规则的问题。

## 待解决问题

- 训练数据规模、更新频率和注入机制的具体细节尚未公开
- 与业界主流代码大模型（如 DeepSeek-Coder、CodeLlama 等）的性能对比未披露