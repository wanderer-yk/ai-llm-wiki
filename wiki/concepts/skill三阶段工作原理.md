---
type: concept
title: Skill 三阶段工作原理
tags: [agent-skill, 运行机制, 工具调用]
related: [skill渐进式披露, description触发准确性权衡, claude-code, agent-skill]
created: 2026-10-08
updated: 2026-10-08
sources: ["[202603091800]打造高效易用的AgentSkill.html"]
---
# Skill 三阶段工作原理

Agent Skill 在运行时的三阶段机制：

1. **常驻索引**：description 注入系统提示词，Agent 知道有什么 Skill 但不知道其内容——成本仅为几十行短文本。
2. **激活读取**：用户请求匹配某 description 后，Agent 用内置 `view`/`read` 工具读取该 Skill 的 SKILL.md，对应 `messages[]` 中的一次工具调用。
3. **执行与深入**：按 SKILL.md 指令执行任务，需要更多细节时按需读取 `references/` 或执行 `scripts/`。

## 关键推论

- **激活本身消耗 1-2 步工具调用**：误触发是浪费、漏触发是能力缺失，description 精度直接影响 Token 消耗与响应速度（见 [[description触发准确性权衡]]）。
- 触发相关信息必须且只能写在 Description——写进 Body 因触发后才加载而无效。

该机制以 [[claude-code]] 的实现为参照描述，是 [[skill渐进式披露]] 在运行时层面的具体化。
