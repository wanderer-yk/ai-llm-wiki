---
type: concept
title: Skill-Creator 三版演进
tags: [skill-creator, 工具链, 评测自动化, 案例研究]
related: [Skill-Creator, anthropic, skill评测三原则, skill迭代闭环, agent-skill]
created: 2026-10-08
updated: 2026-10-08
sources: ["[202603091800]打造高效易用的AgentSkill.html"]
---
# Skill-Creator 三版演进

[[Skill-Creator]]（[[anthropic]] 官方 Skill 创建工具）的三次版本演进，被来源用作"Skill 工具链成熟路径"的案例：从"生成"到"生成+优化"再到"对最终运行效果负责"。

| 版本 | 定位 | 能力 |
|------|------|------|
| 第一版（创建） | 降低上手门槛 | 自然语言描述→SKILL.md，输出格式正确的 Frontmatter 和基本指令结构 |
| 第二版（创建 + 优化） | 能力边界拓展 | 承接几乎所有 Skill 相关工作，description 更激进，可自主改进 Skill 并给建议 |
| 第三版（自动评测优化） | 对最终运行效果负责 | 基于需求生成评测用例、创建评分机制、运行评测、评价汇总、循环改进，完成编写同时给出效果结论 |

## 方法论意义

第三版标志着评测内建于工具本身：Skill 的编写工具直接承担 [[skill评测三原则]] 与 [[skill迭代闭环]] 的职责——"完成编写同时给出效果结论"将"评测是必备环节"从开发者纪律变成工具默认行为。

## 开放问题

- 三版演进的时间线与发布节奏，来源未交代。
