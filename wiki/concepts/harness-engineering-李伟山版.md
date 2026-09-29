---
type: concept
title: Harness Engineering（李伟山版）
tags: [harness-engineering, ai-engineering, 第三次进化, agent-system]
related: [工程三次进化框架, harness-engineering, harness衰变定律, f-harness, human-steer-agents-execute, 验证闭环, 声称完成vs验证完成, 可靠性边界论, 嵌套关系论-李伟山版]
created: 2026-07-20
updated: 2026-07-20
sources: ["[202605201731]干货从PromptContext到Harness工程的三次进化与终局之战原创.html"]
---
# Harness Engineering（李伟山版）

Harness Engineering 是 [[李伟山]] 提出的 [[工程三次进化框架]] 中的**第三次进化**——为 AI 系统设计约束/验证/反馈"马具"的系统工程方法论。

## 核心定义

**比喻**：AI 如野马，Harness 是"马具"，套上才能指哪打哪。

**公式化定义**：完整 AI Agent 系统 = 大模型 + Harness；**Harness 是除大模型本身之外的所有东西**。

## OpenAI 三大策略

文章第 04 章详述 [[openai]] 百万行代码实验中的三大 Harness 策略：

### 上下文治理（Context Governance）

巨型规范文件压缩为索引、动态加载子文档、决策记录迁移至代码仓库。与 [[context-engineering-李伟山版]] 中的"单一事实来源"一脉相承。

### [[验证闭环]]（Verification Loop）

质量保障核心。Chrome DevTools 视觉验证 + 可观测性工具 + 强制 Lint/自动化测试，将"声称完成"变为"验证完成"。详见 [[声称完成vs验证完成]]。

### 技术债清理（Tech Debt Cleanup）

大规模 AI 代码生成必然引入重复命名、风格不一致、废弃文档等技术债。后台 [[codex]] 任务定期扫描修复，类比操作系统垃圾回收机制。

## 双重身份

Harness Engineering 兼具两层身份：

1. **当下必要条件**：模型尚未完美，系统层面约束/验证/反馈不可缺失
2. **未来过渡性技术**：模型持续内化系统规则，Harness 逐步衰变（详见 [[harness衰变定律]]）

## 与爱奇艺版的区别

本文 Harness Engineering（李伟山版）与爱奇艺 [[harness-engineering]]（[[数据库团队]]版）需系统对比：

- **李伟山版**：理论框架定义，核心公式"Harness = 除大模型外的一切"，关注三次进化的宏观叙事
- **爱奇艺版**：五要素工程化模型（任务入口、执行依据、工具边界、验证反馈、结果记录），关注具体工程落地

两者从不同视角（理论框架 vs 工程实践）探讨同一主题。

## 与现有 Wiki 概念的关联

- 本文"验证闭环"与 [[pre-pr机制]]（美团）、[[高阶模型审查低阶模型]]（美团）形成跨来源验证体系网络
- 本文"技术债清理"与 [[零排期渐进式重构]]（美团）、[[专家经验定向ai辅助排查]]（美团）形成技术债管理方法论对照
- 本文 [[可靠性边界论]] 与 [[agent生产落地环境重构论]]（vivo 丁俊杰）相关联