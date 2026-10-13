---
type: source
title: "阿里 Assistant Agent 开源，助力开发者快速构建答疑、诊断智能助手"
tags: [AssistantAgent, spring-ai-alibaba, code-as-action, graalvm, java, 智能助手, 答疑客服, 诊断]
related: [AssistantAgent, spring-ai-alibaba, GraalVM, 评估图, PromptBuilder条件化动态组装, code-as-action, CodeactTool工具体系, 回复渠道路由, 多数据源统一检索, 经验学习与快速响应, 学习模块四通道提取, 触发器三类型与自我调度, 千问AI平台]
created: 2026-10-13
updated: 2026-10-13
authors: [残风, 栀七]
year: 2026
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247557465&idx=1&sn=76a5a8c192cf450955cea5c89dab32e9&chksm=e833aef343ac3e097088817a56d167875e6a857a59245721f58d290af41d40baaebbb9f26ee7#rd"
venue: "千问AI平台（微信公众号）"
sources: ["[202601150830]阿里AssistantAgent开源助力开发者快速构建答疑诊断智能助手.html"]
---
# 阿里 Assistant Agent 开源，助力开发者快速构建答疑、诊断智能助手

**发布信息**：作者残风、栀七；发布于微信公众号"千问AI平台"（[[千问AI平台]]，IP 属地浙江），发布时间 2026-01-15 08:30。文内 `ne-clipboard` 元数据显示内容转载自阿里语雀官方项目文档（`aliyuque.antfin.com/gvr7dx/bhqi33/...`），即公众号文章为官方项目文档/README 的搬运。

## 一句话总览

本文系统介绍了 [[AssistantAgent]]——基于 [[spring-ai-alibaba]] 构建的企业级 Java 智能助手框架，核心范式为代码即行动（[[code-as-action]]，配置命名空间 `spring.ai.alibaba.codeact`），AI 生成的代码在 [[GraalVM]] 多语言沙箱中安全执行；全文以"特性/能力/场景 → 项目结构与快速启动 → 核心模块介绍（评估图、Prompt Builder、学习、经验、触发器、回复渠道、动态工具、知识检索）→ 致谢与联系方式"的结构展开。

## 核心声明

- Assistant Agent 定位为"能理解、能行动、能学习"的助手解决方案，目标场景：智能答疑客服、系统诊断、运维助手、业务助理、AIOps。
- 六项技术特性：代码即行动；安全沙箱（GraalVM 多语言沙箱 + 资源隔离）；多维评估（评估图 Graph 多层次意图识别）；Prompt 动态组装（按场景/评估结果注入经验与知识）；经验学习（自动积累成功经验）；快速响应（熟悉场景跳过 LLM 推理——后文精化为需显式配置 `fastIntentConfig`）。
- 四项核心能力：智能问答（SPI 扩展的多数据源统一检索，答案可溯源）；工具调用（MCP、HTTP API/OpenAPI）；主动服务（定时任务、延迟执行、事件回调）；多渠道触达（内置 IDE 回复，SPI 可扩展钉钉/飞书/企微/Webhook）。
- 适用场景：智能客服（接入企业知识库）、运维助手（对接监控/工单）、业务助理（连接 CRM/ERP）。
- 前置要求：Java 17+、Maven 3.8+、DashScope API Key；默认模型 `qwen-max`（关联 [[qwen]]、[[百炼]]）。
- 致谢三项依赖 URL 完整：Spring AI（`https://github.com/spring-projects/spring-ai`）、Spring AI Alibaba（`https://github.com/alibaba/spring-ai-alibaba`）、GraalVM（`https://www.graalvm.org/`）。
- 社区渠道：钉钉群 `130240015687`；微信群二维码加入，过期则转钉钉群。
- 否定性证据：文章**不含**"整体工作原理"章节，正文以致谢+联系方式直接结束；"整体工作原理""为什么选择 Assistant Agent""对比传统方案"均为不可提取图片。

## 结构化数据（原文逐字保全）

### 项目结构

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

### 最小配置（`assistant-agent-start/src/main/resources/application.yml`）

```yaml
spring:
  ai:
    dashscope:
      api-key: ${DASHSCOPE_API_KEY}
      chat:
        options:
          model: qwen-max
```

### 快速启动命令

```bash
git clone https://github.com/spring-ai-alibaba/AssistantAgent
cd assistant-agent
mvn clean install -DskipTests
```

```bash
export DASHSCOPE_API_KEY=your-api-key-here
```

```bash
cd assistant-agent-start
mvn spring-boot:run
```

### 知识库搜索默认配置（知识检索模块完整版）

```yaml
spring:
  ai:
    alibaba:
      codeact:
        extension:
          search:
            enabled: true
            knowledge-search-enabled: true   # 知识库（默认 Mock 实现）
            project-search-enabled: false    # 项目代码（默认 Mock 实现）
            web-search-enabled: false        # Web 搜索（默认 Mock 实现）
            default-top-k: 5
            search-timeout-ms: 5000
```

### SearchProvider SPI 接入示例（Java；HTML 中误标为 typescript 语言）

```java
package com.example.knowledge;

import com.alibaba.assistant.agent.extension.search.spi.SearchProvider;
import com.alibaba.assistant.agent.extension.search.model.*;
import org.springframework.stereotype.Component;
import java.util.*;

@Component  // 添加此注解，Provider 会自动注册
public class MyKnowledgeSearchProvider implements SearchProvider {

    @Override
    public boolean supports(SearchSourceType type) {
        return SearchSourceType.KNOWLEDGE == type;
    }

    @Override
    public List<SearchResultItem> search(SearchRequest request) {
        List<SearchResultItem> results = new ArrayList<>();
        // 1. 从你的知识源查询（向量数据库、ES、API 等）
        // 示例：List<Doc> docs = vectorStore.similaritySearch(request.getQuery());
        // 2. 转换为 SearchResultItem
        // item.setId / setSourceType(SearchSourceType.KNOWLEDGE) / setTitle
        // / setSnippet / setContent / setScore
        return results;
    }

    @Override
    public String getName() {
        return "MyKnowledgeSearchProvider";
    }
}
```

### 评估图（Evaluation Graph）层级结构

| 层级 | 执行方式 | 评估项 | 输出 |
|------|---------|--------|------|
| 输入 | — | 用户输入（示例："查询今日订单"） | 原始 Query |
| Layer 1 | 并行执行 | 是否模糊？（清晰/模糊）；输入改写（增强） | 模糊标记 + 改写后内容 |
| Layer 2 | 基于改写内容，并行执行 | 检索经验（有/无）；匹配工具（有/无）；搜索知识（有/无） | 三维评估结果 |
| 整合 | 汇聚 | 整合不同维度评估结果 → 传递给后续模块 | 评估上下文 |

评估模块核心能力 API 签名：

```java
// Rule-based 评估：用户自定义规则函数
Function<CriterionExecutionContext, CriterionResult>
// 依赖声明：评估项前置依赖
dependsOn
// 评估 Prompt 自定义配置项
customPrompt; description; workingMechanism; fewShots
// 评估结果类型
BOOLEAN | ENUM | SCORE | JSON | TEXT
```

### Prompt Builder 条件匹配规则

| 评估条件 | 注入片段 |
|---------|---------|
| 模糊=是 | [澄清引导 Prompt] |
| 模糊=否 | [直接执行 Prompt] |
| 经验=有 | [历史经验参考] |
| 工具=有 | [工具使用说明] |
| 知识=有 | [相关知识片段] |
| 组合：模糊+无工具+无知识 | [追问用户 Prompt] |
| 组合：清晰+有工具+有经验 | [快速执行 Prompt] |

最终动态 Prompt = [系统提示] + [澄清引导] + [历史经验] + [工具说明] + [用户问题] → 模型。

### 学习模块四通道

| 学习通道 | 说明 |
|---|---|
| After-Agent 学习 | 每次 Agent 运行完成后提取经验 |
| After-Model 学习 | 每次模型调用后提取经验 |
| Tool Interceptor | 从工具调用中提取经验 |
| 离线学习 | 批量分析历史数据提取模式 |

学习上下文捕获四要素：用户输入、中间推理步骤、生成的代码、执行结果；三类提取器：经验提取器（成功模式）、模式提取器（通用模式）、错误提取器（失败教训）。

### 触发器三类型

| 触发器类型 | 标识 | 能力 |
|---|---|---|
| 定时触发 | `TIME_CRON` | 支持 Cron 表达式定时触发任务 |
| 延迟触发 | `TIME_ONCE` | 支持一次性延迟触发 |
| 回调触发 | `CALLBACK` | 支持回调事件触发（外部系统发事件到 Webhook） |

### 回复渠道四类型（图中短名与正文长名并存，正文长名为准）

| 渠道标识 | 回复形式 | 落地端 |
|------|------|------|
| DEFAULT | 文本回复 | 控制台终端回复 |
| IDE_CARD | 卡片展示 | IDE 富文本卡片 |
| IM_NOTIFICATION（图内短名 IM_NOTIFY） | 消息推送 | IM（钉钉/飞书/企微，可扩展） |
| WEBHOOK_JSON（图内短名 WEBHOOK） | JSON 推送 | 第三方系统 |

### CodeactTool 五类工具

| 工具类别 | 覆盖能力 |
|------|------|
| MCP Tools | 任意 MCP Server |
| HTTP API Tools | REST API / OpenAPI |
| Search Tools | 知识检索、项目搜索 |
| Trigger Tools | 定时任务、事件回调 |
| 自定义 Tools | 实现 `CodeactTool` 接口 |

## 核心模块与 SPI 扩展面

七大 extension 模块（评估/学习/经验/触发器/回复渠道/动态工具/知识检索）全部经 SPI 式扩展，确认三个 SPI 面：

1. `SearchProvider`（`com.alibaba.assistant.agent.extension.search.spi`）：`supports()` / `search()` / `getName()` 三方法，`@Component` 自动注册（chunks 6、8）。
2. `ReplyChannelDefinition`：扩展回复渠道（`IDE_CARD` / `IM_NOTIFICATION` / `WEBHOOK_JSON` 均需自行实现），方法签名未披露（chunk 8）。
3. `CodeactTool`：自定义工具接口，方法签名未披露（chunk 8）。

内置于 core 的执行模块（GraalVM 执行器）全程未以文字专节出现，仅在技术特性与致谢中提及。

## 与本 Wiki 的关联

- **阿里系版图**：Assistant Agent（Java/Spring 生态）与 [[agentscope]]（Python 多智能体框架）、[[阿里商旅]]、[[tair]]、[[阿里云]] 同属阿里系实践，技术栈不同，暂无直接关系主张。与 [[spring-ai-alibaba-studio]]（Dify 导出 Spring AI 代码工具）同源生态。
- **机制对照**：Code-as-Action ↔ [[codeact架构]]；MCP 一等公民 ↔ [[mcp]]、[[mcp薄协议转换层]]、[[function-calling-mcp-skills本质差异对比]]；评估图 Layer 1 输入改写 ↔ [[Query改写]]、[[意图规划]]；快速意图 ↔ [[意图路由]]；Prompt Builder ↔ [[动态prompt生成机制]]、[[结构化上下文组装]]；知识前置注入 ↔ [[静态动态双路径知识注入]]、[[知识优先级机制]]；Mock 默认 + SPI 生产切换 ↔ [[三阶段平台演进路线]]（同构渐进策略）；学习/经验 ↔ [[多智能体自进化]]、[[skill功能聚合]]、[[sop-doc可召回排查文档]]；ReAct 经验 ↔ [[react-agent]]。
- **比较候选**：触发器 ↔ [[openclaw定时任务系统]]；工具体系 ↔ [[openclaw工具系统六层架构]]；回复渠道 ↔ [[openclaw-channel架构]]、[[即时响应到异步交付]]；GraalVM 沙箱 ↔ [[docker沙箱隔离]]。

## 提取说明与证据缺口

- 9/9 chunks 提取完毕；chunks 1-4 为微信页面模板 CSS，正文自 chunk 5 起；评论区无可提取内容。
- "整体工作原理""为什么选择 Assistant Agent""对比传统方案"（imgfileid 100073693）均为图片，内容不可提取。
- 待外部验证/未披露：GraalVM 沙箱执行细节；`ReplyChannelDefinition`/`CodeactTool` 接口方法签名；"配置驱动"回复工具的具体配置格式；`fastIntentConfig` 配置格式；经验库存储介质与相似匹配算法；检索聚合排序策略与多 Provider 结果冲突裁决机制。
