---
type: concept
title: Skill 目录结构与命名规范
tags: [agent-skill, 文件结构, 命名规范]
related: [agent-skill, skill渐进式披露, skill-git统一管理, skills-cli跨平台安装]
created: 2026-10-08
updated: 2026-10-08
sources: ["[202603091800]打造高效易用的AgentSkill.html"]
---
# Skill 目录结构与命名规范

Skill 在文件系统中的物理形态：**文件夹即 Skill**，最小结构只需一个 SKILL.md 文件。

## 标准目录结构

```bash
your-skill-name/
├── SKILL.md          # 必须，入口文件
├── scripts/          # 可选，可执行脚本
├── references/       # 可选，参考文档
└── assets/           # 可选，模板、图标等资源
```

## 命名规则

- Skill 文件夹用 **kebab-case** 命名。
- 入口文件精确命名 `SKILL.md`（**大小写敏感**）。
- **禁止**在 Skill 文件夹内放 README.md。

## SKILL.md 结构

YAML Frontmatter（`name` 与 `description` 为必填字段）+ Markdown 正文（Agent 执行指令）：

```markdown
---
name: my-skill-name
description: 做什么。在用户说"XXX"时使用。核心能力包括 A、B、C。
---
# My Skill Name
（正文：Agent 执行指令）
```

多 Skill 的团队级组织见 [[skill-git统一管理]]，安装分发见 [[skills-cli跨平台安装]]，运行时加载见 [[skill三阶段工作原理]]。
