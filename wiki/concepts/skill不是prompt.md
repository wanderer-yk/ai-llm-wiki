---
type: concept
title: Skill 不是 Prompt
created: 2026-10-13
updated: 2026-10-13
tags: [skill, 定义, agent]
related: [skill生态知识体系, skillmd格式规范, 生产级skill, skill开放标准]
sources: ["[202605120830]AgentSkill规范构建与设计模式.html"]
---
# Skill 不是 Prompt

"Skill 不是 Prompt"是《Agent Skill规范、构建与设计模式》的定义级核心论断：**Skill 是围绕任务、工具、流程和输出边界的结构化行为设计**，而非单纯的一段提示词文本。Skill 的"可复用 Prompt 增强包"定位通过渐进式加载机制为 Agent 注入领域知识和工作流程，其最小形态是一个 SKILL.md 文件，但其价值在于结构化的行为边界而非文字本身。

## 支撑论据

- 第四章 Google 视角的补充论证：规范（SKILL.md 格式）只定义"Skill 长什么样"，不定义"内部逻辑该怎么设计"——"一个封装 FastAPI 规范的 Skill 和一个分 4 步执行的文档流水线 Skill，虽然外表都叫 SKILL.md，但内部结构完全不是一回事"
- [[skill五种设计模式]] 的存在本身即证明：同一种外壳（SKILL.md）之下有多种截然不同的内部行为设计

## 与既有概念的关系

本论断与 [[生产级skill]]（具备完整文件结构/设计决策/安装流程/版本管理的可复用 SOP 封装，腾讯视角）互为印证，共同否定"Skill = 更长的 prompt"的简化理解。
