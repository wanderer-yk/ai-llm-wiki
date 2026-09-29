---
type: entity
title: JoyAgent
tags: [平台, 知识库, rag, 京东, ai工具]
related: [joyagent-genie, joyspace, 双rag架构, 自定义知识库绑定机制, 京东物流]
created: 2026-06-22
updated: 2026-06-22
sources: ["[202602032040]基于知识工程JoyAgent双RAG的智能代码评审系统的探索与实践.html"]
---
# JoyAgent

JoyAgent 是京东的知识库检索平台，官网 joyagent.jd.com。它支持自定义智能体创建、工作流绑定知识库、在线文档（[[joyspace|Joyspace]]）与离线文档（PDF/Word）多格式接入。

## 核心特性

**[[自定义知识库绑定机制]]** 是 JoyAgent 的核心灵活性特性：接入者可在平台自定义智能体并绑定知识库，无需修改 Prompt 即可定制评审规则。

## 在双 RAG 架构中的角色

在 [[京东物流]]的 [[双rag架构|双 RAG 架构]][[智能代码评审系统]]中，JoyAgent 承担知识库检索层职责，与知识工程层的代码知识检索形成双路径组合。

## 优势与不足

**优势**：自定义知识库绑定机制提供灵活性，支持多格式文档接入。

**不足**（在代码评审场景中暴露）：
- [[知识归纳失真]]：LLM 对 Code Diff 总结不稳定
- [[检索与生成联动失效]]：归纳失真传导至后续检索和生成环节
- 缺乏 [[项目身份感知能力]]：无法识别 EDI 项目类型

## 开源生态

[[joyagent-genie|JoyAgent-Genie]] 是 JoyAgent 的开源子项目（GitHub: jd-opensource/joyagent-jdgenie）。
