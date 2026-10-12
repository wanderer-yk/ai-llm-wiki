---
type: concept
title: langchain语义标准化
tags: [langchain, 标准化, message, kv-cache]
related: [langchain, agent编排四等式, 结构化输出, kv-cache时间窗优化, 推理成本博弈论]
created: 2026-10-12
updated: 2026-10-12
sources: ["[202603240830]业务逻辑的坍塌当应用层只剩下胶水代码在AIAgent时代我们该构建什么-上.html", "[202603240830]业务逻辑的坍塌当应用层只剩下胶水代码在AIAgent时代我们该构建什么-下.html", "[202603240830]业务逻辑的坍塌当应用层只剩下胶水代码在AIAgent时代我们该构建什么.html"]
---
# langchain语义标准化

[[韩春]] 对 LangChain 的核心评价：其最大贡献不是编排能力，而是在**语义层面完成标准化**——与模型厂商形成 Message 四角色共识（System / User / Assistant / Tool），一并解决四个问题：

1. 角色边界模糊；
2. 信息来源混乱；
3. 无法利用 KV Cache；
4. 不同模型需要不同格式。

## 原文对照代码

```python
# 标准化之前：一切拼进一个 prompt
prompt = f"""
系统说明：你是一个助手。
用户之前说：你好
助手回复：你好，有什么可以帮你的？
用户现在说：帮我查天气
工具返回：北京今天晴天，25度
请根据以上信息回复用户。
"""

# 标准化之后，通过 Message 标识为
messages = [
    {"role": "system", "content": "你是一个助手"},
    {"role": "user", "content": "你好"},
    {"role": "assistant", "content": "你好，有什么可以帮你的？"},
    {"role": "user", "content": "帮我查天气"},
    {"role": "tool", "content": "北京今天晴天，25度", "tool_call_id": "xxx"},
]
```

## 定位的不稳固性

该标准化定位随基座模型能力演进并不稳固：确定性达阈值后将向完全 ReAct 迁移；注意力机制重大优化后输入处理职责将转移回基础模型。标准化的成本红利（KV Cache 前缀缓存）同时连接 [[推理成本博弈论]] 与 [[kv-cache时间窗优化]]。