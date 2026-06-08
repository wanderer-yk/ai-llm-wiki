---
type: source
title: "别让AI瞎猜了：用Harness Engineering终结无限返工"
authors: [数据库团队]
year: 2026
url: ""
venue: 爱奇艺技术产品团队（微信公众号）
tags: [harness-engineering, ai-coding, agent协作, 工程化, 返工治理]
related: [harness-engineering, shu-ju-ku-tuan-dui, specflow, ren-ren-dui-qi-ren-ji-dui-qi, spec-driven-development, ai-bian-cheng-huan-jue]
created: 2026-06-08
updated: 2026-06-08
sources: ["别让AI瞎猜了用HarnessEngineering终结无限返工.html"]
---
# 别让AI瞎猜了：用Harness Engineering终结无限返工

**作者**：数据库团队（[[ai-qi-yi-ji-shu-chan-pin-tuan-dui|爱奇艺技术产品团队]]）
**发布日期**：2026-05-14
**来源**：微信公众号"爱奇艺技术产品团队"原创文章

## 摘要

本文提出 **[[harness-engineering|Harness Engineering]]** 方法论，定义harness为"让agent能稳定参与研发的工程安排"。核心论点是AI返工的根因不在模型能力不足，而在于任务交接前工程准备不完整——结构未定、状态缺失、边界不清、口径不一、记录无落点，导致agent被迫猜测并产生累积偏移。

## 核心内容

### 理论篇

1. **[[harness-wu-yao-su|五要素定义]]**：任务入口、执行依据、工具边界、验证反馈、结果记录
2. **[[san-chong-cai-ce-wen-ti|三重猜测问题]]**：仅有自然语言描述时，agent同时猜测外观/状态/代码拆分，输出不可控
3. **Prompt局限性**：[[prompt-kou-tou-chuan-tong-xian-jing|口头传统陷阱]]与[[prompt-tuo-li-xiang-mu-xian-jing|脱离项目陷阱]]
4. **第一性原理**：瓶颈从"谁来写"转向"任务说清/边界定住/验证能跑/结果有人接"
5. **[[agent-san-yuan-ze|Agent三原则]]**：无法访问的知识=不存在、无法执行的工具=没有、无法验证的目标=无法持续修正

### 实践篇

1. **[[harness-wu-ceng-zhi-ze-mo-xing|五层职责模型]]**：任务编排→执行依据→状态暴露与验证→agent执行→评审收口
2. **[[qian-hou-duan-san-ceng-jia-gou|前后端三层架构]]**：执行依据层→状态暴露层→交付实现层
3. **[[wu-ceng-yan-zheng-ti-xi|五层验证体系]]**：静态检查→单元验证→链路验证→失败验证→回写验证
4. **[[harness-san-jie-duan-luo-di-lu-jing|三阶段落地路径]]**：入口可找→任务可复用→重复可机械化
5. **项目文件模板**：`AGENTS.md`入口地图、`.agent/PLANS.md`计划协议、`docs/harness/`约束文档、`docs/test/`验证记录、`scripts/harness/`检查脚本

### 核心金句

> "prompt解决的是这一轮怎么说清楚，harness解决的是项目里如何持续做对。"

## 方法论溯源

文章引用 OpenAI Harness Engineering 框架图作为方法论源头，本文为爱奇艺数据库团队对该理念的实践解读与二次加工。开源模板仓库：GitHub SisyphusSQ/harness-template。

## 关联

- 与[[治愈CursorAI编程的幻觉用它就够了]]（Specflow方案）同属[[ai-qi-yi-ji-shu-chan-pin-tuan-dui]]公众号，Specflow聚焦前端规格契约，Harness Engineering聚焦全栈agent工程条件，构成**层级互补**
- 与[[ren-ren-dui-qi-ren-ji-dui-qi]]（人人对齐→人机对齐）形成呼应：HE三阶段路径的前两阶段做"人人对齐"（隐性知识外化），第三阶段做"人机对齐"（规则机械化）
- 与[[spec-driven-development]]形成方法论栈：SDD解决需求契约层，HE解决执行工程层

## 局限

- 全文为定性方法论论述，无量化数据支撑
- 未提供代码级落地方案或具体工具链集成案例
- 方法论部分汲取自OpenAI，原创增量在实践解读和前后端场景化