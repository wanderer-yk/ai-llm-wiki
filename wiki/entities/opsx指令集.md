---
type: entity
title: opsx 指令集
tags: [openspec, codebuddy, 命令, 工作流]
related: [openspec, codebuddy, ai-native研发模式, 原子化变更原则, mr双重视角审查]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202604031907]当整个团队开始0人工Coding一份万字AINative研发实战手册.html"]
---
# opsx 指令集

[[openspec]] + [[codebuddy]] 的命令式工作流体系，包含 8 条命令，覆盖从需求探索到归档同步的完整研发流程。

## 核心流程（4 条）

| 指令 | 功能 | 关键说明 |
|------|------|---------|
| `/opsx:explore` | 需求探索 | 与 AI 讨论需求、沉淀文档，**绝对不写代码** |
| `/opsx:propose` | 规划文档 | 一键生成 proposal.md + design.md + tasks.md + specs/，**严禁直接让 AI 写业务代码** |
| `/opsx:apply` | AI 按图施工 | 读取 tasks 清单上下文，跨文件批量生成/修改，每完成一项自动打钩，开发者只需做 Code Review |
| `/opsx:archive` | 归档同步 | MR 通过后将规范增量合并到 `openspec/specs/` 主目录，实现文档与代码永远同步 |

## 扩展指令（4 条）

| 指令 | 功能 | 适用场景 |
|------|------|---------|
| `/opsx:new` | 创建空脚手架 | 新模块/新功能初始化 |
| `/opsx:continue` | 步进式生成 | 复杂需求，先 Proposal→Review→再 Design，逐步推进 |
| `/opsx:ff` | 快进补全 | 一次性补全所有规划文档 |
| `/opsx:verify` | AI 代码审计 | 比对 design.md 进行代码审查 |

## 关键工程约束

- **tasks.md 15 项上限**：超过 15 项时 AI 因上下文过长易产生幻觉
- **幂等安装**：Skill 安装脚本设计为已装不重装，中断后可重来
- **扩展模式按需开启**：通过 `openspec config profile` 选择"Workflows only"后勾选启用

## 文档产出结构

每次变更产出标准化文档集于 `openspec/changes/[变更名]/` 目录下：
- **proposal.md**：为什么做 / 做什么
- **design.md**：怎么做
- **tasks.md**：施工清单（≤15 项）
- **specs/**：规范增量 / 差异记录

## 与其他工作流的对比

- 与爱奇艺 [[单指令状态机]]（`/specflow` 一入口）异曲同工但命令更丰富
- 与爱奇艺 [[ssot单文档策略]]（`plan.md` 单文件）不同，腾讯版采用多文件分目录策略