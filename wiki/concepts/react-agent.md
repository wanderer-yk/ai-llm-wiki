---
type: concept
title: ReActAgent
tags: ["ReAct", "Agent", "LLM推理", "持续思考", "agent范式", "推理", "工具调用", "lynxe"]
related: ["agentscope", "ai狼人杀", "多智能体消息机制", "agent-loop", "react五要素", "思考内化论", "plan-and-execute模式", "reflection模式", "workflow优先于agent", "codeact架构", "lynxe菱科斯原-jmanus", "沈询", "ReAct推理模块"]
created: 2026-06-12
updated: 2026-10-12
sources: ["[202601082000]什么我的狼人杀水平还不如AI.html", "[202602240830]AIAgent系列什么是ReActAgent.html"]
---
# ReActAgent

ReActAgent（也常写作 ReAct Agent）是基于 ReAct（Reasoning + Acting，推理 + 行动）范式构建的 AI Agent：LLM 在一个整体目标下“一边想一边做”，通过**观察 → 思考 → 行动 → 再观察**的循环推进任务，直到完成，而不是事先把整个流程规划好再执行。

[[agentscope|AgentScope]] 框架基于该范式内置了 ReActAgent 组件，支持**持续思考**；它既是 [[ai狼人杀|AI 狼人杀]] 游戏中所有 AI 角色的基础实现，也是多智能体场景的核心实现组件。在 Lynxe 等工程框架中，ReAct 范式被进一步拆解为可复用的实现要素。

## 范式核心

- **增量式决策**：每一步根据当前的历史上下文和环境信息动态决定下一步该做什么；如果某一步失败（如搜索失败），可以换关键词、换策略，而不是卡死在预设流程上
- **沈询的类比**：传统方式像出门前查好全部路线严格照走；ReAct 像走到路口看路牌再决定左转还是右转，边走边找咖啡店
- ReAct 是基础 Agent 模式之一，与 [[plan-and-execute模式]]（先规划后执行）、[[reflection模式]]（反思改进）并列，另见 [[codeact架构]]（以可执行代码统一行动空间）

## AgentScope 中的 ReActAgent

[[agentscope]] 框架内置 ReActAgent 组件，支持**持续思考**，用于让每个 AI 玩家具备持续思考和对话的能力，是 [[ai狼人杀]] 等多智能体场景的核心实现组件。

### 四个关键配置

| 配置 | 说明 |
|------|------|
| `name` | 玩家标识，如“1号玩家”、“3号玩家" |
| `sysPrompt` | 角色提示词，定义角色的行为规则和策略 |
| `model` | 底层 LLM 模型（如 qwen3-plus） |
| `memory` | 对话记忆（InMemoryMemory） |

### 游戏中的 ReAct 循环

在 [[ai狼人杀|AI 狼人杀]] 中，ReAct 让 LLM 遵循“思考-行动-再思考”的循环：

1. **Reasoning** — 基于历史对话和当前局势进行推理
2. **Acting** — 根据推理结果做出发言或决策
3. **循环** — 将新信息写入 Memory，作为下一轮推理的输入

### 角色提示词设计

不同角色的 sysPrompt 差异显著：

- **狼人**： 需学会撒谎配合，内置悍跳狼（冒充预言家）和深水狼（伪装村民）两种策略
- **神职角色**： 需懂得使用技能（预言家查验、女巫救人/毒人、猎人开枪）
- **村民**： 需分析逻辑，识别可疑发言

### 与 UserAgent 的关系

ReActAgent 与 [[human-in-the-loop|UserAgent]] 实现同一接口。在 [[ai狼人杀|AI 狼人杀]] 中，只需将某个位置的 ReActAgent 替换为 UserAgent 即可让人类玩家加入，编排器代码无需任何修改。

## 工程实现五要素（沈询/Lynxe 视角）

沈询基于 Lynxe 框架实战将 ReAct 实现拆解为五要素（详见 [[react五要素]]）：历史上下文（History）、当前环境信息（Current Environment Information）、LLM Thinking（大脑）、工具/动作（toolcall，手脚）、观察结果（toolcall 结果）。

## 固定提示词模板

ReAct 轮次推进的工程本质：**提示词跨轮次固定不变，仅历史上下文增长**。

```
已知：
当前历史上下文：&{历史上下文}
当前环境信息：&{当前环境信息}
用户目标："帮我查一下北京今天天气怎么样，适合穿什么衣服。"

做出下一步的决策

你必须最少使用一个工具来实现该决策
```

## 思考是可选输出（工程变体）

学术 ReAct 通常要求显式的 Thought-Action-Observation 文本轨迹；而 Lynxe 的工程实现中思考过程隐藏、直接输出 toolcall——推理能力隐式存在于 toolcall 的生成过程中。这是对经典表述的工程化变体而非否定，两种形态并存，需注意区分（详见 [[思考内化论]]）。

## 与 agent-loop 的关系

主循环伪代码：While 循环 + 未完成判断 + `最大轮次 = 10` 上限，每轮“获取环境信息→构建提示词→调用 LLM→解析并执行工具→将行动与观察追加进历史上下文”。这是 [[agent-loop]] 概念的直接工程实例。

## 权衡：灵活性 vs 可预测性

- **优势**：能应对 API 报错、数据格式不对等意外情况，根据实际情况灵活调整策略
- **代价**：行为可能不那么可预测，AI 有时会“犯傻”
- **张力**：这一权衡与有赞 [[workflow优先于agent]]（“确定性 Workflow 优先”）的立场形成对照；在处理复杂、不确定的任务时，边想边做的方式仍有优势，而确定性要求高的场景则倾向 Workflow

## 其他应用场景

除 [[ai狼人杀]] 外，在 [[智空间]] 的助手工厂架构中，ReAct 推理模块被用于 [[意图路由]] 场景（见 [[ReAct推理模块]]、[[ReAct双引擎场景适配]]）。