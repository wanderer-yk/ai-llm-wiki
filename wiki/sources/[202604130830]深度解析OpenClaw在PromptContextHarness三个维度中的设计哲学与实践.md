---
type: source
title: "深度解析 OpenClaw 在 Prompt / Context / Harness 三个维度中的设计哲学与实践"
tags: [openclaw, prompt-engineering, context-engineering, harness-engineering, 源码拆解, agent架构]
related: [openclaw, 飞樰, 千问AI平台, prompt-context-harness三阶段, openclaw系统提示词23模块, promptmode三级模式, openclaw工作区md文件族, text大于brain落盘原则, 心跳机制heartbeat, heartbeat-vs-cron选型, prompt极简主义, context-engineering三支柱, context-window三段构成, compaction双触发模式, 自适应分块压缩, 摘要分层降级策略, 工具结果头尾修剪, kv-cache时间窗优化, openclaw双层记忆系统, 记忆时间衰减, openclaw三层沙箱纵深防御, harness与workflow主导权之辨, agent裸奔四问题, harness四强制约束, 全生命周期hook机制, 上下文压缩策略, 记忆写入双路径, memory-flush机制, 混合召回, markdown多层记忆体系, Evergreen免衰减, human-in-the-loop, agent-control-plane, 验证门禁化, 多层安全护栏, workflow与agent控制权分界, harness-engineering, claude-code, pi-agent]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践.html"]
authors: [飞樰]
year: 2026
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247559511&idx=1&sn=64e933b0264e47f0940e693e315e0c82"
venue: "千问AI平台（微信公众号）"
---

# 深度解析 OpenClaw 在 Prompt / Context / Harness 三个维度中的设计哲学与实践

## 文献信息

- **作者**：[[飞樰]]（个人原创署名，文末自陈"一家之言"）
- **发布平台**：[[千问AI平台]]（阿里系 AI 技术公众号，"阿里妹导读"编辑体例，IP 属地浙江，带原创标记与个人观点免责声明）
- **发布时间**：2026-04-13 08:30
- **系列定位**：飞樰 "Prompt / Context / Harness" 源码拆解系列第三篇。前两篇：Claude Code（[[sources/[202604200830]深度解析ClaudeCode在PromptContextHarness的设计与实践|[202604200830]]]）与 HermesAgent（[[sources/[202604240830]深度解析HermesAgent如何实现自进化及其PromptContextHarness的设计实践|[202604240830]]]）
- **分析对象**：[[openclaw]]（GitHub 开源库，分析截止 2026-03 底架构形态）
- **完备性**：全文 9/9 chunk 完结，References [1]-[5] URL 全部兑现。本页是 wiki 中 OpenClaw Prompt / Context / Harness 设计细节最完整的一手证据来源。

## 文章定位与核心论点

1. **集成升华论**：OpenClaw 并非凭空诞生的全新物种，而是对近年 Agent 关键技术（动态 Prompt 组装、Context 压缩、Memory 管理、Skills 模块化复用与渐进式披露、Hook 机制、Guardrail、工具调用、Computer Use）的系统性集成与升华；类比 2018 BERT、2022 底 ChatGPT、2025 初 DeepSeek 的"量变引起质变"（技术集大成质变论）。
2. **三阶段递进框架**：Prompt Engineering → Context Engineering → Harness Engineering 是现代 AI 系统的三大关键阶段，分别聚焦"如何说"→"让 AI 看什么"→"构建怎样的运行环境"，详见 [[prompt-context-harness三阶段]]。
3. **三阶段精炼定位**：Prompt Engineering 告诉模型 **What & How**，Context Engineering 让模型 **How Better**，Harness Engineering 确保 **How Controlled**。
4. **形态不可复刻论**：OpenClaw 形态不能直接复刻到生产，尤其 to-B 场景受时效性要求、数据安全红线与可控性标准约束；其真正价值在设计哲学（System Prompt 模块化组装、Skills+压缩+分层记忆、执行校验约束）。
5. **开源学习范本论**：相较 [[claude-code]]、Codex 等闭源产品只能黑盒推演，OpenClaw 完全开源可深入源码理解每个设计决策；核心命题是"如何构建一套优秀的架构体系，让通用的基座模型能够稳定、高效、可控地完成复杂的、长程的任务"。OpenClaw 是当今 AI Agent 领域一次重要技术里程碑。

## 一、Prompt Engineering：动态组装与文件驱动

### System Prompt 动态组装与 PromptMode

System Prompt 由 `src/agents/system-prompt.ts` 中的 `buildAgentSystemPrompt()` 函数运行时动态拼装，接收几十个参数、按固定顺序组合约 23 个模块（详见 [[openclaw系统提示词23模块]]），并定义三种 `PromptMode`（详见 [[promptmode三级模式]]）：

| 模式 | 适用场景 | 加载范围 |
|------|---------|---------|
| full（完整模式） | 主 Agent 与用户直接对话 | 所有模块全部加载 |
| minimal（精简模式） | 子 Agent 执行独立任务 | 仅核心模块（工具、工作区、运行时信息） |
| none（极简模式） | 极简场景 | 基本只有一行身份标识 |

### Markdown 文件驱动的注入机制

选 Markdown 的三个理由：文件系统操作方便、格式表达力强、grep/Shell 可管理。工作区 `.md` 文件族（AGENTS/SOUL/IDENTITY/USER/TOOLS/HEARTBEAT/BOOTSTRAP/BOOT/MEMORY + `memory/YYYY-MM-DD.md`）通过模块 15 全文注入，与 SKILL.md 的渐进式披露形成"直接注入 vs 按需加载"双轨，详见 [[openclaw工作区md文件族]]。

AGENTS.md 中的关键原则：Session Startup 顺序、Memory 分层规则（[[text大于brain落盘原则]]："Mental notes don't survive session restarts. Files do."）、MEMORY.md 主会话安全隔离（群聊模式不加载，防个人上下文泄露）、`trash` > `rm` 红线、External vs Internal 行动分级（内部动作自主、外部动作先问）、群聊参与纪律（"Quality > quantity"、避免 triple-tap、Participate don't dominate）。

### 心跳机制

定期唤醒 Agent 主动执行定时任务（查邮件/日历/提及/天气，每日 2-4 次轮询），无事回复 `HEARTBEAT_OK`；HEARTBEAT.md 为空可跳过心跳 API 调用（省 token 设计）；心跳期还承担记忆维护——"Daily files are raw notes; MEMORY.md is curated wisdom"（详见 [[心跳机制heartbeat]] 与 [[heartbeat-vs-cron选型]]）。

### Prompt 极简主义

"质量大于数量"：`Quality > quantity` 一句替代冗长群聊规范，极简指令为业务数据腾出 Context Window 额度；"优秀的 Prompt 不是写得越长越好，而是越清晰、越模块化、越节省资源越好"（详见 [[prompt极简主义]]）。

## 二、Context Engineering：三支柱

动机：上下文窗口爆炸与 Lost in the Middle——一味堆砌 prompt/历史/工具结果导致推理耗时+成本飙升+注意力稀释。三支柱框架详见 [[context-engineering三支柱]]。

### 支柱一：可扩展 Skills 机制

Agent Skills 核心理念源自 Anthropic，具备可复用性与渐进式披露（Progressive Disclosure）；任务需要时才注入 Skill 名称与描述，实现"能力近乎无限 + 日常上下文轻量"（与 [[渐进式披露替代向量检索]]、[[skill轻量索引按需加载]] 三方互证）。AGENTS.md 硬约束："never read more than one skill up front"。Skills 双刃剑：可执行脚本包带来病毒/后门/WebShell 供应链攻击风险，OpenClaw 近期强化 ClawHub 来源管控、鉴权与未知 Skills 识别。

### 支柱二：动态 Compaction 与 Pruning

Context Window 三段构成（详见 [[context-window三段构成]]），History 是唯一可大幅节省的空间。压缩（Compaction）双触发（详见 [[compaction双触发模式]]）；`src/agents/compaction.ts` 揭示自适应分块（详见 [[自适应分块压缩]]）与三层摘要降级（详见 [[摘要分层降级策略]]）；修剪（Pruning）见 [[工具结果头尾修剪]]；另有 [[kv-cache时间窗优化]]。

分块逻辑（`src/agents/compaction.ts`，原文）：

```
关键常量：
  BASE_CHUNK_RATIO = 0.4      （基础分块比率：每块占上下文的40%）
  MIN_CHUNK_RATIO  = 0.15     （最小分块比率：每块至少占15%）
  SAFETY_MARGIN    = 1.2      （20% 安全缓冲）
  SUMMARIZATION_OVERHEAD_TOKENS = 4,096  （为Summary指令和推理预留的token）

工作原理：
  1. 计算所有需要压缩的消息的总token数
  2. 根据平均消息大小动态调整chunk比率（小消息多 → 每个chunk可以装更多消息）
  3. 按token数量等比分割消息为多个部分
  4. 每块加上20%安全缓冲
```

摘要分层设计（原文）：

```
顶层策略: summarizeInStages()
  ├── 判断消息量小/token少？ → 直接走 兜底方案 summarizeWithFallback()
  └── 否则，按token比例分割 → 各块summarizeChunks()  → 合并summary → 最终summary

分块策略: summarizeChunks()
  ├── 处理单个消息块
  ├── 支持最多3次重试
  └── 每个chunk生成summary结束后，合并最终Summary

兜底策略: summarizeWithFallback()
  ├── 先尝试完整Summary
  ├── 如果失败 → 排除过大的消息后再试
  └── 如果还失败 → 返回默认文本 "No prior history."
```

压缩 vs 修剪四维对比（原文）：

| 特性 | 压缩（Compaction） | 修剪（Pruning） |
|------|------|------|
| 核心操作 | 生成Summary替换旧消息 | 直接删减部分工具或会话结果 |
| 信息保留 | 摘要保留关键信息 | 信息直接丢失 |
| 成本 | 需要调用LLM来生成摘要 | 规则修剪，低成本 |
| 使用场景 | 对话历史记录太长 | 工具结果占用太大或会话太多 |

其他关键参数：压缩超时 `EMBEDDED_COMPACTION_TIMEOUT_MS = 300000`（5 分钟）、会话写锁、`identifierPolicy: "strict"`、压缩模型可经 `openclaw.json` → `agents.defaults.compaction.model` 配置为更便宜模型。

### 支柱三：分层 Memory

LLM 被作者戏称"高度定时失忆患者"，需"小本本"辅助回忆。OpenClaw 双层记忆系统（详见 [[openclaw双层记忆系统]]）：

- 引擎代码 `src/memory/manager.ts`、工具代码 `src/agents/tools/memory-tool.ts`
- 长期记忆 `MEMORY.md`：每次对话自动注入 System Prompt，截断至 200 行，须精简、重要信息前置
- 每日记忆 `memory/日期.md`：低频细节，防核心过载
- 写入双策略：显式写入（"请记住…"）+ 隐式闪存（Memory Flush，会话结束/新 Session/触发压缩时自动提炼归档）——与 [[记忆写入双路径]]、[[memory-flush机制]] 直接互证
- 召回：每日记忆切片+向量化+SQLite 索引；被动注入（BM25+向量双路，即 [[混合召回]]）、`memory search` 主动搜索、按行号深层钻取三级入口
- 遗忘：无自动删除需人工清理；MEMORY.md 永不衰减；每日笔记按时间衰减（详见 [[记忆时间衰减]]）

时间衰减公式（原文）：

```
时间衰减公式：

  衰减系数 = e^(-λ × 天数)

  其中 λ = ln(2) / 半衰期天数（默认 30 天）
```

衰减数值示例（原文）：

```
1天前的记忆：衰减系数 ≈ 0.977（几乎不变）
7天前的记忆：衰减系数 ≈ 0.851（轻微降低）
30天前的记忆：衰减系数 = 0.500（减半）
60天前的记忆：衰减系数 = 0.250（只剩1/4）
90天前的记忆：衰减系数 = 0.125（只剩1/8）
```

双层记忆六维对比（原文）：

| 特性 | MEMORY.md（长期记忆） | memory/日期.md（每日笔记） |
|------|----------------------|---------------------------|
| 文件数量 | 只有一个 | 每天一个 |
| 写入方式 | 整理后写入（覆盖或编辑） | 追加写入（append） |
| 内容类型 | 持久的事实和偏好 | 每日的上下文笔记 |
| 注入方式 | 每次对话都注入到系统提示词 | 只通过搜索访问 |
| 时间衰减 | 不衰减（"保持常青"的内容） | 随时间衰减 |
| 适合记什么 | 比较重要的项目名称 | 今天讨论了API重构问题 |

Context Engineering 总结："像图书管理员，懂得何时把书放进仓库（压缩/记忆），何时迅速抽出递给你（检索/注入）"——在有限窗口内实现无限知识扩展、高效对话管理和持久记忆保持。

## 三、Harness Engineering：Hook、沙箱与人工接管

### 术语溯源与定义

2025-11 Anthropic 博文提 "Harnesses"，2026-02 OpenAI 文章出现 "Harness Engineering"，中文译名不统一（驾驭工程/马具工程/脚手架工程）。Harness 定义：在大模型之外构建外部运行环境与约束机制，通过接口（Interface）、钩子（Hooks）、护栏（Guardrails）约束、引导、检验、评估 Agent 行为，使其可靠完成复杂长周期任务。注意与爱奇艺 [[harness-engineering]]（具体五要素方法论）是"通用概念 vs 具体方法论"的层次关系。

### 裸奔四问题与四强制约束

无 Harness 时 Agent"裸奔"四大问题：过早终止/缺乏反思/死循环陷阱/高风险场景（详见 [[agent裸奔四问题]]）。软件开发四强制约束：分步执行→强制测试→闭环修复→最终验收（详见 [[harness四强制约束]]），"带着镣铐跳舞"换取确定性/健壮性/成功率。

### Harness vs Workflow 主导权之辨

Workflow = 硬编码固定路径、主导权在人；Harness = 动态软约束、保留 Planning 与 Looping、主导权在 AI。基础大模型越强，Workflow 弊端越大（详见 [[harness与workflow主导权之辨]]，与 [[workflow与agent控制权分界]] 呼应）。

### OpenClaw 全生命周期 7 Hook（完整清单）

| 钩子名称 | 触发时机 | 典型用途 |
|----------|----------|----------|
| `before_prompt_build` | 构建提示词之前 | 注入额外上下文 |
| `before_tool_call` | 执行工具之前 | 拦截或修改工具参数 |
| `after_tool_call` | 工具执行之后 | 处理工具结果 |
| `before_compaction` | 上下文压缩之前 | 观察或标注压缩过程 |
| `after_compaction` | 上下文压缩之后 | 后处理 |
| `message_received` | 收到消息时 | 消息预处理 |
| `message_sending` | 发送消息前 | 消息后处理 |

Hook 系统贯穿全生命周期，实现"事前预防"和"事后纠偏"，是 [[全生命周期hook机制]] 的 OpenClaw 侧完整清单（与 HermesAgent 文构成同术语跨框架双源证据）。两个实战场景：

1. **参数校验拦截**：大模型易混淆阿里云各产品实例 ID 格式（ECS 以 `i-` 开头；轻量应用服务器为 32 位数字/字母组合）。无 Harness 时报错→中断或错误循环；有 Harness 时 `before_tool_call` Hook 脚本正则拦截、返回"参数错误"迫使模型重新发起 tool_call；一次配置对所有工具生效，大幅提升调用成功率。
2. **强制测试器**：AI Coding 场景经 Hook 配置——代码生成后自动触发语法检查/单元测试，Bug 立即反馈日志要求修复，直到测试通过才允许交付（"写完即止"→"写完必测"），与 vivo [[验证门禁化]] 同构。

### 三层沙箱纵深防御与四防

文件系统沙箱（Workspace 目录禁锢）→ 命令执行沙箱（Security 白名单 / Ask 人工确认 / safeBins 只读豁免）→ 网络访问沙箱（域名白名单 + 防泄露），三层独立互补；底层操作系统最小权限兜底（运行时安全解耦为独立进程插件与可选编排服务）。四防：防注入攻击/防越权调用/防敏感泄露/防恶意篡改。核心思想：不依赖模型自我约束，而以系统级强制力（"电子围栏"）保障安全。详见 [[openclaw三层沙箱纵深防御]]，与 [[多层安全护栏]]（HermesAgent）对照，是 [[agent-control-plane]] 的最强具象证据之一。

### 规定动作与 Human-in-the-Loop

HEARTBEAT.md（心跳强制巡检）与 BOOTSTRAP.md（启动身份确认/环境检查）是 Harness 强加的"规定动作"，非模型自发行为。Human-in-the-Loop 是 Harness 赋予人类对 Agent 的最终控制权：遇不确定情况/高风险操作时暂停等待人类指令，"随时可接管"是避免失控的约束手段；[[claude-code]] 同类（几乎每一个"写类型"的操作都必经人工确认，直接互证）。

### 演进评价（注意版本时效）

Harness Engineering 2026 年 2 月才被业界广泛关注。OpenClaw 早期版本细粒度 Harness 约束单薄、更多依赖模型"自觉"，不及 OpenAI/Anthropic 的专门优化；最近更新明显加强（ClawHub Skills 鉴权等强管控），未来将引入更多细粒度约束策略。**引用 OpenClaw Harness 能力时须注意版本时效**（分析截止 2026-03 底；"前几天的更新"曾致大量"龙虾"实例崩溃）。马具隐喻收束："不要指望一匹野马能自己认路，也不要指望它能自己避开前方的坑"——需设计精密"马具"与"赛道"。

## 四、与既有 Wiki 的关系

- **同对象互补**：[[sources/[202604082000]从OpenClaw看Agent架构设计|[202604082000] 从 OpenClaw 看 Agent 架构设计]]（vivo，架构四决策）——本文的 `summarizeInStages()` 为 vivo 文"分阶段压缩"描述提供代码级佐证；[[pi-agent]] 的 4 工具极简设计为 yabohe 文所述工业验证。
- **同系统互证（最强 comparison 候选）**：[[sources/[202604151800]OpenClaw长期记忆优秀管线与玄学效果|[202604151800] OpenClaw 长期记忆优秀管线与玄学效果]]——写入双路径/Memory Flush/混合召回/时间衰减全互证，本文"保持常青"与 [[Evergreen免衰减]] 字面级确认同一体系，与 [[markdown多层记忆体系]] 互证。
- **跨框架对照**：Claude Code（[[autocompact水位线机制]]、[[microcompact工具白名单]]、[[函数结果清理机制]]、[[九段式结构化摘要模板]]、[[system-prompt动态组装机制]]）；HermesAgent（[[全生命周期hook机制]]、[[多层安全护栏]]）。
- **落地视角对话**：本文"形态不可复刻论"与 [[sources/[202604152000]OpenClaw落地到生产实际应用的一种可能的路径|[202604152000] OpenClaw 落地到生产实际应用的一种可能的路径]]（vivo 丁俊杰，环境重构论）形成 OpenClaw 生产化的两种警示路径。
- **既有概念获一手证据**（更新而非另设页）：[[prompt-context-harness三阶段]]、[[上下文压缩策略]]、[[human-in-the-loop]]（"最终控制权"新语境，与狼人杀玩家加入语境跨域同名需消歧）等。

## 参考资料（原文）

```
[1] OpenClaw Github库：https://github.com/openclaw/openclaw
[2] DEV Community：https://dev.to/ljhao/prompt-engineering-vs-context-engineering-vs-harness-engineering-whats-the-difference-in-2026-37pb
[3] ClawHub 官方库：https://clawhub.ai/
[4] Anthropic：https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents
[5] OpenAI：https://openai.com/zh-Hans-CN/index/harness-engineering/
```

## 备查（源文内部小瑕疵与一致性说明）

- 正文小标题写 "AGENT.md（总纲）"，而代码与模块 15 均为 "AGENTS.md"，疑为源文笔误，引用时以 AGENTS.md 为准。
- 每日笔记"只通过搜索访问"（六维对比表）与 BM25+向量双路被动注入可兼容——混合召回是"搜索访问"的实现路径，表格是对注入模型的精化而非否定。
- 作者引用自己的旧文《Agent / Skills / Teams架构演进过程及技术选型之道》"Agent Skills：可复用与渐进式的能力披露"一节（元数据待溯源）；引用 [2]（DEV Community, ljhao）标题即三工程 2026 差异对比，疑为本文三段式框架来源线索。
