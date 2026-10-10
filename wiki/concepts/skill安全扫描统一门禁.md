---
type: concept
title: skill 安全扫描统一门禁
tags: [hermes-agent, 安全, skill, 门禁]
related: [hermes-agent, 记忆内容威胁模式扫描, skill局部patch修补, 多层安全护栏, 验证门禁化, agent-control-plane]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604230830]深入源码HermesAgent如何实现SelfImproving.html"]
---
# skill 安全扫描统一门禁

skill 安全扫描统一门禁是 [[hermes-agent]] Skill 侧的安全机制（`tools/skill_manager_tool.py:56-74`）：**Agent 自主创建的 Skill 与从 Skill Hub 安装的 Skill 走同一套检查**——`scan_skill(skill_dir, source="agent-created")` 产出扫描结果，`should_allow_install` 判定放行与否；不通过即阻止创建/安装（patch 场景则回滚，见 [[skill局部patch修补]]）。设计理由："Memory/Skill 最终都会进入系统提示词，是一等安全边界"。

## 源码证据

```python
# tools/skill_manager_tool.py:56-74
def _security_scan_skill(skill_dir):
    result = scan_skill(skill_dir, source="agent-created")
    allowed, reason = should_allow_install(result)
    if allowed is False:
        report = format_scan_report(result)
        return f"Security scan blocked this skill ({reason}):\n{report}"
```

## 关键设计点：自创与安装同权同责

统一门禁消除了"自己创建的东西免检"的漏洞——Agent 自创内容与外部社区内容面对同等安全标准，配合 [[记忆内容威胁模式扫描]] 覆盖 Memory 侧，形成写入侧的双通道防线。

## 同族方案

与 [[验证门禁化]]（vivo 丁俊杰：验证从建议升级为硬性阻断）、[[多层安全护栏]]（0424 飞樰文）、[[agent-control-plane]]（zhiyuanfu）同属"未通过检查即强制阻断"的门控共识。