---
type: concept
title: Code-Review Skill 多 Agent 架构
tags: [案例研究, subagent, code-review, 多agent, 评测]
related: [skill迭代闭环, skill-body评测对照实验, 四核心子代理角色, SubAgent与AgentTeams双模式, pre-pr机制, 高阶模型审查低阶模型, agent-skill]
created: 2026-10-08
updated: 2026-10-08
sources: ["[202603091800]打造高效易用的AgentSkill.html"]
---
# Code-Review Skill 多 Agent 架构

来源的案例二：作者将 Code-Review Skill 从单 Prompt 升级为 SubAgent 多阶段架构，并以相同评测集实测验证改进效果的完整迭代路径。

## v1 → v2 的动因

- **v1（直白 Markdown 指令）**：质量波动大；git diff 过多时上下文超限失败。
- **v2（SubAgent 架构）**：每个 Subtask Agent 只持有一个文件 diff + 源码，保持干净的注意力窗口，消除串行审查的上下文污染；阶段间明确输入输出契约与质量检查点；依赖文件系统可恢复，单任务失败不影响整体。

## 五阶段拆解（逐字保留）

**总览分析**（掌握全局）→ **分维度审查**（安全/性能/可维护性分别深入）→ **子 agent 交叉验证**（排除误报）→ **去重合并**（消除冗余）→ **最终报告**（按优先级排序输出）

## 量化验证

以 20 个 PR 组成相同评测集，由独立的 Grader Agent 评估三项指标：**输出质量、覆盖率、误报率**——v2 三项指标均明显提升。具体数值未公开。

## 跨来源同构

- 子代理分工审查与得物 [[四核心子代理角色]]、ConardLi [[SubAgent与AgentTeams双模式]] 同构。
- 作为提交前质检环节与美团 [[pre-pr机制]] 同构。
- 独立 Grader Agent 评分与美团 [[高阶模型审查低阶模型]]（Judge Model）同构。
- 本案例是 [[skill迭代闭环]] 在真实工程工件上的完整实例，评测方法即 [[skill-body评测对照实验]]。
