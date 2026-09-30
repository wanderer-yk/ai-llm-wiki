---
type: concept
title: 轻量级单进程Agent框架
tags: [agent框架, 极简实现, typescript, openclaw, 单进程]
related: [薄抽象设计哲学, 有意取舍边界声明, 子agent单进程并发模型, 入站消息总线, tool抽象四要素, 子agent工具排除机制, exec-tool三层防护, 简化cron解析, pi-agent, 极简agent设计哲学, openclaw, anthropic]
created: 2026-09-30
updated: 2026-09-30
sources: ["[202604241629]800行代码实现OpenClaw的Tool消息总线子Agent管理架构.html"]
---
# 轻量级单进程Agent框架

轻量级单进程 Agent 框架指不依赖任何 Agent 框架、以单进程单线程运行为约束、用最薄抽象层实现 Agent 核心能力（工具调用、消息分发、子 Agent 管理、主循环）的实现路线。本 wiki 中该词特指苏雄在《800行代码实现OpenClaw的Tool消息总线子Agent管理架构》中给出的实现：**Anthropic Claude API + TypeScript + 单进程 Node.js，约 800 行，零框架依赖，直接基于 `@anthropic-ai/sdk`**，覆盖四个核心模块（Tool layer / MessageBus / SubagentManager / REPL 主循环），明确不涉及上层 Bot 接入层、持久化与 Context/Memory 系统（ContextBuilder 仅作启动组件出现，见来源页「表述张力」记录）。

## 内置工具面（≥9 个）

| 工具 | 类别 | 行为要点 |
|------|------|----------|
| ReadFileTool | 文件操作 | 动态 `import("node:fs/promises")` 加载模块 |
| WriteFileTool | 文件操作 | 写入前 `mkdir` 递归创建父目录 |
| EditFileTool | 文件操作 | 唯一匹配强制替换：0 次报错、>1 次拒绝写入 |
| ListDirTool | 文件操作 | 列目录，`[folder]`/`[file]` 前缀区分类型 |
| ExecTool | 命令执行 | 三层防护（正则黑名单/资源限制/输出截断） |
| WebSearchTool | Web 能力 | 封装 Brave Search API，`count` 默认 5 上限 10 |
| WebFetchTool | Web 能力 | 纯正则 `htmlToText`，20,000 字符截断 |
| MessageTool | 通信 | 构造注入 `sendCallback` 出站通道 |
| CronTool | 调度 | `add`/`list`/`remove` 三 action，对接 CronService |
| SpawnTool | 子 Agent | function calling 触发子 Agent，返回 ID + 运行中数量 |

## 与相关实现的对比

- **vs [[pi-agent]]**（OpenClaw 底层 Agent Core，仅 Read/Write/Edit/Shell 4 个核心工具）：本框架工具面 ≥9 个，且多出子 Agent 体系、CronService 定时能力、MessageBus 入站总线、启动加载 skills/memory，复刻面显著更宽；但共享「文件操作四件套 + Shell」的核心同构。
- **vs [[极简agent设计哲学]]**（279 行 Python 单文件）：同属极简谱系，共享零框架依赖与显式取舍声明；本框架以约 800 行换取了更完整的工程能力（并发控制、消息双通道、定时调度）。
- 核心实现哲学见 [[薄抽象设计哲学]]；显式取舍清单见 [[有意取舍边界声明]]。
