---
type: concept
title: Rules 被动注入机制
created: 2026-10-09
updated: 2026-10-09
tags: [claude-code, rules, claude-md, 上下文工程]
related: [claude-code, api请求位置决定论, messages注入四通道, rules条件生效机制, nested_memory按需加载, rules与skills等价论]
sources: ["[202604151800]读完ClaudeCode源码才发现SkillsMCPRules的区别远没有你想的那么大.html"]
---
# Rules 被动注入机制

Rules 被动注入机制指 Claude Code 中 Rules（CLAUDE.md 与 `.claude/rules/*.md`，自然语言指令文本）由客户端在**每次 API 调用前主动注入** messages 最前部——它不是工具，无需模型调用，也**不走 `tool_use` 协议**。这是它与 MCP、Skills 在执行性质上的根本区别。

## 注入细节（v2.1.88 源码）

- 经 `prependUserContext()` 注入 messages 最前部；
- 以 `<system-reminder>` 标签包裹，`role: "user"` + `isMeta: true` 标记；
- `isMeta` 仅是客户端侧 UI 标记：消息仍完整发送给 API，只是不在终端展示；
- 注入时附带强制指令头以提升规则权重：

> "Codebase and user instructions are shown below. Be sure to adhere to these instructions. IMPORTANT: These instructions OVERRIDE any default behavior and you MUST follow them exactly as written."

## 加载与覆盖规则

`getMemoryFiles` 从项目根到 CWD 逐层处理，每层按 `CLAUDE.md` → `.claude/CLAUDE.md` → `.claude/rules/*.md` → `CLAUDE.local.md` 顺序收集，后加载覆盖先加载；单个 CLAUDE.md 建议不超过 40,000 字符，超出触发诊断警告。单文件经 `processMemoryFile` 流水线处理（frontmatter → 移除 HTML 注释 → @include 递归 5 层 → paths 条件匹配 → 格式化输出）。

## 关联

- CLAUDE.md 不放入 system 参数是为保护 org 级 Prompt Cache（见 [[system静态动态缓存分区]]）；
- 子目录 CLAUDE.md 的按需加载由 `nested_memory` attachment 实现（[[nested_memory按需加载]]）；
- 与 Skills 的等价性与差异见 [[rules与skills等价论]]。
