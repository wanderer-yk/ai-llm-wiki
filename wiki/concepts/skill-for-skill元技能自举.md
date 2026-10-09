---
type: concept
title: Skill for Skill 元技能自举
created: 2026-10-09
updated: 2026-10-09
tags: [skill, meta-skill, 自举, 长程任务, harness-engineering]
related: [long-term-task-orchestration, Skill-Creator, skill-creator三版演进, 自举式开发, skill迭代闭环, skills-cli跨平台安装, skill评测三原则, harness-engineering]
sources: ["[202604081800]HarnessEngineering让CodingAgent可靠完成长程任务.html"]
---
# Skill for Skill 元技能自举

Skill for Skill 元技能自举是《Harness Engineering: 让 Coding Agent 可靠完成长程任务》08 节提出的自举模式："用 Agent 来强化 Agent 的工作能力"——不是让 Agent 做一次任务，而是让 Agent 生产出能反复做这类任务的工具，从而省去每次长程任务都由工程师亲自编排的成本。

## 形成依据

作者在建设大量 Long Term Task Skill 后发现，各类长程任务 Skill 的骨架高度一致：SKILL.md（Phase 定义 + 会话恢复检测 + 完成标准）、scripts 脚本组、references 分阶段指令、evals 评估用例。既然骨架可复制，"创建长程任务 Skill"本身就可以固化为一个元技能（meta-skill）。

## 落地物：long-term-task-orchestration

[[long-term-task-orchestration]] 不直接执行业务任务，而是"教 Agent 如何创建新的长程任务 Skill"：生成完整骨架（SKILL.md + 6 个脚本 + 4 个 Phase reference + evals.json），并自动运行随装的 skill-eval 完成评测闭环（自动启动评测、可视化展示评测结果、确认后进入循环修复）。在 Claude Code 环境下经 skills CLI 安装，配合 [[Skill-Creator]] 使用：

```bash
npx skills add hixuanxuan/long-running-agent-tasks -y
/long-term-task-orchestration 创建skill实现React Compiler迁移并下线全部memo。
```

## 适用判定

作者给出的元技能适用判定："对大量同类目标执行相同的操作，并逐个验证结果"。

## 与相关方法论的关系

- 与 [[自举式开发]]（用自己的工具管理自身迭代）同属"用系统生产系统"的路线；
- 与 [[skill-creator三版演进]]、[[skill评测三原则]] 构成跨团队的"Skill 自生成 + 自评测"同向实践，[[skill迭代闭环]] 提供迭代方法论基础；
- 安装方式依赖 [[skills-cli跨平台安装]] 的 skills CLI 机制（`npx skills add` 为直接证据）。

来源：[[sources/[202604081800]HarnessEngineering让CodingAgent可靠完成长程任务|[202604081800]HarnessEngineering让CodingAgent可靠完成长程任务]]（无糖可乐，百度Geek说，2026-04-08）。
