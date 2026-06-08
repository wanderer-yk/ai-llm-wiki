---
type: entity
title: Harness Engineering
tags: [方法论, agent协作, 工程化, 爱奇艺]
related: [shu-ju-ku-tuan-dui, specflow, ren-ren-dui-qi-ren-ji-dui-qi, spec-driven-development, ai-bian-cheng-huan-jue, openai]
created: 2026-06-08
updated: 2026-06-08
sources: ["别让AI瞎猜了用HarnessEngineering终结无限返工.html"]
---
# Harness Engineering

**定义**：让agent能稳定参与研发的工程安排，通过结构化的工程条件替代依赖prompt的猜测式开发。

**提出方**：方法概念源自 OpenAI，[[shu-ju-ku-tuan-dui|爱奇艺数据库团队]]于2026年5月发布文章进行系统化实践解读。

**开源资源**：GitHub SisyphusSQ/harness-template（项目模板仓库）

## 核心框架

### 五要素

1. **任务入口**：目标/范围/背景/责任人/反馈的统一承接
2. **执行依据**：结构/对象/状态的冻结与文档化
3. **工具边界**：agent可调用的工具范围与权限
4. **验证反馈**：可执行的验证入口与反馈链路
5. **结果记录**：结果同步回任务系统/PR/仓库文档

### 五层职责模型

| 层次 | 典型工具 | 核心职责 |
|------|---------|---------|
| 任务编排层 | Linear/JIRA/PMS | 目标/范围/状态/责任 |
| 执行依据层 | Pencil/docs/plan | 结构冻结/边界定义 |
| 状态暴露与验证层 | Storybook/runbook | 运行状态显式化 |
| agent执行层 | Cursor/Claude Code | 代码生成与修改 |
| 评审收口层 | GitHub/GitLab | CI/评审/合并/留痕 |

### 核心原则

- **工具与harness分离**：工具负责执行能力，harness负责工程条件，工具可替换但职责位置不可缺位
- **人判断不退出**：信息进入系统不等于人退出判断，需求澄清/架构取舍/风险评估仍需人负责
- **三高适用原则**：系统化高风险、高协作成本、高复用价值的部分，短平快保留轻量路径

## 与其他方法论的关联

- **[[spec-driven-development|规格驱动开发]]**：SDD解决需求契约层（消除猜测），HE解决执行工程层（稳定交付），形成层级互补
- **[[specflow|Specflow]]**：同组织（[[ai-qi-yi-ji-shu-chan-pin-tuan-dui]]）不同团队方案，Specflow聚焦前端规格驱动流程，HE聚焦全栈agent工程条件
- **[[ren-ren-dui-qi-ren-ji-dui-qi|人人对齐→人机对齐]]**：HE三阶段路径（入口可找→任务可复用→重复可机械化）完美呼应此方法论
- **[[always-ji-bie-ai-rule|always级别AI Rule]]**：HE的"规则从提醒→检查升级"与AI Rule规范化本质相同