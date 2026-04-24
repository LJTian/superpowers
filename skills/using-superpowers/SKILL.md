---
name: using-superpowers
description: 开始任何对话时使用；说明如何发现和使用技能，并要求在任何回复（包括澄清问题）前先调用相关技能
---

<SUBAGENT-STOP>
如果你是被派发来执行具体任务的子代理，跳过此技能。
</SUBAGENT-STOP>

<EXTREMELY-IMPORTANT>
如果你认为某个技能哪怕只有 1% 可能适用于当前任务，你也绝对必须调用该技能。

如果某个技能适用于你的任务，你没有选择。必须使用它。

这不可协商，也不是可选项。不要为跳过它找理由。
</EXTREMELY-IMPORTANT>

## 指令优先级

Superpowers 技能会覆盖默认系统提示中的行为，但**用户指令始终优先**：

1. **用户显式指令**（CLAUDE.md、GEMINI.md、AGENTS.md、直接请求）— 最高优先级
2. **Superpowers 技能** — 与默认系统行为冲突时覆盖默认行为
3. **默认系统提示** — 最低优先级

如果 CLAUDE.md、GEMINI.md 或 AGENTS.md 写着“不要使用 TDD”，而某个技能写着“始终使用 TDD”，遵循用户指令。用户掌控方向。

## 如何访问技能

**在 Claude Code 中：** 使用 `Skill` 工具。调用技能后，其内容会被加载并展示给你，直接遵循它。不要用 Read 工具读取技能文件。

**在 Copilot CLI 中：** 使用 `skill` 工具。技能会从已安装插件中自动发现。`skill` 工具与 Claude Code 的 `Skill` 工具作用相同。

**在 Gemini CLI 中：** 通过 `activate_skill` 工具激活技能。Gemini 在会话开始时加载技能元数据，并按需激活完整内容。

**在其他环境中：** 查看平台文档，确认技能如何加载。

## 平台适配

技能中使用 Claude Code 的工具名。非 Claude Code 平台请查看工具映射：`references/copilot-tools.md`（Copilot CLI）、`references/codex-tools.md`（Codex）。Gemini CLI 用户会通过 GEMINI.md 自动加载工具映射。

# 使用技能

## 规则

**在任何回复或行动前，先调用相关或用户要求的技能。** 即使某个技能只有 1% 可能适用，也应调用它检查。如果调用后发现不适用，就不需要继续使用。

```dot
digraph skill_flow {
    "User message received" [shape=doublecircle];
    "About to EnterPlanMode?" [shape=doublecircle];
    "Already brainstormed?" [shape=diamond];
    "Invoke brainstorming skill" [shape=box];
    "Might any skill apply?" [shape=diamond];
    "Invoke Skill tool" [shape=box];
    "Announce: 'Using [skill] to [purpose]'" [shape=box];
    "Has checklist?" [shape=diamond];
    "Create TodoWrite todo per item" [shape=box];
    "Follow skill exactly" [shape=box];
    "Respond (including clarifications)" [shape=doublecircle];

    "About to EnterPlanMode?" -> "Already brainstormed?";
    "Already brainstormed?" -> "Invoke brainstorming skill" [label="no"];
    "Already brainstormed?" -> "Might any skill apply?" [label="yes"];
    "Invoke brainstorming skill" -> "Might any skill apply?";

    "User message received" -> "Might any skill apply?";
    "Might any skill apply?" -> "Invoke Skill tool" [label="yes, even 1%"];
    "Might any skill apply?" -> "Respond (including clarifications)" [label="definitely not"];
    "Invoke Skill tool" -> "Announce: 'Using [skill] to [purpose]'";
    "Announce: 'Using [skill] to [purpose]'" -> "Has checklist?";
    "Has checklist?" -> "Create TodoWrite todo per item" [label="yes"];
    "Has checklist?" -> "Follow skill exactly" [label="no"];
    "Create TodoWrite todo per item" -> "Follow skill exactly";
}
```

## 危险信号

出现这些想法就停下，你正在合理化跳过流程：

| 想法 | 现实 |
|---------|---------|
| “这只是个简单问题” | 问题也是任务。检查技能。 |
| “我需要先了解更多上下文” | 技能检查在澄清问题之前。 |
| “我先探索一下代码库” | 技能会告诉你如何探索。先检查。 |
| “我可以快速看一下 git/文件” | 文件缺少对话上下文。检查技能。 |
| “我先收集信息” | 技能会告诉你如何收集信息。 |
| “这不需要正式技能” | 如果技能存在，就使用它。 |
| “我记得这个技能” | 技能会演进。阅读当前版本。 |
| “这不算任务” | 行动就是任务。检查技能。 |
| “这个技能太重了” | 简单事情也会变复杂。使用它。 |
| “我先做这一件小事” | 做任何事前先检查。 |
| “这样很有产出” | 无纪律行动会浪费时间。技能用于防止这种情况。 |
| “我知道它是什么意思” | 知道概念 ≠ 使用技能。调用它。 |

## 技能优先级

多个技能可能适用时，按以下顺序：

1. **流程技能优先**（brainstorming、debugging）- 它们决定如何处理任务
2. **实现技能其次**（frontend-design、mcp-builder）- 它们指导执行

“构建 X” → 先 brainstorming，再实现技能。
“修复这个 bug” → 先 debugging，再领域技能。

## 技能类型

**刚性技能**（TDD、debugging）：严格遵循。不要以“适配”为名丢掉纪律。

**弹性技能**（模式类）：把原则适配到上下文。

技能自身会说明它是哪一类。

## 用户指令

指令说明“做什么”，不等于说明“如何做”。“添加 X”或“修复 Y”不代表可以跳过工作流。
