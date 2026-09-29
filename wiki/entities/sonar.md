---
type: entity
title: Sonar
tags: [静态扫描, 代码质量, 工具]
related: [六步智能提示词生成法, ast加符号表联合分析, pre-pr机制, code-insight]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202512261820]回收团队基于Cursor集成MCP的智能代码修复提示词生成实践.html"]
---
# Sonar

代码静态扫描工具，用于检测代码质量问题并提供修复建议。在[[回收团队]]的实践中，Sonar 作为问题数据来源，其扫描结果被自动转换为结构化 AI 提示词。

## 覆盖规则

转转实践中涉及的 Sonar 规则体系：

| 规则 ID | 问题类型 | 说明 |
|---------|---------|------|
| java:S2259, java:S2637 | 空指针 | NullPointerException 风险 |
| java:S2095, java:S2093 | 资源泄露 | 未正确关闭资源 |
| java:S3649 | SQL 注入 | SQL 拼接风险 |
| java:S2076 | 命令注入 | 命令拼接风险 |
| java:S117 | 命名规范 | 变量/方法命名不符合规范 |
| java:S1192 | 重复字符串 | 字符串字面量重复使用 |

## 转转内部实例

转转内部部署了 Sonar 平台（sonar.zhuanspirit.com），通过 Cookie 鉴权访问 `/api/issues/search` 接口，支持分页查询（ps/p 参数）、分支过滤和新代码周期过滤。

## 跨源关联

Sonar 与 AI 的自动流水线呼应美团 [[pre-pr机制]] 中"AI 多轮自查前置"的自动化思路。与有赞 [[code-insight]] 的 [[ast加符号表联合分析]] 互补：有赞侧重调用关系图谱做影响范围分析，转转侧重将 Sonar 问题结果转化为精准提示词。