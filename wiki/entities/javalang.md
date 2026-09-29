---
type: entity
title: javalang
tags: [ast, java, python, 代码解析]
related: [ast精准方法定位, 六步智能提示词生成法, 代码索引, ast加符号表联合分析]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202512261820]回收团队基于Cursor集成MCP的智能代码修复提示词生成实践.html"]
---
# javalang

纯 Python 实现的 Java 语法解析库，无需安装 JDK 即可完成 AST 解析。通过 `pip install javalang` 安装，支持 Java 8+ 语法特性。

## 在转转实践中的应用

[[回收团队]]在[[六步智能提示词生成法]]第二步中使用 javalang 解析 Java 源码 AST，按行号区间匹配定位 [[sonar]] 问题行所属方法，提取方法名、参数、返回值、修饰符等结构化上下文。核心类为 `JavaCodeParser.extract_method_context()`。

## 选型优势

- **零 Java 依赖**：纯 Python 实现，无需 JDK，降低部署门槛
- **轻量级**：仅需 `pip install javalang`
- **精准**：基于 AST 的语法级理解，不受注释/字符串/Lambda 干扰，准确率 99%+

## 跨源关联

javalang 补充了 Wiki [[代码索引]] 主题下的工具链选型。与有赞 [[ast加符号表联合分析]] 使用的 AST 技术路径相似但目标不同：有赞侧重调用关系图谱构建，转转侧重方法级上下文精准提取。