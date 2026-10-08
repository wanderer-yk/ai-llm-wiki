---
type: concept
title: Skill Git 统一管理
tags: [git, agent-skill, 团队协作]
related: [skills-cli跨平台安装, skill目录结构与命名规范, agent-skill]
created: 2026-10-08
updated: 2026-10-08
sources: ["[202603091800]打造高效易用的AgentSkill.html"]
---
# Skill Git 统一管理

Skill 数量变多后散落各处会失控，来源推荐从一开始就用 Git 仓库统一管理，三个好处：**版本有记录、团队能协作、跨仓库安装迅速**。

团队 Skill 库的目录组织示例（逐字保留）：

```
team-skills/
├── code-review/
│   └── SKILL.md
├── react-state-management/
│   ├── SKILL.md
│   └── references/
├── sprint-planning/
│   ├── SKILL.md
│   └── scripts/
└── .
```

与 [[skills-cli跨平台安装]] 配合使用：Git 仓库作为唯一真源，`npx skills add <GitHub URL>` 作为分发通道。这一"知识文件随仓库管理"的思路与有赞 [[分布式wiki架构]]（`.wiki/` 跟随 Git submodule）形成跨来源呼应。
