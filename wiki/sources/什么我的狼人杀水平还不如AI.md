---
type: source
title: "什么？我的狼人杀水平还不如AI？"
authors: [亦盏, 望宸]
year: 2026
url: ""
venue: 微信公众号（阿里巴巴中间件）
tags: [agentscope, 多智能体, 狼人杀, 人机对战, reAct, java]
related: [agentscope, agentscope-java, ai-werewolf-game, msg-hub, multi-agent-formatter, react-paradigm]
created: 2026-06-08
updated: 2026-06-08
sources: ["什么我的狼人杀水平还不如AI.html"]
---
# 什么？我的狼人杀水平还不如AI？

## 基本信息

- **作者**：亦盏、望宸
- **发布方**：阿里巴巴中间件（微信公众号）
- **发布时间**：2026年1月8日
- **IP 属地**：浙江
- **标记**：原创

## 核心内容

文章以狼人杀这一社交推理博弈游戏为场景，完整展示了使用 [[agentscope|AgentScope Java 版]]（release/1.0.5）构建支持人机混合对战的 AI 狼人杀游戏的技术实践。

### 动机

作者因凑齐狼人杀人数困难，决定开发 AI 版本实现随时开局。

### 六大工程挑战与对应能力

| 挑战 | AgentScope 核心能力 |
|------|---------------------|
| Agent 需持续思考推理 | [[react-paradigm|ReActAgent]]（思考-行动-再思考循环） |
| 不同角色信息隔离 | [[msg-hub|MsgHub]]（频道隔离 + 广播控制） |
| 多说话者记忆混乱 | [[multi-agent-formatter|多智能体格式化器]]（消息标记 + 合并） |
| Agent 决策需明确可解析 | [[function-calling-structured-output|结构化输出]]（Function Calling） |
| 人类玩家接入 | [[agent-interface-polymorphism|UserAgent 接口多态]]（替换 ReActAgent 即可） |
| 游戏进程实时展示 | [[dual-perspective-architecture|SSE 双视角推送]]（玩家视角 + 上帝视角） |

### 技术栈

- **语言**：Java 17+ / Maven / Spring Boot
- **LLM 平台**：[[bailian|阿里云百炼]]（DashScope），使用 [[qwen3-plus|qwen3-plus]] 模型
- **示例项目**：[[werewolf-hitl|werewolf-hitl]]（`agentscope-examples/werewolf-hitl`）

### 关键设计

1. **角色 Prompt 策略**：狼人具备悍跳狼（冒充预言家）与深水狼（伪装村民）双策略
2. **MsgHub 广播模式**：讨论阶段自动广播（实时），投票阶段手动广播（延迟公布防跟票）
3. **格式化器**：通过 `Msg.name` 标记 + `<history>` 合并，将 18 条 user 消息压缩为 1 条，解决 LLM API 三角色限制
4. **结构化输出**：Java POJO → JSON Schema → 临时工具 `generate_response` → 自动验证，失败自动重试
5. **游戏循环**：夜晚（狼人 MsgHub 讨论 + 神职独立调用）→ 白天讨论 → 投票放逐 → [[werewolf-game-loop|GameState]] 胜负判定
6. **Human in the Loop**：[[sinks-one-async-pattern|WebUserInput 基于 Reactor Sinks.One]] 实现异步等待，UserAgent 与 ReActAgent 同接口

### 未来方向

- 多人混合对战模式（多人 + AI）
- 全模态交互（TTS + 角色专属声线，Python 版已实现）
- 社区贡献：新角色（白痴、守卫、丘比特）

## 贡献度评估

文章从一个完整的游戏场景出发，系统展示了多智能体框架在信息不对称、博弈推理、人机协作等场景下的工程实践。六大挑战与六项能力的对应关系清晰，代码示例完整，是理解 [[agentscope|AgentScope]] 框架设计理念的优质案例。