---
type: concept
title: "SubAgent 与 Agent Teams 双模式"
tags: [claude-code, 多agent, 并发, 质检]
related: [claude-code, 并行开发隔离机制, 三级评审执行方式, 子agent单进程并发模型, 互斥锁与暂存队列, 硬性自检规则]
created: 2026-10-08
updated: 2026-10-08
sources: ["[202605090830]Harness实践让Agent自动制作知识讲解视频.html"]
---
# SubAgent 与 Agent Teams 双模式

本文对 Claude Code 两种多 Agent 能力的系统性对比与实战分工（截至成文，Agent Teams 为 Claude Code 独有且属实验性功能）。

## 双模式对比

| 维度 | SubAgent | Agent Teams |
|------|----------|-------------|
| 作者比喻 | 三个人分赴三个城市办事回来汇报 | 三人拉进一间会议室边讨论边改方案 |
| 结构 | 子进程：独立上下文、互不可见、不讨论 | 项目组：组长+组员独立会话、可互发消息、共享任务列表 |
| 适合任务 | 结果导向的并行任务（并行写章节） | 需要来回反馈的任务（质检） |
| 代价 | 省 Token、易调度 | 每个组员都是完整会话，Token 开销更高 |

## 实验开关与可视化

```json
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  },
}
```

配合 tmux 可在每个终端面板中观察组员行为。

## 实战分工

- **质检**：Agent Teams——独立 Reviewer Agent 从零逐项核查（Reviewer 发现字号太小→Developer 改大→复查）。
- **章节并行**：SubAgent 或 Agent Teams（"用 subagent 并行做第 2~N 章"，最大并行 3）。

该双模式与苏雄 800 行框架的 [[子agent单进程并发模型]]、[[互斥锁与暂存队列]] 构成多 Agent 并发治理的不同路线，值得对比研究。