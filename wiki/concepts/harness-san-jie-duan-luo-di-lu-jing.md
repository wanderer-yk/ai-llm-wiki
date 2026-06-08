---
type: concept
title: Harness三阶段落地路径
tags: [harness-engineering, 落地实施, 渐进式]
related: [harness-engineering, ren-ren-dui-qi-ren-ji-dui-qi, always-ji-bie-ai-rule, gui-fan-luo-di-ai-gong-ju-lian]
created: 2026-06-08
updated: 2026-06-08
sources: ["别让AI瞎猜了用HarnessEngineering终结无限返工.html"]
---
# Harness三阶段落地路径

[[harness-engineering|Harness Engineering]]的渐进式落地实施路径，不要求一次完成，采用试运行策略。

## 三阶段定义

### 阶段1：让agent找得到入口

- 补 `AGENTS.md`（项目入口和验证命令）
- 补验证命令（可执行入口）
- 补任务计划模板（复杂任务的计划协议）
- **核心交付物**：agent能沿统一路径找到依据/边界/验证入口

### 阶段2：让任务能被复盘和复用

- 固定plan格式（Scope/Non-Goals/Validation/Rollback）
- 固定runbook（可复现验证命令）
- 固定验证摘要（成功/失败判定）
- 固定PR/MR回写格式（变更/评审/剩余风险）
- **核心交付物**：任务可复盘、经验可沉淀

### 阶段3：让重复问题逐步机械化

- 高频review问题升级为lint
- 继而升级为script
- 再升级为test
- 最终升级为CI gate
- **核心交付物**：同类问题不再依赖人工提醒

## 与已有方法论的呼应

- 阶段1-2做[[ren-ren-dui-qi-ren-ji-dui-qi|人人对齐]]（隐性知识外化为项目文件）
- 阶段3做[[ren-ren-dui-qi-ren-ji-dui-qi|人机对齐]]（规则机械化）
- 阶段3的review升级与[[always-ji-bie-ai-rule|always级别AI Rule]]高度一致
- 整体路径与[[gui-fan-luo-di-ai-gong-ju-lian|规范落地AI工具链]]理念完全吻合

## 四条实践口诀

1. 任务别只留在聊天里
2. 边界别只靠人记
3. 验证别只停在本机
4. 结果别只存在这一轮对话里

## 试运行建议

- **前端**：从复杂页面或组件库开始
- **后端**：从链路型工具或集成任务开始
- 每次任务后补计划+验证+回写