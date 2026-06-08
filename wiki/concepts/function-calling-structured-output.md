---
type: concept
title: 基于 Function Calling 的结构化输出
tags: [结构化输出, function-calling, json-schema, agentscope]
related: [agentscope, ai-werewolf-game, bailian]
created: 2026-06-08
updated: 2026-06-08
sources: ["什么我的狼人杀水平还不如AI.html"]
---
# 基于 Function Calling 的结构化输出

[[agentscope|AgentScope]] 核心能力之一，确保 Agent 决策可被程序可靠解析，而非依赖正则匹配模糊自然语言。

## 问题

Agent 可能输出"我觉得 3 号有点可疑"，程序无法可靠提取投票目标。需要结构化输出如 `{targetPlayer: 3, reason: "..."}`。

## 实现流程

```
Java POJO (VoteModel)
    → JSON Schema
    → 注册为临时工具 generate_response
    → LLM 调用工具生成符合格式的响应
    → 自动验证转为 Java 对象
    → 失败自动重试
    → 清理临时工具
```

## VoteModel 示例

```java
// 投票结构化输出 POJO
public class VoteModel {
    private int targetPlayer;  // 投票目标
    private String reason;     // 投票理由
}
```

调用方式：`.call(prompt, VoteModel.class)` → `getStructuredData()`

## 优势

- 不依赖正则匹配，可靠性高
- 失败时框架自动提示修正并重试
- 开发者无需手动处理解析异常