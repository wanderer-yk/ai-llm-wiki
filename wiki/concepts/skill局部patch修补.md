---
type: concept
title: skill 局部 patch 修补
tags: [hermes-agent, skill, self-improving]
related: [hermes-agent, skill自动创建触发条件, skill安全扫描统一门禁, skill生命周期元数据]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604230830]深入源码HermesAgent如何实现SelfImproving.html"]
---
# skill 局部 patch 修补

skill 局部 patch 修补是 [[hermes-agent]] 对已有 Skill 的增量维护机制，对应源码 `_patch_skill`（`tools/skill_manager_tool.py:397-485`）：用 `fuzzy_find_and_replace` **模糊匹配**定位修改点（容忍 Agent 给出的 old_string 与原文的格式差异），而非全量重写；每次修改后过 `_security_scan_skill()` 安全扫描，不通过则**自动回滚**；配合原子写入与修改前备份。典型场景是"踩坑当场补 Pitfalls"。

## 设计取舍（文章设计取舍表第 6 条）

| 设计决策 | 表面效果 | 背后的考量 |
|---------|---------|-----------|
| patch 优先于全量重写 | 局部修复 Skill | 保留已验证的稳定部分，只改需要改的 |

## 案例体现

在 [[三会话自进化实证案例]] 会话 2 中，Agent 遭遇 Skill 未覆盖的 Django DisallowedHost 错误后，Review Agent 通过 patch 为 `flask-k8s-deploy` Skill 补上 ALLOWED_HOSTS 坑——Skill 在使用中完成自我进化。

安全方面，patch 与 Skill 创建/安装走 [[skill安全扫描统一门禁]] 同一套检查。