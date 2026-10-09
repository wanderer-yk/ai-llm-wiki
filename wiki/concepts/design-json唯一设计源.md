---
type: concept
title: design-json唯一设计源
tags: [design-tokens, 设计规范, 单一可信源]
related: [设计即代码, F2C, deepseek, 通用技术Prompt模板, 无需走查的代码集成]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604271800]柚漫剧AI全流程提效拆解从单点提效到工程融合.html"]
---

# design-json唯一设计源

[[柚漫剧团队]] guidelines 撰写的可执行约束（3.2）：**所有组件的设计规范基于 design.json 设计令牌（Design Tokens），明确忽略 theme.css**，形成"唯一设计源"；设计规范主要内容交 [[deepseek]] 润色成 guidelines 描述。

## 核心规则（verbatim 摘录）

```makefile
 **忽略theme.css的设计规范，所有组件的设计规范基于design.json**
# 字号
单列标题用subtitle-lg，用户名文字用subtitle-xs，按钮字号用caption-lg，来源信息用caption-md，标签文字用caption-sm，内容用标题展示最多两行
# 字体加粗
标题用regular
# 圆角
标签用rounded-xxs，图片用rounded-md，卡片用rounded-lg，按钮用rounded-full
# 间距
标签内部用space-6xs，卡片内信息上下间距用space-xl，卡片内信息左右间距用space-xl，文字与图片间距用space-md
# 颜色
标题用text，来源和辅助信息用text-slim
# 图片
单图大图比例16:9，三图的图片比例3:2，三图只有第一张左边和第二张右边加圆角
```

完整的令牌映射 prompt（字号/字重/颜色/圆角/间距/图片布局逐项绑定 design.json 令牌，并以"输出要求：确保所有值均引用自 design.json"收尾）见来源页 [[sources/[202604271800]柚漫剧AI全流程提效拆解从单点提效到工程融合]] 的结构化数据节。

## 机制意义

单一可信源约束消除了"theme.css 与 design.json 双源冲突"，使 [[设计即代码]] 链路中 AI 生成的样式必然落在受控令牌集内，是 [[无需走查的代码集成]] 的前提之一。
