---
type: concept
title: Rules 条件生效机制
created: 2026-10-09
updated: 2026-10-09
tags: [claude-code, rules, frontmatter]
related: [claude-code, rules被动注入机制, nested_memory按需加载, rules与skills等价论, api请求位置决定论]
sources: ["[202604151800]读完ClaudeCode源码才发现SkillsMCPRules的区别远没有你想的那么大.html"]
---
# Rules 条件生效机制

Rules 条件生效机制指 `.claude/rules/*.md` 可通过 frontmatter 的 `paths` 字段声明生效范围：只有当模型处理匹配路径的文件时，该规则才会被注入。这直接回应了「Rules 只能始终生效、Skills 才能按需引入」的流行认知——**Rules 也可以按需生效**。

## 示例（原文保留）

```text
---
paths:
- "src/components/**/*.tsx"
- "src/hooks/**/*.ts"
---
在 React 组件中始终使用函数式组件和 hooks。
```

## 处理流水线（`processMemoryFile`）

```text
读取文件
↓
解析 frontmatter（提取 paths 等条件匹配字段）
↓
移除 HTML 注释
↓
处理 @include 引用（最大递归深度 5 层）
↓
条件规则匹配（paths 字段匹配当前文件路径）
↓
格式化输出
```

## 与 Skills 按需引入的界限

既然 Rules 可经 `paths` 按需生效，它与 Skills「按需引入」的界限就不再是「是否按需」，而是触发方式（路径匹配 vs 模型/用户触发）、执行隔离与组织管理属性——完整分析见 [[rules与skills等价论]]。子目录级按需加载另见 [[nested_memory按需加载]]。
