---
type: concept
title: Harness 衰变定律
tags: [harness-engineering, 理论规律, 模型能力, 过渡性技术]
related: [harness-engineering-李伟山版, 工程三次进化框架, 动态harness思维, claude-3-与-claude-3-5, prompt-engineering-李伟山版, 脚手架优于模型]
created: 2026-07-20
updated: 2026-07-20
sources: ["[202605201731]干货从PromptContext到Harness工程的三次进化与终局之战原创.html"]
---
# Harness 衰变定律

Harness 衰变定律是 [[李伟山]] 文章第 06 章提出的核心理论规律，被描述为"最深刻也最容易被误解的规律"。

## 核心命题

**模型能力越强，所需 Harness 越简单。**

模型能力与 Harness 复杂度呈反比关系——这是本文最具理论原创性的发现之一。

## 实证依据

[[anthropic]] 的版本对比研究：

- **Claude 3.0 时代**：需要极严格约束——逐个功能点执行、频繁重置上下文、大量硬编码检查规则
- **Claude 3.5 时代**：模型全局统筹能力、长上下文处理能力、自我校验能力大幅提升后，许多 Harness 规则自然失效

模型逐步内化了原本需要外部约束才能保证的系统规则。

## 两层深意

1. **Harness Engineering 是当下的现实答案**：模型尚未完美，系统约束不可缺失
2. **Harness Engineering 可能是过渡性技术**：模型持续进化，未来可能内化大部分系统规则

## 实践建议

集中精力在两类不可替代场景：

- **业务逻辑边界**：行业规则、合规要求、复杂协同——模型短期内无法自行掌握
- **外部环境接口**：工具调用、API 集成、权限管控——模型无法自行建立的外部连接

## 内在张力

本文"Harness 可能是过渡性技术"与"Harness 是除大模型之外的一切"构成内在张力——如果 Harness 会衰变，其"一切"地位是否会被侵蚀？这一张力有待进一步讨论。

## 与现有 Wiki 概念的关联

- Harness 衰变定律与 [[prompt-engineering-李伟山版]] 中 Prompt Engineering 边际衰减形成**跨阶段呼应**——每个进化阶段都可能随模型能力提升而面临边际效益递减
- Harness 衰变定律与 [[脚手架优于模型]]（[[zhiyuanfu]]：成本+50%效果+200%）的关系复杂化：如果模型升级会让 Harness 衰变，那么投资脚手架的 ROI 计算需要加入**时间衰减因子**

## 开放问题

- Harness 衰变速率是否有量化经验公式
- Claude 3.0 → 3.5 具体哪些 Harness 规则被废弃