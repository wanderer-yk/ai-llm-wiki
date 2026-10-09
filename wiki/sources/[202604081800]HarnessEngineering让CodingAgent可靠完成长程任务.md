---
type: source
title: "Harness Engineering: 让 Coding Agent 可靠完成长程任务"
tags: [harness-engineering, coding-agent, 长程任务, 任务编排, 状态持久化, meta-skill]
related: [harness-engineering, long-term-task-orchestration, 百度Geek说, 无糖可乐, 长程任务三特征, 长程任务三困难, 长程任务四原则, 任务边界三模式, file-as-progress状态持久化, skill-for-skill元技能自举, harness边界移动论, harness基础设施论]
created: 2026-10-09
updated: 2026-10-09
authors: [无糖可乐]
year: 2026
url: "https://mp.weixin.qq.com/s?__biz=Mzg5MjU0NTI5OQ==&mid=2247606577&idx=1&sn=3b4b049bb7f6463f7dc68d06f94c789e"
venue: "百度Geek说（微信公众号，GEEK TALK 栏目）"
sources: ["[202604081800]HarnessEngineering让CodingAgent可靠完成长程任务.html"]
---
# Harness Engineering: 让 Coding Agent 可靠完成长程任务

百度Geek说公众号（GEEK TALK 栏目，IP 属地上海）2026-04-08 18:00 发布的原创文章，作者无糖可乐，全文 11157 字，预计阅读 16 分钟。文章摘要："随着任务规模的不断增长，Agent 的可靠性可能出现问题，聊聊如何用 Harness 让 Agent 稳定跑完长程任务。"

本文是 [[harness-engineering]] 概念的**第三个独立来源**（百度版），与爱奇艺数据库团队五要素版（2026-05-14）、ConardLi [[Harness核心价值三元组织论]]版（2026-05-09）并列，三方定义谱系素材至此齐备。

## 核心定义（两个）

- **01 节定义**：Harness 英文本意"缰绳"，在 Agent 场景即让强模型在**安全边界内被稳定地约束、引导和复用**。
- **09 节结语定义**：Harness 是"团队基础设施建设的一部分，解决 Agent 完成大规模任务时的不确定性，并提供可量化的结果评估能力"（→ [[harness基础设施论]]）。

## 文章骨架

01 长程任务的特征 → 02 关注的点 → 03 困难在哪里 → 04 核心原则 → 05 理念 → 06 技巧（6.1–6.5）→ 07 示例（7.1 Code Review / 7.2 JS to TS）→ 08 从经验到框架：Skill for Skill → 09 结语 → 推荐阅读（5 条链接 biz ID 均与本文一致，为同号互推，无实质内容）。

## 各节要点

### 01 长程任务的特征
三个工程化实例：①21 个前端模块 JS 全量迁移 TypeScript；②几十个模块全量 Code Review + 批量修复几十上百条意见；③中文硬编码全量提取为 i18n 资源。共同特征：**规模大**（成百上千文件）、**运行时间长**（一次跑不完、跨多个会话）、**消耗 Token 极高**（几千万到上亿 Token 量级）→ [[长程任务三特征]]。

### 02 关注的点
**效果**（能否完成 / 完成真实性 / 中断后连续性 / 结果可验证性）、**速度**（1000 文件×30 秒/文件串行需 8+ 小时，10 路并发 1 小时内）、**成本**（重试 3 次成本翻 3 倍；长会话偏离预期致前置几十万 Token 白费）→ [[效果速度成本三关注点]]、[[完成真实性]]。

### 03 困难在哪里
①上下文耗尽（压缩必然丢信息且逐轮叠加劣化，Opus 级顶尖模型亦不能幸免；引发"上下文焦虑"提前收尾谎报完成）②中断要重来（网络断开/Token 用尽/模型超时是常态，Agent 无跨会话记忆）③规模大了行为不可控（单文件好≠一千个文件都好）→ [[长程任务三困难]]、[[上下文焦虑]]。

### 04 核心原则
任务拆解、并行执行、可续传、有完成条件，与三困难一一对应，分别作用于效果/速度/成本三维度 → [[长程任务四原则]]。

### 05 理念
任务边界清晰（输入/输出/约束三要素；[[任务边界三模式]]：无依赖直接并行 / 有依赖拓扑排序 / 有冲突 Git Worktree 物理隔离；[[agent-teams最后选项论]]）、错误在最小范围内解决（[[错误最小范围解决]]）、步骤间双轨校验（[[双轨校验]] + [[自我说服效应]] + [[跨模型评估]]）、允许局部失败（[[局部失败容忍与妥协分级]]，DONE_WITH_WARNINGS 妥协分级）。

### 06 技巧
6.1 任务粒度（[[任务粒度三因素]]、[[3000行经验上限]]、80% 上下文占用检验标准、[[同目录文件同组原则]]）；6.2 子任务的 CLI 化与并发调度（[[子任务CLI化]]、[[prompt确定性]]、[[主agent转述失真]]、[[随到随补调度]]、[[双通道输出设计]]）；6.3 File As Progress（作者称"长程任务编排中最核心的设计"→ [[file-as-progress状态持久化]]）；6.4 任务状态设计（[[任务状态自描述]]、[[IN_PROGRESS残留产出物判定]]）；6.5 多轮重试（[[多轮重试三层]]）。

### 07 示例
**7.1 全量 Code Review**（21 个前端模块）：模块→目录→超限独立成组三级拆解；MAX_LINES=500 的 [[分块token预算推导]]；dispatch CLI 并行 + `segments/{chunkId}.json` 存在性判完成；[[批判性evaluator校验]]三角色架构；TSV 进度文件续传。
**7.2 JS to TS 迁移**：累计 ≤3000 行归组 + 依赖拓扑排序（叶子文件先行，同优先级并行、跨优先级串行）；Babel AST 对比 + tsc 类型检查双验证，双过才标 DONE（[[双轨校验]]实证）；失败文件保留原始 JS 标 FAILED，整体仍可构建（[[局部失败容忍与妥协分级]]实证）；`migration-tasks.tsv` 四条件启动检查；每个 Phase 对应独立 reference 文件按需读取。

### 08 从经验到框架：Skill for Skill
各长程任务骨架高度一致 → 统一 Skill 模板（SKILL.md + 6 脚本 + 4 Phase reference + evals.json）→ meta-skill [[long-term-task-orchestration]] 教 Agent 创建长程任务 Skill 并自动跑 skill-eval 评测闭环 → [[skill-for-skill元技能自举]]。

### 09 结语
[[harness边界移动论]]：Harness 每个环节都隐含"当前模型做不到"的假设，随模型进化过期，但"哪些交给模型、哪些留在框架"的判断不会消失；[[harness基础设施论]]：Harness 是团队基础设施建设的一部分。

## 结构化数据（原文保留）

**dispatch 接口统一参数：**

| 参数 | 含义 |
|------|------|
| `--root` | 项目目录 |
| `--concurrency` | 并发数 |
| `--dry-run` | 预览模式 |
| `--retry-failed` | 重试失败任务 |

**主 Agent 调度循环：**

```bash
while true; do
    node scripts/poll.js --task-list task_list.json
    if [ $? -eq 2 ]; then break; fi
    sleep 60
done
```

poll.js 退出码语义：exit 0 = 仍有活跃任务（pending 或 running），主 Agent sleep 后继续调用；exit 2 = 所有任务已到终态，可进入下一阶段（如合并结果）。

**主 Agent 转述失真——期望下发指令：**

```bash
使用 subAgent 完成 code review 任务，任务 Prompt 如下：
---
请审查以下文件，按 error/warn/style 三级分类产出审查意见。
待审查文件：src/components/UserCard.tsx, src/components/UserList.tsx, src/components/UserDetail.tsx
---
```

**主 Agent 实际转述结果：**

```javascript
你需要审查以下组件代码。重点关注边界情况和错误处理。
以下是文件内容：
// === src/components/UserCard.tsx ===
import React from 'react';
...（200 行代码被直接贴入）
// === src/components/UserList.tsx ===
...（150 行代码被直接贴入）
// === src/components/UserDetail.tsx ===
...（300 行代码被直接贴入）
请按 error/warn/style 三级分类产出审查意见。
```

**双通道输出——终端给人看的进度概览：**

```bash
[Progress] 45/120 DONE | 10 IN_PROGRESS | 3 FAILED | 62 TODO
[Speed] avg 35s/task | elapsed 28min | ETA ~22min
[Failed] group_12 (timeout), group_27 (compile error), group_33 (timeout)
```

**粗粒度状态机：**

```
TODO → IN_PROGRESS → DONE
                   → FAILED
                   → SKIPPED
```

**细粒度状态机（分析→执行→校验三步子任务）：**

```
TODO → ANALYZING → ANALYZED → EXECUTING → EXECUTED → VERIFYING → DONE
                                                            → FAILED
```

**长程任务 Skill 统一目录模板（08 节 verbatim）：**

```text
<skill-name>/
├── SKILL.md                    # Phase 定义 + 会话恢复检测 + 完成标准
├── scripts/
│   ├── discover.js             # 扫描目标，生成任务清单（幂等）
│   ├── dispatch.js             # 读清单，分组，并发调度 subagent
│   ├── build-prompt.js         # 程序化构建子任务 Prompt
│   ├── poll.js                 # 轮询子任务状态 + 补位启动
│   ├── merge.js                # 收集子任务结果，合并为最终产物
│   └── status.js               # 查询整体进度
├── references/
│   ├── phase0_setup.md         # 环境配置指令
│   ├── phase1_analyze.md       # 分析规划指令
│   ├── phase2_dispatch.md      # 批量执行指令
│   └── phase3_finalize.md      # 收尾验证指令
└── evals/
    └── evals.json              # 评估用例
```

**meta-skill 安装命令与示例 prompt：**

```bash
npx skills add hixuanxuan/long-running-agent-tasks -y
/long-term-task-orchestration 创建skill实现React Compiler迁移并下线全部memo。
```

**7.1 分块 Token 推导（估算口径）：**

| 构成 | 数值 | 说明 |
|------|------|------|
| 源文件正文 | 1 行 ≈ 14 tokens | 按 50 字符/行、3.5 字符/token 估算 |
| 固定开销 | ≈ 8,000 tokens | prompt 模板、规则、文件元信息、review 输出及 agent 中间步骤 |
| 单 chunk（500 行） | ≈ 20,000 tokens | 占 Claude Sonnet（200K）窗口 10%，为并发子任务对话历史和输出留余量 |

**6.1 任务粒度 Token 消耗估算（JS to TS，Claude Sonnet，~200K 有效上下文）：**

| 组成项 | Token 估算 |
|--------|-----------|
| Prompt 模板（任务说明、规则约束、输出格式要求） | ~1K |
| 输入文件内容（约 10-20 Token/行 × 3000 行） | 30K-60K |
| Agent 工作过程（读文件/推理/写代码/跑验证/修复，为输入的 2-3 倍） | 60K-180K |
| **合计** | **90K-240K** |

**文件路径约定**：`inputs/{chunkId}-input.json`（subAgent 输入）、`segments/{chunkId}.json`（审查结果，存在性=完成信号）、`migration-tasks.tsv`（迁移状态）、`.agent.env`（token 配置）。

**migration-tasks.tsv 四条件启动检查**（顺序检查，从第一个不满足条件对应 Phase 继续）：①`.agent.env` 含 token → ②tsv 已生成 → ③无 IN_PROGRESS 残留 → ④全部完成。

## 与既有 Wiki 的关系

- [[harness-engineering]] 第三来源：与爱奇艺五要素版、ConardLi 三元组织论版构成三方谱系，synthesis 素材齐备
- poll.js 轮询+补位启动 ↔ [[文件轮询架构]]（同为"文件+轮询+外部脚本"范式）；`npx skills add` ↔ [[skills-cli跨平台安装]]；Phase reference 按需读取 ↔ [[分阶段文档按需加载]] / [[渐进式披露替代向量检索]]；产出物程序化判定 ↔ [[验证门禁化]]
- [[agent-teams最后选项论]] vs ConardLi [[SubAgent与AgentTeams双模式]]：潜在对比点（语境不同，非矛盾）
- [[跨模型评估]] vs 美团 [[高阶模型审查低阶模型]]：同型实践；[[Skill-Creator]] 配合使用；模型自主并发"过于谨慎"与 [[人工并发天花板]] 互补——并发控制交脚本；脚本接管确定性逻辑呼应 [[agent-control-plane]] 方向

## 证据状态与量化声明判定

全文为设计推演 + 说明性算术示例，**无实测基准数据、无"提升 X%"类量化效果声明**；token 数字均明示为估算；"可量化的结果评估能力"为能力声明而非效果数据 → **不纳入** [[ai工程量化效果声明追踪]]。源文 "Cluade Code" 为 Claude Code 之笔误。

## 开放问题

1. hixuanxuan（GitHub 仓库所有者）与作者"无糖可乐"的身份关系未在源文中证实
2. skill-eval 评测细节（body 评测口径/可视化形式/循环修复机制）仅有概述，待追踪 hixuanxuan/long-running-agent-tasks 仓库补全
3. 五项 comparison 候选与 harness 三方谱系 synthesis 待立项（见本页 REVIEW 记录）