---
type: source
title: "读完 Claude Code 源码才发现：Skills、MCP、Rules 的区别，远没有你想的那么大"
authors: [Cheer]
year: 2026
url: "https://mp.weixin.qq.com/s?__biz=Mzg5MjU0NTI5OQ==&mid=2247606609&idx=1&sn=20ef8bf4ac3cae6de02209687b8fbdff"
venue: "百度Geek说（微信公众号，GEEK TALK 栏目）"
created: 2026-10-09
updated: 2026-10-09
tags: [claude-code, mcp, skills, rules, 源码分析, prompt-cache, 上下文工程]
related: [Cheer, 百度Geek说, claude-code, mcp, anthropic, api请求位置决定论, rules被动注入机制, system静态动态缓存分区, messages注入四通道, rules条件生效机制, nested_memory按需加载, mcp内置工具同构论, mcp-instructions落地缺位, skill提示词注入本质, skill列表token预算, skill强制触发指令, skill双执行模式, skill触发可靠性痛点, rules与skills等价论, skills真正价值三场景, skill嵌套编排, 触发质量决定论, skill流程非代码化, skill质量等式, 提示词工程统一论, rules-skills-mcp选型指南, mcp与bash对比, 三大武器库, agent欠触发倾向]
sources: ["[202604151800]读完ClaudeCode源码才发现SkillsMCPRules的区别远没有你想的那么大.html"]
---
# 读完 Claude Code 源码才发现：Skills、MCP、Rules 的区别，远没有你想的那么大

## 基本信息

- **作者**：[[Cheer]]，发布于微信公众号「[[百度Geek说]]」（IP 属地上海，GEEK TALK 栏目，原创标记）
- **发布时间**：2026-04-15，全文 6858 字（预计阅读 10 分钟）
- **分析对象**：Claude Code v2.1.88 泄漏源码（文中称 claude-code-source-code）
- **文内引用姊妹篇**：《Claude Code 架构解析：从 Skill 调用到 Prompt Cache》（未收录，待获取）

## 核心论点：API 请求位置决定论

文章核心声明（导读原文）：

> 通过对 Claude Code 源码的分析，揭示了 Rules、MCP、Skills 三个概念的底层实现机制。Rules 是项目级行为规范，通过 messages 被动注入；MCP 是标准化工具协议，在 system 和 tools 中注册并调用外部服务；Skills 是可复用提示词，通过 tool_use 触发后注入指令文本。三者的核心区别在于信息在 API 请求中的位置不同，而非功能本质。

正文对导读口径做了两次关键修正，最终定谳图景为：

| 机制 | 准确定位 |
|------|---------|
| Rules | `prependUserContext()` 注入 messages 最前部，**不走 `tool_use` 协议**，是每次 API 调用的被动注入上下文（详见 [[rules被动注入机制]]） |
| MCP | `tools[]` 注册 + system 动态区 instructions 双位置注入，执行是真实 JSON-RPC 调用（详见 [[mcp内置工具同构论]]） |
| Skills | 两阶段机制：`skill_listing` attachment 常驻注册 + `tool_use` 触发后注入 SKILL.md 提示词文本（详见 [[skill提示词注入本质]]） |

方法论声明：这些疑问「靠读文档和博客是答不清楚的」，因为它们是「实现层面的问题」。

## 导读三问与源码级解答

- **Q1：Rules 与 Skills 的界限？** 对模型没有本质区别——都是 messages 中 `role: "user"` 文本；真正区别仅触发方式、执行隔离、组织管理属性三点（详见 [[rules与skills等价论]]）。
- **Q2：MCP 工具与内置 Tools 的区别？** 对模型没有任何区别——`tools[]` 格式、调用方式一样，区别纯粹在 Agent 侧执行路由（详见 [[mcp内置工具同构论]]）。
- **Q3：Skills 的「标准化工作流」是真流程吗？** 不是代码层面的流程化——源码中没有任何代码逻辑控制执行步骤，就是结构化 Markdown，完全靠模型指令遵循能力（详见 [[skill流程非代码化]]）。

## 结构化数据（源码级证据，原文保留）

### API 请求三参数结构

```js
anthropic.messages.create({
    system: TextBlockParam[],  // 静态角色定义和行为规范
    tools: BetaToolUnion[],  // 工具定义（name + description + input_schema）
    messages: MessageParam[],  // 动态对话内容
})
```

### tool_use 协议流程

```js
用户消息
↓
模型推理 → 输出 tool_use 块
{ "type": "tool_use", "id": "toolu_xxx", "name": "工具名", "input": { ...参数... } }
↓
调用方（Agent）执行工具
↓
将结果作为 tool_result 追加到对话
{ "type": "tool_result", "tool_use_id": "toolu_xxx", "content": "执行结果" }
↓
继续下一轮模型推理
```

### messages 四类内容注入通道

| 注入通道 | 源码标识 | 内容 | 位置/标记 |
|----------|---------|------|----------|
| 系统上下文注入 | `prependUserContext` | CLAUDE.md 内容、当前日期等 | messages；`isHidden: true` + `isMeta: true`，`<system>` 包裹 |
| 系统提示上下文 | `appendSystemContext` | git 状态等 | system 参数 |
| 动态附件 | Attachments | Skill 列表、计划模式指令、子目录 CLAUDE.md 等 | messages |
| 真实对话历史 | — | 用户输入、模型回复、工具调用结果 | messages |

### Rules 文件发现机制

| 项 | 值 |
|----|-----|
| 源码函数 | `getMemoryFiles` |
| 处理方向 | 从项目根到 CWD 逐层处理 |
| 每层收集顺序 | `CLAUDE.md` → `.claude/CLAUDE.md` → `.claude/rules/*.md` → `CLAUDE.local.md` |
| 覆盖规则 | 后加载的覆盖先加载的 |
| 大小限制 | 单个 CLAUDE.md 建议不超过 40,000 字符，超出触发诊断警告 |

### `processMemoryFile` 处理流水线

```text
读取文件
↓
解析 frontmatter（提取 paths 等条件匹配字段）
↓
移除 HTML 注释
↓
处理 @include 引用（最大递归深度 5 层）
↓
条件规则匹配（.claude/rules/*.md 中 paths 字段匹配当前文件路径）
↓
格式化输出
```

### 条件规则 paths 示例

```text
---
paths:
- "src/components/**/*.tsx"
- "src/hooks/**/*.ts"
---
在 React 组件中始终使用函数式组件和 hooks。
```

### Rules 注入标记与强制指令头

> "Codebase and user instructions are shown below. Be sure to adhere to these instructions. IMPORTANT: These instructions OVERRIDE any default behavior and you MUST follow them exactly as written."

### nested_memory attachment 处理

```javascript
case "nested_memory":
return [createMessage({
    content: `Contents of ${attachment.content.path}:\n\n${attachment.content.content}`,
    isMeta: true
})];
```

### toolToAPISchema 核心逻辑

```javascript
async function toolToAPISchema(tool, options) {
    return {
        name: tool.name, // 如 "mcp__github__create_issue"
        description: await tool.prompt(), // 工具描述 → tools[].description
        input_schema: tool.inputJSONSchema // 参数 schema
    };
}
```

### getMcpInstructions（源码路径：`src/constants/prompts.ts`）

```javascript
function getMcpInstructions(mcpClients) {
    const clientsWithInstructions = mcpClients
        .filter(c => c.type === "connected")
        .filter(c => c.instructions); // 只取有 instructions 的 Server
    if (clientsWithInstructions.length === 0) return null;
    return `
# MCP Server Instructions
The following MCP servers have provided instructions for how to use their tools and resources:
${clientsWithInstructions.map(c => `## ${c.name}\n${c.instructions}`).join("\n\n")}
`;
}
```

### MCP 执行流程

```text
模型输出 tool_use: { name: "mcp__github__create_issue", input: {...} }
↓
Claude Code 识别 mcp__ 前缀，路由到对应 MCP Client
↓
MCP Client 发送 JSON-RPC 请求到 MCP Server 进程
↓
MCP Server 执行实际操作（如调用 GitHub API）
↓
返回真实结果
↓
tool_result.content = MCP Server 的真实输出
↓
模型读取结果，继续推理
```

配置位置：MCP 服务器定义于 `~/.claude.json`（user scope）或项目根目录 `.mcp.json`（project scope）；工具命名模式 `mcp__<serverName>__<toolName>`；`initialize` 握手返回工具列表 + 可选 `instructions` 字段。

### skill_listing attachment 处理

```php
case "skill_listing": {
    return [createMessage({
        content: `The following skills are available for use with the Skill tool:\n\n${attachment.content}`,
        isMeta: true
    })];
}
```

### Skill Inline 模式执行流程（默认模式）

```text
模型输出 tool_use: { name: "Skill", input: { skill: "commit", args: "" } }
↓
Claude Code 读取本地 SKILL.md 提示词文本
↓
将提示词内容包装为 isMeta: true 的 user 消息，注入到对话历史中
↓
tool_result 仅返回一个标签："Launching skill: commit"
↓
下一轮 API 调用时，对话历史中已包含完整的 Skill 指令
↓
模型读到指令后，按步骤调用工具（Read、Edit、Bash 等）执行任务
```

### Skill 工具 description 内嵌强制触发指令

> "When a skill matches the user's request, this is a BLOCKING REQUIREMENT: invoke the relevant Skill tool BEFORE generating any other response about the task"

### 手动触发链路对照

```text
手动触发 Skill：
你输入 /commit
→ Claude Code 查找对应 SKILL.md
→ 包装为 tool_use 调用
→ 读取 Markdown 文本
→ 注入到 messages 中
→ 模型读到这段文本，按指令执行

手动引用 Rules 文件：
你输入 @commit-rules.md + "帮我提交代码"
→ Claude Code 读取文件内容
→ 作为 FileAttachment 注入到 messages 中
→ 模型读到这段文本，按规范执行
```

### 多步组合任务对照示例

```text
用户："帮我完成这个 feature，包括写代码、写测试、提交"

手动引用方式：
@coding-rules.md @test-rules.md @commit-rules.md
→ 用户需要知道有哪些规则、叫什么名字、在哪里

Skill 自动触发：
→ 模型识别任务，依次自动调用 coding / test / commit skill
→ 用户只说了目标，工具选择完全交给模型
```

### 4.4 实际使用建议（原文清单）

| 何时使用 | 适用条件 |
|---------|---------|
| Rules | ①项目级编码规范、技术栈约定、代码风格要求；②文本短（几百字以内），每次注入不心疼 token；③需要「始终生效」的指令，不依赖模型判断 |
| Skills | ①指令文本较长（几百行级别），不适合每次注入；②有明确触发时机（用户主动 `/commit`、`/review-pr`）；③需要执行隔离（Fork 模式独立上下文，不污染主对话） |
| MCP | ①需要持久化连接/状态管理（数据库连接池、认证 session）；②复杂多步操作需要原子封装；③需要权限隔离，不想给模型万能 Bash；④简单 CLI 操作（`gh`、`curl`、`psql`）直接用 Bash，别折腾 MCP |

### 关键参数汇总

- Skill 列表 token 预算：上下文窗口 **1%**（默认 **8000 字符**）
- 单 Skill 描述上限：**250 字符**
- Fork 触发配置：`context: 'fork'`
- CLAUDE.md 单文件上限：**40,000 字符**
- `@include` 最大递归深度：**5 层**
- org 级 Prompt Cache 后续调用费用：**0.1x**

## 证据等级警示

- 论证基于 **v2.1.88 泄漏源码**，版本特定、非官方口径，结论时效性受版本演进影响。
- 累计函数名六个：`getMemoryFiles` / `processMemoryFile` / `toolToAPISchema` / `getMcpInstructions` / `createMessage` / `skill_listing` case；文件路径一个：`src/constants/prompts.ts`。
- 轻微引用瑕疵：文末参考源码标注为「泄漏源码 claude-code-source-code」，所附链接却是官方仓库 https://github.com/anthropics/claude-code（官方仓库不发布源码），引用链条不严谨。
- 不可提取图片：3.3.2 Skills 文件发现位置、4.1 三者核心对比表、4.2 全貌图、MCP 传输方式列表，均需外部补证。

## 与既有 Wiki 的关联

- 与 [[claude-code]] 既有「纯 grep 代码匹配方案」结论并存，本文补充注入机制层证据。
- 「API 请求位置决定论」与 [[三大武器库]]（腾讯知识库/MCP/Skills 三层赋能叙事）存在持续张力，候选比较页。
- Skill 欠触发常态与 [[agent欠触发倾向]] 构成源码级强互证；BLOCKING REQUIREMENT、250 字符预算、触发质量决定论与 [[description三大要素]]、[[负向触发说明]]、[[description触发准确性权衡]] 互证。
- `nested_memory` 与 [[skill渐进式披露]] 构成「按需披露」两种触发方式的比较素材；主 Skill 嵌套编排子 Skill 与 [[superpowers插件]] 互证。

## 开放问题

1. 三处图片内容外部补证（Skills 文件发现位置、4.1 对比表、4.2 全貌图）+ MCP 传输方式列表图。
2. `whenToUse` 字段的完整 frontmatter 规范。
3. `isMcpInstructionsDeltaEnabled()` 的默认状态与 rollout。
4. 泄漏源码仓库确切出处（文中链接与「泄漏源码」表述不对应）。
5. 姊妹篇《Claude Code 架构解析：从 Skill 调用到 Prompt Cache》获取，与 `prependUserContext()` 源码还原互证。
6. `CLAUDE.local.md` 的定位与典型用途细节。
