---
type: concept
title: 角色专属 Prompt 策略
tags: [prompt设计, 博弈策略, 狼人杀, 角色]
related: [agentscope, react-paradigm, ai-werewolf-game]
created: 2026-06-08
updated: 2026-06-08
sources: ["什么我的狼人杀水平还不如AI.html"]
---
# 角色专属 Prompt 策略

在 AI 狼人杀中，不同角色使用差异化的 System Prompt 定义推理行为与博弈策略。

## 狼人双策略示例

狼人角色 Prompt 包含明确的欺骗策略层次：

### 策略A：悍跳狼
- 第一轮起跳冒充预言家
- 语气坚定，反指对手
- 目标：混淆好人阵营判断

### 策略B：深水狼
- 发言精简避免成为焦点
- 伪装普通村民
- 目标：低调存活到最后

## 设计要点

- Prompt 已涵盖博弈论层面的策略选择，不仅限于角色能力描述
- 不同角色（预言家、女巫、猎人、村民）的 Prompt 策略差异大
- 策略选择影响 [[react-paradigm|ReAct 循环]]的推理方向