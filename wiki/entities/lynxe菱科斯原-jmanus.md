---
type: entity
title: Lynxe（菱科斯，原 JManus）
tags: [func-agent, 框架, spring-ai-alibaba, function-calling, lynxe, agent框架, github, 千问AI平台, 产品, 阿里巴巴]
related: ["沈询", "spring-ai-alibaba-studio", "一切都是函数", "任务流程定义困难问题", "skills两大未解问题", "mcp与skills竞争关系论", "function-calling桥梁论", "ai工程量化效果声明追踪", "react-agent", "react五要素", "思考内化论", "agent-loop", "千问AI平台", "决策权三分法", "编程与工作流同源替代论", "业务自闭环", "ai增强与agent体验二分选型", "sources/[202602260830"]
created: 2026-10-12
updated: 2026-10-12
sources: ["[202602240830]AIAgent系列什么是ReActAgent.html", "[202602250830]AIAgent系列深入了解智能体工作流核心Agentvs传统编程vsWorkflow的本质区别.html", "[202602260830]AIAgent系列深入解析FunctionCallingMCP和Skills的本质差异与最佳实践.html"]
---
# Lynxe（菱科斯，原 JManus）

Lynxe（中文名"菱科斯"，原名 JManus）是 [[沈询]] 课余时间开发并持续完善的自研 Func-Agent（函数智能体）框架，位于 GitHub `spring-ai-alibaba` 组织下，定位为探索 Agent 的前沿最佳实践。仓库地址：`https://github.com/spring-ai-alibaba/Lynxe`。

## 基本信息

- 仓库：https://github.com/spring-ai-alibaba/Lynxe（位于 spring-ai-alibaba GitHub 组织下）
- 作者：[[沈询]]
- 名称沿革：JManus → Lynxe（菱科斯）
- 类型：Func-Agent（函数智能体）框架

## 自报定位与能力声明

以下均为作者自述：

- "这是一个非常完善的产品级的 Func-Agent 框架"
- "核心目的就是探索 Agent 的前沿最佳实践"
- 沈询称"目前已经有所小成，Lynxe 能解决我自己面对的 80% 以上的问题"——无任务范围定义、无评测/度量口径与量化数据的自报量化声明，证据强度低，按 [[ai工程量化效果声明追踪]] 口径登记存疑。

## 设计哲学：一切都是函数

Lynxe Func-Agent 的设计哲学为"一切都是函数，函数才是第一公民"：每个 Agent 能力以函数暴露，接受结构化参数输入，使 Agent 得以超越对话框形态集成进既有系统（详见 [[一切都是函数]]）。

## 四项函数化能力

1. 接受结构化的参数输入，而不是依赖纯文本描述
2. 返回结构化结果，方便与现有系统集成
3. 支持多种调用方式（不限聊天界面）
4. 与现有业务逻辑、表单、API 无缝对接

双优势总结：既保留了 Agent 处理不确定性的核心优势，又解决了结构化输入输出和系统集成的问题（定性主张，无量化口径）。其中第②③④项逐条正面回应 [[skills两大未解问题]] 之"聊天式接驳"，使 Func-Agent 在文内被定位为 Skills 缺陷的解法。

## 与「AI Agent 系列」文章的关系

- **系列构成**：「AI Agent 系列」共五篇文章（什么是 ReAct Agent / Agent vs 传统编程 vs Workflow / Function Calling、MCP 和 Skills / 上下文管理实践 / 并行执行实践），发布于[[千问AI平台]]
- **来源关系**：系列内容均来自 Lynxe 上的实验验证，全部基于作者对 Lynxe 的实战经验总结；Lynxe 由此成为前沿最佳实践的实验场
- **系列自述目的**："把这些实验下来有效的东西写出来，方便大家快速入门"，并引导读者访问 Lynxe 仓库阅读详细源码学习 agent 最佳实践
- **理论呼应**：Func-Agent 框架与系列第三篇"[[一切都是函数]]"的理念一脉相承

## 工程特征（出自系列第一篇）

- **ReAct 主循环实现**：While 循环驱动，含任务完成判断与 `最大轮次 = 10` 的硬编码上限，防止无限循环——是 [[agent-loop]] 概念的具体落地实例
- **固定提示词模板**：跨轮次不变，仅历史上下文字段随轮次增长，实现轮次推进
- **思考过程隐藏**：调用 LLM 时思考是可选输出，直接输出 toolcall（见 [[思考内化论]]）
- **ReAct 五要素实现**：历史上下文 + 当前环境信息 + LLM Thinking + toolcall + 观察结果（见 [[react五要素]]）

## new_branch 七步流程（实际运行实例）

Lynxe 实际运行的 `new_branch` 发布流程以 markdown 文字化定义（原文七步，原样保留）：

```text
1) 确认本地的 VERSION 与 pom.xml 与 本地branch 中的版本一致，不一致的话以pom.xml为准
2) mvn package 
3) 进入 ui-vue3 运行pnpm lint 
4) 退回项目目录， git merge upstream/main
5) 项目目录，运行 make ui-deploy
6) git 提交 branch到origin
7) git 打包 tag名字与pom的版本号一致，先删除远程tag（如果存在）：git push upstream :refs/tags/v{版本号}，然后上传tag到 upstream (上传之前请先用git remote 看一下upstream是哪里，确认是spring-ai-alibaba/JManus)
```

作者自报该流程"实际跑的过程中只有很小的概率会出错"（无量化口径），被用作"文字化流程可行"的实证（[[任务流程定义困难问题]]）。

## 仓库命名出入（未决）

来源内证据存在命名不一致：一处记录 GitHub 地址为 `spring-ai-alibaba/Lynxe`，而 `new_branch` 流程第 7 步要求确认 upstream 为 `spring-ai-alibaba/JManus`——疑似仓库更名前后的引用残留，全文完结仍未澄清。JManus 作为曾用名/upstream 仓库名记录，不单独建页。

## 生态背景与同生态工具的区分

背景说明，独立可查证：Spring AI Alibaba 是围绕 Spring 生态的开源 AI 应用框架体系，本 Wiki 已收录同生态工具 [[spring-ai-alibaba-studio]]；Lynxe 是该组织下的 Agent 框架项目。

主体区分：[[spring-ai-alibaba-studio]] 是将 Dify 应用逻辑导出为标准 Spring AI 工程代码的工具；Lynxe 是 Func-Agent 框架。二者同处 Spring AI Alibaba 生态语境（Lynxe 仓库位于 spring-ai-alibaba 组织），但主体不同，不应混淆。

## 相关来源

- [[sources/[202602250830]AIAgent系列深入了解智能体工作流核心Agentvs传统编程vsWorkflow的本质区别|AI Agent 系列（二）]]
- [[sources/[202602260830]AIAgent系列深入解析FunctionCallingMCP和Skills的本质差异与最佳实践|AI Agent 系列（三）]]

## 关联

- 作者：[[沈询]]
- 发布渠道：[[千问AI平台]]
- 同生态工具：[[spring-ai-alibaba-studio]]
- 理论展开：[[react-agent]]、[[agent-loop]]、[[react五要素]]、[[思考内化论]]
- 议论脉络：[[一切都是函数]]、[[任务流程定义困难问题]]、[[skills两大未解问题]]、[[mcp与skills竞争关系论]]、[[function-calling桥梁论]]、[[ai工程量化效果声明追踪]]