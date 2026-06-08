---
type: entity
title: Agent评测系统
tags: [agent, 评测, 多模态, 美团]
related: [mei-tuan-ye-wu-yan-fa-ping-tai-tuan-dui, ren-ren-dui-qi-ren-ji-dui-qi, ai-you-hao-yan-fa-gui-fan]
created: 2026-06-08
updated: 2026-06-08
sources: ["用Agent评测思路管理AICoding31万行代码AI重构的实践.html"]
---
# Agent评测系统

[[mei-tuan-ye-wu-yan-fa-ping-tai-tuan-dui|美团业务研发平台团队]]负责的核心业务系统，承载多模态数据评测、流程编排、质量控制等能力。

## 规模演变

- **2025年6月**：不足5万行代码
- **2026年5月（文章发布时）**：31万行代码

## 系统复杂性

面临"笛卡尔积"级场景矩阵：

- **6种**多模态数据评测
- **多种**任务视图
- **十余种**质检机制

## 三重复杂性来源

1. 多模态数据评测的组合爆炸
2. 业务快速迭代（月均16个需求，80%业务+20%技术）
3. AI Coding加速代码膨胀

## 技术债问题

系统积累了大量[[ji-shu-zhai|技术债]]，包括：

- 业务模型缺陷
- 数据库查询性能隐患
- 状态管理债
- 索引债

旧架构呈[[yan-tong-shi-gong-neng-kai-fa|烟囱式功能开发]]特征，每新增业务形式都需新增代码，缺乏数据模型扩展能力。

## 重构成果

通过[[ren-ren-dui-qi-ren-ji-dui-qi|人人对齐→人机对齐]]方法论，完成十余个核心包从面条式包结构到[[si-ceng-jia-gou|标准四层架构]]的迁移。