---
type: concept
title: always级别AI Rule
tags: [ai-rule, 规范, 约束, ai-coding]
related: [ai-you-hao-yan-fa-gui-fan, ren-ren-dui-qi-ren-ji-dui-qi, prompt-based-tool-injection, skill]
created: 2026-06-08
updated: 2026-06-08
sources: ["用Agent评测思路管理AICoding31万行代码AI重构的实践.html"]
---
# always级别AI Rule

工程规范从文档层面升级为AI编码过程中**始终强制执行**的约束。是[[ren-ren-dui-qi-ren-ji-dui-qi|人人对齐→人机对齐]]方法论中"人机对齐"的核心落地手段。

## 核心特征

- **始终加载**：不依赖人记忆或手动选择，AI编码时自动遵守
- **强制执行**：不可被AI忽略或绕过
- **可更新**：随[[ren-ren-dui-qi-ren-ji-dui-qi|人人对齐]]共识迭代而更新

## 与Skill的关系

[[skill|Skill]]承载按功能维度聚合的工具调用知识，而AI Rule承载工程规范约束。两者共同构成AI Coding的约束体系：

- **Rule** = "必须这样做 / 禁止这样做"
- **Skill** = "这类事情按这个模式做"

## 关联

- 与[[prompt-based-tool-injection|Prompt级工具注入]]属于同类实践的不同实现层级——都是将约束嵌入AI上下文
- 与[[skill-as-knowledge-cache|Skill作为工具调用知识缓存层]]互补——Rule管边界，Skill管模式