---
type: concept
title: exec-tool三层防护
tags: [安全, 命令执行, agent框架, 防护机制]
related: [agent-control-plane, 有意取舍边界声明, 子agent工具排除机制, 轻量级单进程agent框架, 代码安全扫描]
created: 2026-09-30
updated: 2026-09-30
sources: ["[202604241629]800行代码实现OpenClaw的Tool消息总线子Agent管理架构.html"]
---
# exec-tool三层防护

exec-tool三层防护指苏雄 800 行框架中 ExecTool（shell 命令执行工具）内建的三层安全防线：**危险命令正则黑名单 → 资源限制 → 输出截断**，全部内建于工具实现、无中间件。作者同时明示边界：正则黑名单只是最低限度的防线，不能替代沙箱隔离。

## 第一层：危险命令正则黑名单

7 条正则模式拦截高危 shell 命令（覆盖 `rm -rf /`、`rm -rf ~`、`mkfs`、`dd if=`、fork bomb、写裸设备、`chmod -R 777 /`）：

```typescript
const DANGEROUS_PATTERNS: RegExp[] = [
  /rm\s+(-[a-zA-Z]*f[a-zA-Z]*\s+)?(-[a-zA-Z]*r[a-zA-Z]*\s+)?\/($|\s)/,
  /rm\s+-[a-zA-Z]*rf?\s+~($|\/|\s)/,
  /mkfs\b/,
  /dd\s+if=/,
  /:\(\)\s*\{\s*:\|:\s*&\s*\}\s*;/,  // fork bomb
  />\s*\/dev\/[sh]d[a-z]/,            // 写入裸设备
  /chmod\s+-R\s+777\s+\//,
];
```

**绕过风险（作者明示）**：LLM 可通过变量展开、别名、管道组合绕过黑名单；生产环境应使用容器沙箱或受限用户执行。

## 第二层：资源限制

默认 30 秒超时 + 2MB `maxBuffer`；超时后进程被 kill 并返回超时提示。

## 第三层：输出截断（首尾保留）

超 10,000 字符时取首尾各 5,000、中间加截断标记。设计动机：「命令输出的末尾通常包含最有价值的信息（错误信息、统计摘要等）」——保留首尾而非只取前 N 字符：

```typescript
function truncateOutput(text: string): string {
  if (text.length <= MAX_OUTPUT_LENGTH) return text;
  const half = Math.floor(MAX_OUTPUT_LENGTH / 2);
  return (
    text.slice(0, half) +
    `\n\n--- truncated (${text.length} chars total) ---\n\n` +
    text.slice(-half)
  );
}
```

## 关联

- 从 [[agent-control-plane]] 视角看，三层防护是轻量级执行边界的下限实现，与 [[子agent工具排除机制]]（能力边界）、互斥锁串行化（并发边界）共同构成该框架的完整安全证据链。
- 与 [[代码安全扫描]]（模型调用前敏感信息扫描脱敏）互补：前者防危险执行，后者防信息泄露。
