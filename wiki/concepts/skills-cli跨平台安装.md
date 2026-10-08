---
type: concept
title: Skills CLI 跨平台安装
tags: [vercel, cli, agent-skill, 安装分发]
related: [skill-git统一管理, claude-code, cursor, agent-skill]
created: 2026-10-08
updated: 2026-10-08
sources: ["[202603091800]打造高效易用的AgentSkill.html"]
---
# Skills CLI 跨平台安装

各 Agent 平台的 Skill 存放路径约定不同，Vercel 开源了 `skills` CLI 工具，一条命令自动识别当前环境并适配多平台：

```bash
# 从 GitHub 安装，自动识别当前环境并放到正确的位置
npx skills add https://github.com/your-team/skills/tree/main/code-review
# 支持 Claude Code、Cursor、Windsurf 等主流 Agent 平台
# 无需关心各平台的路径差异
```

也可手动放置。以 [[claude-code]] 为例的路径约定（逐字保留）：

```bash
# 项目级（只在当前项目生效）
.claude/skills/code-review/SKILL.md
# 全局级（所有项目生效）
~/.claude/skills/code-review/SKILL.md
```

配套的跨平台生态事实：[[cursor]]、Windsurf 已跟进 Skill 类机制；Asana、Atlassian、Figma、Sentry、Zapier 已为自家 MCP Server 配套 Skill；独立开发者持续贡献前端设计/代码审查/数据分析/项目管理类 Skill。Skill 源管理见 [[skill-git统一管理]]。
