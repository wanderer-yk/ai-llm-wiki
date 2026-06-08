---
type: concept
title: 五层验证体系
tags: [harness-engineering, 验证, 质量保障]
related: [harness-engineering, harness-wu-yao-su, yan-fa-fan-shi-qian-yi, blocker-gate]
created: 2026-06-08
updated: 2026-06-08
sources: ["别让AI瞎猜了用HarnessEngineering终结无限返工.html"]
---
# 五层验证体系

[[harness-engineering|Harness Engineering]]提出的后端验证层次模型，强调**验证应前置于任务设计**而非仅前移实现动作。

## 五层定义

| 验证层次 | 应回答问题 | 典型证据 |
|---------|-----------|---------|
| **静态检查** | 代码能否通过lint/类型/编译？ | lint输出/编译结果 |
| **单元验证** | 核心函数/边界是否正确？ | 单测通过率/覆盖率 |
| **链路验证** | 入口到输出是否连通？ | 集成测试/端到端结果 |
| **失败验证** | 异常/超时/回滚是否按预期？ | 异常注入结果/回滚日志 |
| **回写验证** | 结果是否同步到任务系统/PR/文档？ | PR状态/任务更新/文档diff |

## 核心洞见

- 后端任务"完成"判断必须按非视觉形态拆解：命令/定时任务/消费链路/API/数据同步/配置驱动
- 决定可交付性的不是"有没有代码"，而是"有没有团队共同认可的验证条件"
- 第五层"回写验证"将验证闭环延伸到结果记录，与[[harness-wu-yao-su|五要素]]中的"结果记录"直接对应

## 与已有概念的关系

- [[yan-fa-fan-shi-qian-yi|研发范式前移]]：验证前置于任务设计是Shift Left的进一步深化
- [[blocker-gate|Blocker Gate]]：与验证层次中的gate机制理念一致
- [[pre-pr-ji-zhi|Pre-PR预审]]：对应"评审收口层"的验证动作