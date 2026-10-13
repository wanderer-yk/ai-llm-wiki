---
type: concept
title: Ralph Loop
tags: [agent范式, 持续迭代, 循环控制, ralph-wiggum]
related: [stop-hook拦截机制, 机器可验证完成条件, 循环控制外部化, llm自我评估不可靠, 状态外置持久化, 上下文腐烂, hitl到afk渐进路径, ai编程四阶段演进, ralph成本控制, ralph替代循环, agent-loop, react-agent, plan-and-execute模式, task-driven对goal-driven, claude-code, deepagents, kimi-cli, ralph-loop-agent, agents-md, docker沙箱隔离]
created: 2026-10-13
updated: 2026-10-13
sources: ["[202601270830]从ReAct到RalphLoopAIAgent的持续迭代范式.html"]
---

# Ralph Loop

Ralph Loop 是一种以持续迭代修正为中心的 agent 运行范式：同一提示词被循环注入执行，直到满足明确的机器可验证完成条件或达到迭代上限；通过 Stop Hook 拦截退出信号并以外部状态（文件系统、测试结果、Git 历史）形成自我参照反馈循环，使代理"未达客观成功标准就无法下班"。名称来自《辛普森一家》角色 Ralph Wiggum，诞生于 Claude Code 社区，哲学根源为 Bash 循环思维——"将失败视为数据、将持久性置于完美之上"。

## 与一般 agent loop 的关系（正交而非替代）

文章结论显式确认：Ralph Loop 与一般 agent loop **并不冲突**（[[agent-loop]]）。一般 agent loop 是内部感知—决策—执行—反馈循环，按状态/输入动态调整，适合决策型任务与战略规划；Ralph Loop 是自动驱动的 refine-until-done 模式，专注可验证任务的持续迭代修正，而非全面 agent lifecycle 管理。与 [[react-agent]]（动态适应性好但受制自我评估）和 [[plan-and-execute模式]]（结构化分解但环境适应性差）构成三种循环范式的对照。

## 核心三要素

1. **明确任务 + 完成条件**（可验证成功标准，详见 [[机器可验证完成条件]]）
2. **Stop Hook 阻止提前退出**（详见 [[stop-hook拦截机制]]）
3. **max-iterations 安全阀**（防无限循环，详见 [[ralph成本控制]]）

## 关键机制

- **循环控制外部化**："是否继续"的决策权从模型内部上下文移到外部 bash/Stop Hook 层，本质是强制性的——不依赖智能体主观判断，依赖外部验证（[[循环控制外部化]]）。根因是 [[llm自我评估不可靠]]。
- **状态外置持久化**：progress.txt（追加日志）+ prd.json（结构化任务清单，`passes: true` 标记）+ Git 提交（回滚与 Diff 评估）三组件（[[状态外置持久化]]）。
- **刷新上下文**：每轮循环视为全新会话，靠文件读取同步状态，解决 [[上下文腐烂]]。
- **工具输出升格**：常规循环中工具输出仅是推理参考；Ralph 循环中 Bash/Git/Linter/Test Runner 的输出成为决定循环存续的"客观事实"。

## 最简实现

```bash
while :; do
  cat PROMPT.md | claude-code --continue
done
```

## AI 编程四阶段演进中的位置

Vibe 编程 → 规划模式 → 多阶段计划 → **Ralph**。每阶段解决前一阶段的局限，Ralph 终态 = 代理自主选任务、人只定义最终状态（详见 [[ai编程四阶段演进]]）。"代理从 PRD 中选择下一步"是 [[task-driven对goal-driven]] 跃迁的直接表述。

## 场景适用边界

✅ 适合：TDD 开发（写测试→跑失败→改代码→重复直到全绿）、Greenfield 项目（定义好需求过夜执行）、有自动验证的任务（测试/Lint/类型检查）、代码重构（机械化重构、大规模测试迁移）、测试迁移（Jest→Vitest）。

❌ 不适合：需要主观判断或人类设计抉择、没有明确成功标准的任务、整体策略规划和长期决策（常规 Agent Loop 更适合，呼应 [[自动化决策层级]]——确定性任务用确定性机制）、成本敏感场景（可能运行数小时甚至几十个小时）。

裁决逻辑：按任务可验证性/主观性/成本三维判定——可验证目标条件任务用强化迭代；战略规划与长期决策回退到常规 Agent Loop。

## 可配置性核心论断

- "代理选择任务，而不是你。你只是改变该列表的位置。"（任务源可替换：本地 prd.json → 任意来源）
- "任何可以描述为'查看仓库，改进某些东西，报告发现'的任务都适合 Ralph 模式。循环是相同的。只有提示改变。"
- 输出可替换：开分支提 PR / 评论 issues / 更新变更日志。
- 四种替代循环见 [[ralph替代循环]]。

## 工具生态

官方插件 ralph-wiggum（[[claude-code]]）、框架级原生支持（[[deepagents]]）、`loop_control` 配置（[[kimi-cli]]）、编程式 JS 包（[[ralph-loop-agent]]）。安全运行依赖 [[docker沙箱隔离]] 与 [[hitl到afk渐进路径]]。

## 跨来源印证

- 可验证门控：[[验证门禁化]]、[[闭环负反馈控制]]、[[reflection模式]]。
- 状态落盘：[[text大于brain落盘原则]]、[[文件轮询架构]]、[[ssot单文档策略]]、[[sdd留痕进化论]]。
- 小步提交：[[原子化变更原则]]（[[小步迭代]]）。
- 过夜无人值守：[[24h打工人]]（Greenfield 过夜执行对照）。