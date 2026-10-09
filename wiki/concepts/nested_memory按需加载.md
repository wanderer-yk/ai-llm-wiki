---
type: concept
title: nested_memory 按需加载
created: 2026-10-09
updated: 2026-10-09
tags: [claude-code, rules, attachments]
related: [claude-code, rules被动注入机制, rules条件生效机制, messages注入四通道, skill渐进式披露]
sources: ["[202604151800]读完ClaudeCode源码才发现SkillsMCPRules的区别远没有你想的那么大.html"]
---
# nested_memory 按需加载

nested_memory 按需加载是 Claude Code 对**子目录 CLAUDE.md** 的动态注入机制：当模型实际接触某个子目录时，客户端才检查该目录下的 CLAUDE.md 并加载，经 `nested_memory` attachment 以 `isMeta: true` 注入 messages。它是 Rules 体系内的「按需披露」实现——根级 Rules 常驻注入（[[rules被动注入机制]]），子目录 Rules 惰性加载。

## 源码片段（v2.1.88）

```javascript
case "nested_memory":
    return [createMessage({
        content: `Contents of ${attachment.content.path}:\n\n${attachment.content.content}`,
        isMeta: true
    })];
```

## 关联

- 与 `skill_listing`（[[skill列表token预算]]）同属 messages 动态附件通道（[[messages注入四通道]]）；
- 与 Agent Skills 的渐进式披露（[[skill渐进式披露]]）构成「按需披露」的两种触发方式比较素材：前者按**文件路径接触**触发，后者按**任务语义匹配**触发。
