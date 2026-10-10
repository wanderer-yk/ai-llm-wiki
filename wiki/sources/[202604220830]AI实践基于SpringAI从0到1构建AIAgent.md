---
type: source
title: "AI实践｜基于 Spring AI 从0到1构建 AI Agent"
tags: [spring-ai, ai-agent, function-calling, rag, mcp, subagent, java]
related: [觖弦, spring-ai, aiagentdemo, 千问AI平台, GLM-4, mcp, harness工程三部曲演进论, LLM问答黑箱论, function-calling大地基论, 上下文窗口四要素, 工具定义最终论, 三层上下文压缩, SubAgent记忆隔离, SubAgent生命周期工具化, RRF排名融合算法, 查询改写召回路, 意图识别前置门控, 可插拔工具注册机制, 两类八种文档分块策略, MCP运行时动态管理, MCP持久化与自动恢复, MCP双规范版本锚定, RAG工具化双路径, RAG能力MCP服务化, command-vs-skill]
authors: [觖弦]
year: 2026
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247559646&idx=1&sn=1abc5788cfe44b9820e5a0f4dfb3a336&chksm=e89581cb06e371e696a6d0389b8feaaab1bdc9d1ee0e405b0b6ceabd989d10bfbd271b737b41#rd"
venue: "千问AI平台（微信公众号）"
sources: ["[202604220830]AI实践基于SpringAI从0到1构建AIAgent.html"]
created: 2026-10-10
updated: 2026-10-10
---
# AI实践｜基于 Spring AI 从0到1构建 AI Agent

《AI实践｜基于 Spring AI 从0到1构建 AI Agent》是[[觖弦]]发表于微信公众号[[千问AI平台]]（IP 属地浙江，标记原创，正文带「阿里妹导读」导语）的技术实践长文，发布于 2026 年 4 月 22 日。全文结构终局为**六章正文 + 结尾感言**：一、核心编排器 AgentCore；二、Tool 机制（Function Calling）；三、RAG 模块；四、Command 与 Skill；五、SubAgent；六、MCP 双端支持。无第七章、无运行效果章节、无总结章节。全部技术内容落地于开源仓库 [[aiagentdemo]]（GitHub `q644266189/aiagentdemo`），核心框架为 [[spring-ai]]，模型选型为 [[GLM-4]] + embedding-3。

## 核心叙事：让 AI 生成学习资料

作者的自述学习动机是「最好的学习资料是代码，让 AI Agent 本身帮我生成学习资料」：项目代码几乎全部由 AI 生成，作者本人仅担任「指挥家与验收员」。知识库的 category 枚举（java_basic / jvm / concurrent / spring / design_pattern）实证知识库内容即 Java 学习资料，与该元叙事互相印证。这一工作方式与 [[自举式开发]]、[[vibe-coding]] 呼应，但作者明确以「验收员」角色保留了人工质检环节。

## 快速开始

环境要求：Java 21+、Maven 3.9+。代码仓库：

```
github地址：https://github.com/q644266189/aiagentdemo
git clone git@github.com:q644266189/aiagentdemo.git
```

模型配置（Spring AI OpenAI 兼容接入智谱）：

```ini
spring.ai.openai.base-url=https://open.bigmodel.cn/api/paas/v4
spring.ai.openai.api-key=你的API密钥
spring.ai.openai.chat.options.model=glm-4
spring.ai.openai.embedding.options.model=embedding-3
```

核心模块表（正文原表 7 行）：

| 模块 | 说明 |
|------|------|
| AgentCore | 核心编排器，具备意图识别、记忆管理与大模型调用等能力。 |
| ChatMemory | 对话记忆管理，支持三层上下文压缩（摘要压缩 → Assistant 裁剪 → 滑动窗口）。 |
| Tool（Function Calling） | 可插拔的工具注册机制，通过 `InnerTool` 统一接口注册，LLM 自主决策调用 |
| RAG | 完整的检索增强生成流水线：文档加载 → 文档分块 → 向量化 → 向量存储 → 多路召回（语义 + BM25 + 查询改写）→ RRF 融合 → Rerank 重排 → LLM → 内容生成 |
| Command & Skill | 两种 Markdown 驱动的 Prompt 模板机制：Command 由用户主动调用，Skill 本质作为 Tool 由 LLM 决策调用。 |
| SubAgent | 拥有独立记忆的子代理，支持内部 SubAgent 和外部 IdeaLab Agent 两种形态 |
| MCP | 双向 MCP 支持：作为 Client 动态连接外部 MCP 服务，作为 Server 对外暴露服务 |

Web 前端（`src/main/resources/static/index.html`，访问 `http://localhost:8080`）四特性：流式对话（SSE 逐字输出）、Markdown 渲染、命令面板（输入 `/` 唤起快捷命令）、会话管理（清空对话历史）。

## 一、核心编排器：AgentCore

两个业务 REST 端点：

```bash
# 非流式对话
curl -X POST http://localhost:8080/api/chat \
  -H "Content-Type: application/json" \
  -d '{"message": "你好，介绍一下你的能力", "sessionId": "test-001"}'
# 流式对话（SSE）
curl -X POST http://localhost:8080/api/chat/stream \
  -H "Content-Type: application/json" \
```

AgentCore 执行五段编排：**意图识别 → RAG 注入 → 记忆管理 → 模型调用 → 工具执行**。RAG 注入使用自然语言模板拼接：

```
"以下是从知识库中检索到的相关参考资料，请结合这些资料回答用户的问题：\n\n" + ragContext + "\n\n用户问题：" + userInput
```

**Spring AI 框架已内建 Agent Loop**，载体为 `org.springframework.ai.chat.client.advisor.ToolCallAdvisor#adviseCall` 的 do-while 循环：

```
boolean isToolCall = false;
do {
    isToolCall = chatResponse != null && chatResponse.hasToolCalls();
    if (isToolCall) {
        ToolExecutionResult toolExecutionResult =
            toolCallingManager.executeToolCalls(prompt, chatResponse);
        if (toolExecutionResult.returnDirect()) {
            // 工具结果直接返回客户端，中断循环 break
        }
        instructions = doGetNextInstructionsForToolCall(request, response, result);
    }
} while (isToolCall);  // loop until no tool calls are present
```

`returnDirect` 机制：工具执行结果可跳过 LLM 直接返回客户端，中断 tool calling 循环。意图识别由 IntentRecognizer 以 LLM 判别 `Intent.RAG / Intent.GENERAL` 双意图并前置门控（详见 [[意图识别前置门控]]）。

ChatMemory 按 sessionId 以 `ConcurrentHashMap<String, ChatMemory>` 隔离，支持多客户端并发；历史消息超 16 条触发第一层摘要压缩。压缩逻辑完全封装于 `getMessages()` 内部（SummaryCompressor 为私有静态内部类），调用方无感知。摘要注入标记：`"\n\n【以下是之前对话的摘要，请参考】\n" + summaryText`。三层压缩完整参数见 [[三层上下文压缩]]。

运行时支持动态切换模型提供商（如智谱切到通义千问 [[qwen]]）无需重启，并可动态调参 temperature/maxTokens/topP。

## 二、Tool 机制（Function Calling）

Function Calling 本质：LLM 只「想」不「做」——Agent 服务端告知工具清单，LLM 返回调用意向，真实执行发生在 Agent 服务端。所有工具实现统一 `InnerTool` 接口：

```java
public interface InnerTool {
    List<ToolCallback> loadToolCallbacks();
}
```

Spring 启动自动扫描 Bean 注册到 AgentCore，新增工具只实现接口、不改已有代码（开闭原则）；`ToolCallbackBuilder` 将工具名、描述、JSON Schema 参数定义、执行函数组装为标准 ToolCallback。详见 [[可插拔工具注册机制]]。

工具调用流程示例（杭州天气）：用户提问 → LLM 决定调用 `get_weather({"city": "杭州"})` → Spring AI 自动执行 → 返回「杭州，晴，22°C」→ LLM 生成最终回复。

内置工具一览表（完整）：

| 工具名 | 功能 | 说明 |
|--------|------|------|
| `knowledge_search` | 知识库检索 | 将 RAG 检索能力封装为工具，LLM 可主动检索 |
| `create_sub_agent` | 创建子代理 | 创建拥有独立记忆的 SubAgent |
| `chat_with_sub_agent` | 与子代理对话 | 在 SubAgent 的独立上下文中继续对话 |
| `destroy_sub_agent` | 销毁子代理 | 释放 SubAgent 资源 |
| `call_ideas_{name}` | 调用 IDEAs 应用 | 调用外部 IdeaLab 平台的 AI 应用（支持多个） |
| `{skill_name}` | 执行技能 | 由 Markdown 文件定义的技能，动态注册 |
| `{mcp_tool_name}` | MCP 工具 | 从外部 MCP Server 发现并注册的工具 |
| `get_weather` | 天气查询 | 示例工具 |
| `get_stock_price` | 股票价格查询 | 示例工具 |

## 三、RAG 模块：检索增强生成

核心论断：「分块质量直接决定检索质量」（与 [[gigo原则]] 同构）。

**确定规则分块（Definite）**：

| 策略 | 原理 | 适用场景 |
|------|------|------|
| TextSplitter（默认） | 递归语义分块，按标题 → 段落 → 句子 → 固定字符的优先级依次尝试切分 | 通用文档，兼顾语义完整性 |
| FixedSizeSplitter | 按固定字符数切分 | 结构不明确的纯文本 |
| ParagraphSplitter | 按段落（连续换行）切分 | 段落结构清晰的文档 |
| SentenceSplitter | 按句子（句末标点）切分 | 需要细粒度检索的场景 |
| SlidingWindowSplitter | 滑动窗口切分，相邻块有重叠 | 需要保留上下文连续性 |

**智能分块（Intelligent）**：

| 策略 | 原理 | 适用场景 |
|------|------|------|
| SemanticChunkSplitter | 基于语义相似度判断切分点 | 语义边界不明确的长文本 |
| PropositionSplitter | 将文本拆解为独立命题 | 需要精确事实检索 |
| AgenticSplitter | 使用 LLM 判断最佳切分方式 | 复杂混合格式文档 |

默认分块：`TextSplitter`，分块大小 **500 字符**、重叠 **50 字符**（与有赞 [[文档分块参数调优]] 的 600/100 构成两套可对照参数）。

检索核心代码：

```java
public String query(String question) {
    // 1. 多路召回（语义 + BM25 + 查询改写，共 9 个候选）
    List<Document> candidates = multiRecaller.recall(question, RECALL_CANDIDATE_COUNT);
    // 2. Rerank 重排（取最相关的 3 个）
    List<Document> relevantDocuments = llmReranker.rerank(question, candidates, TOP_K);
    // 3. 拼接上下文
    StringBuilder contextBuilder = new StringBuilder();
    for (int i = 0; i < relevantDocuments.size(); i++) {
        contextBuilder.append("【参考资料 ").append(i + 1).append("】\n");
        contextBuilder.append(relevantDocuments.get(i).getContent()).append("\n\n");
    }
    return contextBuilder.toString().trim();
}
```

召回策略表：

| Retriever | 原理 | 擅长 |
|------|------|------|
| SemanticRetriever | 基于 EmbeddingModel 的向量余弦相似度检索 | 语义相近但措辞不同的查询 |
| Bm25Retriever | 基于 BM25 算法的关键词匹配（TF-IDF 变体） | 精确关键词匹配 |
| QueryRewriteRetriever | 先用 LLM 将问题改写为 3 种不同表达，再分别做向量召回 | 扩大语义覆盖面 |

查询改写从「预处理流程」升级为「独立召回路」，详见 [[查询改写召回路]]。单一召回策略总有盲区，三路召回经 [[RRF排名融合算法]]（k=60）融合：

```java
// MultiRecaller 核心逻辑
public List<Document> retrieve(String query, int topK) {
    Map<String, Double> rrfScores = new HashMap<>();
    Map<String, Document> keyToDocument = new LinkedHashMap<>();
    for (Recaller retriever : retrievers) {
        List<Document> results = retriever.retrieve(query, PER_ROUTE_CANDIDATE_COUNT);
        // RRF 公式：score(d) = Σ 1 / (k + rank)，k=60 为平滑常数
        accumulateRrfScores(results, rrfScores, keyToDocument);
    }
    return rrfScores.entrySet().stream()
            .sorted(Map.Entry.<String, Double>comparingByValue().reversed())
            .limit(topK)
            .map(entry -> keyToDocument.get(entry.getKey()))
            .toList();
}
```

Rerank 链：多路召回 9 候选 → 专用 Rerank 模型（本项目为 LLM Reranker）精排 → top 3。VectorStore 为轻量级内存实现（Spring AI EmbeddingModel + 余弦相似度），适合中小规模知识库，生产环境可替换 Milvus、Pinecone（仅一句提及，无实质内容）。

## 四、Command 与 Skill

Skill 文件 = YAML Front Matter（name + description）+ Prompt 模板：

```markdown
---
name: summarize
description: 对用户提供的文本内容进行摘要总结
---
请对以下文本进行摘要总结，提取核心要点：
{{input}}
```

Command 文件 = 纯 Prompt 模板，文件名即命令名：

```markdown
请对以下代码进行 Code Review，从代码质量、潜在 Bug、性能、可读性等维度给出改进建议：
{{input}}
```

SkillManager 启动扫描 `classpath:skill/*.md`，SkillTool 将每个技能转为 ToolCallback 注册，LLM 依 description 自主判断调用；CommandManager 启动扫描 `classpath:command/*.md`，用户经 `POST /api/command/execute` 主动执行。六维度对比与「Command 确定性 / Skill 智能化」互补论详见 [[command-vs-skill]]。

## 五、SubAgent：独立记忆的子代理

动机：需要独立上下文的任务（如多轮完善一篇技术文章）不应污染主对话记忆。核心机制：

```java
public SubAgent(String id, String name, String systemPrompt, ChatClient chatClient) {
    this.memory = ChatMemory.forSubAgent();  // 独立记忆！
    this.memory.setSystemPrompt(systemPrompt);
    // ...
}
```

独立 ChatMemory + 独立 systemPrompt，共享主 Agent 的 ChatClient（共享同一大模型连接），「共享模型、隔离记忆」；三效果：不污染主上下文 / 多 SubAgent 并存互不干扰 / 销毁即释放记忆。详见 [[SubAgent记忆隔离]]。

SubAgent 三工具表（能力经 Function Calling 暴露，全生命周期由主 LLM 驱动）：

| 工具 | 参数 | 说明 |
|------|------|------|
| create_sub_agent | name、system_prompt、task | 创建 SubAgent 并执行首个任务 |
| chat_with_sub_agent | agent_id、message | 与已有 SubAgent 继续对话 |
| destroy_sub_agent | agent_id | 销毁 SubAgent，释放资源 |

详见 [[SubAgent生命周期工具化]]。

## 六、MCP 双端支持

[[mcp]] 是 Anthropic 提出的开放协议，让 AI 应用标准化连接外部工具和数据源；本项目同时实现 MCP Server 与 MCP Client（双端）。Server 侧 SimpleMcpServer 对外暴露 knowledge_query 知识库检索工具，内部调用 RagService——RAG 能力可被任何支持 MCP 协议的 AI 应用复用（详见 [[RAG能力MCP服务化]]）。

knowledge_query 参数表：

| 参数 | 类型 | 说明 |
|------|------|------|
| keyword | String | 检索关键词 |
| category | String | 知识分类（java_basic / jvm / concurrent / spring / design_pattern / all） |
| maxResults | int | 返回的最大结果条数，默认 3 |

Client 侧 McpClient.connect() 核心代码：

```java
public ToolCallback[] connect(String serverUrl) {
    McpSyncClient mcpClient;
    McpSchema.InitializeResult initResult;
    // 优先尝试 Streamable HTTP，失败后回退到 SSE
    try {
        mcpClient = connectWithStreamableHttp(serverUrl);
        initResult = mcpClient.initialize();
    } catch (Exception streamableException) {
        mcpClient = connectWithSse(serverUrl);
        initResult = mcpClient.initialize();
    }
    // 自动发现远程工具
    SyncMcpToolCallbackProvider provider = SyncMcpToolCallbackProvider.builder()
            .mcpClients(mcpClient).build();
    ToolCallback[] toolCallbacks = provider.getToolCallbacks();
    // 持久化 URL，下次启动自动恢复
    store.add(serverUrl);
    return toolCallbacks;
}
```

6.2 关键特性（完整三条）：

1. **传输协议自动适配**：优先 Streamable HTTP（2025-03-26 规范），失败自动回退 SSE（2024-11-05 规范）——版本锚定详见 [[MCP双规范版本锚定]]
2. **工具自动发现**：连接成功后自动获取远程工具，转换为 `ToolCallback` 注册到 Agent
3. **持久化与自动恢复**：URL 持久化到 `mcp-servers.json`，应用重启时自动重连——详见 [[MCP持久化与自动恢复]]

运行时动态管理接口表（详见 [[MCP运行时动态管理]]）：

| 接口 | 方法 | 说明 |
|------|------|------|
| `/api/manage/mcp/connect` | POST | 连接新的 MCP 服务，工具立即可用 |
| `/api/manage/mcp/disconnect` | POST | 断开 MCP 服务，移除对应工具 |
| `/api/manage/mcp/list` | GET | 查看所有 MCP 服务及其工具列表 |

## 结尾感言

作者在结尾提出全文哲学总结，四个论断分别沉淀为：[[LLM问答黑箱论]]、[[harness工程三部曲演进论]]、[[function-calling大地基论]]、[[上下文窗口四要素]] 与 [[工具定义最终论]]。关键原句：

> 「从 Prompt Engineering 到 Context Engineering 到 Harness Engineering 本质解决的就是『有限的上下文窗口中该放什么内容』」「Function Calling 是 Harness 的大地基」「Skill 本质就是一种 Tool，而 RAG、SubAgent 与外部 MCP 服务等能力在工程实践中也大量被做成一种 Tool 由 LLM 决策调用」

## 跨源关联

- **Harness 三部曲 ↔ [[harness-engineering]]（爱奇艺数据库团队）**：两独立来源对 Harness Engineering 趋同——爱奇艺提五要素工程化约束，本文提「Prompt→Context→Harness 演进链 + Function Calling 大地基」，另见 [[harness四要素]]
- **LLM 问答黑箱论 ↔ [[上下文工程]]（马上消费）**：同以「上下文窗口放什么」为工程化核心命题
- **MCP 双协议回退 + 运行时动态管理 ↔ [[多协议mcp-server框架]]（京东）/ [[less模式]]**：Client 侧双协议降级与运行时增删 vs Server 侧单码库三协议
- **Skill YAML Front Matter ↔ [[anthropics-skills]] / [[agentskillsioagent-skills-开放标准]]**：name+description 结构同构
- **Command/Skill 职责 ↔ [[workflow与agent控制权分界]]（同公众号）/ [[skill-command-mcp三层架构]]（seanguo）**：控制权分界的项目级落地
- **三层上下文压缩 ↔ [[上下文压缩策略]] / [[比例阈值压缩]] / [[记忆容量上限倒逼压缩]]**：阈值驱动压缩路线
- **Spring AI 内建 Agent Loop ↔ [[agent-loop]] / [[react-agent]]**：「自研 vs 框架内建」对照
- **RAG 工具化双路径 ↔ [[意图规划]] / [[意图识别保守策略]] / [[workflow优先于agent]]**；**RAG 上下文注入模板 ↔ [[结构化上下文组装]]**（自然语言模板 vs XML 分层）
- **RAG 能力 MCP 服务化 ↔ [[mcp]] / [[mcp工具封装模式]] / [[代码问答]]**：检索能力跨应用复用的独立样本
- **MCP 工具自动发现 ↔ [[渐进式工具加载]]**：连接时批量发现 vs 会话中按需追加
- **框架选型 ↔ [[spring-ai-alibaba-studio]]**（同生态）、**[[dify]]**（低代码 vs Java 工程化路线对照）

## 待核与开放问题

- 「六个核心模块」与表格 7 行的口径：第六章 MCP 落定后六模块（AgentCore 编排 / Tool / RAG / Command&Skill / SubAgent / MCP）全部对齐自洽，仅表格多出一行的性质待核
- 各 Splitter/Retriever 与 `ChatMemory.forSubAgent()` 的归属（Spring AI 原生 vs 项目自研扩展）未澄清
- 「Linux说过一句很经典的话」系对 Linus Torvalds 的口语化指代（非严格错引）
- `mcp-servers.json` 具体格式；connect 端点请求体格式；disconnect 后工具移除的底层机制；MCP 连接鉴权；SimpleMcpServer 传输协议
- `PER_ROUTE_CANDIDATE_COUNT` 数值、`maxRounds` 数值、`PRESERVE_RECENT_MESSAGES` 与「3 条 Assistant」的对应关系
- LlmReranker 使用的 Rerank 模型、prompt 与成本；RAG 流水线图内容（仅图片）
- IntentRecognizer prompt 设计、误判兜底与准确率；returnDirect 实际使用场景
- chat_with_sub_agent 返回结果如何写回主上下文；SubAgent 数量上限与资源回收策略
- IdeaLab 平台本体定义与接入协议（仅知 `call_ideas_{name}` 可调用其 AI 应用，支持多个）
- Skill 文件是否支持更多 front matter 字段；Command 与 Skill 文件是否可热加载
- 「千问AI平台」与阿里/通义千问的正式关系
- 本项目路线（Spring AI + Function Calling 地基论）与 AgentScope、Dify 框架路线的对比；「工具定义走到最后」预测与 Memory/知识库路线的长期验证