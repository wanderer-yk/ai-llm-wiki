---
type: source
title: "用Agent评测思路管理AI Coding —— 31万行代码AI重构的实践"
authors: [业务研发平台, 美团技术团队]
year: 2026
url: ""
venue: 微信公众号"美团技术团队"
tags: [ai-coding, 重构, agent评测, 美团, 工程实践, 技术债, pre-pr, sop]
related: [agent-ping-ce-xi-tong, mei-tuan-ye-wu-yan-fa-ping-tai-tuan-dui, ren-ren-dui-qi-ren-ji-dui-qi, ai-you-hao-yan-fa-gui-fan, pre-pr-ji-zhi]
created: 2026-06-08
updated: 2026-06-08
sources: ["用Agent评测思路管理AICoding31万行代码AI重构的实践.html"]
---
# 用Agent评测思路管理AI Coding —— 31万行代码AI重构的实践

## 基本信息

- **发布公众号**：美团技术团队
- **作者署名**：业务研发平台（[[mei-tuan-ye-wu-yan-fa-ping-tai-tuan-dui|美团业务研发平台团队]]）
- **发布时间**：2026年05月07日
- **IP属地**：北京

## 核心主张

当90%以上代码由AI生成时，决定系统走向的不是生成速度，而是**约束AI的能力**。AI Coding不会自动收敛复杂度，没有统一规范约束反而会加速系统腐化。

## 核心方法论

文章提出 [[ren-ren-dui-qi-ren-ji-dui-qi|人人对齐→人机对齐]] 方法论——先拉齐团队所有人的评判标准（人人对齐），再将共识固化为AI可执行的约束（人机对齐），顺序不可颠倒。

## 文章结构

### 背景：Agent评测系统的三重复杂性

[[agent-ping-ce-xi-tong|Agent评测系统]]承载多模态数据评测、流程编排、质量控制等能力，代码从2025年6月不足5万行膨胀至31万行。系统面临"笛卡尔积"级场景矩阵：6种多模态数据评测 × 多种任务视图 × 十余种质检机制。

### 三阶段重构时间线

1. **阶段一：AI辅助技术债梳理**（[[zhuan-jia-jing-yan-ding-xiang-ai-fu-zhu-pai-cha|专家经验定向+AI辅助排查]]）
   - 核心开发圈定高危边界，AI做穷举扫描
   - AI定位10个隐藏极深的性能隐患；完成3个P0 + 2个P1技术债梳理
   - [[jing-yan-jia-zhi-zhuang-yi|经验价值转移]]：从"能看全"到"能判断什么重要"

2. **阶段二：制定AI友好研发规范**
   - [[ai-you-hao-yan-fa-gui-fan|AI友好研发规范]]：规范从协作建议升级为约束AI产出、阻止系统长新债的基础设施
   - "1个独裁者好过10个民主者"——标准对齐需强力角色
   - 人机一致率需达~90%才可信

3. **阶段三：建立SOP、渐进式重构**（2026年3-4月）
   - [[si-ceng-jia-gou|四层架构重构]]（Starter/Application/Infrastructure/Common）
   - [[ling-pai-qi-zhong-gou|零排期重构]]：技术债拆解为业务需求顺带动作
   - [[zhu-r-da-yang-sop-fen-fa|主R打样→SOP分发→全组并行执行]]
   - [[po-quan-lu-lou-lu-zhi-li|PO全链路泄露治理]]三步法
   - [[always-ji-bie-ai-rule|always级别AI Rule]]固化约束
   - [[bian-pai-lei-yu-neng-li-lei|编排类与能力类]]职责边界划分

### 质量保证

- [[pre-pr-ji-zhi|Pre-PR预审机制]]：提交前AI多轮自查，Reviewer仅聚焦业务语义
- [[gao-jie-mo-xing-shen-cha-di-jie-mo-xing|高阶模型审查低阶模型]] + [[kua-chang-shang-mo-xing-dui-kang|跨厂商模型对抗审核]]
- [[ren-ji-xie-zuo-ce-shi-sop|人机协作测试SOP]]（Human-in-the-loop五步法）

### 行动指南（四步）

1. 拉齐标准——人人对齐
2. 规范落地——固化为always加载的Rule/Skill
3. SOP分发——主R打样后全组并行执行
4. Pre-PR机制——"不能省"

## 关键证据

- 31万行代码从不足5万行膨胀；月均16个需求（80%业务+20%技术）
- AI辅助定位10个隐藏性能隐患
- 团队未申请一天专门重构时间，31万行在业务交付中渐进消化
- 十余个核心包完成工程结构迁移
- 路线A（AI全自动测试）暴露严重问题；路线B（人工主导AI辅助）沉淀为SOP

## 显著缺失

文章未给出31万行重构的最终量化成果指标（缺陷率、交付速度、代码质量度量变化），也未展开AI Rule具体文件格式、Skill加载机制、高阶/低阶模型选型等技术细节。

## 关联

- 与 [[从OpenClaw看Agent架构设计]] 同属Agent工程实践领域，前者侧重"约束AI Coding"，后者侧重"四大架构决策"
- [[always-ji-bie-ai-rule]] 与 [[prompt-based-tool-injection]] 属同类实践不同实现层级
- [[pre-pr-ji-zhi]] 与 [[safety-iron-rule]] 共享"前置校验"工程哲学
- [[ren-ji-xie-zuo-ce-shi-sop]] 与 [[small-model-beats-llm-in-classification]] 共同印证"AI不取代判断，AI放大覆盖"