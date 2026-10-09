---
type: entity
title: tree-sitter
tags: [tree-sitter, 解析器, AST, 代码索引, 增量解析, 开源工具]
related: [cursor, umodel, tree-sitter双用途分野, 代码索引, ast确定性提取+llm语义增强分层置信度, umodel六阶段构建流水线, 代码理解的五种范式]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604231830]从可观测到可理解用UModel构建Agent原生的代码知识图谱.html"]
---

# tree-sitter

tree-sitter 是一个开源的增量语法解析工具库，被编辑器与代码索引系统广泛用于将源代码解析为语法树。其作为被广泛使用的解析器基础设施，属于可独立核验的公开技术背景。

## 技术特性（据来源文章描述）

《从可观测到可理解用UModel构建Agent原生的代码知识图谱》的作者称其为 "PEG 增量解析器" 生成工具，可将源代码解析为具体语法树（AST）。要点如下（该描述为来源自述口径）：

- PEG 增量解析器，增量解析代价低。
- 支持 40+ 种编程语言。
- `tags.scm` 查询规则文件提供跨语言一致的提取接口。

## 在来源文章中的角色：双用途分野的核心

文章指出 [[cursor]] 等 CodeIndex 工具与 [[umodel|UModel]] 都使用 tree-sitter，但目标截然不同（详见 [[tree-sitter双用途分野]]）——"同一个解析器，服务于完全不同的目标"：

- **语义切片（[[cursor]] 路线）**：在 [[cursor]] 等 CodeIndex 工具的混合索引策略中，tree-sitter 充当切片引擎：解析出 AST → 按语义单元切片（函数、类、逻辑块）→ 生成向量 embedding → 存入向量数据库（Cursor 管线：tree-sitter → AST → 切片 → embedding → Turbopuffer → Merkle Tree）。产出是**向量**。
- **结构提取（[[umodel|UModel]] 路线）**：tree-sitter 作为 UModel AST 轨道的提取引擎，在 EXTRACT 阶段经 `tags.scm` 规则跨语言一致地提取定义/引用/结构/导入/调用/继承六类关系，全部置信度 1.0（详见 [[umodel六阶段构建流水线]]、[[ast确定性提取+llm语义增强分层置信度]]）。产出是**图谱**。

这解释了为何工具相同而结果迥异：语义切片产出向量（概率性检索），结构提取产出图谱（确定性关系）。

## 关联

- **与 [[代码索引]] 主流方案**：tree-sitter 是向量索引路线与图谱路线共用的底层解析组件，工具相同不等于范式相同。
- **与有赞 [[code-insight]]（[[ast加符号表联合分析]] 路线）的对照**：后者在 AST 之上叠加符号表解决跨文件引用，同为结构化提取方向，但未指明是否使用 tree-sitter；UModel 则以 RESOLVE 阶段做确定性跨文件解析，两者能力边界对比待成文。