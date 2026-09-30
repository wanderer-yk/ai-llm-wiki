---
type: source
title: "800行代码实现OpenClaw的Tool消息总线子Agent管理架构"
tags: [agent框架, openclaw, 源码解析, 消息总线, 子agent, typescript, 极简实现]
related: [苏雄, 会员技术团队, 轻量级单进程agent框架, 薄抽象设计哲学, 有意取舍边界声明, 同步异步双路径, 互斥锁与暂存队列, 子agent单进程并发模型, 入站消息总线, 子agent工具排除机制, tool抽象四要素, exec-tool三层防护, 简化cron解析, llm并发状态感知, openclaw, pi-agent, anthropic, 大淘宝技术, 淘天集团, 极简agent设计哲学, agent-loop, agent-control-plane]
created: 2026-09-30
updated: 2026-09-30
authors: [苏雄]
year: 2026
url: "https://mp.weixin.qq.com/s?__biz=MzAxNDEwNjk5OQ==&mid=2650543287&idx=1&sn=a871de8246ffbb5733259e5434958404"
venue: "大淘宝技术（微信公众号）"
sources: ["[202604241629]800行代码实现OpenClaw的Tool消息总线子Agent管理架构.html"]
---
# 800行代码实现OpenClaw的Tool消息总线子Agent管理架构

## 文献信息

- **作者**：苏雄（文末团队介绍署名实名），单位全称「淘天集团-会员技术团队」；文章头部署名为「会员技术团队」
- **发布**：微信公众号「[[大淘宝技术]]」（别名 AlibabaMTT，biz id `MzAxNDEwNjk5OQ==`），2026-04-24 16:29，IP 属地浙江，原创标注，阿里系 spm 埋点 `ata.21736010`
- **类型**：源码解析 / 架构复刻实践文（HTML 导出件，13 个语义分块全部处理完毕，全文定稿）

## 定位与核心论点

本文是对 [[openclaw]] 的 Tool、消息总线、子 Agent 管理架构的**研究学习 + 最小可运行实现**（非 OpenClaw 原始代码；源文标题拼写为 "Open Claw"，沿 wiki 规范名 [[openclaw]]）。核心论点：对 Tool 调用、消息分发、子 Agent 管理三类核心组件，优先采用**薄抽象、显式控制流、贴近模型 API** 的实现，比多层中间件更易获得工程确定性，且利于后续扩展 Memory、调度、持久化。

技术底座：Anthropic Claude API + TypeScript + 单进程 Node.js；**不依赖 LangChain 等 Agent 框架**，直接基于 [[anthropic]] SDK（`@anthropic-ai/sdk`）。

## 框架范围

| 模块 | 说明 |
|------|------|
| Tool layer（工具系统） | 本文覆盖 |
| MessageBus（消息总线） | 本文覆盖 |
| SubagentManager（子 Agent 管理） | 本文覆盖 |
| REPL 主循环 | 本文覆盖 |
| 上层 Bot 接入层 | 明确不涉及 |
| 持久化 | 明确不涉及 |
| Context / Memory 系统 | 明确不涉及（见文末「表述张力」记录） |

## 模块一：Tool 抽象与 ToolRegistry

Tool 抽象类（四要素：`name` + `description` + `input_schema` + `execute`；`toSchema()` 无中间层直接输出 Anthropic function calling schema，`input_schema` 类型直接复用 SDK 的 `Tool` 类型）：

```typescript
export abstract class Tool {
  abstract readonly name: string;
  abstract readonly description: string;
  abstract readonly input_schema: AnthropicTool["input_schema"];
  abstract execute(args: Record<string, unknown>): Promise<unknown>;
  toSchema(): AnthropicTool {
    return {
      name: this.name,
      description: this.description,
      input_schema: this.input_schema,
    };
  }
}
```

ToolRegistry 完整实现（本质是一个 `Map<string, Tool>`；`exclude()` 为子 Agent 生成受限工具子集，返回**新**实例、不修改原注册表）：

```typescript
export class ToolRegistry {
  private tools = new Map<string, Tool>();
  register(tool: Tool) {
    this.tools.set(tool.name, tool);
  }
  async execute(name: string, args: Record<string, unknown>) {
    const tool = this.tools.get(name);
    if (!tool) throw new Error(`Tool "${name}" not found`);
    return tool.execute(args);
  }
  getToolDefinition(): AnthropicTool[] {
    return Array.from(this.tools.values()).map((tool) => tool.toSchema());
  }
  exclude(names: string[]): ToolRegistry {
    const excludeSet = new Set(names);
    const filtered = new ToolRegistry();
    for (const [name, tool] of this.tools) {
      if (!excludeSet.has(name)) {
        filtered.register(tool);
      }
    }
    return filtered;
  }
}
```

**取舍一（参数校验）**：schema 用运行时普通对象而非 Zod / JSON Schema 库——零依赖、直接对齐 SDK 类型，代价是无运行时校验，LLM 传入格式错误的参数只能在执行阶段暴露；作者判断「当前规模可接受」。

## 模块二：内置工具一览

内置工具按 文件操作 / 命令执行 / Web 能力 / 通信与调度 多类别组织，实际 ≥9 个，覆盖面显著宽于 [[pi-agent]] 的 4 个核心工具。

### 文件操作（4 个）

- **ReadFileTool**：读文件，动态 `import("node:fs/promises")` 加载模块
- **WriteFileTool**：写入前调用 `mkdir(dirname(path), { recursive: true })` 自动创建父目录
- **EditFileTool**：精确文本替换，强制唯一匹配（0 次报错、>1 次拒绝写入），防止 LLM 模糊替换目标导致意外修改多处代码：

```javascript
const occurrences = content.split(oldText).length - 1;
if (occurrences === 0) {
  return `Error: old_text not found in ${filePath}`;
}
if (occurrences > 1) {
  return `Warning: old_text found ${occurrences} times in ${filePath}. Please provide a more unique text snippet. No changes made.`;
}
const updated = content.replace(oldText, newText);
```

- **ListDirTool**：列出目录内容，每个条目做 `stat`，用 `[folder]`/`[file]` 前缀区分类型

### 命令执行：ExecTool 三层防护

第一层为危险命令正则黑名单（7 条正则）；第二层资源限制（默认 30 秒超时 + 2MB `maxBuffer`，超时 kill 进程并返回提示）；第三层输出截断（超 10,000 字符时取首尾各 5,000）：

```typescript
const DANGEROUS_PATTERNS: RegExp[] = [
  /rm\s+(-[a-zA-Z]*f[a-zA-Z]*\s+)?(-[a-zA-Z]*r[a-zA-Z]*\s+)?\/($|\s)/,
  /rm\s+-[a-zA-Z]*rf?\s+~($|\/|\s)/,
  /mkfs\b/,
  /dd\s+if=/,
  /:\(\)\s*\{\s*:\|:\s*&\s*\}\s*;/,  // fork bomb
  />\s*\/dev\/[sh]d[a-z]/,            // 写入裸设备
  /chmod\s+-R\s+777\s+\//,
];
```

```typescript
function truncateOutput(text: string): string {
  if (text.length <= MAX_OUTPUT_LENGTH) return text;
  const half = Math.floor(MAX_OUTPUT_LENGTH / 2);
  return (
    text.slice(0, half) +
    `\n\n--- truncated (${text.length} chars total) ---\n\n` +
    text.slice(-half)
  );
}
```

作者明示：正则黑名单是最低限度的防线，不能替代沙箱隔离；LLM 可通过变量展开、别名、管道组合绕过，生产环境应使用容器沙箱或受限用户执行。首尾保留（而非只取前 N 字符）的设计动机：「命令输出的末尾通常包含最有价值的信息（错误信息、统计摘要等）」。

### Web 能力（2 个）

- **WebSearchTool**：封装 Brave Search API，返回结构化结果；`count` 默认 5、上限 10
- **WebFetchTool**：纯正则 `htmlToText`，明确不用 cheerio/jsdom 等 DOM 解析库；常规网页够用，「对复杂嵌套结构可能丢失语义」；内容超 20,000 字符截断

```typescript
function htmlToText(html: string): string {
  return html
    .replace(/<script[\s\S]*?<\/script>/gi, "")
    .replace(/<style[\s\S]*?<\/style>/gi, "")
    .replace(/<(br|\/p|\/div|\/li|\/tr|\/h[1-6])[^>]*>/gi, "\n")
    .replace(/<[^>]+>/g, "")
    // HTML 实体解码.
    .replace(/[ \t]+/g, " ")
    .replace(/\n{3,}/g, "\n\n")
    .trim();
}
```

### 通信与调度（2 个）

- **MessageTool**：出站消息通道，构造时注入 `sendCallback`；REPL 下即 `console.log`，Bot 下替换为 Telegram/Discord 发送函数；「工具本身不关心消息最终去向」：

```typescript
export class MessageTool extends Tool {
  constructor(private sendCallback: SendCallback) {
    super();
  }
  async execute(args: Record<string, unknown>): Promise<string> {
    const content = args.content as string;
    const channel = (args.channel as string) ? "repl";
    const chatId = (args.chat_id as string) ? "default";
    await this.sendCallback({ channel, chatId, content });
    return `Message sent to ${channel}:${chatId}`;
  }
}
```

- **CronTool + CronService**：服务层/工具层分离；基于 `setInterval`，支持 `every_seconds`（直接转毫秒）与 `cron_expr`（解析为**近似间隔**）；CronTool 对外暴露 `add`/`list`/`remove` 三个 action。简化 cron 解析完整实现（「复杂表达式静默降级为每分钟执行一次，这是一个已知的精度妥协」）：

```typescript
private parseCronInterval(expr: string): number {
  const parts = expr.trim().split(/\s+/);
  if (parts.length !== 5) return 60_000;
  const [minute, hour] = parts;
  // */N * * * * → 每 N 分钟
  if (minute.startsWith("*/") && hour === "*") {
    const n = parseInt(minute.slice(2), 10);
    if (!isNaN(n) && n > 0) return n * 60_000;
  }
  // 0 */N * * * → 每 N 小时
  if (minute === "0" && hour?.startsWith("*/")) {
    const n = parseInt(hour.slice(2), 10);
    if (!isNaN(n) && n > 0) return n * 3600_000;
  }
  if (minute === "*" && hour === "*") return 60_000;     // 每分钟
  if (minute === "0" && hour === "*") return 3600_000;   // 每小时
  if (minute === "0" && hour === "0") return 86400_000;  // 每天
  return 60_000; // 复杂表达式降级为每分钟
}
```

## 模块三：MessageBus 入站消息总线

`InboundMessage` 四字段结构：

| 字段 | 含义 |
|------|------|
| `channel` | 消息通道 |
| `senderId` | 发送者标识 |
| `chatId` | 关联会话，格式 `originChannel:originChatId` |
| `content` | 消息内容 |

MessageBus 完整类实现（保留导出件原貌；`!add`/`?delete`/`[.this.queue]` 为疑似渲染损伤，应为 `!.add`/`?.delete`/`[...this.queue]`）：

```typescript
export class MessageBus {
  private listeners = new Map<string, Set<MessageHandler>>();
  private queue: InboundMessage[] = [];
  subscribe(channel: string, handler: MessageHandler): () => void {
    if (!this.listeners.has(channel)) {
      this.listeners.set(channel, new Set());
    }
    this.listeners.get(channel)!add(handler);
    return () => { this.listeners.get(channel)?delete(handler); };
  }
  async publish(message: InboundMessage): Promise<void> {
    const handlers = this.listeners.get(message.channel);
    if (handlers && handlers.size > 0) {
      for (const handler of handlers) {
        await handler(message);
      }
    } else {
      this.queue.push(message);
    }
  }
  drain(channel?: string): InboundMessage[] {
    if (!channel) {
      const msgs = [.this.queue];
      this.queue = [];
      return msgs;
    }
    const matched = this.queue.filter((m) => m.channel === channel);
    this.queue = this.queue.filter((m) => m.channel !== channel);
    return matched;
  }
}
```

关键机制：`subscribe` 实时回调（常驻服务场景）+ `drain` 轮询取空（同步消费场景）双消费模式；**单路径路由规则**——有订阅者走回调、无订阅者入队，消息绝不同时触发回调和入队。MessageTool（出站）与 MessageBus（入站）方向相反、无直接代码耦合，作者总结命名为「双通道消息机制」。

## 模块四：SubagentManager 后台子 Agent

**架构**：单进程并发模型——每个子 Agent 是一个 Promise，共享同一 Node.js 事件循环，无多进程、无 Worker。每个子 Agent 拥有独立 `AgentLoop` 实例与自己的 ReAct 循环，无历史上下文，单任务即弃。

**工具排除**（index.ts）：

```javascript
const subagentTools = tools.exclude(["spawn", "message", "edit_file", "cron"]);
```

逐项理由：`spawn` 防止子 Agent 递归创建子 Agent；`message` 防止子 Agent 直接向用户发消息（应经 MessageBus 回传主 Agent 处理）；`edit_file` 限制子 Agent 写入能力；`cron` 避免子 Agent 创建定时任务。

**生命周期**（导出件疑有渲染损伤：`...` 疑折叠为 `.`；label 赋值疑丢失 `??`）：

```javascript
spawn(params: { task: string; label?: string; ... }): string {
  const id = `subagent-${++this.counter}`;
  const label = params.label ? `Task ${this.counter}`;
  const promise = this.runSubagent(id, params.task, label, ...);
  promise.finally(() => {
    this.runningTasks.delete(id);
  });
  this.runningTasks.set(id, { id, label, promise });
  return id;
}
```

**无历史注入**（`buildMessages` 只传当前任务）：

```javascript
buildMessages: (_history, userMessage) => [
  { role: "user" as const, content: userMessage },
],
```

**完成回传**（成功/失败统一走同一回传路径，仅 content 不同）：

```javascript
await this.bus.publish({
  channel: "system",
  senderId: "subagent",
  chatId: `${originChannel}:${originChatId}`,
  content: `[Subagent "${label}" (${id}) completed]\n\n${result}`,
});
```

**迭代上限不对称**：子 Agent 最大迭代 15 次，主 Agent 10 次——暗示子 Agent 被预期承担更长的独立执行链。`promise.finally()` 负责从 `runningTasks` Map 中自动清理已完成任务。

## SpawnTool 接口

LLM 经 function calling 调用；入参 `task`（必填）+ `label`（可选）；返回值含子 Agent ID 与当前运行中子 Agent 数量——设计意图是「让 LLM 对并发状态有感知」。

## 模块五：REPL 入口与主循环

`index.ts` 六步启动流程：

1. 创建 Anthropic 客户端和 MessageBus 实例
2. 注册所有工具到 ToolRegistry，区分主 Agent 工具集和子 Agent 受限工具集
3. 初始化 CronService，触发回调通过 `bus.publish()` 写入 system channel
4. 创建 SubagentManager，注册 SpawnTool（最后注册，因为依赖 SubagentManager 实例）
5. 构建 ContextBuilder，加载 skills 和 memory
6. 创建 AgentLoop 实例，使用 `readline/promises` 启动交互循环

### 并发控制

问题：用户输入与子 Agent 回传双事件源都会触发 `agent.run()`，共享 `history` 数组不可并发修改。解决：`processing` 布尔互斥锁 + `pendingSubagentResults` 暂存队列：

```javascript
let processing = false;
const pendingSubagentResults: InboundMessage[] = [];
async function drainPendingResults(): Promise<void> {
  while (pendingSubagentResults.length > 0) {
    const msg = pendingSubagentResults.shift()!;
    const systemContent = [
      "[SYSTEM NOTIFICATION - Subagent Result]",
      msg.content,
      "Please summarize the above subagent result for the user.",
    ].join("\n\n");
    const reply = await agent.run(systemContent, history);
    history.push({ role: "user", content: systemContent });
    history.push({ role: "assistant", content: reply });
    console.log(`\nBot > ${reply}\n`);
  }
}
async function tryDrainPending(): Promise<void> {
  if (processing) return;
  processing = true;
  try {
    await drainPendingResults();
  } finally {
    processing = false;
  }
  rl.prompt();
}
```

用户输入处理（释放锁之前先 drain，「保证子 Agent 结果不会无限滞后」；首行注释 `// .` 为疑似第六处渲染损伤）：

```ts
rl.on("line", async (input) => {
  // .
  processing = true;
  try {
    const reply = await agent.run(trimmed, history);
    history.push({ role: "user", content: trimmed });
    history.push({ role: "assistant", content: reply });
    await drainPendingResults();
  } finally {
    processing = false;
  }
  rl.prompt();
});
```

### 消息订阅与 MessageTool 接线

订阅 handler 只做两件事：入队、尝试消费：

```js
bus.subscribe("system", (msg) => {
  pendingSubagentResults.push(msg);
  void tryDrainPending().catch(console.error);
});
```

REPL 场景 MessageTool 接线（`sendCallback` 直接输出终端）：

```js
tools.register(
  new MessageTool((msg) => {
    console.log(`\n[Message → ${msg.channel}:${msg.chatId}] ${msg.content}\n`);
  }),
);
```

Bot 场景迁移承诺：只需替换回调为 Telegram/Discord 等平台发送函数，**工具系统和 Agent 逻辑零改动**。

## 模块协作全景与双路径

```
终端 stdin
   │
   ▼
REPL 主循环 (index.ts)
   │  ┌──────────────────────── history (共享，互斥访问)
   │  │
   ▼  ▼
AgentLoop.run() ──→ Tool 调用
   │                  ├── 文件/命令/网络工具 → 直接返回结果
   │                  ├── SpawnTool → SubagentManager.spawn()
   │                  │                  └── 子 AgentLoop (独立 ReAct)
   │                  │                        └── bus.publish("system", 结果)
   │                  ├── MessageTool → sendCallback → stdout
   │                  └── CronTool → CronService
   │                                     └── setInterval → bus.publish("system", 触发通知)
   │
   │  ◄── bus.subscribe("system") ◄── pendingSubagentResults 队列
   │
   ▼
stdout 输出
```

| 数据流路径 | 链路 | 定性 |
|---|---|---|
| 同步路径 | 用户输入 → REPL → `agent.run()` → 工具调用 → 结果回传模型 → 最终回复 → stdout | 「标准的 ReAct 循环」 |
| 异步路径 | SpawnTool / CronService → `bus.publish("system")` → handler 入队 → `tryDrainPending()` → `agent.run()` | 内部消息经总线/队列消费，互斥锁保证与同步路径不冲突 |

## 设计选择与局限（7 项显式声明）

| 设计选择 | 内容 | 收益 | 代价/局限 |
|---|---|---|---|
| 零框架依赖 | 不依赖 LangChain 等 Agent 框架，直接基于 Anthropic SDK 构建 | 完全控制 API 交互细节，调试时不需要穿透框架抽象层 | 部分基础能力需要自己实现 |
| schema 定义方式 | 运行时对象而非 Zod / JSON Schema 库 | 降低复杂度和依赖数量 | 缺乏运行时校验，格式错误参数只能在执行阶段暴露 |
| 子 Agent 无持久记忆 | 每次 spawn 从零开始 | 适合一次性并行任务（搜索、分析、计算） | 不适合需要跨任务积累上下文的场景 |
| CronService 的 cron 表达式 | 近似实现，只支持常见的等间隔模式 | — | 复杂表达式静默降级为每分钟执行、不报错；精确语义需引入 cron 解析库 |
| MessageBus 无持久化 | 纯内存队列 | REPL 场景足够 | 进程重启后队列消息丢失；Bot 场景需接入持久化存储 |
| ExecTool 安全边界 | 正则黑名单只是最低防线 | — | LLM 可通过变量展开、别名、管道组合绕过；生产环境应使用容器沙箱或受限用户执行 |
| REPL 并发模型 | 布尔锁单用户场景足够；Node.js 单线程模型保证 `processing` 标志不会出现竞态 | — | 多用户（Bot 同时处理多会话）需每个会话独立 history 和更完整的队列/锁机制 |

## 总结：四部分核心设计与三条扩展路径

四部分核心设计：

1. **Tool 抽象 + Registry 模式** — 统一的工具注册和调用接口，通过 `exclude()` 实现能力隔离。
2. **双通道消息机制** — 出站走 `sendCallback`（MessageTool），入站走 MessageBus，方向明确，互不耦合。
3. **Promise 并发的子 Agent** — 共享事件循环，独立 ReAct 循环，通过 MessageBus 回传结果。
4. **互斥锁驱动的 REPL 主循环** — 布尔标志 + 暂存队列，保证 history 的一致性。

三条零侵入扩展路径：「扩展新工具只需继承 `Tool` 并注册。切换接入层（REPL → Bot）只需替换 `sendCallback` 和输入源。子 Agent 的能力边界通过 `exclude()` 控制。」

## 作者与团队归属

文末团队介绍：本文作者**苏雄**，来自**淘天集团-会员技术团队**；业务负责 88VIP、天猫积分、省钱卡、大会员、消费券等淘宝核心会员业务，支撑淘宝、千问、闪购等阿里业务的账号互联互通；技术方向为「深耕 AI 与业务融合，为消费者带来全新体验，为业务创造新增量」。

## 表述张力与遗留记录项

- **范围声明张力（存续，全文未获作者澄清）**：正文范围声明称「不涉及 Context/Memory 系统」，但 `index.ts` 启动流程第 5 步明确「构建 ContextBuilder，加载 skills 和 memory」。更可能的解释是 Context/Memory 属简化组件而非核心章节，作为张力记录。
- **遗留记录项**：`drainPendingResults`/`tryDrainPending` 函数体差异；ContextBuilder 内部实现无独立章节；`label` 参数具体用途；`originChannel`/`originChatId` 闭包来源；`AgentLoop` 完整定义未展示。
- **口径类**：「800 行」统计口径未说明；「Open Claw」与 [[openclaw]]/[[pi-agent]] 的层级关系未澄清；导出件累计六处代码渲染损伤（疑丢 `??`、`...`、`!.add` 等）待对照原仓库修复。

## 公众号元数据

profile 卡片属性（原样保留）：

```
data-alias: AlibabaMTT
data-nickname: 大淘宝技术
data-signature: 大淘宝技术官方账号
data-id: MzAxNDEwNjk5OQ==
data-origin_num: 911
data-verify_status: 2
data-biz_account_status: 0
```

「拓展阅读」披露的 6 大内容专辑（base: `https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzAxNDEwNjk5OQ==&action=getalbum&album_id=...`）：

| 专辑名 | album_id |
|--------|----------|
| 3DXR技术 | 2565944923443904512 |
| 终端技术 | 1533906991218294785 |
| 音视频技术 | 1592015847500414978 |
| 服务端技术 | 1539610690070642689 |
| 技术质量 | 2565883875634397185 |
| 数据算法 | 1522425612282494977 |

六专辑中无 AI 专属专辑，本文在账号内容矩阵中属跨栏目技术分享。

## Wiki 关联

- 与 [[pi-agent]]：本框架 ≥9 工具 + 子 Agent 体系 + skills/memory 加载 + 完整并发控制，复刻面显著更宽；与 [[极简agent设计哲学]]（279 行）同谱系但差异扩大
- 与 [[claude-code]]：子 Agent 工具受限/防递归 spawn 设计理念一致
- 与 [[agent-loop]] / [[react-agent]]：`AgentLoop.run()` 迭代上限 10/15；同步路径被文称「标准的 ReAct 循环」
- 与 [[文件轮询架构]]（[[24h打工人]]）：`bus.subscribe` + `pendingSubagentResults` + `tryDrainPending` 同属暂存-轮询消费范式
- 与 [[agent-control-plane]]：ExecTool 三层防护 + 工具排除 + 互斥锁串行化 + 「生产环境应用容器沙箱或受限用户」建议，构成轻量并发与安全边界完整证据链
- 与 [[skill功能聚合]] / [[上下文压缩策略]]：启动加载 skills/memory 是 OpenClaw 技能与记忆机制的最小化镜像（深度未展开）
- 与 [[ai工程量化效果声明追踪]]：「设计选择与局限」7 项显式局限声明属明示局限的强正向样本
