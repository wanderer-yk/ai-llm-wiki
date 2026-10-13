---
type: comparison
title: "Skill 五种设计模式与四类型分类法对比"
created: 2026-10-13
updated: 2026-10-13
tags: [skill设计模式, skill类型, 分类法, google-adk, writing-skills]
related: [skill五种设计模式, skill四类型分类法, google-adk, writing-skills, superpowers插件, skill生态知识体系, skillmd格式规范, blocker-gate, 验证门禁化]
sources: ["[202605120830]AgentSkill规范构建与设计模式.html"]
---
# Skill 五种设计模式与四类型分类法对比

珂罗《Agent Skill规范、构建与设计模式》并置了两套跨厂商 Skill 分类法：第四章来自 Google ADK 团队的 [[skill五种设计模式]]（出处：Google Cloud Tech X 帖，归属 [[google-adk]]），第三章 3.4 节来自 Anthropic Writing-Skills 的 [[skill四类型分类法]]。二者同源于本文，但分属 [[skill生态知识体系]] 中"设计模式"与"构建方法论"两个不同层面。

## 逐维对比

| 维度 | 五种设计模式（Google ADK） | 四类型分类法（Writing-Skills） |
|---|---|---|
| 分类对象 | Skill 的**内部逻辑/架构**模式 | Skill 的**内容性质/功能**类型 |
| 类别清单 | Tool Wrapper / Generator / Reviewer / Inversion / Pipeline | 纪律执行型 / 技术指导型 / 思维模式型 / 参考资料型 |
| 回答的问题 | "内部逻辑该怎么设计？" | "这类 Skill 该怎么测试、怎样算成功？" |
| 配套决策工具 | 4.2 选择指南表 + 4.3 组合推荐表（Pipeline+Reviewer / Generator+Inversion / Pipeline+Tool Wrapper / Inversion+Pipeline）+ 默认策略"不确定从 Tool Wrapper 开始" | 每类型绑定的测试方法 + 成功标准四列总表 |
| 研究方法 | 跨生态归纳（Anthropic 仓库、Vercel、Google 内部指南）中反复出现的模式 | 基于 TDD 实践的测试经验归纳 |
| 可组合性 | 模式可组合（四种官方推荐组合） | 类型描述内容本质，一个 Skill 通常归属单一主类型 |

## "规范 vs 内部逻辑"定位声明

来源的关键论断为两套分类法划界："规范（SKILL.md 格式）只定义 Skill 长什么样，不定义内部逻辑该怎么设计"——"一个封装 FastAPI 规范的 Skill 和一个分 4 步执行的文档流水线 Skill，虽然外表都叫 SKILL.md，但内部结构完全不是一回事"。五种设计模式正是对这层"内部逻辑"的分类；四类型分类法则服务于测试策略（纪律执行型测压力守规、参考资料型测可发现性与完整性等，详见 [[skill四类型分类法]]）。

## 映射关系（开放问题）

来源未给出两套分类法的映射。wiki 侧直觉候选（**未经来源验证，仅作假设**）：

- Tool Wrapper ↔ 参考资料型：核心均为按需加载参考知识（SKILL.md 告诉 Agent 去哪加载规范 ↔ 主要测可发现性）；
- Reviewer ↔ 纪律执行型：均强制约束输出合规（审查清单 ↔ 压力下守规）。

但该映射不完整——Generator/Inversion/Pipeline 与技术指导型/思维模式型难以一一对应，说明两套分类法是不同维度（构建架构 vs 内容性质）的切分，而非同一空间的两种等价划分。

## 跨来源注记

Pipeline 模式"确认前不得继续"的用户确认硬门控，与爱奇艺 [[blocker-gate]]、vivo [[验证门禁化]] 形成三方独立趋同，是硬门控跨厂商共识的证据（模式细节见 [[skill五种设计模式]]）。
