---
type: entity
title: AssistantAgent（阿里 Assistant Agent）
tags: [AssistantAgent, spring-ai-alibaba, code-as-action, java, 智能助手框架, 开源]
related: [spring-ai-alibaba, GraalVM, code-as-action, 评估图, PromptBuilder条件化动态组装, CodeactTool工具体系, 回复渠道路由, 多数据源统一检索, 经验学习与快速响应, 学习模块四通道提取, 触发器三类型与自我调度, agentscope, 千问AI平台]
created: 2026-10-13
updated: 2026-10-13
sources: ["[202601150830]阿里AssistantAgent开源助力开发者快速构建答疑诊断智能助手.html"]
---
# AssistantAgent（阿里 Assistant Agent）

AssistantAgent（Assistant Agent）是阿里开源的企业级智能助手构建框架，基于 [[spring-ai-alibaba]]（Java/Spring 生态 AI 应用框架）开发，采用代码即行动（Code-as-Action，见 [[code-as-action]]）范式——通过生成和执行代码来编排工具、完成任务，定位为"能理解、能行动、能学习"的助手解决方案。开源仓库为 `spring-ai-alibaba/AssistantAgent`（GitHub：`https://github.com/spring-ai-alibaba/AssistantAgent`），目标场景包括智能答疑客服、系统诊断、运维助手、业务助理与 AIOps。AI 生成代码在 [[GraalVM]] 多语言沙箱中安全运行（具备资源隔离能力；沙箱执行细节原文文字未披露，待外部验证）。

## 六项技术特性

1. **代码即行动**：生成并执行代码编排工具，而非仅调用预定义工具。
2. **安全沙箱**：GraalVM 多语言沙箱 + 资源隔离。
3. **多维评估**：评估图（Graph）多层次意图识别（详见 [[评估图]]）。
4. **Prompt 动态组装**：按场景与评估结果注入经验与知识（详见 [[PromptBuilder条件化动态组装]]）。
5. **经验学习**：自动积累成功经验（详见 [[学习模块四通道提取]]、[[经验学习与快速响应]]）。
6. **快速响应**：熟悉场景跳过 LLM 推理、基于经验直接响应——需显式配置 `fastIntentConfig` 方可启用（opt-in 机制，非自动）。

## 四项核心能力

- **智能问答**：SPI 扩展的多数据源统一检索（[[多数据源统一检索]]），答案准确可溯源。
- **工具调用**：MCP、HTTP API/OpenAPI（[[CodeactTool工具体系]]）。
- **主动服务**：定时任务、延迟执行、事件回调（[[触发器三类型与自我调度]]）。
- **多渠道触达**：内置 IDE 回复，SPI 可扩展钉钉/飞书/企微/Webhook（[[回复渠道路由]]）。

## 项目结构

```
assistant-agent/
├── assistant-agent-common          # 通用工具、枚举、常量
├── assistant-agent-core            # 核心引擎：GraalVM 执行器、工具注册表
├── assistant-agent-extensions      # 扩展模块：
│   ├── dynamic/               #   - 动态工具（MCP、HTTP API）
│   ├── experience/            #   - 经验管理与快速意图配置
│   ├── learning/              #   - 学习提取与存储
│   ├── search/                #   - 统一搜索能力
│   ├── reply/                 #   - 多渠道回复
│   ├── trigger/               #   - 触发器机制
│   └── evaluation/            #   - 评估集成
├── assistant-agent-prompt-builder  # Prompt 动态组装
├── assistant-agent-evaluation      # 评估引擎
├── assistant-agent-autoconfigure   # Spring Boot 自动配置
└── assistant-agent-start           # 启动模块
```

## SPI 扩展面

七大 extension 模块（评估/学习/经验/触发器/回复渠道/动态工具/知识检索）全部经 SPI 式扩展，三个已确认 SPI 面：

| SPI 接口 | 用途 | 已披露签名 |
|---|---|---|
| `SearchProvider` | 接入真实知识源（向量数据库、Elasticsearch、企业知识库 API 等），替换默认 Mock | `supports()` / `search()` / `getName()`，`@Component` 自动注册 |
| `ReplyChannelDefinition` | 扩展回复渠道（IDE_CARD / IM_NOTIFICATION / WEBHOOK_JSON 均需自行实现） | 未披露 |
| `CodeactTool` | 自定义工具 | 未披露 |

注意：开箱即用面较薄——知识检索默认仅 Mock 实现（`knowledge-search-enabled: true`），回复渠道开箱仅内置演示渠道 `IDE_TEXT`，生产化成本主要在用户侧扩展。

## 模型接入与前置要求

- 默认模型 `qwen-max`，经 DashScope API Key 接入（关联 [[qwen]]、[[百炼]]）。
- 前置要求：Java 17+、Maven 3.8+、DashScope API Key。
- 快速启动：`git clone` → `mvn clean install -DskipTests` → 配置 `DASHSCOPE_API_KEY` → `mvn spring-boot:run`（完整命令见来源页）。

## 社区渠道

钉钉群 `130240015687`（搜索加入）；微信群二维码加入，过期转钉钉群。

## 与本 Wiki 的关系

- 与 [[agentscope]]（Python 多智能体框架）同属阿里系但**并非同一体系**：Assistant Agent 是 Java/Spring AI 生态的企业助手框架，AgentScope 是 Python 多智能体应用框架，二者形成 Java/Python 框架互补，无直接关系主张。
- 与 [[spring-ai-alibaba-studio]]（将 Dify 应用导出为 Spring AI 工程代码的工具）共享 Spring AI Alibaba 生态底座。
- 跨框架比较候选：触发器 vs [[openclaw定时任务系统]]；工具体系 vs [[openclaw工具系统六层架构]]；回复渠道 vs [[openclaw-channel架构]]；GraalVM 沙箱 vs [[docker沙箱隔离]]。

## 待验证问题

- GraalVM 沙箱执行细节（权限模型、资源限额）全文文字未披露，可能仅存在于不可提取架构图中。
- `fastIntentConfig` 配置格式、经验库存储介质与相似匹配算法未披露。
- 检索聚合排序策略与多 Provider 结果冲突裁决机制未披露。
