---
type: entity
title: Knowledge Wiki
tags: [知识层, AI-Coding, 有赞, Git-submodule, Skill]
related: [知识库降熵论, 渐进式披露替代向量检索, 分布式wiki架构, 多agent逆向工程初始化, 知识复利效应, 最小作用域原则, 知识准入控制, harness-engineering, 有赞共享技术]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202604071830]KnowledgeWiki面向AI的项目知识层建设实践.html"]
---
# Knowledge Wiki

有赞共享技术团队开发的面向 AI 的项目知识层系统，核心理念是将团队隐性经验结构化、外化为 AI 可消费的知识资产。

## 系统架构

- **存储**：分布式 `.wiki/` 目录跟随 Git submodule，知识与代码在同一分支提交，天然同步
- **检索**：渐进式披露——目录结构即检索路径，AI 按 workspace→app→context 逐层下钻
- **交付**：以可安装复用的 Skill 形式交付，覆盖知识库全生命周期

## 三种 Skill 模式

| 模式 | 功能 | 说明 |
|------|------|------|
| **read** | 逐层下钻读取 | AI 按 workspace→app→context 路径逐步获取上下文 |
| **init** | 多 Agent 初始化 | Controller→Implementer→Reviewer 三角色协作逆向工程 |
| **update** | 生成提案→用户确认→写回 | 六步闭环：判断价值→提炼候选→回看 wiki→输出提案→用户确认→写回 |

## 持续更新机制

- **AGENTS.md 常驻指令**：IDE 级别"始终在场"指令，包含 `Knowledge Wiki First`（遇事先查 wiki）和 `Knowledge Capture & Update`（有收获就提案）两个指令块
- 选择 AGENTS.md + Skill 作为最轻量方案，因为 Skill 的 description 匹配率不够理想

## 方案选型排除

评估过但未采用的工具：
- **RepoWiki**：业界 Code Wiki 类工具，自动生成架构设计和功能模块说明——全量 Code Wiki 太重，AI 理解代码能力已强、代码变更频繁同步难
- **DeepWiki**：开源 Code Wiki 项目——同样原因未采用

## 效果证据

定量数据（来自 [[知识复利效应]]）：
- 配置问题排查时间：30-60 分钟 → 1-2 分钟
- 团队对知识库反馈问题：每周 10+ → 1-2 个
- 1000+ 文件代码库效果尤其明显

## 未来规划

- 近期：接入飞书（工单→排查→沉淀闭环）
- 中期：服务端统一管理 + 向量检索
- 长期：从工具到范式

## 方法论定位

Knowledge Wiki 是 [[harness-engineering]] 在知识层面的系统性落地，以四维框架运作：约束（准入规则）、反馈（AI 行为质量反证）、验证（人工确认）、治理（作用域+分层+审查）。