---
type: concept
title: Skill 目录结构
created: 2026-10-13
updated: 2026-10-13
tags: [skill, 目录, 结构]
related: [skillmd格式规范, skill渐进式加载机制, skill打包分发]
sources: ["[202605120830]AgentSkill规范构建与设计模式.html"]
---
# Skill 目录结构

Skill 目录结构是 SKILL.md 单文件格式之外的包级结构约定：一个 Skill 目录以 SKILL.md 为必需入口，另设三个可选目录承载不同职责的辅助资产。

```bash
skill-name/
├── SKILL.md          # 必需：YAML 元数据 + Markdown 指令
├── scripts/          # 可选：可执行脚本
├── references/       # 可选：按需加载的参考文档
└── assets/           # 可选：模板、资源文件
```

## 三目录职责

- **scripts/**：可执行代码（支持 Python/Bash/JavaScript）
- **references/**：按需读取的补充文档，文件越小上下文消耗越少
- **assets/**：模板、图片、Schema 数据

引用规范：使用相对 Skill 根目录路径，建议保持一层深度。

## 设计依据

Skill-Creator"提取重复模式"思想为 scripts/ 目录提供依据：测试中 Agent 反复独立编写的辅助脚本，是应下沉为 Skill 直调脚本的强信号。Tool Wrapper 设计模式（[[skill五种设计模式]]）中"指向 references/conventions.md"的写法是该结构的模式化应用。
