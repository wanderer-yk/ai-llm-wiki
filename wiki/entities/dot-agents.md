---
type: entity
title: dot-agents
tags: [配置仓库, claude-code, 工具链]
related: [seanguo, claude-code, skill-command-mcp三层架构]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202604171736]从VibeCoding到AgenticEngineering重构后台开发全流程.html"]
---
# dot-agents

[[seanguo]] 的工具链配置仓库，存放 [[skill-command-mcp三层架构|Skill/Command/MCP]] 的完整配置。

## 仓库信息

- 地址：`git.example.com/alice/dot-agents`
- 配置根目录：`~/.claude-internal/`

## 目录结构

```
~/.claude-internal/
├── CLAUDE.md        # 全局指令/代码规范/偏好
├── settings.json    # 权限白名单/模型选择/插件启用
├── commands/        # 斜杠命令（Command 层）
└── skills/          # 技能库（Skill 层）
```

该目录结构与 [[skill-command-mcp三层架构]] 概念完全对应——commands/ 对应 Command 层、skills/ 对应 Skill 层、settings.json 管理权限对应 Control Plane。

## 关联来源

- [[sources/[202604171736]从VibeCoding到AgenticEngineering重构后台开发全流程|[202604171736]从 Vibe Coding 到 Agentic Engineering：重构后台开发全流程]]