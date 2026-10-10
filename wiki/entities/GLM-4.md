---
type: entity
title: GLM-4
tags: [大模型, 智谱, chat-model, embedding]
related: [aiagentdemo, spring-ai, qwen]
sources: ["[202604220830]AI实践基于SpringAI从0到1构建AIAgent.html"]
created: 2026-10-10
updated: 2026-10-10
---
# GLM-4

GLM-4 是智谱 AI（Zhipu AI）推出的大语言模型（此身份为可独立核实的公开背景）；本文实证其作为[[aiagentdemo]]项目的 chat 模型，与 embedding 模型 embedding-3 搭配使用，经智谱 OpenAI 兼容端点 `https://open.bigmodel.cn/api/paas/v4` 接入 [[spring-ai]]。在本 wiki 中，GLM-4 与 [[deepseek-v3]]、[[qwen]]、[[gpt-4-1]] 同属「国内技术团队文章选型模型」序列。

## 项目中的配置

```ini
spring.ai.openai.base-url=https://open.bigmodel.cn/api/paas/v4
spring.ai.openai.api-key=你的API密钥
spring.ai.openai.chat.options.model=glm-4
spring.ai.openai.embedding.options.model=embedding-3
```

- **chat**：glm-4，驱动 AgentCore 的模型调用、意图识别、摘要压缩与 LLM Rerank
- **embedding**：embedding-3，驱动向量召回路与内存 VectorStore 的向量化

## 运行时可替换性

文章强调项目支持运行时动态切换模型提供商且无需重启，给出的示例即「从智谱切到通义千问」（[[qwen]]），并可运行时调参 temperature/maxTokens/topP。这表明 GLM-4 在该项目中是默认选型而非强绑定——OpenAI 兼容端点 + Spring AI 抽象层使模型成为可热插拔组件。