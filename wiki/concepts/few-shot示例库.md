---
type: concept
title: few-shot示例库
tags: [提示词工程, few-shot, 代码修复]
related: [六步智能提示词生成法, 分层模板体系, 结构化输出]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202512261820]回收团队基于Cursor集成MCP的智能代码修复提示词生成实践.html"]
---
# few-shot示例库

[[六步智能提示词生成法]]第五步的核心设计。`ExampleRepository` 预置修复前后对比案例，让 AI 通过案例学习修复模式，而非仅依赖抽象文字指令。

## 设计理念

Few-shot 修复前后对比比纯文字描述更有效引导 AI 学习修复模式。这一设计可视为 [[结构化输出]] 在代码修复场景的具体应用。

## 示例配置

| 问题类型 | 示例数量 | 内容 |
|---------|---------|------|
| NullPointer | 2 个 | 单参数检查、多参数检查 |
| ResourceLeak | 1 个 | 资源关闭修复 |
| SQLInjection | 1 个 | 参数化查询修复 |
| 其他 | 无专属示例 | 依赖通用模板 |

## 扩展机制

当前为静态预置示例。开放问题：是否支持从历史修复记录中动态学习和扩展。

## 局限

仅 NullPointer/ResourceLeak/SQLInjection 三种类型有专属示例，与 [[sonar]] 规则映射的 7 种类型存在缺口——CommandInjection、DuplicateString、NamingConvention、General 均无专属示例，修复质量可能参差不齐。