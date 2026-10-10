---
type: concept
title: claude-md四路径分层
created: 2026-10-10
updated: 2026-10-10
tags: [claude-code, context-engineering, 知识管理, 配置分层]
related: [system-reminder注入机制, 渐进式披露替代向量检索, 多源分治策略, ssot单文档策略, 知识准入控制]
sources: ["[202604200830]深度解析ClaudeCode在PromptContextHarness的设计与实践.html"]
---
# claude-md四路径分层

**claude-md 四路径分层**是 [[claude-code]] 用四个不同路径的 CLAUDE.md 文件实现「个人/团队、全局/项目、共享/私有」指令分层的上下文注入体系。CLAUDE.md 被定位为「项目说明书」，经 [[system-reminder注入机制]] 成为对话第一条消息并以强调语注入。

## 四路径（原文还原）

| 路径 | 定位 | Git 策略 |
|------|------|----------|
| `~/.claude/CLAUDE.md` | 个人通用偏好（跨项目全局人设） | 用户维度静态配置 |
| 项目根 `CLAUDE.md` | 项目共享规范（架构/编码规范/构建命令） | 必须提交 Git |
| `CLAUDE.local.md` | 个人私有指令（敏感/个性化信息） | 明确不入 Git |
| `.claude/rules/*.md` | 按文件类型/领域拆分规则，Frontmatter 限定生效路径 | 项目内模块化 |

## 对比视角

与 [[openclaw]] 的多文件人格/知识体系（AGENT.md/SOUL.md/IDENTITY.md 等）同为 Markdown 驱动，但按 Agent 定位差异化：Coding Agent 以项目为中心分层，私人助理以人格为中心分文件。其「按层级约束知识写入」的思路与 [[最小作用域原则]]、[[多源分治策略]] 呼应，而 `.claude/rules/*.md` 的 Frontmatter 限定生效范围则与有赞 [[渐进式披露替代向量检索]] 的「按需披露」异曲同工。
