---
type: source
title: 我把 Karpathy 的 AutoResearch 搬到了软件开发领域，效果炸了
tags: [autoresearch, 多agent交叉审核, 量化评分, 自动化软件开发, karpathy, codex, claude-code]
related: [karpathy, autoresearch, smallnest-autoresearch, 鸟窝, acpx, imclaw, 花叔, 达尔文skill, ralph-wiggum方法, autoresearch软件开发迁移, 多agent交叉审核, 5维度量化评分, 反馈驱动迭代, program-md规则核心, 四阶段优化循环, 硬性保护与软性保护, 人的参与程度反映领域特征, 百度Geek说, ai工程量化效果声明追踪]
created: 2026-10-09
updated: 2026-10-09
authors: [鸟窝]
year: 2026
url: "https://mp.weixin.qq.com/s?__biz=Mzg5MjU0NTI5OQ==&mid=2247606664&idx=1&sn=34e95bd76d66935c85b61ed791983041"
venue: 百度Geek说（微信公众号）
sources: ["[202604201800]我把Karpathy的AutoResearch搬到了软件开发领域效果炸了.html"]
---
# 我把 Karpathy 的 AutoResearch 搬到了软件开发领域，效果炸了

## 基本信息

- 作者：[[鸟窝]]
- 发布平台：[[百度Geek说]]（微信公众号，IP 属地上海），2026-04-20 18:00，原创标记，全文 5262 字（预计阅读 9 分钟）
- 核心产物：[[smallnest-autoresearch]]（`https://github.com/smallnest/autoresearch`）
- Agent 终端：[[codex]] 与 [[claude-code]]，经 [[acpx]] 在命令行中协作
- 演示回放：https://asciinema.org/a/896260（Issue #21）；演示 Issue：https://github.com/smallnest/imclaw/issues/21

## 摘要

文章将 [[karpathy]] 的 [[autoresearch]] 方法论迁移至软件开发领域：以 `program.md` 为规则核心，通过多 AI Agent 交叉审核、5 维度量化评分、反馈驱动迭代三大改进，构建 GitHub Issue 识别 → 代码实现 → 测试验证 → 审核合并的全自动闭环，人只提供 Issue 号。宣称约 10 分钟自主完成一个中等复杂度 Issue，最终评分 9.0/10。

## 原型：Karpathy AutoResearch（02 节）

2026 年 3 月 Andrej Karpathy 发布 autoresearch：几天内 GitHub 收获 5 万+ 星标，介绍视频播放 860 万次，约 600 行的开源 Python 工具，核心思想是"把 AI 研究本身也交给 AI 来自主完成"。机制：给 Agent 一个真实小型 LLM 训练环境（单 GPU、5 分钟训练预算），自主修改 `train.py`、跑实验、检查结果——只有 val loss 改善才 commit，否则 git revert 回滚；人类只需维护一份 `program.md`（"研究章程"）。精髓三原则：① 量化目标（val loss 是唯一判断标准）；② 自主循环（无需人类每轮介入）；③ 只保留改进（退化就回滚，绝不将就）。预计每小时约 12 次实验，一夜可收获上百轮自动优化。

迁移映射：

| Karpathy 原版（ML 研究） | 本项目（软件开发） |
|------|------|
| 修改 train.py | 实现 GitHub Issue |
| 跑 5 分钟实验 | 跑测试 |
| val loss 改善才保留 | 多维评分达标才合并 |

## 三大改进（03 节）

1. **多 Agent 交叉审核**：Codex 与 Claude 轮流担任实现者/审核者（A 写完 B 审、B 写完 A 审），支持 opencode 实现 1-3 个任意组合；作者声明"单 Agent 的效果远不如双 Agent"（自报，无对照数据）。
2. **5 维度加权评分**替代单一 metric：正确性 35% + 测试 25% + 代码质量 20% + 安全 10% + 性能 10%，达标线 9.0/10。
3. **反馈驱动迭代**：审核反馈直接注入下一轮 Agent 提示词，替代 Ralph Wiggum 式盲循环。

近期四项优化：抽取为独立项目、代码重构增加控制、通用化至任意 GitHub 项目、增加 opencode 支持 1-3 个 Coding Agent 任意组合。

## 评分体系（4.2）

| 维度 | 权重 |
|------|------|
| 正确性 | 35% |
| 测试 | 25% |
| 代码质量 | 20% |
| 安全 | 10% |
| 性能 | 10% |

各维度计分档位：无问题 10 分 / 建议改进 9 分 / 一般问题 7 分 / 严重问题 4 分 / 致命问题 1 分。
迭代门控：总分 ≥ 9.0 → 自动提交 PR；< 9.0 → 审核反馈驱动下一轮改进。

## 四阶段优化循环（4.3）

```bash
Phase 1: 环境准备
  └─ 检查依赖 (gh, acpx, go)
  └─ 获取 Issue 信息
  └─ 创建分支 + acpx session
Phase 2: 迭代核心 (自主运行)
  └─ 奇数轮: Codex 审核 → Codex 实现 → 测试 → Claude 审核 → Claude 实现
  └─ 偶数轮: Claude 审核 → Claude 实现 → 测试 → Codex 审核 → Codex 实现
  └─ 评分 ≥ 9.0 → Phase 3
  └─ 评分 < 9.0 → 反馈驱动下一轮
  └─ 测试失败 → 反馈"测试失败"进入下一轮
Phase 3: 自动提交 (评分达标后)
  └─ git commit + push
  └─ gh pr create
  └─ gh pr merge
Phase 4: 记录归档
  └─ 写入 results.tsv
  └─ 更新 workflows/issue-N/log.md
```

迭代示例（演示数据）：5.0 → 7.0 → 9.1 三轮收敛后自动 PR + 合并。默认迭代上限 42 轮。

## 核心文件结构（4.4）

```bash
autoresearch/
├── program.md              # 宪法：实现规则、权限边界、代码规范、质量标准
├── issue-selector.md       # Issue 选择策略：优先级、排除规则、复杂度评估
├── run.sh                  # 编排引擎：完整自动化脚本
├── agents/
│   ├── codex.md            # Codex 角色：实现者指令 + 代码规范 + 自检清单
│   ├── claude.md           # Claude 角色：审核者指令 + 评分标准 + 问题模板
│   └── gemini.md           # Gemini 角色：实现者指令（扩展 Agent）
├── workflows/
│   └── issue-{n}/
│       ├── log.md          # 总日志：迭代记录、评分历史
│       ├── iteration-N-codex.log     # 各轮 Codex 输出
│       ├── iteration-N-claude.log    # 各轮 Claude 输出
│       └── test-N.log                # 各轮测试结果
└── results.tsv             # 全量结果汇总
```

## Issue 选择策略（4.5）

排除规则：不处理含 `wontfix` / `duplicate` / `invalid` / `blocked` / `needs discussion` / `on hold` / `external` 标签、标题含 `[WIP]` / `[DRAFT]`、正文含 `DO NOT IMPLEMENT`、已有 PR 关联的 Issue。

优先级公式：`分数 = 基础权重(15) + 标签权重 + 类型权重 + 时间因子`

| 因子 | 权重 |
|------|------|
| 标签 | critical(100) > high(50) > medium(20) > low(10) |
| 类型 | bug(30) > feature(20) > refactor(10) > test(5) > docs(3) |
| 时间 | 新 Issue +10 / 陈年 Issue +15 / 近期更新 +5 |

## program.md 要点（4.6）

```text
Agent 可以:
  ✓ 修改 internal/, cmd/
  ✓ 创建/修改测试文件
  ✓ 运行测试和 lint
  ✓ 创建本地分支和 commit
  ✓ 在 workflows/ 记录日志
Agent 不可以:
  ✗ 修改 go.mod, .github/, Makefile, CI/CD
  ✗ 删除任何现有文件
  ✗ 推送到远程仓库（由 run.sh 统一处理）
  ✗ 关闭 Issue
  ✗ 修改 autoresearch/ 规则文件
```

Go 代码规范：① 遵循 Effective Go + Go Code Review Comments；② gofmt + goimports + golangci-lint；③ 包名小写、文件名下划线、导出大写；④ 接口用 er 后缀（Reader, Handler）；⑤ 错误用 fmt.Errorf 包装提供上下文。

测试规范：① 所有新功能必须有单元测试；② 覆盖率 ≥ 70%；③ 表格驱动测试；④ 命名 Test\<Function\>_\<Scenario\>；⑤ 禁止 time.Sleep、外部依赖、全局状态、硬编码端口。

## 错误处理（4.7）

指数退避重试（delay = 2^retry × base_delay + random_jitter，上限 60 秒、10 次）、连续失败 ≥ 3 次熔断、测试失败反馈下一轮修复。

## 快速开始（05）

```bash
# GitHub CLI (gh)
gh auth status
# Agent 控制工具 (acpx)
which acpx
# Go 环境
go version
```

```bash
# 进入你要处理的 GitHub 项目目录
cd /path/to/your/github/project
# 处理单个 Issue
/path/to/autoresearch/run.sh 21
# 指定最大迭代次数
/path/to/autoresearch/run.sh 21 10
```

项目级配置覆盖目录（workflows/ 与 results.tsv 为自动生成）：

```
.autoresearch/
├── agents/
│   ├── codex.md     # 自定义 Codex 指令
│   └── claude.md    # 自定义 Claude 指令
├── workflows/       # 自动生成
└── results.tsv      # 自动生成
```

## 实战案例（06，案例级自报数据）

**Issue #21**（`feat: enhance job execution with agent selection and timeout`）：

```
复杂度：中等（涉及 Job 结构体扩展、超时控制、API 增强）
Issue 内容：为 Job 添加 AgentName/Timeout/MaxRetries 字段，超时自动取消，失败自动重试
迭代 1 (Codex):  评分 1.0   → Codex 只读取了代码就结束，功能完全未实现
迭代 2 (Claude): 评分 5.0   → Codex 实现了超时控制和 API 增强，但 Claude 审核发现不足
迭代 3 (Codex):  评分 9.0   → 达标！
→ 自动 commit + PR + 合并 ✓
代码改动：
  - internal/job/job.go: 添加 Timeout 字段和 context.WithTimeout 超时控制
  - internal/job/job_test.go: 新增 TestExecuteJob_Timeout 等测试用例
  - internal/gateway/server.go: REST API 和 JSON-RPC 支持 timeout 参数
总耗时：约 10 分钟（17:38 → 17:47）
总迭代次数：3 轮
最终评分：9.0/10
回放链接：https://asciinema.org/a/896260
```

**Issue #15**（`feat: define source-of-truth event protocol`，仅 2 轮达标）：

```
迭代 1 (Codex):  评分 5.0  → 反馈：设计方向问题
迭代 1 (Claude): 评分 7.0  → 反馈：改进实现细节
迭代 2 (Codex):  评分 9.1  → 达标！
→ 自动 commit + PR + 合并 ✓
总迭代次数：2 轮（奇数轮 Codex 实现 + Claude 审核，Claude 补充实现 + Codex 审核）
```

**Issue #6**（`feat: add web UI for sessions`，高复杂度 5 轮）：

```
复杂度：高（涉及多个模块、需要设计决策）
迭代次数：5 轮
最终评分：15/10（Claude 和 Codex 均评分最高）
→ 自动 PR + 合并 ✓
```

## 最佳实践（07）

1. 从小 Issue 开始（bug fix 最能测试流程）
2. 保持 program.md 更新（"效果不够理想……就可以修改这个文件"——人工介入点在循环外）
3. 关注评分趋势（每次迭代评分记录在 log.md，观察是否稳步上升）
4. 利用多 Agent 对抗（交叉验证减少盲区）
5. 退火重试（API 不稳定时脚本自动退避，无需人工干预）

## 与同类项目对比（三方三洞见）

Karpathy AutoResearch（ML 研究）vs 本项目（通用软件开发）vs [[达尔文skill]]（Skill 优化）：
① 量化目标是共通核心（val loss / 审核评分 / 8 维总分）；② 质量保证机制各有侧重（git revert 硬回退 vs 交叉审核软保护）；③ 人的参与程度反映领域特征（全自主 / 关键节点介入 / 每轮暂停确认）。

## 设计灵感（08）

1. karpathy/autoresearch —— 核心循环：只保留可测量的改进，其余全部回滚
2. acpx —— Agent 命令行协作控制
3. imclaw —— "本项目和 autoresearch 文件"（表述含混，见下）

## 效果证据的边界

全文无总结章节、无整体效果统计数据（文章以设计灵感节直接收尾）。效果证据止于三个案例级自报日志，其中 #21 附 asciinema 回放可部分验证。标题"效果炸了"为夸饰；"单 Agent 远不如双 Agent"无对照数据。相关口径记录于 [[ai工程量化效果声明追踪]]。

## 矛盾与疑点

1. **score=15/10 与 10 分制冲突**：Issue #6 文字日志与 results.tsv 示例（score=15）一致，但与"满分 10、达标线 9.0"体系不符；文中解释"Claude 和 Codex 均评分最高"，疑为双审核者分数聚合字段或展示缺陷。
2. **迭代编号混乱**：Issue #15 日志出现两个"迭代 1"（Codex 5.0、Claude 7.0）后接"迭代 2"，却称"共 2 轮"，轮次计数与条目计数不一致。
3. **imclaw 表述含混**：设计灵感节"本项目和 autoresearch 文件"与 [[imclaw]] 实际功能（IM 蜂群控制）的对应关系未解释。
4. **软保护 vs 硬保护分歧无对照数据**：本项目放弃 git revert 硬回退，理由是"ClaudeCode/Codex 自己足够智能决定回退还是改进上一轮的变动"。

## 推荐阅读与同源关系

| # | 文章标题 | 与 wiki 关系 |
|---|---------|-------------|
| 1 | 读完 Claude Code 源码才发现：Skills、MCP、Rules 的区别，远没有你想的那么大 | 已收录 [202604151800] |
| 2 | Harness Engineering: 让 Coding Agent 可靠完成长程任务 | 已收录 [202604081800] |
| 3 | IMClaw：通过微信/飞书操控ClaudeCode/Codex/GeminiCLI/Pi Agent蜂群 | 未收录，imclaw 专题 |
| 4 | 我用 Go 重写了一个 OpenClaw 框架：这就是 GoClaw | 未收录 |
| 5 | 从心理按摩到实操上手的OpenClaw全指南 | 未收录 |

5 条链接共用同一 `__biz=Mzg5MjU0NTI5OQ==`，确证 [[百度Geek说]] 为本文、[202603091800] AgentSkill 文、[202604081800]、[202604151800] 的共同发布平台。

## 关联页面

[[autoresearch软件开发迁移]]、[[多agent交叉审核]]、[[5维度量化评分]]、[[反馈驱动迭代]]、[[program-md规则核心]]、[[四阶段优化循环]]、[[奇偶轮角色互换]]、[[issue选择策略]]、[[agent权限边界清单]]、[[错误处理三机制]]、[[results-tsv结构化归档]]、[[硬性保护与软性保护]]、[[人的参与程度反映领域特征]]、[[autoresearch三原则]]、[[六条核心原则]]、[[ralph-wiggum方法]]、[[花叔]]、[[auto-optimize-skill]]
