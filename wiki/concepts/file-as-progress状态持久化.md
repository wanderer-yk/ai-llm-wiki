---
type: concept
title: File As Progress 状态持久化
created: 2026-10-09
updated: 2026-10-09
tags: [状态持久化, 可续传, 中断恢复, 长程任务, harness-engineering]
related: [长程任务四原则, 长程任务三困难, 双通道输出设计, 随到随补调度, 文件轮询架构, 验证门禁化, long-term-task-orchestration, 子任务CLI化]
sources: ["[202604081800]HarnessEngineering让CodingAgent可靠完成长程任务.html"]
---
# File As Progress 状态持久化

File As Progress 状态持久化是《Harness Engineering: 让 Coding Agent 可靠完成长程任务》的核心设计：作者在 6.3 专节中称其为"长程任务编排中最核心的设计"——把任务的全部进度状态持久化到文件系统，而非依赖 Agent 记忆或会话上下文，从而支撑中断后的精确续传。

## 三原则

1. 不依赖 Agent 记忆；
2. 不依赖会话上下文；
3. 只依赖磁盘文件。

写入纪律：每完成一步立即写入、不攒批——因为 Agent 随时可能被中断。载体形式（TSV/JSON/纯文本）不重要，关键是"一切状态都在文件里"的约束。

## 状态设计配套理念（6.3–6.5）

- **状态自描述**：仅凭当前状态就能决定下一步做什么，恢复逻辑无需回放历史，每个状态自身携带"接下来该怎么办"；
- **细粒度状态断点**：每多一个状态就多一个可精确恢复的断点，但每个中间状态必须搭配对应的落盘产物；10 秒级子任务用 TODO/DONE/FAILED 三态即可；
- **产出物优于文本输出判定**：判断任务是否完成看产出文件的存在性与合法性（TS 过编译 / JSON 可解析 / Review 符合 schema），不依赖解析 Agent 的文本输出；
- **IN_PROGRESS 残留三步判定**：①检查预期产出文件是否存在；②完整性校验，合法则直接置 DONE；③不存在或不合法则清理半成品（`git status`+`git diff` 识别未提交变更，`git worktree remove` 整体丢弃，或记录 commit hash 后 `git checkout <hash> -- <files>` 恢复），状态重置 TODO；
- **多层重试**：内层恢复会话（conversationId 断点续传降本）、中层带反馈重试（新会话 + 完整报错上下文，限 2-3 次，超限 revert 并标 FAILED）、外层主 Agent 重新调度（FAILED 两三个值得重跑，几十个说明任务规则有问题）。

## 落地证据闭环

- **7.1 全量 Code Review**：审查进度写入 TSV 用于续传（中断后定位 Phase、跳过已完成模块）；subAgent 读 `inputs/{chunkId}-input.json`、写 `segments/{chunkId}.json`，其存在性即为完成信号；
- **7.2 JS to TS 迁移**：`migration-tasks.tsv` 记录任务状态（running/pending/failed/success）、PID、启动时间；启动时做四条件顺序检查（`.agent.env` 含 token → tsv 已生成 → 无 IN_PROGRESS 残留 → 全部完成），从第一个不满足条件对应的 Phase 继续；
- 调度脚本（dispatch.js/poll.js）依据状态文件实现 [[随到随补调度]]，终端输出与状态文件遵循 [[双通道输出设计]]。

## 与相关方法论的关系

- 是 [[长程任务四原则]] 中"可续传"原则的实现机制，直接消解 [[长程任务三困难]] 中"中断要重来"的困难；
- 产出物程序化判定与 [[验证门禁化]] 同向；poll.js 轮询 + 补位与 zhiyuanfu 的 [[文件轮询架构]] 同范式；
- 整套骨架已固化为 [[long-term-task-orchestration]] meta-skill 模板的组成部分，子任务经 [[子任务CLI化]] 作为独立进程运行。

来源：[[sources/[202604081800]HarnessEngineering让CodingAgent可靠完成长程任务|[202604081800]HarnessEngineering让CodingAgent可靠完成长程任务]]（无糖可乐，百度Geek说，2026-04-08）。
