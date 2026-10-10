---
type: concept
title: Skill 知识三层架构
tags: [skill, 渐进式披露, 知识架构, 按需加载]
related: [skill渐进式披露, skilltoken预算, skill目录结构与命名规范, 渐进式披露替代向量检索, cloudflare-deploy, SKILL工具化加载]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604270830]工作流的Skill怎么写从7个顶级Skill中提炼的模式与最佳实践.html"]
---
# Skill 知识三层架构

Skill 知识三层架构是 [[青斧]] 在文章 4.4 节给出的知识组织模型，是 [[skill渐进式披露]] 的量化实例化：

```bash
第 1 层：Frontmatter（~100 tokens）
  → LLM 扫描所有 Skill 的 description，决定是否加载

第 2 层：SKILL.md 正文（<5K tokens）
  → 核心指令、决策树、流程步骤

第 3 层：references/ 和 resources/（按需加载）
  → 详细文档、示例、清单，LLM 用 read 工具按需读取
```

第 1 层解决"要不要加载我"（[[description三大要素]] 的作用域），第 2 层解决"怎么照做"，第 3 层解决"细节按需取"。配套的目录结构（SKILL.md 必须 + scripts/references/resources/examples 四个可选目录）与 [[skill目录结构与命名规范]] 互证；references 按需加载与 [[渐进式披露替代向量检索]] 的"目录即检索路径"思想一致。量化实证来自 [[cloudflare-deploy]]：主文件仅 7KB，references/ 展开至几十万字。Token 预算的分层量化详见 [[skilltoken预算]]。
