---
type: concept
title: SKILL.md 格式规范
created: 2026-10-13
updated: 2026-10-13
tags: [skill, 规范, yaml, frontmatter]
related: [skill开放标准, skill目录结构, description只写触发条件, 模型驱动触发逻辑, skill膨胀风险]
sources: ["[202605120830]AgentSkill规范构建与设计模式.html"]
---
# SKILL.md 格式规范

SKILL.md 格式规范定义了 Skill 的标准文件形态：**YAML frontmatter（元数据）+ Markdown body（指令正文）**。最小形态仅需一个文件；内容多时拆分到独立文件并保持一层引用深度。

## 核心约束

| 字段 | 必填 | 约束 |
|------|------|------|
| name | 是 | 1-64 字符；仅小写字母/数字/连字符；不能以连字符开头或结尾；不含连续连字符 `--`；必须与所在文件夹名一致 |
| description | 是 | 1-1024 字符；描述作用与使用时机；含帮助 AI 识别任务的关键词 |
| license | 否 | 许可证名称或文件引用 |
| compatibility | 否 | ≤500 字符，运行环境或依赖 |
| metadata | 否 | 键值对映射，扩展属性 |
| allowed-tools | 否 | 空格分隔字符串，实验性功能 |

- 正文建议 **≤500 行**（与渐进式加载 L2 层一致），内容多时拆分到单独文件
- 好的 description 含作用 + 使用时机 + 识别性关键词；差的如 "Helps with PDFs."
- 文件引用使用相对 Skill 根目录路径

## 定位声明

"规范只定义外形不定义内部逻辑"：SKILL.md 格式 ≠ 内部逻辑设计，后者属设计模式范畴（[[skill五种设计模式]]）。

## 与膨胀风险的对照

迭代改进的指令/示例/边界处理累积可导致 Skill 体积失控（5KB→50KB，见 [[skill膨胀风险]]），与 500 行上限形成结构性张力。
