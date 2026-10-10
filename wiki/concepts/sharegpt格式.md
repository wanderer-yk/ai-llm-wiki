---
type: concept
title: ShareGPT 格式
tags: [数据格式, 训练数据, hermes-agent]
related: [agent轨迹, 批量数据生成, research-ready训练闭环, hermes-agent]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604240830]深度解析HermesAgent如何实现自进化及其PromptContextHarness的设计实践.html"]
---
# ShareGPT 格式

ShareGPT 格式是 [[hermes-agent]] 整个自进化 Pipeline 的**统一数据格式**：Agent 轨迹（[[agent轨迹]]）经 `agent/trajectory.py` 转换为该格式后，供训练框架消费。选型理由是**生态兼容性**——LLaMA-Factory、FastChat、OpenChat 等主流训练框架均支持该格式；若单独改动 `"gpt"` 标签反而会导致训练数据无法导入主流工具。

## 格式示例（源文原样）

```json
[
  {"from": "system", "value": "你是 Hermes Agent."},
  {"from": "human", "value": "帮我部署这个应用"},
  {"from": "gpt", "value": "好的，我先检查环境."},
  {"from": "tool", "value": "<tool_call>execute_code(.)</tool_call>"},
  {"from": "tool", "value": "<tool_response>成功</tool_response>"},
  {"from": "gpt", "value": "部署完成！"}
]
```

## "gpt" 标签的历史遗留性质

`"from": "gpt"` 是历史遗留的行业约定，并非限定 GPT 模型：训练框架在训练时会把 `"from": "gpt"` 映射到具体模型 chat template 的正确 assistant token，因此非 GPT 模型（Qwen/Kimi/LLaMA）训练不会困惑。