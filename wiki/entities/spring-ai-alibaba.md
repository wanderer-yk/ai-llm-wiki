---
type: entity
title: Spring AI Alibaba
tags: [spring-ai-alibaba, spring-ai, java, alibaba, 开源框架, ai应用框架]
related: [AssistantAgent, GraalVM, spring-ai-alibaba-studio, langchain, agentscope, 百炼, qwen]
created: 2026-10-13
updated: 2026-10-13
sources: ["[202601150830]阿里AssistantAgent开源助力开发者快速构建答疑诊断智能助手.html", "[202601080830]从HITLHumanInTheLoop实践出发看Agent与设计模式的对跖点.html"]
---
# Spring AI Alibaba

Spring AI Alibaba 是阿里开源的面向 Java/Spring 生态的 AI 应用框架，官方仓库为 `https://github.com/alibaba/spring-ai-alibaba`（GitHub org 为 `alibaba`），是 [[AssistantAgent]] 智能助手框架的构建基座；其上游为 Spring 官方 AI 框架 Spring AI（`https://github.com/spring-projects/spring-ai`）。本文来源中框架的配置命名空间为 `spring.ai.alibaba.*`（扩展模块配置位于 `spring.ai.alibaba.codeact.*`），模型接入默认走 DashScope（关联 [[百炼]]、[[qwen]]）。

（独立可验证背景：Spring AI Alibaba 是阿里巴巴基于 Spring AI 构建的开源框架，面向 Java 开发者提供接入通义系等大模型的 AI 应用开发能力，与 LangChain 等 Python 生态框架形成跨语言对照。）

## 在本文中的角色

- [[AssistantAgent]] 基于 Spring AI Alibaba 构建，采用 Code-as-Action 范式（命名空间 `spring.ai.alibaba.codeact` 与技术特性列表、学习模块"代码生成"环节、CodeactTool 工具体系四重互证，见 [[code-as-action]]）。
- 文章致谢三项依赖的第二项（第一项 Spring AI、第三项 [[GraalVM]]），URL 于 chunk 9 完整确认。

## 与本 Wiki 的关系

- 与 [[spring-ai-alibaba-studio]] 同源：后者是将 Dify 应用逻辑导出为标准 Spring AI 工程代码的工具（有赞三阶段平台演进路线中使用），二者共享 Spring AI 生态，但不是同一事物。
- 与 [[langchain]] 形成 Java/Python 两大生态 AI 应用框架的跨生态对照。
- 与 [[agentscope]] 同属阿里系但定位不同：AgentScope 是 Python 多智能体框架，Spring AI Alibaba 是 Java/Spring 生态 AI 应用框架。

## Hook 提供 HITL 能力（奕寰篇，2026-01）

[[奕寰]] 截图佐证 Spring AI Alibaba 的设计利用 Hook 钩子手段提供 HITL（人机回路）能力（API 细节因截图不可恢复待官方文档补证）；其 AI 生成的代码沙箱执行能力另见 [[AssistantAgent]]（Code-as-Action 内核）。
