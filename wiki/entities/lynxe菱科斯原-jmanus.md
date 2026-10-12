---
type: entity
title: "Lynxe（菱科斯，原 JManus）"
tags: [func-agent, 框架, spring-ai-alibaba, function-calling]
related: [沈询, spring-ai-alibaba-studio, 一切都是函数, 任务流程定义困难问题, skills两大未解问题, mcp与skills竞争关系论, function-calling桥梁论, ai工程量化效果声明追踪]
created: 2026-10-12
updated: 2026-10-12
sources: ["[202602260830]AIAgent系列深入解析FunctionCallingMCP和Skills的本质差异与最佳实践.html"]
---
# Lynxe（菱科斯，原 JManus）

Lynxe（中文名"菱科斯"，原名 JManus）是 GitHub `spring-ai-alibaba` 组织下的一个产品级 Func-Agent（函数智能体）框架，由[[沈询]]开发，定位为探索 Agent 前沿最佳实践。仓库地址：`https://github.com/spring-ai-alibaba/Lynxe`。（背景说明，独立可查证：Spring AI Alibaba 是围绕 Spring 生态的开源 AI 应用框架体系，本 Wiki 已收录同生态工具 [[spring-ai-alibaba-studio]]；Lynxe 是该组织下的 Agent 框架项目。）

## 自报定位

作者自述 Lynxe"能解决我自己面对的 80% 以上的问题"——无评测口径的个人自述，登记 [[ai工程量化效果声明追踪]]。

## 设计哲学：一切都是函数

Lynxe Func-Agent 的设计哲学为"一切都是函数，函数才是第一公民"：每个 Agent 能力以函数暴露，接受结构化参数输入，使 Agent 得以超越对话框形态集成进既有系统（详见 [[一切都是函数]]）。

## 四项函数化能力

1. 接受结构化的参数输入，而不是依赖纯文本描述
2. 返回结构化结果，方便与现有系统集成
3. 支持多种调用方式（不限聊天界面）
4. 与现有业务逻辑、表单、API 无缝对接

双优势总结：既保留了 Agent 处理不确定性的核心优势，又解决了结构化输入输出和系统集成的问题（定性主张，无量化口径）。其中第②③④项逐条正面回应 [[skills两大未解问题]] 之"聊天式接驳"，使 Func-Agent 在文内被定位为 Skills 缺陷的解法。

## new_branch 七步流程（实际运行实例）

Lynxe 实际运行的 `new_branch` 发布流程以 markdown 文字化定义（原文七步，原样保留）：

```ruby
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
