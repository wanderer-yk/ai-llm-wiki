---
type: entity
title: smallnest/autoresearch
tags: [autoresearch, 开源项目, 自动化软件开发, 多agent交叉审核, go]
related: [鸟窝, autoresearch, karpathy, acpx, imclaw, 多agent交叉审核, 5维度量化评分, 四阶段优化循环, program-md规则核心, 奇偶轮角色互换, issue选择策略, agent权限边界清单, 错误处理三机制, results-tsv结构化归档, codex, claude-code]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604201800]我把Karpathy的AutoResearch搬到了软件开发领域效果炸了.html"]
---
# smallnest/autoresearch

smallnest/autoresearch 是鸟窝将 Karpathy 的 [[autoresearch]] 方法论迁移到软件开发领域的开源项目（`https://github.com/smallnest/autoresearch`），以 `program.md` 为规则核心，实现 GitHub Issue 识别 → 代码实现 → 测试验证 → 审核合并的全自动闭环：人只提供 Issue 号，实现、测试、审核、迭代、PR 与合并全部自动完成。

## 架构概览

- **双 Agent 终端**：[[codex]] 与 [[claude-code]] 经 [[acpx]] 在命令行中协作，通过 `agents/codex.md`、`agents/claude.md` 角色文件注入指令；另预留 `agents/gemini.md` 扩展位（Gemini 角色：实现者指令，是否实际启用未确认）
- **编排引擎**：`run.sh`，完整自动化脚本，敏感动作（push / PR / merge）统一由脚本执行，Agent 无推送权（见 [[agent权限边界清单]]）
- **迭代调度**：[[奇偶轮角色互换]] + [[5维度量化评分]]（达标线 9.0）+ [[反馈驱动迭代]]，默认上限 42 轮
- **质量哲学**：交叉审核软保护，不做 git revert 回退（见 [[硬性保护与软性保护]]）

近期四项优化：抽取为独立项目、代码重构增加控制、通用化至任意 GitHub 项目、增加 opencode 实现 1-3 个任意组合的 Coding Agent 交叉审核与代码实现。

## 核心文件结构（原文）

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

## 运行方式（原文）

```bash
# 前置条件检查
gh auth status    # GitHub CLI (gh)
which acpx        # Agent 控制工具 (acpx)
go version        # Go 环境

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

## 关键机制索引

- 流程：[[四阶段优化循环]]（环境准备 → 自主迭代 → 自动提交 → 记录归档）
- 需求筛选：[[issue选择策略]]（排除规则 + 优先级公式 + 复杂度评估）
- 稳定性：[[错误处理三机制]]（指数退避/退火重试上限 60 秒 10 次、连续失败 ≥3 次熔断、测试失败反馈下一轮）
- 归档：[[results-tsv结构化归档]]（results.tsv + workflows/issue-N/log.md）

## 实战案例与证据边界

三个案例级自报日志：Issue #21（中等复杂度，约 10 分钟 / 3 轮 / 9.0 分，附 asciinema 回放）、Issue #15（2 轮 / 9.1 分，奇偶轮互换显式注释）、Issue #6（高复杂度，5 轮 / "15/10" 异常评分）。全文无整体统计数据，效果声明属案例级自报，详见来源页与 [[ai工程量化效果声明追踪]]。

## 设计灵感（原文三项）

karpathy/autoresearch（核心循环：只保留可测量的改进，其余全部回滚）、acpx（Agent 命令行协作控制）、[[imclaw]]（"本项目和 autoresearch 文件"——表述含混）。

## 最佳实践（原文五条）

从小 Issue 开始；保持 program.md 更新；关注评分趋势（log.md）；利用多 Agent 对抗（交叉验证减少盲区）；退火重试（自动退避，无需人工干预）。
