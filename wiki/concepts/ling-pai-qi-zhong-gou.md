---
type: concept
title: 零排期重构
tags: [重构, 技术债, 敏捷, ai-coding]
related: [jian-jin-shi-zhong-gou, ji-shu-zhai, zhu-r-da-yang-sop-fen-fa, si-ceng-jia-gou]
created: 2026-06-08
updated: 2026-06-08
sources: ["用Agent评测思路管理AICoding31万行代码AI重构的实践.html"]
---
# 零排期重构

将[[ji-shu-zhai|技术债]]拆解为业务需求的"顺带动作"，不申请专门重构时间，随业务迭代消化。与[[jian-jin-shi-zhong-gou|渐进式重构]]紧密关联。

## 核心理念

**重构不需要排期，需要拆解能力。**

传统做法是申请专门的重构排期（需要与业务方博弈），零排期重构将技术债拆解为每个业务需求的附带任务，在完成业务交付的同时逐步消化技术债。

## 实践证据

[[mei-tuan-ye-wu-yan-fa-ping-tai-tuan-dui|美团业务研发平台团队]]在31万行代码重构中：

- 未申请一天专门重构时间
- 31万行在业务交付中渐进消化
- 月均16个需求（80%业务+20%技术）

## 关键前提

1. [[zhuan-jia-jing-yan-ding-xiang-ai-fu-zhu-pai-cha|技术债已被清晰梳理]]（P0/P1分级）
2. [[ai-you-hao-yan-fa-gui-fan|AI友好研发规范]]已就位，阻止新债产生
3. [[zhu-r-da-yang-sop-fen-fa|主R打样→SOP分发]]模式确保团队可并行执行
4. [[si-ceng-jia-gou|目标架构]]清晰，每个需求的"顺带动作"有明确方向