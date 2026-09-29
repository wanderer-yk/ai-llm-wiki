---
type: concept
title: 生产级 Skill
tags: [skill, sop, 自动化, 知识复用]
related: [codebuddy, 三大武器库, opsx指令集]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202604031907]当整个团队开始0人工Coding一份万字AINative研发实战手册.html"]
---
# 生产级 Skill

具备完整文件结构、设计决策、安装流程、版本管理的可复用 SOP 封装，是 [[三大武器库]] 中 Skills 层的标准化产物。核心理念：**写一次，全团队永久受益**。

## 标准文件结构

```
skill-name/
├── SKILL.md           # SOP 灵魂（frontmatter 元数据 + 600+ 行 SOP 正文）
│   ├── frontmatter    # name/description/version/author/center/module/tags
│   └── 正文           # 可执行 SOP 步骤，每步含 bash 代码块
├── version.json       # 版本控制 + 完整元数据
├── scripts/           # 跨平台自动化脚本
│   ├── install.sh     # 幂等安装
│   └── self_update.sh # 自身版本检测和更新
└── templates/         # 配置模板（预填默认值）
```

## SKILL.md Frontmatter 元数据

- **description 双重职责**：让人理解 Skill 做什么 + 让 AI 知道何时触发（关键词匹配）
- **center 字段**：标识 Skill 适用范围（如"公共"=通用）

## 九大工程实践要点

1. SKILL.md 定义完整 SOP
2. 每步幂等（可重复执行，中断后可重来）
3. 跨平台脚本（macOS/Linux/Windows）
4. 配置模板预填默认值
5. [[bridge-rule]] 解决 AI 上下文断裂
6. Token 安全防护（自动 .gitignore）
7. 三级降级策略（MCP→本地→静默跳过）
8. version.json 热更新
9. 用 [[openspec]] 管理自身迭代（[[自举式开发]]）

## 典型示例

**openspec-installer**：将 8 步手动环境搭建（至少 1 小时）压缩为 1 条命令，当前版本 v0.4.3。经历 v0.3.0→v0.4.1 的密集迭代（3 天 4 版），每次变更均有 proposal/design/spec/tasks 完整记录，体现 [[自举式开发]] 理念。