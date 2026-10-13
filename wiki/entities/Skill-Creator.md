---
type: entity
title: Skill-Creator
tags: ["anthropic", "工具", "agent-skill", "评测", "tool", "skill", "evaluation", "元工具"]
related: ["anthropic", "skill-creator三版演进", "agent-skill", "skill评测三原则", "skill-creator三agent评估链", "skill评测数据管道", "像做机器学习一样做prompt工程", "skill-creator六大局限", "skill开发六阶段生命周期", "writing-skills"]
created: 2026-10-08
updated: 2026-10-13
sources: ["[202603091800]打造高效易用的AgentSkill.html", "[202605120830]AgentSkill规范构建与设计模式.html"]
---

# Skill-Creator

Skill-Creator 是 Anthropic 官方提供的 Skill 创建工具，收录于 `anthropics/skills` 官方仓库（同名 Skill 为 `skill-creator`）。它被定位为"**用来创建 Skill 的 Skill**"——一个以工程化、可评测方式生成与迭代 Agent Skill 的元工具，其自指结构与 [[自举式开发]]、[[agent自举三前提]] 的自举模式同构。

它不是静态工具，而是经历了三个版本的演进（详见 [[skill-creator三版演进]]）。在《打造高效易用的 Agent Skill》中，这一演进被用作案例，论证 Skill 工具链的成熟路径：从"生成"到"生成+优化"再到"对最终运行效果负责"——即评测内建于工具本身（呼应 [[skill评测三原则]]）。《AgentSkill规范构建与设计模式》则进一步给出其设计哲学、开发流程与评估基础设施的详细刻画。

## 三版演进

| 版本 | 能力定位 |
| --- | --- |
| 第一版"创建" | 自然语言描述 → SKILL.md，降低上手门槛 |
| 第二版"创建 + 优化" | 承接几乎所有 Skill 相关工作；description 更激进；可自主改进 Skill |
| 第三版"自动评测优化" | 生成评测用例、创建评分机制、运行评测、评价汇总、循环改进，对最终运行效果负责 |

## 设计哲学

核心主张是"像做机器学习一样做 Prompt Engineering"（详见 [[像做机器学习一样做prompt工程]]）：将 CI/CD、A/B 测试、性能基准等软件工程实践移植到 Skill 开发，使 Skill 开发具备训练集/测试集/评估指标/迭代优化循环/防过拟合机制。

三大核心思想：

1. **泛化而非过拟合**
2. **解释"为什么"而非堆砌"必须"**：LLM 有良好心智理论，解释动机优于 ALWAYS/NEVER 式死板约束，与 [[代码库模式大于指令]] 精神相通。
3. **提取重复模式**：测试中 Agent 反复独立编写的辅助脚本，应下沉为 `scripts/` 直调脚本。

## 开发流程：六阶段生命周期

需求捕获 → 编写 Skill → 测试执行（`with_skill`/`without_skill` 并行 A/B）→ 评估与评审（Eval Viewer + `feedback.json`）→ 迭代改进（`iteration` 目录）→ 优化与发布（`run_loop.py` 优化 description + `package_skill` 打包 `.skill`）。详见 [[skill开发六阶段生命周期]]。

## 评估基础设施

评估基础设施由两部分构成：

- **三 Agent 评估链**：Grader（评分者，含自我批评机制）/ Comparator（双盲盲比）/ Analyzer（双重角色），详见 [[skill-creator三agent评估链]]、[[grader自我批评机制]]。
- **JSON 数据管道**：`evals.json` → timing/metrics/grading/benchmark/comparison/analysis/history，详见 [[skill评测数据管道]]。

单轮评测子 Agent 公式：`N×2 + N + 1 + 可选 N`。

## 优势与局限

以下为《AgentSkill规范构建与设计模式》中的总结。

**五优势（作者自评）**：

- 方法论完整
- 评估体系严谨
- 零依赖可移植（纯 Python stdlib + `claude` CLI）
- 人机协作设计（Eval Viewer）
- 自举式架构

**六局限**：

- Token 消耗极高、成本不透明（Issue #514：60 个 Opus 会话耗 69% 的 5 小时配额、0 条可执行结果）
- 流程冗长（9 个交互确认节点）
- 子任务并发复杂
- 操作型 Skill 触发失效（recall 0%）
- Skill 膨胀
- 学习曲线陡峭

详见 [[skill-creator六大局限]]。

## 方法论定位

在《AgentSkill规范构建与设计模式》的总结中，Skill-Creator 与 [[superpowers插件]] 的 Writing-Skills 并列为 Skill 生态"构建方法论"层：Skill-Creator 为 **ML 式自动评测路线**（`run_loop.py` 自动训练/测试分割与迭代），Writing-Skills 为 **TDD 式路线**（测试先行）；二者"目标相似，但方法论截然不同"（元工具 vs 元技能）。该来源未做正面对比。

## 开放问题

- 三版演进的时间线与发布节奏未在来源中交代。
- 六阶段生命周期、三 Agent 评估链等具体机制与三版演进（尤其是第三版）的对应关系未在来源中明确说明。