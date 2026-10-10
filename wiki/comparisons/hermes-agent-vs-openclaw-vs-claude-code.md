---
type: comparison
title: "Hermes Agent vs OpenClaw vs Claude Code"
tags: [对比, agent架构, hermes-agent, openclaw, claude-code]
related: [hermes-agent, openclaw, claude-code, 内外双路径自进化, SQLite全量对话持久化, 内外双驱记忆架构, 即时上下文注入, 全生命周期hook机制, 结构化错误分类自愈体系, 受控子Agent机制, 比例阈值压缩, 双压缩范式对比, Agent发展三阶段, 插件化生态扩展, 多层安全护栏]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604240830]深度解析HermesAgent如何实现自进化及其PromptContextHarness的设计实践.html"]
---
# Hermes Agent vs OpenClaw vs Claude Code

来源文章（"项目深度解析"系列第 3 篇，前两篇分别为《深度解析 OpenClaw》与《深度解析 Claude Code》）对三个 Agent 系统做了持续逐项对比。作者的总结定性：**Hermes 的基础架构、System Prompt 拼装逻辑与上下文管理同 OpenClaw、Claude Code 高度相似，核心突破在于解决了二者未解决的"Agent 无法自我学习和进化"痛点**——即以 [[内外双路径自进化]]（Skill 动态沉淀 + RL 闭环训练）完成 [[Agent发展三阶段]] 中"自主 → 自进化"的跨越。

## 逐维度对比

| 维度 | Hermes Agent | OpenClaw | Claude Code |
|---|---|---|---|
| 定位 | 部署在服务器上的持久化、自进化自主智能体 | 自主 Agent 基座 | 编码 Agent CLI |
| SQLite 持久化内容 | 全部每日对话历史 | Memory Chunk 索引（非全文） | — |
| 记忆架构 | 内外双驱（内部文件 + 外部记忆服务，见 [[内外双驱记忆架构]]） | 单一内部记忆 | — |
| 上下文压缩触发 | 比例阈值（窗口 50%，见 [[比例阈值压缩]]），泛化强 | 绝对阈值，需按窗口调配置 | 摘要式压缩（参见 [[上下文压缩策略]]） |
| Token 计数 | 实时粗估（4字符≈1Token）/ 离线 HuggingFace Tokenizer 精确计数（见 [[双压缩范式对比]]） | — | — |
| 上下文注入 | `@` 即时挂载、主动预加载（见 [[即时上下文注入]]） | 被动工具调用 | 被动工具调用 |
| 工具引导 | `agent.tool_use_enforcement` 按模型"性格差异"因材施教（auto/true/false/模型列表四级） | — | — |
| Hook 机制 | 9 项，覆盖压缩/记忆/委派子系统（见 [[全生命周期hook机制]]） | 有 | 有（三家共有，Hermes 粒度更细） |
| 错误自愈 | 14 类标准化错误分类 + Recovery Strategy（见 [[结构化错误分类自愈体系]]） | 未详细呈现 | 未详细呈现 |
| 子 Agent 沙箱 | 显式工具黑名单 + `MAX_CONCURRENT_CHILDREN = 3` + `MAX_DEPTH = 2`（见 [[受控子Agent机制]]） | 未展开 | 未展开 |
| 插件生态与安全护栏 | 插件系统（[[插件化生态扩展]]）+ Prompt 注入检测 + Skill 静态扫描（[[多层安全护栏]]） | — | — |
| 自进化 | Skill 动态沉淀 + RL 闭环训练 | 无（经验不自动沉淀） | 无 |

注："—"表示来源文章在该维度未给出该系统的对应细节，并非"不存在该能力"。

## 对比要点

1. **趋同面**：System Prompt 动态拼装（身份 → SOUL.md → 工具指南+元数据）、SQLite 持久化、Hook 机制三家共有——基础架构层呈行业收敛态势
2. **Hermes 差异面**：持久化对象（全量对话 vs 索引）、记忆双层、比例阈值压缩、@ 即时注入、错误自愈体系、子 Agent 沙箱显式约束、插件化生态
3. **根本分歧**：自进化能力——传统范式"每一次任务执行往往都是从零开始的探索，过往的弯路、纠错过程以及人工干预的经验，大多随着会话结束而消散"；Hermes 打通"任务执行→经验记录→Skill 抽象→模型再训练"完整链路

## 口径注意事项

- 来源内部对 OpenClaw 持久化的表述经历了"无状态 → SQLite 持久化 → 存 Memory Chunk 索引（非全文）"的精确化过程，引用时以最终口径为准
- 子 Agent 嵌套存在散文（单层）与代码注释（`MAX_DEPTH = 2` 两层）的口径张力，见 [[受控子Agent机制]]
