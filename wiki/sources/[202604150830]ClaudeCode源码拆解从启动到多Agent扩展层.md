---
type: source
title: "Claude Code 源码拆解：从启动到多 Agent 扩展层"
tags: [claude-code, agent-runtime, source-analysis, architecture, query-loop, tool-runtime, permission-system, multi-agent]
related: [无岳, claude-code, 千问AI平台, mcp, agent-loop, agent-control-plane, 三主干链路, 五条带走结论, 复杂度承接之问, 组织接入交付链路论, 动态能力面稳定内部对象, 任务对象化判据]
created: 2026-10-10
updated: 2026-10-10
authors: [无岳]
year: 2026
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247559548&idx=1&sn=e6a3f5d3b01eb32e14743130fb3a6d2a"
venue: "微信公众平台（千问AI平台，IP 属地浙江）"
sources: ["[202604150830]ClaudeCode源码拆解从启动到多Agent扩展层.html"]
---
# Claude Code 源码拆解：从启动到多 Agent 扩展层

## 基本信息

- **作者**：[[无岳]]（自述在内部从事 Agent 平台、研发工具链、Vibe Coding 产品工作）
- **发布渠道**：[[千问AI平台]] 公众号，IP 属地浙江，带「阿里妹导读」标签与 `ata.21736010` spm 锚点，提示阿里系开发者社区转载背景（正式关系未证实）
- **发布时间**：2026-04-15 08:30
- **分析对象**：[[claude-code]] 源码架构（启动 → REPL → Query Loop → Tool Runtime → Permission System → Task/多 Agent → 扩展层）
- **源文件状态**：9 chunk 全部到齐，文章完整闭环；chunk 1-4 为微信页面模板资产，正文自 chunk 5 起

## 核心命题

「把不稳定、概率化、容易失误的模型能力，装进一套稳定、可恢复、可解释、可扩展的工程运行时。」——决定 Agent 能否长期活下去的不是模型，而是围着模型搭起来的运行时。文章回答的问题是：为什么有些 Agent 系统一复杂就散架，而 Claude Code 没有。

## 章节结构与要点

**开篇立论**：Demo→生产的失稳点出现在三五个工具、多模式、多权限规则之后；运行时决定 Agent 存活。

**一、入口与启动链路**：[[启动三段式]]（入口分流→进程级初始化→会话级准备）；[[进程状态与交互状态分离]]——进程状态（cwd/projectRoot/sessionId/telemetry/token 成本计数）沉入 bootstrap/state 类全局状态，交互状态（tasks/MCP clients/plugin 状态/permission context/UI 选择状态）进 AppState；[[复杂度前置]]——执行边界在第一轮请求前定型。

**二、REPL / UI Orchestration**：[[repl运行时控制台]]——REPL 不是视图而是 runtime orchestrator；输入经五步前置（本地命令判断→上下文组装→[[能力面汇总]]→系统约束准备→才进 `query(.)`）；[[统一事件协议]]——消费六类结构化语义事件（assistant message、tool progress、compact boundary、pending permission、task notification、API error）；运行时编排复杂度应集中而非伪装成小组件。

**三、Query Loop / QueryEngine**：[[query双层结构]]（headless 会话外壳 + 状态机内核）；[[queryloop状态机]]——agent turn 是被压缩/恢复/工具回灌/预算/中断反复改写的「运行」，状态须升格为 runtime 对象；[[模型节点论]]；四类 runtime 机制：长上下文治理（snip/microcompact/collapse/autocompact）、失败恢复（reactive compact/max output recovery/fallback model）、[[工具结果协议化回灌]]、[[空隙期异步预取]]；归纳为[[运行时课题论]]与[[坏路径主路径设计]]。压缩机制清单与 [[autocompact水位线机制]]、[[三层上下文压缩]]、[[microcompact工具白名单]]、[[函数结果清理机制]] 构成跨源互证。

**四、Tool Runtime**：[[受控syscall工具观]]——工具是带完整运行时语义的厚对象（schema/校验/权限/并发安全/中断语义/结果回填）；[[工具四段式执行链]]；[[并发策略由工具语义决定]]（按 `isConcurrencySafe` 区分并发/串行）；[[流式工具执行建模]]；[[横切复杂度下沉]]——六项横切关注点（参数校验/权限检查/并发治理/进度上报/错误归一化/结果回填）收敛到统一 runtime；学习要点是[[工具有制度]]（务实起点：统一校验、授权、结果格式三件事）；[[spec与toolruntime分工]]——Spec 管目标、Tool Runtime 管行动。

**五、Permission System**：[[权限执行链论]]——权限是可解释执行链而非确认框；[[权限四层链路]]（规则层→运行时判定层→交互层→执行隔离层）；[[逻辑授权与执行隔离分离]]；[[权限决策对象化]]（PermissionDecision 使权限从 boolean 变为「为什么过、卡在哪、下一步怎么处理、能不能先自动判一轮」）；[[权限拒绝协议内包装]]；[[自动模式危险能力裁剪]]；[[沙箱即权限落地点]]。

**六、Task / 多 Agent / 后台执行**：[[多agent即多执行体论]]；[[统一任务抽象先行]]——主会话后台化/本地 subagent/in-process teammate/remote agent 全部映射进同一任务语义；[[子agent即任务对象]]；[[前后台差异在调度]]；[[异步上下文隔离]]；[[任务结果回流]]；[[任务生命周期治理]]（retain/diskLoaded/evictAfter）；[[任务对象化判据]]；[[执行收回论]]；[[多agent系统工程论]]；[[spec与任务系统分工]]。

**七、MCP / Skills / Plugins 扩展层**：核心命题「外部可以热闹，内部必须收敛」——[[动态能力面稳定内部对象]]；[[mcp翻译收敛]]（四映射 + auth tool 注入）；[[skill能力声明对象]]（SkillDescriptor）；[[plugin能力组合包]]（加载层是小型平台分发问题）。

**八、总结与总架构**：七层职责清单（启动层定边界 / REPL 接人机 / Query Loop 连续运行 / Tool Runtime 行动制度化 / Permission 打通允许与隔离 / Task Runtime 统一长时执行与多 Agent 生命周期 / 扩展层让能力增长不失控）；[[三主干链路]]总架构；[[复杂度安放位置论]]；[[每种复杂度只在一个地方爆炸]]；[[运行泥球反模式]]；8 行穿透清单。

**九、带走结论与结语**：[[五条带走结论]]（五大章自我蒸馏）；[[组织接入交付链路论]]（SDD 给目标和边界、运行时给运行和落地）；终极之问[[复杂度承接之问]]。

## 源码骨架全集（verbatim 保真）

### Query Loop 状态骨架（第三章）

```
state = {
  messages,
  toolUseContext,
  maxOutputTokensOverride,
  autoCompactTracking,
  maxOutputTokensRecoveryCount,
  hasAttemptedReactiveCompact,
  turnCount,
  pendingToolUseSummary,
  transition,
}
```

### 主循环最小骨架（第三章）

```typescript
while (true) {
  prefetchMemoryAndSkills()
  messagesForQuery = applyBudget(messages)
  messagesForQuery = snipAndCompact(messagesForQuery)
  assistant = streamModel(messagesForQuery)
  if (!assistant.hasToolUse) return finishTurn(assistant)
  toolResult = runToolUse(assistant.toolUse, toolUseContext)
  state.messages = writeBack(messages, assistant, toolResult)
}
```

### 控制权转移缩略链（第三章）

```
messagesForQuery = getMessagesAfterCompactBoundary(messages)
assistantMessages = streamModel(normalize(messagesForQuery))
toolUseBlocks = collectToolUses(assistantMessages)
toolUpdates = runTools(toolUseBlocks, assistantMessages, canUseTool, toolUseContext)
toolResults = normalizeToolResults(toolUpdates)
state.messages = [.messages, .assistantMessages, .toolResults]
continue
```

### 工具抽象对比（第四章）

普通团队的工具抽象：

```typescript
type Tool = (input: unknown) => Promise<string>
```

Claude Code 的工具抽象：

```typescript
interface Tool {
  name: string
  inputSchema: Schema
  canRunInParallel: boolean
  validate(input): ValidationResult
  execute(input, context): AsyncIterable<ToolEvent>
  toModelResult(output): StructuredResult
}
```

### runToolUse 执行链（第四章）

```typescript
async function* runToolUse(toolUse, assistantMessage, canUseTool, ctx) {
  const tool = findToolByName(ctx.options.tools, toolUse.name) ? findAlias(toolUse.name)
  if (!tool) return toolResultError(toolUse.id, 'No such tool available')
  if (ctx.abortController.signal.aborted) return cancelled(toolUse.id)
  yield* streamedCheckPermissionsAndCallTool(
    tool,
    toolUse.id,
    toolUse.input,
    ctx,
    canUseTool,
  )
}
```

### ToolUseContext 上下文骨架（第四章）

```typescript
type ToolUseContext = {
  options: { tools; commands; mcpClients; refreshTools? }
  abortController: AbortController
  messages: Message[]
  setAppState(.)
  setInProgressToolUseIDs(.)
  setResponseLength(.)
}
```

### 权限决策链：tool_use → permission → execution（第四章）

```typescript
parsedInput = tool.inputSchema.safeParse(input)
validatedInput = tool.validateInput?(parsedInput.data)
hookResult = runPreToolUseHooks(tool, validatedInput)
permissionDecision = resolveHookPermissionDecision(
  hookResult,
  tool,
  validatedInput,
  canUseTool,
)
if (permissionDecision.behavior !== 'allow') return rejectAsToolResult()
result = tool.call(callInput, toolUseContext, canUseTool, assistantMessage)
mapped = tool.mapToolResultToToolResultBlockParam(result.data, toolUseID)
return createUserMessage({ content: [mapped], sourceToolAssistantUUID })
```

### PermissionDecision 权限决策骨架（第五章）

```typescript
type PermissionDecision =
  | { behavior: 'allow'; updatedInput?; decisionReason? }
  | {
      behavior: 'ask'
      message: string
      suggestions?: PermissionUpdate[]
      blockedPath?: string
      pendingClassifierCheck?: PendingClassifierCheck
    }
  | { behavior: 'deny'; message: string; decisionReason: string }
```

### LocalAgentTaskState 本地子 Agent 任务状态骨架（第六章）

```typescript
type LocalAgentTaskState = {
  agentId: string
  prompt: string
  progress?: AgentProgress
  error?: string
  result?: AgentToolResult
  messages?: Message[]
  isBackgrounded: boolean
  pendingMessages: string[]
  retain: boolean
  diskLoaded: boolean
  evictAfter?: number
}
```

### spawnAgent 反模式与 Task 接口对照（第六章）

```typescript
spawnAgent(prompt): Promise<string>
```

```typescript
interface Task {
  id: string
  status: 'pending' | 'running' | 'blocked' | 'done' | 'failed'
  progress: ProgressState
  output: StructuredOutput[]
  notifications: Notification[]
  cancel(): void
  resume(): void
}
```

### SkillDescriptor 能力声明骨架（第七章）

```typescript
type SkillDescriptor = {
  description: string
  allowedTools: string[]
  whenToUse?: string
  model?: Model
  effort?: Effort
  hooks?: Hooks
  executionContext?: 'fork'
  agent?: string
}
```

### MCP 翻译收敛映射（第七章，正文叙述）

| MCP 原生对象 | Claude Code 内部对象 |
|---|---|
| MCP prompt | Command |
| MCP tool | Tool |
| MCP resource | 资源/资源工具体系 |
| （鉴权需要时） | 注入 auth tool |

### 三主干链路（第八章）

| 链路 | 回答的问题 | 覆盖层 |
|---|---|---|
| 控制链 | 这一轮在什么制度下运行；怎么想、怎么续跑 | 启动层定边界 → REPL 汇总能力面/会话状态 → Query Loop 连续运行 |
| 执行链 | 模型决定行动后如何稳落真实世界；怎么动、怎么受约束 | Tool Runtime → Permission → sandbox → 文件/命令/网络等外部副作用 |
| 任务链 | 持续/后台/分身运行如何不乱；怎么并发、怎么持续、怎么回流 | Task Runtime（多 Agent 置于此链，非 Query Loop） |

### 一次请求穿透七层的 8 行清单（第八章，verbatim）

1. 启动层先决定这次会话处在什么边界里运行
2. REPL 把当前输入、能力面、权限状态、任务状态打包成一个 turn
3. Query Loop 接手，开始做预算、压缩、预取和流式推理
4. 模型一旦发出 `tool_use`，控制权切到 Tool Runtime
5. Tool Runtime 先走 Permission Decision，再决定能不能真正执行
6. 执行过程中如果拉起子执行体，就进入 Task Runtime 管生命周期和回流
7. 扩展层把 MCP、skills、plugins 翻译成统一内部对象，供上面几层消费
8. 最终所有结果再回到 Query Loop 和 REPL，成为下一轮上下文和用户可见状态

### 五条带走结论（第九章，verbatim）

1. 先定义执行边界，再发起第一轮推理。
2. 当 Agent 进入连续运行阶段，query loop 就必须升级成 runtime。
3. 工具一旦开始碰副作用，工具层就必须制度化。
4. 权限系统的核心不是确认框，而是可解释的执行链。
5. 多 Agent 的前提不是 prompt 分工，而是统一任务抽象。

## 保真度说明

- 所拆源码版本/repo 出处全文未披露，「从源码看」类声明不可独立复核
- `runToolUse` 首行三元表达式缺 else 分支，疑排版/截断失真
- 正文称并发字段为 `isConcurrencySafe`，示例 Tool 接口写作 `canRunInParallel`，命名不一致待核
- PermissionDecision 骨架中 `updatedInput?` 未标类型
- Task 接口代码块在原文被误标 `data-lang="cs"`、spawnAgent 签名缺参数类型标注——更接近作者示意骨架而非严格源码
- 四张截图（权限系统、扩展层、总架构、三链）不可机读，内容待人工核读
- 文末无技术附注（仅微信互动模板）

## 开放问题

- 源码版本/repo 出处；Task/SkillDescriptor 骨架是否为真实源码节选
- 作者[[无岳]]与阿里系的正式关系（阿里妹导读 + ata spm 锚点 + 浙江属地仅为间接证据）
- 四张截图内容待人工核读；若发现与 [[claude-code]] 页「纯 grep、Token 消耗多、上下文冗余」记载相左的检索策略证据，按矛盾流程建 query 页
- 比较页统一评估（四个候选，见 REVIEW）

## 跨源关联

- `SkillDescriptor.executionContext?: 'fork'` 与 [[fork-sub-agent机制]] 直接同构；任务章与 [[SubAgent生命周期工具化]]、[[受控子Agent机制]] 互证
- [[mcp翻译收敛]] 与 [[skill-command-mcp三层架构]] 互为镜像（消费侧收敛 vs 供给侧分层），回链 [[mcp]]
- [[组织接入交付链路论]] 与 [[agent生产落地环境重构论]]（改造环境而非提升模型）高度同构，与 [[agent-control-plane]] 互补；与 [[规格驱动ai开发]]、[[vibe-coding]] 明确汇合
- [[skill能力声明对象]] 与 [[生产级skill]]、[[skill知识三类架构]]、[[skill功能聚合]] 互证 skill 即结构化声明
- [[运行泥球反模式]] 是 [[agent-loop]] 的对照面（loop 本身没错，错在把一切塞进单一循环）
