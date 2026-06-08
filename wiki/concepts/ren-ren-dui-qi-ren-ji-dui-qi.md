---
type: concept
title: "人人对齐→人机对齐"
tags: [方法论, ai-coding, 团队协作, 标准, 规范]
related: [ai-you-hao-yan-fa-gui-fan, always-ji-bie-ai-rule, jing-yan-jia-zhi-zhuang-yi, agent-ping-ce-xi-tong]
created: 2026-06-08
updated: 2026-06-08
sources: ["用Agent评测思路管理AICoding31万行代码AI重构的实践.html"]
---
# 人人对齐→人机对齐

从Agent评测业务实践中沉淀的核心方法论，应用于AI Coding管理。核心理念：**先拉齐团队所有人的评判标准（人人对齐），再将共识固化为AI可执行的约束（人机对齐）**。

## 顺序不可颠倒

这是该方法论最重要的原则：

1. **先人人对齐**——团队内部必须先形成统一共识
2. **后人机对齐**——共识才能被固化为AI可执行的[[always-ji-bie-ai-rule|Rule]]/[[skill|Skill]]

没有统一共识，AI Rule会被不同人解释为不同版本，沦为一纸空文。

## 关键原则

### "1个独裁者好过10个民主者"

标准对齐需要强有力的角色统一所有角色的评判标准。不能靠民主投票，需要有人"拍板"。

### 人机一致率阈值

人机一致率需达到基本阈值（如**90%**）才能认为机器评价可信。低于此阈值说明规范本身或AI理解存在偏差。

### 规范必须落地到AI工具链

[[gui-fan-luo-di-ai-gong-ju-lian|规范不落地到AI工具链里就只是一纸空文]]，必须固化为always加载的Rule/Skill。

## 来源

该方法论源自[[mei-tuan-ye-wu-yan-fa-ping-tai-tuan-dui|美团业务研发平台团队]]对[[agent-ping-ce-xi-tong|Agent评测系统]]31万行代码AI重构的实践，本质上是用Agent评测的思路来管理AI Coding——评测的核心是"标准对齐"，而AI Coding的核心是"约束对齐"。

## 与其他Wiki概念的关联

- 与[[context-engineering|上下文工程]]在理念层面高度互补：前者管"人→AI的标准传递"，后者管"信息→模型的准确交付"
- [[always-ji-bie-ai-rule|always级别AI Rule]]是"人机对齐"的具体落地手段
- 与[[prompt-based-tool-injection|Prompt级工具注入]]属于同类实践的不同实现层级