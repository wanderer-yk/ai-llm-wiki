---
type: concept
title: 编排类与能力类
tags: [架构, 职责边界, skill, 重构]
related: [si-ceng-jia-gou, skill-as-knowledge-cache, ai-you-hao-yan-fa-gui-fan]
created: 2026-06-08
updated: 2026-06-08
sources: ["用Agent评测思路管理AICoding31万行代码AI重构的实践.html"]
---
# 编排类与能力类

[[si-ceng-jia-gou|四层架构]]中职责边界的划分维度，共识沉淀为渐进式加载的[[skill|Skill]]。

## 定义

- **编排类（Orchestration）**：业务流程编排，串联多个能力完成端到端业务
- **能力类（Capability）**：独立可复用的原子能力，无业务上下文依赖

## 意义

1. **架构层面**：明确职责边界，避免编排逻辑与能力实现耦合
2. **AI Coding层面**：能力类可沉淀为[[skill|Skill]]，编排类按需组合Skill
3. **[[skill-as-knowledge-cache|Skill作为知识缓存层]]**：能力类共识直接对应Skill定义

## 关联

与Wiki已有概念[[skill-as-knowledge-cache|Skill作为工具调用知识缓存层]]直接对应——职责边界共识沉淀为Skill。