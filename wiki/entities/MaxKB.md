---
type: entity
title: MaxKB
tags: [知识库, rag, 开源]
related: [aligo, aligo企业级知识库, dify, knowledge-wiki]
created: 2026-10-13
updated: 2026-10-13
sources: ["[202602140830]准确率提升至90阿里商旅基于AgentScope构建多智能体差旅助手最佳实践.html"]
---
# MaxKB

MaxKB 是一个开源知识库基础框架，技术栈为 Python + PostgreSQL，截至 2026-02 在 GitHub 拥有 19k+ stars（以上信息来自本 Wiki 来源文章的选型陈述）。在阿里商旅 [[aligo]] 差旅助手体系中，MaxKB 被选为企业级知识库系统的基座，供 RAG 子智能体通过标准化 API 接入知识库检索。

## AliGo 选型四理由

| 选型理由 | 说明 |
|---------------|------|
| 技术栈匹配 | Python + PostgreSQL，与 Aligo 技术栈一致 |
| 功能完善且符合诉求 | 文档解析、向量检索、多模型对接 |
| 快速落地 | 开源成熟，二次开发成本低 |
| 社区活跃 | GitHub 19k+ stars，持续维护 |

## 在本 Wiki 中的位置

- 与 [[dify]]、[[llamaindex]] 同属知识/检索基础设施粒度的工具实体，但定位为集中式企业知识库（多企业隔离、图文混排文档处理、本地化容器化部署）。
- 与有赞 [[knowledge-wiki]]（`.wiki/` + Git submodule 的分布式项目知识层）形成集中式 vs 分布式两条知识层路线对照（另见 [[知识库降熵论]]）。
- 知识库能力四项的完整描述见 [[aligo企业级知识库]]。
