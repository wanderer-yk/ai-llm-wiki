---
type: entity
title: long-term-task-orchestration
tags: [meta-skill, 长程任务, 任务编排, skill]
related: [skill-for-skill元技能自举, harness-engineering, Skill-Creator, skills-cli跨平台安装, file-as-progress状态持久化, 分块token预算推导, 批判性evaluator校验]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604081800]HarnessEngineering让CodingAgent可靠完成长程任务.html"]
---
# long-term-task-orchestration

**long-term-task-orchestration 是一个开源的 meta-skill（元技能）**：它不直接执行业务任务，而是"教 Agent 如何创建新的长程任务 Skill"——输入一句目标描述，产出一套完整的可复用长程任务执行框架。由百度Geek说公众号作者无糖可乐在《Harness Engineering: 让 Coding Agent 可靠完成长程任务》（2026-04-08）08 节"从经验到框架：Skill for Skill"中发布，开源仓库为 `https://github.com/hixuanxuan/long-running-agent-tasks`（GitHub）。注意：仓库所有者账号 hixuanxuan 与作者"无糖可乐"的对应关系未在源文中证实。它是 [[skill-for-skill元技能自举]] 理念的实体载体。

## 生成产物：统一 Skill 骨架

作者观察"各类长程任务的骨架高度一致"，故 meta-skill 按统一模板生成完整骨架：

- **SKILL.md**：Phase 定义 + 会话恢复检测 + 完成标准
- **scripts/** 六脚本：`discover.js`（扫描目标生成任务清单，幂等）、`dispatch.js`（读清单分组并发调度 subagent）、`build-prompt.js`（程序化构建子任务 Prompt）、`poll.js`（轮询子任务状态+补位启动）、`merge.js`（合并最终产物）、`status.js`（查询整体进度）
- **references/** 四个 Phase reference（`phase0_setup.md` / `phase1_analyze.md` / `phase2_dispatch.md` / `phase3_finalize.md`），Agent 只读当前 Phase 所需指令
- **evals/evals.json**：评估用例

完整目录树见 [[sources/[202604081800]HarnessEngineering让CodingAgent可靠完成长程任务|[202604081800]HarnessEngineering让CodingAgent可靠完成长程任务]] 结构化数据一节。骨架内部方法（File As Progress、状态机、三层重试、批判性 Evaluator）分别对应 [[file-as-progress状态持久化]]、[[任务状态自描述]]、[[多轮重试三层]]、[[批判性evaluator校验]]。

## 安装与配套

```bash
npx skills add hixuanxuan/long-running-agent-tasks -y
```

- 安装方式为 `npx skills add`（[[skills-cli跨平台安装]] 的直接证据），源文环境为 Claude Code（源文"Cluade Code"为笔误）
- **随装附带 `skill-eval`**：自动启动评测、将 body 评测结果可视化展示并询问是否继续、确认后进入循环修复（评测细节仅有概述，待追踪开源仓库补全）
- **配合 `skill-creator` 一起使用**（→ [[Skill-Creator]]）
- 示例 prompt：`/long-term-task-orchestration 创建skill实现React Compiler迁移并下线全部memo。`

## 适用判定

作者给出的使用边界："**对大量同类目标执行相同的操作，并逐个验证结果**"的任务适合沉淀为长程任务 Skill 并由本 meta-skill 生成。

## Wiki 定位

- [[skill-for-skill元技能自举]]（概念）的实体承载
- 与 ConardLi [[Skill-Creator]] / [[skill-creator三版演进]] / skill 评测体系构成"自生成+自评测闭环"的比对候选（潜在 comparison 立项项）