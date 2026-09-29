---
type: entity
title: CodeBuddy
tags: [腾讯, ai-coding, 工具, mcp, skill]
related: [openspec, opsx指令集, binxiong, 三大武器库]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202604031907]当整个团队开始0人工Coding一份万字AINative研发实战手册.html"]
---
# CodeBuddy

腾讯团队使用的 AI 编码工具，与 [[openspec]] 配合构成 AI Native 研发的全链路方案。

## 核心特性

- 支持 [[三大武器库]] 三大机制：知识库（知道）、MCP（连接）、Skills（掌握）
- 提供 CLI 版和 IDE 插件版两种形态
- 推荐使用策略：CLI 主攻（核心指令执行、跨项目协同）+ IDE 辅助（文档审查、冲突解决、单行优化）
- bash 执行环境为非 TTY，交互需在 SKILL.md 对话层完成

## 技术架构

- **知识库**：双通道注入——OpenSpec specs/ 核心记忆（自动读写）+ MCP Knot 辅助记忆（非结构化、按需检索）
- **MCP 工具连接**：4 个配置——TCS Component（前端组件库）、TAPD（需求平台）、iWiki（文档平台）、极光流水线（CI/CD）
- **Skills**：统一管理于 tcsc-skills 仓库（`ted.aurora/tcsc-skills`），按业务域分类，通过 SkillHub 市场（`skillhub.dev.jiguang.woa.com`）预览分发

## 与其他工具的关系

- 与 Cursor 的关系待确认（文中未明确说明是自研还是第三方）
- 支持通过 `.codebuddy/rules/` 目录加载 Bridge Rule 规则文件
- 配置文件为 `.mcp.json`（需自动加入 .gitignore 防 Token 泄露）

## 知识库五分类

| 知识类别 | 来源 |
|---------|------|
| 业务知识 | iWiki |
| 架构知识 | iWiki + Git |
| 项目规范 | OpenSpec specs/ |
| 历史决策 | iWiki + OpenSpec |
| 常见问题 | iWiki |