---
type: concept
title: Harness五要素
tags: [harness-engineering, 工程条件, agent协作]
related: [harness-engineering, harness-wu-ceng-zhi-ze-mo-xing, san-chong-cai-ce-wen-ti, cong-prompt-dao-harness]
created: 2026-06-08
updated: 2026-06-08
sources: ["别让AI瞎猜了用HarnessEngineering终结无限返工.html"]
---
# Harness五要素

[[harness-engineering|Harness Engineering]]定义的五个核心工程条件要素，是"让agent能稳定参与研发的工程安排"的最小完备集合。

## 五要素定义

1. **任务入口**：统一承接目标/范围/背景/责任人/反馈的位置（任务系统/PR/MR）
2. **执行依据**：结构/对象/状态的冻结文档（AGENTS.md/plan/docs）
3. **工具边界**：agent可调用的工具范围与权限（execute/搜索/修改/执行）
4. **验证反馈**：可执行的验证入口与反馈链路（test/lint/review gate/runbook）
5. **结果记录**：结果同步回项目体系（验证摘要/review结论/文档更新）

## 与返工根因的对应

| 返工根因 | 对应要素 |
|---------|---------|
| 页面结构未定 | 执行依据 |
| 状态未补齐 | 执行依据 + 验证反馈 |
| 接口边界未说清 | 工具边界 |
| 验证口径不统一 | 验证反馈 |
| 结果记录无固定落点 | 结果记录 |

## 三大期待映射

| 团队期待 | 对应工程条件 |
|---------|------------|
| 多做一点 | 上下文工具完整（入口+依据充分） |
| 少问一点 | 边界和非目标验收清楚（边界+依据冻结） |
| 出错少一点 | 验证和回写形成稳定链路（验证+记录闭环） |

## 关键论断

缺一即返工概率陡增。五要素的核心逻辑：agent可靠参与研发不能只靠模型回答，项目必须准备这五项工程条件，让agent"无需凭空理解整个组织流程，只需在每个位置完成明确动作"。