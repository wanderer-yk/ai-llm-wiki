---
type: concept
title: 全生命周期hook机制
tags: [扩展机制, harness-engineering, hermes-agent]
related: [hermes-agent, openclaw, claude-code, 插件化生态扩展, harness-engineering, agent-loop, 结构化错误分类自愈体系, 受控子Agent机制]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604240830]深度解析HermesAgent如何实现自进化及其PromptContextHarness的设计实践.html"]
---
# 全生命周期hook机制

全生命周期hook机制指 Agent 框架在运行生命周期的关键节点预留钩子（Hook），允许在不修改核心代码的前提下注入自定义逻辑（日志、权限校验、业务规则等）。来源文章指出该机制为 [[hermes-agent]]、[[openclaw]]、[[claude-code]] 三家共有，是 Agent 框架扩展性的行业趋同设计；Hermes 的差异在于钩子覆盖面更细——达 9 项，并延伸到压缩、记忆、委派三大子系统。

## Hermes Hook 生命周期全表（9 项）

| Hook 函数 | 触发时机 |
|---|---|
| `on_agent_start()` | 初始化 |
| `on_tool_call()` | 工具调用前 |
| `on_tool_result()` | 工具返回后 |
| `on_agent_end()` | Agent 关闭时 |
| `on_turn_start()` | 每轮开始时 |
| `on_pre_compress()` | 压缩前，可以在消息被丢弃前提取有用信息 |
| `on_memory_write()` | 写入内置记忆时 |
| `on_delegation()` | 子 Agent 完成任务后 |
| `on_session_end()` | 会话结束 |

## 关键特征

- **零核心代码改动**：Hook 与外部记忆组件、自定义工具一样以插件形式存在（见 [[插件化生态扩展]]），这是扩展性与内核稳定性解耦的架构基础
- **覆盖面超出主循环**：`on_pre_compress`（压缩前信息抢救）、`on_memory_write`（记忆写入拦截）、`on_delegation`（委派回调，配合 [[受控子Agent机制]]）使 Hook 不仅包裹 [[agent-loop]] 主循环，还包裹压缩、记忆、委派子系统
- 与 [[结构化错误分类自愈体系]] 配合，共同构成 Hermes Harness 的运行时监控与自愈能力

## 开放问题

- `on_pre_compress()` 中"提取有用信息"的去向（写入内置记忆还是 SQLite 轨迹库）未说明
- 各 Hook 的注册方式、执行顺序保证与异常隔离机制未披露
