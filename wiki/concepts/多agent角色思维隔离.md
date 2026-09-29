---
type: concept
title: 多 Agent 角色思维隔离
tags: [多代理, 角色隔离, prompt工程, 内生审计]
related: [specflow, bmad-method, ssot单文档策略]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202603261200]治愈CursorAI编程的幻觉用它就够了.html"]
---
# 多 Agent 角色思维隔离

多 Agent 角色思维隔离是 [[specflow|Specflow]] 的核心设计理念之一（源自 [[bmad-method|BMAD-METHOD]] 的启发），通过特定 Prompt 为每个开发阶段分配**专家角色**，确保"产出质量不受个体经验偏差影响"，实现内生审计与校准。

## Specflow 四角色体系

| 角色 | 阶段 | 职责 |
|------|------|------|
| PM（产品经理） | Specify | 扫描代码库提取业务规则、需求澄清 |
| TL（技术负责人） | Plan | 制定功能契约与执行路径 |
| Dev（工程师） | Implement | 按 Group 顺序原子化编码 |
| Admin（知识管理员） | Archive | 知识脱水、文档归档、索引更新 |

## 内生审计机制

每个阶段的角色产出即为下一阶段角色的输入校验依据，形成自校准闭环：
- PM 产出的 `specify.md` 供 TL 审查
- TL 产出的 `plan.md` 供 Dev 执行
- Dev 的编码产出供 Admin 归档

## 向 Specflow 2.0 演进

在 Specflow 2.0 规划中，角色思维隔离将升级为 **Subagents**：
- 角色原子化：从 Prompt 模拟升级为独立子代理
- 上下文精准注入：主代理按阶段投喂必要文档，裁剪非必要历史推理
- 自主验证闭环：提交结果前须完成单测或静态检查