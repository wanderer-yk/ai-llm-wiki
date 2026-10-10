---
type: comparison
title: Skill 设计模式"五加一"对比
tags: [skill, 设计模式, 对比, 决策树, token预算]
related: [skill线性流程模式, skill决策树加按需加载模式, skill循环迭代模式, skill接力棒循环模式, skill多阶段检查点编排模式, skill思维框架模式, 模式选择决策树, vercel-deploy, cloudflare-deploy, cloudflare导航型skill, test-driven-development, stitch-loop, discovery-process, audit-context-building]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604270830]工作流的Skill怎么写从7个顶级Skill中提炼的模式与最佳实践.html"]
---
# Skill 设计模式"五加一"对比

本页基于 [[青斧]]《工作流的 Skill 怎么写？》第八章作者亲制速查表（权威终局数据源）与各模式正文，对 5 种核心设计模式 + 1 个特殊模式做横向对比。

## 7 个 Skill 速查表（verbatim）

| # | Skill | 来源 | 模式 | 行数 | 一句话精髓 |
|---|-------|------|------|------|-----------|
| 1 | [[vercel-deploy]] | OpenAI | 线性 | 77 | 最小但完整的 Skill 模板 |
| 2 | [[cloudflare-deploy]] | OpenAI | 线性+决策树 | 224 | 大平台的渐进式披露 |
| 3 | [[cloudflare导航型skill\|cloudflare]] | OpenCode | 纯决策树 | 211 | 导航型 vs 操作型的区别 |
| 4 | [[test-driven-development]] | obra | 循环迭代 | 371 | 堵死 LLM 偷懒的所有退路 |
| 5 | [[stitch-loop]] | Google Labs | 接力棒循环 | 203 | 文件即状态，跨 session 持久化 |
| 6 | [[discovery-process]] | Dean Peters | 多阶段+检查点 | 502 | 编排器模式，调度 10+ 子 Skill |
| 7 | [[audit-context-building]] | Trail of Bits | 思维框架 | 302 | 控制 LLM "怎么想"而非"做什么" |

行数序列 77 / 224 / 211 / 371 / 203 / 502 / 302 与流程复杂度正相关：越靠后的模式，需要 LLM 自主管理的状态、阶段与决策越多，Skill 文本越长。

## 判据与结构对比

| 模式 | 适用判据 | 时间尺度 | 代表结构 | 关键技巧集 |
|------|----------|----------|----------|-----------|
| 1 线性流程（[[skill线性流程模式]]） | "先做 A，再做 B，最后做 C"的明确步骤 | 单次执行 | 标题/Prerequisites/Quick Start/Fallback/Troubleshooting | 安全默认值、具体命令、超时提示、降级方案、负面指令 |
| 2 决策树+按需加载（[[skill决策树加按需加载模式]]） | 10+ 分支且每分支大量详细文档 | 单次选型 | 认证前置→意图分类决策树→产品索引表 | 用户意图分类、渐进式披露（7KB→几十万字） |
| 3 循环迭代（[[skill循环迭代模式]]） | 单次会话内"做→验证→改进" | 分钟~小时 | Iron Law→Red-Green-Refactor→Rationalizations→Checklist | Good/Bad 对比、12 借口反驳、8 项 checklist、人类兜底 |
| 4 接力棒循环（[[skill接力棒循环模式]]） | 跨 session 持续工作或多 Agent 协作 | 天~周 | The Baton System→Execution Protocol 6 步→Orchestration Options | 文件即状态、续命机制（Step 6 Critical+MUST）、文件协议、编排无关 |
| 5 多阶段+检查点（[[skill多阶段检查点编排模式]]） | 跨越多天/多周且有 Go/No-Go 决策 | 多天~多周 | Key Concepts→Phase 1-6→Complete Workflow→References | 统一阶段模板、Go/No-Go 检查点、编排器（10+ 子 Skill）、时间影响、交互协议分离 |
| 特殊 思维框架（[[skill思维框架模式]]） | 需控制"思维质量"而非"操作步骤" | 深度分析任务 | Purpose→三阶段分析→Stability Rules→Non-Goals→子 Agent | Purpose 声明、量化阈值（3 不变量/5 假设）、反幻觉规则、思维工具注入 |

## 模式 3 vs 模式 4 四维对比

| 维度 | 循环迭代（模式 3） | 接力棒循环（模式 4） |
|------|-------------------|---------------------|
| 状态存储 | 对话上下文 | 外部文件（`next-prompt.md` 接力棒） |
| 跨 session | 否（单次会话） | 是（跨 session 持久化） |
| 循环退出 | Verification Checklist 打勾 | 路线图清空 |
| 适用时长 | 分钟~小时 | 天~周 |

## 操作型 vs 思维型

前五个模式控制**行为**（做什么、按什么步骤做）；思维框架模式控制**思维**（怎么想、分析多深、不许跳步）。前者的质量抓手是结构、检查点与防偷懒武器；后者的质量抓手是 Purpose 定位、量化阈值、非目标约束与反幻觉规则。这与 [[reflection模式]] 的"反思改进决策"脉络以及 [[验证门禁化]] 的"未过验证不推进"理念分别呼应。

## 选型入口

按任务特征分流见 [[模式选择决策树]]（六分支决策树）。同域拆分实例（导航型 vs 操作型）见 [[导航型与操作型skill拆分]]。
