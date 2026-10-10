---
type: concept
title: memdir结构化记忆系统
created: 2026-10-10
updated: 2026-10-10
tags: [claude-code, agent记忆, context-engineering, 长期记忆]
related: [三层上下文压缩, 内外双驱记忆架构, 声明式事实记忆, 即时上下文注入, memory与skill职责边界, SQLite全量对话持久化, 上下文压缩策略]
sources: ["[202604200830]深度解析ClaudeCode在PromptContextHarness的设计与实践.html"]
---
# memdir结构化记忆系统

**memdir 结构化记忆系统**是 [[claude-code]] 的跨会话记忆架构：以目录化文件（`memdir/`）按语义分类存取记忆，配合预算化加载与 LLM 语义检索，使 Agent 从"用完即走"变为具备持续学习和自我修正能力。

## 四类核心记忆

| 类型 | 内容 |
|------|------|
| User | 用户偏好、习惯、指令风格 |
| Feedback | 行为修正与避坑指南 |
| Project | 技术选型、架构决策、约束 |
| Reference | 通用文档片段与代码模式 |

## 加载与检索

- **记忆加载预算裁剪**：`loadMemoryPrompt`（`memdir/memdir.ts`）扫描归类 + 按上下文窗口大小动态裁剪 + 格式化注入，防止记忆过载。
- **LLM-in-the-loop 记忆检索**：`findRelevantMemories.ts` 用 Sonnet 充当"图书管理员"做语义相关性判断，强制最多返回 5 条，平衡召回率与精确度。

## 记忆定位差异论

Claude Code 偏向记忆「项目文档、参考、用户偏好和反馈」；[[openclaw]] 记录「对话中的重点历史信息」（长期 MEMORY.md + 每日 memory/日期.md + 检索 + 时间衰减，模拟人的记忆）——Agent 系统定位差异决定 Memory 设计差异。与 HermesAgent 的 [[内外双驱记忆架构]]、[[声明式事实记忆]]、[[即时上下文注入]] 构成跨框架记忆机制对比素材。
