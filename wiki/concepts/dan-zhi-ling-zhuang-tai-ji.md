---
type: concept
title: 单指令状态机
tags: [状态机, specflow, ux, 心智负担]
related: [specflow, ssot-dan-wen-dang-ce-lue]
created: 2026-06-08
updated: 2026-06-08
sources: ["治愈CursorAI编程的幻觉用它就够了.html"]
---
# 单指令状态机

**单指令状态机** 是 [[specflow|Specflow]] 的核心 UX 设计。开发者仅需记忆 `/specflow` 一条万能指令，系统通过扫描 `ai-docs/` 目录下文件状态自动判定当前阶段（PM/架构师/开发者模式），实现"自动寻迹"。

## 设计动机

社区 SDD 方案（[[openspec|OpenSpec]]、[[github-spec-kit|GitHub Spec Kit]]、[[bmad-method|BMAD-METHOD]]）在 Cursor 中的核心摩擦之一是"心智负担重"——开发者需要记忆多个指令、手动管理阶段流转。单指令状态机彻底消除了这一摩擦。

## 工作原理

1. 开发者输入 `/specflow`
2. 系统扫描 `ai-docs/{ID}/` 目录下的文件状态
3. 根据文件存在性和内容完整性自动判定当前阶段：
   - 无 `specify.md` → 进入 Specify 阶段（PM 模式）
   - 有 `specify.md` 但无 `plan.md` → 进入 Plan 阶段（架构师模式）
   - 有 `plan.md` 且存在未完成的 Group → 进入 Implement 阶段（开发者模式）
4. 自动加载对应阶段的 Prompt 和角色设定

## 核心价值

- **零心智负担**：开发者无需记忆阶段流转规则
- **防误操作**：系统自动保证阶段顺序的正确性
- **状态持久化**：文件系统作为状态存储，解决了社区方案中"状态易丢失"的问题